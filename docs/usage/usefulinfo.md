# Useful information

### Folder setup
The ngistTutorial folder contained within the `nGIST-supplementary-public` repo is your working directory. Within it, you will find four folders: `configFiles`, `inputData`, `results`, and `spectralTemplates`. 

- `configFiles`: contains your `MasterConfig.yaml` file, as well as some files describing the LSF of your chosen instrument and SSP template set. Also included are some files describing the emission lines to fit, and the line strengh indices. 

- `inputData`: contains the IFS datacube that you wish to analyse and its associated spatial mask.

- `results`: this is the folder that your results will save in to.

- `spectralTemplates`: Place the SSP models that you wish to use in this folder. 

### Example data 
The gistTutorial folder comes with the default NGC0000.fits and NGC0000_mask.fits files. These are small MUSE data cubes which may be used for initial testing because they will run quicker.

### Basic usage
nGIST takes the settings from your input MasterConfig file and feeds them into the nGIST pipeline. Therefore, all you need to edit is the MasterConfig.yaml file before each run. 

### Stellar templates and SSPs
nGIST is provided with the MILES spectral templates and associated read-in routines, and we thank Alex Vazdekis for giving his permission to do so. We note that nGIST has also been tested with eMILES, SMILES, IndoUS, XShooter XSL, BC03, Walcher, and the Conroy alphaMC stars/templates.

### Instruments 
nGIST is most exhaustively developed and tested for use with MUSE data, in both wide-field (nominal and extended wavelength ranges) and narrow-field mode. It has also been tested on CALIFA, SAMI, and MaNGA data, though these instruments are not (yet) publicly supported. In theory, it should be easy to write your own read-in routine for your favourite integral field spectrograph. If this is something that interests you, please get in touch with the lead developers, who are happy to support such an endeavour, and would love to keep up-to-date with community development efforts.
