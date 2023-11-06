# Stellar populations

## Purpose 

The starFormationHistories module derives non-parametric star formation histories from the observed spectra. gist-geckos is currently equipped with one routine that can readily be used by setting the configuration parameter `SFH: METHOD` to `'ppxf'`. In particular, this routine employs the pPXF routine of Cappellari & Emsellem (2004).

There are several options for the treatment of emission lines in this module:

- To run the SFH module using the cleaned, emission-line subtracted spectra, you simply need to make sure that you ran the emission line module previously (i.e. set `GAS: METHOD: 'ppxf'`)
- To run the SFH module using the original (un-subtracted) spectrum, run the SFH module with the GAS module turned off (i.e. set `GAS: METHOD: False`).
- To run the SFH module and mask all emission lines, use `SFH: SPEC_MASK: 'specMask_KIN'`
- To run the SFH module and not mask any emission lines, use `SFH: SPEC_MASK: 'specMask_SFH'`.

## Config file input 

```py
SFH :
  METHOD : 'ppxf' # Name of the routine in starFormationHistories/ (without .py) to perform the tasks. Set 'False' to turn off module. Set 'ppxf' to use the standard GIST implementation, exploiting the pPXF routine of Cappellari & Emsellem (2004).
  LMIN : 4750 # Rest-frame wavelength range used for the star formation histories analysis [in Angst.]
  LMAX : 5500
  SPEC_MASK : 'specMask_SFH' # File to define wavelength ranges to be masked during the star formation histories analysis. The specified path is relative to the configDir path in defaultDir.
  MOM : 4 # Number of kinematic moments to be extracted. If you use FIXED = True, this should be same number of moments used to extract the stellar kinematics before. Otherwise the parameter can be set independently.
  MDEG : 12 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials. Note that additive Legendre polynomials cannot be used for this module.
  REGUL_ERR : 1000000. # Regularisation error for the regularised run of pPXF. Note: Regularisation = 1 / REGUL_ERR
  NOISE : 1. # Set a wavelength independent noise vector to be passed to pPXF.
  FIXED : True # Fix the stellar kinematics to the results obtained with the stellar kinematics module [True / False]. If 'False', please provide an initial guess on the velocity dispersion of the systems [in km/s] by adding the parameter SIGMA.
  MC_PPXF : 0 # Added to add the option of running MC on the SFH module
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS (not recc.)'
  LIBRARY : 'MILES_SFH/' # options are 'MILES/', 'miles_ssp_ch/', or 'IndoUS/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
  DOCLEAN : True # Keyword to turn on/off the sigma clipping. Set to 'False' for testing.
```

## Output 

### Files

- `_sfh.fits`:

    - Columns: `AGE` Age | `METAL` Metallicity | `ALPHA` Alpha enhancement | `V SIGMA H3 H4` Stellar kinematics | `FORM_ERR_*` Formal errors on the stellar kinematics

    - Rows: One line per bin.

- `_sfh-weights.fits`: The weights assigned to each template during the fit

    - Columns: `WEIGHTS` The weights

    - Rows: One line per bin.

- `_sfh-bestfit.fits`, Extension 1:

    - The best fit to the spectrum

    - Columns: `BESTFIT` The best fit to the spectrum

    - Rows: One fit per bin.

- `_sfh-bestfit.fits`, Extension 2:

    - Columns: `LOGLAM` The corresponding wavelength array

- `_sfh-bestfit.fits`, Extension 3:

    - Columns: `GOODPIX` The spectral pixels included in the fit

### Function returns

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.