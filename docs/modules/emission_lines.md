# Emission lines  

## Purpose 

This module performs a full emission-line analysis of the observed spectra. nGIST is currently equipped with three routines that can readily be used by setting the configuration parameter `GAS: METHOD` to `ppxf`, or `gandalf`.

### `pPXF`
Simultaneously fits the stellar continuum (using a set of SSPs) and emission lines (using a set of single-component Gaussian templates) to the spectra. This method is significantly faster than the gandalf method, and hence is the recommended method. This module is based on the PHANGS DAP emission line module, but includes the option of a 3-step sigma-clipping technique to accurately characterise the noise, and reject spurrious pixels. 

### `gandalf` (previously `MAGPI-gandalf`)
This routine employs a Python translation of the original GandALF routine by Sarzi et al. (2006) and Falcon-Barroso et al. (2006). The `gandalf` routine implemented by the original GIST pipeline has been updated by the MAGPI team for nGIST, with changes aimed at reducing the number of spectra for which the galdalf fit fails, particularly at low signal-to-noise ratio. Their fix was to take an MC approach to fitting by making slight changes to the starting guesses for the kinematic parameters. The new routine creates multiple
starting guesses and takes the minimum χ2 of these. The initial starting guess is a 3×3 matrix of guesses around the stellar kinematics values. While testing is continuing, initial results are promising: the
new version of Gandalf seems to fit more bins (and in a sensible manner). However, the MC approach slows this module down considerably. 

---

For all cases, the routine can be run on `BIN` and `SPAXEL` level data (configuration parameter `GAS: LEVEL`). With both these options, the analysis is performed using the full set of spectral templates. Alternatively, with the configuration parameter `BOTH`, a bin-level run using the full set of spectral templates is performed. Thereafter, a spaxel-level run is performed, using for each spaxel the optimal template derived in the bin-level run (of the bin the corresponding spaxel belongs to) instead of the full set of spectral templates.

## Config file input 

```py
GAS :
  METHOD : 'ppxf' # options are 'ppxf', 'gandalf' or 'MAGPI_gandalf' Name of the routine in emissionLines/ (without .py) to perform the tasks. Set 'False' to turn off module. 
  SPEC_MASK : 'specMask_GAS' # Text file containing the wavelength ranges to be masked.
  LEVEL : 'BIN' # Set 'BIN' to perform the analysis bin-by-bin, 'SPAXEL' for a spaxel-by-spaxel analysis, and 'BOTH' to perform a spaxel-by-spaxel analysis that is informed by a previous bin-by-bin analysis.
  LMIN : 4750 # Rest-frame wavelength range used for the emission-line analysis [in Angst.]
  LMAX : 7100
  ERRORS : 0 # Derive errors on the emission-line analysis (0 No / 1 Yes). Note: Due to limitations in pyGandALF, this is currently not possible. We expect a new pyGandALF version to be published soon.
  REDDENING : False # Include the effect of reddening by dust in the pyGandALF fit. Put in the form 0.1,0.1 without any spaces. If you intend to use multiplicative polynomials instead, set REDDENING to 'False' and add a MDEG keyword in the GAS section to set the polynomial order. Additive polynomials cannot be used with pyGandALF.
  MDEG : 8 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials.
  FIXED : True # Fix the stellar kinematics to the results obtained with the stellar kinematics module [True / False]
  MOM : 2 # Gas moments. Set to 2 for V and sigma. Higher orders not yet tested
  EBmV : null # De-redden the spectra for the Galactic extinction in the direction of the target previously to the analysis. Use e.g. EBmV = A_v / 3.2
  EMI_FILE : 'emissionLines_ppxf.config' # Emission line set-up file for emline fitter. The specified path is relative to the configDir path in defaultDir. Set to 'emissionLines_ppxf.config' when using the 'ppxf' routine, and 'emissionLines_gandalf.config' when using the 'gandalf' routine.
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS'
  LIBRARY : 'MILES_EMLINES/' # options are 'MILES_EMLINES/' (contains asmaller subset of templates sim to that used by PHANGS) 'MILES/', 'miles_ssp_ch', or 'IndoUS'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
```

## Output 

### Files

All outputs can be available at the BIN and SPAXEL level.

- `_gas_bin.fits`: Extension 1:

    - Columns: [LineName][LineWavelength]_*: F Flux; A Amplitude; V Velocity; S Sigma| EBmV_0 Screen-like extinction from the stellar continuum | EBmV_1 Extinction in emission-line regions from Balmer decrement

    - Rows: One line per bin/spaxel.

- `_gas_bin.fits` Extension 2:

    - Columns: Same as in extension 1, but for errors

    - Rows: One line per bin/spaxel.

- `_gas-bestfit_bin.fits`, Extension 1:

    - The best fit to the spectrum, including continuum and emission lines

    - Columns: BESTFIT The best fit to the spectrum

    - Rows: One fit per bin/spaxel.

- `_gas-bestfit_bin.fits`, Extension 2:

    - Columns: LOGLAM The corresponding wavelength array

- `_gas-bestfit_bin.fits`, Extension 3:

    - Columns: GOODPIX The spectral pixels included in the fit

- `_gas-cleaned_bin.fits`: Extension 1:

    - Columns: SPEC Emission-line subtracted spectra

    - Rows: One line per bin/spaxel.

- `_gas-cleaned_bin.fits`: Extension 2:

    - Columns: `LOGLAM`: The corresponding wavelength array

- `_gas_bin_maps.fits`

    - 2D maps images of various emission line quantities for each line.

    - Extensions: [LineName][LineWavelength]_*: `FLUX`, `FLUX_ERR`, `VEL`, `VEL_ERR`, `SIGMA`, `SIGMA_ERR`, `SIGMA_CORR`

### Function returns

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.
