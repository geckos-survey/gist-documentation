# Read data 

## Purpose

Read the input data, for instance the IFU cube, and return the relevant information in a dictionary. Such a read-in routine has to perform the following tasks:

- Read spectra and error spectra

- Get the wavelength information

- Get the spatial coordinates

- Shift the spectra to restframe, according the configuration parameter GENERAL|REDSHIFT (Note that this is required for user-defined read-in routines, in order to work consistently with the subsequent modules).

- Shorten the wavelength range of the spectra

- Compute a signal-to-noise ratio for each spaxel

- Determine the velscale of the spectra

Currently, gist-geckos has been tested only with the following read in routine(s):
 - `MUSE_WFM` The widefiels mode of the MUSE spectrograph without adaptive optics.

## Config file input

 ```py
 READ_DATA :
  METHOD : 'MUSE_WFM' # Name of the routine in readData/ (without .py) to be used to read-in the input data.
  DEBUG : FALSE # Switch to activate debug mode [True/False]: Pipeline runs on one, central line of pixels. Keep in mind to clean output directory after running in DEBUG mode!
  ORIGIN : 222,223 #217,218 # Origin of the coordinate system in pixel coordinates: x,y (Indexing starts at 0).
  LMIN_TOT : 4800 # Spectra are shortened to the rest-frame wavelength range defined by LMIN_TOT and LMAX_TOT. Note that this wavelength range should be longer than all other wavelength ranges supplied to the modules [in Angst.]
  LMAX_TOT : 7000
  LMIN_SNR : 4800 # Rest-frame wavelength range used for the signal-to-noise calculation [in Angst.]
  LMAX_SNR : 7000
  EBmV : 0.104575 # [mag] Galactic foreground dust reddening correction for the RA, DEC of the target. Or set to null for no redenning correction.
 ```

## Output

### Files
None

### Function returns 

- `x` x-coordinate for each spaxel [array, size=(nspaxel,)]

- `y` y-coordinate for each spaxel [array, size=(nspaxel,)]

- `wave` Wavelength array [array, size=(nspecs,)]

- `spec` Spectra [array, size=(nspecs,nspaxel)]

- `error` Error spectra [array, size=(nspecs,nspaxel)]

- `snr` Signal-to-noise ratio for each spaxel [array, size=(nspaxel,)]

- `signal` Signal in each spaxel [array, size=(nspaxel,)]

- `noise` Noise in each spaxel [array, size=(nspaxel,)]

- `velscale` Spectral pixelsize in velocity space [float]

- `pixelsize` Spatial pixelsize of the instrument [float]

(nspaxel denotes the total number of spatial pixels; nspecs denotes the total number of spectral pixels)