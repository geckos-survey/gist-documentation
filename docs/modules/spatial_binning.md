# Spatial binning

## Purpose 

The purpose of the module is to spatially bin the data, for instance in order to obtain an approximately constant signal-to-noise ratio throughout the field-of-view. Alternatively, one could use this step in the workflow to isolate spectra of specific features, e.g. globular cluster or planetary nebulae, and continue the analysis only with these. In this way the GIST could simply be turned into a specific tool for the analysis of globular clusters or planetary nebulae.

nGIST is currently equipped with one routine to spatially bin the data. This routine is included in the source and exploits the adaptive Voronoi tesselation routine of Cappellari & Copin (2003). It can readily be used by setting the configuration parameter `SPATIAL_BINNING:METHOD` to `'voronoi'`.

In addition, the module determines the closest valid Voronoi bin for all masked spaxels. These are assigned a negative BIN_ID, with the absolute value stating the BIN_ID of the closest valid Voronoi bin. A BIN_ID 0 is a valid Voronoi bin, in particular the one containing the spaxel with the highest signal-to-noise ratio.

The routine finally saves a tables that defines which spaxel belongs to which bin, including basic properties of bins and spaxels, such as position, flux, signal-to-noise, etc.

## Config file input

```py
SPATIAL_BINNING :
  METHOD : 'voronoi' # Name of the routine in spatialBinning/ (without .py) to perform the tasks. Set 'False' to turn off module. Set 'voronoi' to use the standard GIST implementation, exploiting the Voronoi tesselation routine of Cappellari & Copin (2003).
  TARGET_SNR : 500.0 # Target signal-to-noise ratio for the Voronoi binning
  COVARIANCE : 0.0 # Correct for spatial correlations of the noise during the Voronoi binning process according to the empirical equation SNR /= 1 + COVAR_VOR * np.log10(NSPAXEL) with NSPAXEL being the number of spaxels per bin (see e.g. Garcia-Benito et al. 2015).
```

## Output 

### Files

- `_table.fits`: Contains information to reconstruct the spatial binning pattern. Most importantly, it defines the `BIN_ID`, that assigns each spaxel to a bin.

    - Columns: `ID` Spaxel ID | `BIN_ID` Bin ID | `X` `Y` Pixel coordinates of spaxels | `XBIN` `YBIN` Pixel coordinates of bins | `FLUX` Flux in the spaxel | `SNR` Signal-to-noise ratio in the spaxel | `SNRBIN` Signal-to-noise ratio in the bin | `NSPAX` Number of spaxels in the bin

    - Rows: One line per spaxel.

    - Header: The header of extension 0 has to contain the entry `PIXSIZE` defining the angular size of one spatial pixel.

### Function Returns
The module should end with `return(None)`. If an uncaught exception occurs in the module, all following modules will be skipped for this galaxy. If you intend to manually skip all subsequent modules, simply raise an exception.