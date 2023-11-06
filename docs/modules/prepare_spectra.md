# Prepare spectra

## Purpose

The purpose of the module is to prepare the spectra for the analysis, in particular by applying the binning pattern to the spectra and log-rebinning them. Note that the natural logarithm is used. gist-geckos is currently equipped with one routine to prepare the observed spectra for the analysis. This routine is included in the source code and can readily be used by setting the configuration parameter `PREPARE_SPECTRA: METHOD` to `'default'`.

In particular, it

- Applies the spatial binning pattern to the linearly binned spectra and saves them

- Logarithmically-rebins the spectra and saves them; Note that the natural logarithm is used

- Applies the spatial binning pattern to the logarithmically binned spectra and saves them

## Config file input 
```py
PREPARE_SPECTRA :
  METHOD : 'default'
  VELSCALE : 41 # [km/s]. Velocity scale of the spectrum.
```

## Output

### Files
- `_AllSpectra.fits`, Extension 1:

    - Contains the spectrum of each spaxel.

    - Columns: `SPEC` Spectra | `ESPEC` Variance spectra

    - Rows: One line per spaxel.

    - Header: The header in extension 0 must contain the keyword `VELSCALE` which defines the width of one spectral pixel in velocity space.

- `_AllSpectra.fits`, Extension 2:

    - Columns: `LOGLAM` The corresponding wavelength array

- `_BinSpectra.fits`, Extension 1:

    - Contains the Voronoi-binned spectra equally spaced in velocity domain (logarithmically binned).

    - Columns: `SPEC` Spectra | `ESPEC` Variance spectra

    - Rows: One line per bin.

    - Header: The header in extension 0 must contain the keyword `VELSCALE` which defines the width of one spectral pixel in velocity space.

- `_BinSpectra.fits`, Extension 2:

    - Columns: `LOGLAM` The corresponding wavelength array

    - `_BinSpectra_linear.fits`, Extension 1:

    - Contains the Voronoi-binned spectra equally spaced in wavelength domain (linearly binned).

    - Columns: `SPEC` Spectra | `ESPEC` Variance spectra

    - Rows: One line per bin.

- `_BinSpectra_linear.fits`, Extension 2:

    - Columns: `LOGLAM` The corresponding wavelength array

### Function Returns

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.