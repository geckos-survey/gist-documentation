# Spatial masking

## Purpose
nGIST is currently equipped with one routine to mask spatial regions in the IFU cube. This routine is included in the source code and can readily be used by setting the configuration parameter `SPATIAL_MASKING: METHOD` to default. This routine performs three tasks:

- It masks defunct spaxels, i.e. spaxels with negative mean values or those containing np.nan’s.

- It applies a signal-to-noise threshold. In particular, it masks all spaxels below the isophote level which has a mean signal-to-noise ratio of `SPATIAL_MASKING: MIN_SNR`.

- It applies a manually defined mask. In particular, it reads a fits-file which path is given by the parameter `SPATIAL_MASKING:MASK` (path relative to `GENERAL:INPUT`; set to False to turn this option off). The file is expected to have the same spatial dimensions as the input IFU cube. Unmasked spaxels should have the value 0, while masked spaxels should have the value 1.

## Config file input

```py
SPATIAL_MASKING :
  METHOD : 'default' #'default' # Name of the routine in spatialMasking/ (without .py) to perform the tasks. 'default' to use the standard nGIST implementation.
  MIN_SNR : 1.5 # Spaxels below the isophote level which has this mean signal-to-noise level are masked.
  MASK : 'NGC0000_mask.fits' # File containing a spatial mask (Set 'False' to not include a file).
  THRESHOLD_METHOD : 'isophote' # Optional keyword to further define how the masking is performed. Default = 'isophote'. Options include 'actual'(use the actual snr map + a minimum snr), or 'smooth' (use a smoothed version of the snr map + minimum snr)

```

## Output 

### Files

- `*_mask.fits`: Contains information on the spatial masking.

    - Columns: `MASK` Combination of all masks | `MASK_DEFUNCT` Mask based on defunct spaxels | `MASK_SNR` Mask based on signal-to-noise criterion | `MASK_FILE` Mask based on file

    - Rows: One line per spaxel.


### Function returns 

The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.
