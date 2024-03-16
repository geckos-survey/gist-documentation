# Useful information

### Folder setup
The gistTutorial folder contained within the `nGIST-supplementary-public` repo is your working directory. Within it, you will find four folders: `configFiles`, `inputData`, `results`, and `spectralTemplates`. 

- `configFiles`: contains your `MasterConfig.yaml` file, as well as some files describing the LSF of your chosen instrument and SSP template set. Also included are some files describing the emission lines to fit, and the line strengh indices. 

- `inputData`: contains the IFS datacube that you wish to analyse and its associated spatial mask.

- `results`: this is the folder that your results will save in to.

- `spectralTemplates`: Place the SSP models that you wish to use in this folder. 

### Example data 
The gistTutorial folder comes with the default NGC0000.fits and NGC0000_mask.fits files. These are small MUSE data cubes which may be used for initial testing because they will run quicker.

### Basic usage
nGIST takes the settings from your input MasterConfig file and feeds them into the nGIST pipeline. Therefore, all you need to edit is the MasterConfig.yaml file before each run. 