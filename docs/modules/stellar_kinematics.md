# Stellar kinematics

## Purpose 
The `stellarKinematics` module measures the stellar kinematics of the observed spectra. nGIST is currently equipped with one routine that can readily be used by setting the configuration parameter `KIN: METHOD` to `'ppxf'`. In particular, this routine employs the pPXF routine of Cappellari and Emsellem (2004). More precisely:

- Initial guesses: The used initial guess for the stellar velocity is 0 (as all spectra have been shifted to rest-frame). The initial guess for the velocity dispersion is defined in the configuration parameter `KIN: SIGMA`. Alternatively, one can define initial guesses for all bins separately, i.e. by supplying the file `*_kin-guess.fits` in the output directory before the run. This file must contain a column `V` and `SIGMA` (as in the output file `*_kin.fits`), defining an initial guess for each spatial bin.

- A spectral mask is applied, according to the masking file defined by `KIN: SPEC_MASK`. See Configuration for details.

- The module calculates the stellar velocity, sigma, and higher-order velocity moments (depending on how many are specified, currently up to 6 have been tested). An iterative sigma-clipping process can be turned on to clean the spectra.

- A set of SSP templates must be input. Currently, nGIST has been tested on the MILES stars and SSPs, eMILES, SMILES libraries, the IndoUS library, the X-shooter stellar library, and Walcher+2009 templates. Only the MILES SSPs are included in the default nGIST distribution, though any other library can be employed by writing a read-in module for it, following the example in your `/ngist/ngistPipeline/prepareTemplates/miles.py` directory. 

## Config file input 

```py
KIN :
  METHOD : 'ppxf' # Name of the routine in stellarKinematics/ (without .py) to perform the tasks. Set 'False' to turn off module. Set 'ppxf' to use the standard GIST implementation, exploiting the pPXF routine of Cappellari & Emsellem (2004).
  SPEC_MASK : 'specMask_KIN' # File to define wavelength ranges to be masked during the stellar kinematics fit. The specified path is relative to the configDir path in defaultDir.
  LMIN : 4750 # Rest-frame wavelength range used for the stellar kinematics analysis [in Angst.]
  LMAX : 7100
  SIGMA : 100 # Initial guess of the velocity dispersion of the system [in km/s]
  MOM : 4 # Number of kinematic moments to be extracted
  ADEG : 12 # Degree of the additive Legendre polynomial. Set '-1' to not include any additive polynomials
  MDEG : 0 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials
  REDDENING : null # Initial guess on the stellar reddening E(B-V), in order to measure the stellar reddening. Note: This cannot be used together with multiplicative polynomials.
  MC_PPXF : 0 # Number of Monte-Carlo simulations to extract errors on the stellar kinematics. Formal errors are saved in any case.
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # 
  LIBRARY : 'MILES/' # options are 'MILES/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
  DOCLEAN : True # Keyword to turn on/off the sigma clipping. Set to 'False' for testing.
```

## Output 

### Files

- _kin.fits: Results of the extraction of stellar kinematics and their errors

    - Columns: `V SIGMA H3 H4 etc` Stellar kinematics. The number of columns here will correspond to value of the `MOM` keyword. For example, `MOM: 4` will return `V SIGMA H3 H4`, while `MOM:6` will return `V SIGMA H3 H4 H5 H6`  | `ERR_*` Errors on the stellar kinematics from MC-simulations | `FORM_ERR_*` Formal errors on the stellar kinematics | `REDDENING` Extinction calculated by PPXF

    - Rows: One line per bin.

- _kin-bestfit.fits, Extension 1:

    - The best fit to the spectrum

    - Columns: `BESTFIT` The best fit to the spectrum

    - Rows: One fit per bin.

- _kin-bestfit.fits, Extension 2:

    - Columns: `LOGLAM` The corresponding wavelength array

- _kin-bestfit.fits, Extension 3:

    - Columns: `GOODPIX` The spectral pixels included in the fit

- _kin-optimalTemplates.fits: Extension 1

    - Optimal template for the fit on each bin.

    - Columns: `OPTIMAL_TEMPLATES` Optimal templates

    - Rows: One template per bin.

- _kin-optimalTemplates.fits: Extension 2

    - Columns: `LOGLAM_TEMPLATE` The corresponding wavelength array

### Function returns

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.
