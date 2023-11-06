# Line strengths

## Purpose 

The `lineStrengths` module measures line strength indices from the observed spectra. gist-geckos is currently equipped with one routine that can readily be used by setting the configuration parameter `LS: METHOD` to `'default'`. In particular, this routine employs a Python translation of the line strength measurement routine of Kuntschner et al. (2006) and the routine of Martin-Navarro et al. (2018) in order to match the measured line strength indices to single stellar population equivalent population properties.

## Config file input 

```py
LS :
  METHOD : False #'default' # Name of the routine in lineStrengths/ (without .py) to perform the tasks. Set 'False' to turn off module. Set 'default' to use the standard GIST implementation, exploiting the routines of Kuntschner et al. (2006) and Martin-Navarro et al. (2018).
  TYPE : 'SPP' # Set 'LS' to only measure line strength indices, or 'SPP' to also match these indices to stellar population properties.
  LS_FILE : 'lsBands.config' # File to define the wavelength band of the line strength indices to be measured. The specified path is relative to the configDir path in defaultDir.
  CONV_COR : 8.4 # Spectral resolution [in Angst.] at which the line strength indices are measured.
  SPP_FILE : 'MILES_KB_LIS8.4.fits' # File providing predictions on line strength indices for a set of single stellar population models
  MC_LS : 30 # Number of Monte-Carlo simulations in order to obtain errors on the line strength indices. Note: This must be turned on.
  NWALKER : 10 # Number of walkers for the MCMC algorithm (used for the conversion of indices to population properties)
  NCHAIN : 100 # Number of iterations in the MCMC algorithm (used for the conversion of indices to population properties)
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS'
  LIBRARY : 'MILES/' # options are 'MILES/', 'miles_ssp_ch/', or 'IndoUS/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
```

## Output 

### Files

All outputs are available in the `OrigRes` and `AdapRes` versions, as derived at the original and adapted spectral resolution of the spectra.

- `_ls.fits`, Extension 1: Line strength indices and their errors as estimated from MC-simulations

    - Columns: `Fe5015` Line strength index, names as defined in `lsBands.config` | `ERR_*` Errors on the line strength indices as estimated from MC-simulations

    - Rows: One line per bin

- `_ls.fits`, Extension 2: Single stellar population equivalent population properties as estimated with a MCMC algorithm from the line strength indices

    - Columns: `[lineLabel]_PERCENTILES` Percentiles of the resulting MCMC distribution for the line denoted [lineLabel] in `lsBands.config`

    - Rows: One line per bin

- `_ls-cleaned_linear.fits`: Extension 1:

    - Emission-subtracted, linearly binned spectra

    - Columns: `SPEC` Emission-subtracted, linearly binned spectra | `ESPEC` Variance spectra

    - Rows: One line per bin.

- `_ls-cleaned_linear.fits`: Extension 2:

    - Columns: `LAM` The corresponding wavelength array

### Function returns

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.