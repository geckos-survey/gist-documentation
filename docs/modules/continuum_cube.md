# Continuum cube creation

## Purpose 
Create continuum-only and continuum-subtracted line-only cubes. nGIST is currently equipped with one routine that can readily be used by setting the configuration parameter `CONT: METHOD` to `'ppxf'`. In particular, this routine employs the pPXF routine of Cappellari and Emsellem (2004). pPXF is run with multiplicative polynomials (more appropriate to match the shape of the spectra), and the bestfit saved as the continuum cube. Subtracting the continuum cube from the original spectrum gives the line-only cube. 

**Fitting procedure**

This module uses a multi-step fitting approach to determine the best-fit parameters. nGIST first determines an optimal template set (`OPT_TEMP`, optional), measures and then applies (optional) a dust correction (`DUST_CORR`), rescales the noise, and cleans (`DOCLEAN`, optional) the spectrum of outliers (e.g. >3σ noise, sky lines, and emission lines).

To speed up the overall fitting process, the `OPT_TEMP` keyword can be used to pass a subset of templates to pPXF during the preparation steps (`DUST_CORR`, noise rescaling, and `DOCLEAN`). Note that in the final fit (used to determine the best-fit parameters), all templates are always provided. If `OPT_TEMP` is set to `'galaxy_single'` or `'galaxy_set'`, pPXF is first run once on a single mean spectrum of the entire datacube to identify templates with non-zero weights. For `'galaxy_single'`, these templates are combined into a single ‘optimal’ template, whereas `'galaxy_set'` passes all non-zero-weight templates to pPXF during the preparation steps. If `OPT_TEMP` is set to `'default'`, the optimal template set from the dust preparation step is used.

In the dust preparation step, nGIST determines a best-fit dust vector following the procedure described in Cappellari (2023). No additive or multiplicative polynomials are used in this fit. If `DUST_CORR: True`, all consecutive fits will use this dust vector within pPXF as a fixed component.

Next, a new estimate of the noise is derived by fitting the spectrum using pPXF with a constant noise vector. The input noise vector (either variance or constant) is rescaled using the bi-weight estimate (~standard deviation) of the fit residuals (data minus the best-fit model).

If `DOCLEAN: True`, we use the `clip_outlier` function to remove all 3σ outliers based on the best-fit from the noise estimation step.

In the final step, pPXF is run with all templates to determine the best-fit parameters.

**Notes**
-	The `DEBUG` keyword within the module can be used to fit specific bin(s), which can be useful when debugging. This option only works if `PARALLEL` is set to `False` in `GENERAL`.
-	The `PLOT` keyword within the module can be used to provide two figures for each spaxel. The first figure (step 1) shows the spectrum (black), the best-fit (red), and residuals (green) from the noise preparation step, whereas the second figure (step 3) shows the results of the final fit. Here, the vertical grey lines are the input mask, and the vertical pink lines show the spectral regions masked by `DOCLEAN = True`.
Using `PLOT = True` will slow down the fit significantly and will create a large number of files if you are fitting many bins.

## Config file input 
```py
# Continuum Cube module
CONT :
  METHOD : 'ppxf' # Name of the routine in stellarKinematics/ (without .py) to perform the tasks. Set 'False' to turn off module. Set 'ppxf' to use the standard GIST implementation, exploiting the pPXF routine of Cappellari & Emsellem (2004).
  SPEC_MASK : 'specMask_KIN' # File to define wavelength ranges to be masked during the stellar kinematics fit. The specified path is relative to the configDir path in defaultDir.
  LMIN : 4750 # Rest-frame wavelength range used for the stellar kinematics analysis [in Angst.]
  LMAX : 7100
  SIGMA : 100 # Initial guess of the velocity dispersion of the system [in km/s]
  MOM : 4 # Number of kinematic moments to be extracted
  ADEG : 0 # Degree of the additive Legendre polynomial. Set '-1' to not include any additive polynomials
  MDEG : 8 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials
  REDDENING : null # As opposed to None. Initial guess on the stellar reddening E(B-V), in order to measure the stellar reddening. Note: This cannot be used together with multiplicative polynomials.
  MC_PPXF : 0 # Number of Monte-Carlo simulations to extract errors on the stellar kinematics. Formal errors are saved in any case.
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS' or 'walcher'
  LIBRARY : 'MILES/' # options are 'MILES/', 'miles_ssp_ch/', 'IndoUS', and 'Walcher/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
  DOCLEAN : True # Keyword to turn on/off the sigma clipping. Set to 'False' for testing.
  OPT_TEMP : 'default' # Keyword to select Optimal Template method. Options are 'default', 'galaxy_single', 'galaxy_set'
  DUST_CORR : True # True/False, fixes the dust in the final pPXF fit 
  NOISE : 'variance' # Keyword to set noise to be used in pPXF run. Option 'constant' and 'variance'
  PLOT : False # produce extra output plots
  DEBUG_BIN : False # Optional Keyword - array of bins [1,2,3] - only works when parallel = False. 
```

## Output 

### Files

- `_orig_cube.fits`, Extension 1:
    - The original cube, trimmed to the wavelength range defined by the `CONT: LMIN` and `CONT: LMAX` keywords. 

- `_cont_cube.fits`, Extension 1:
    - The bestfit to the continuum derived from full spectral fitting.

- `_line_cube.fits`, Extension 1:
    - The line-only cube, derived by original cube - continuum cube.

### Function returns

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.
