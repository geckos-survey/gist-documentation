# Continuum cube creation

## Purpose 
Create continuum-only and continuum-subtracted line-only cubes. nGIST is currently equipped with one routine that can readily be used by setting the configuration parameter `CONT: METHOD` to `'ppxf'`. In particular, this routine employs the pPXF routine of Cappellari and Emsellem (2004). pPXF is run with multiplicative polynomials (more appropriate to match the shape of the spectra), and the bestfit saved as the continuum cube. Subtracting the continuum cube from the original spectrum gives the line-only cube. 

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
