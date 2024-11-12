# Configuration

### Working directory
While the nGIST pipeline is installed in the system and can be executed from anywhere, directories containing the input, output, configuration, and spectral templates have to be defined. In the example working directory this is done with the file `defaultDir` in which the absolute paths to these directories should be defined. The path to this file needs to be supplied to the mGIST pipeline with the command-line argument `default-dir`.

An overview of these directories and some important files is provided below:

```py
workingDirectory/
|
|- inputData/
|  |- NGC0000.fits
|  |_ NGC0000_mask.fits
|
|- spectralTemplates/
|  |- MILES/
|  |_ MILES_KB_LIS8.4.fits
|
|- configFiles/
|  |- MasterConfig.yaml
|  |- defaultDir
|  |- lsf_MUSE-WFM
|  |- lsf_MILES
|  |- emissionlinesPHANGS.config
|  |- lsBands.config
|  |- specMask_KIN
|  |_ specMask_SFH
|
|_ results/

```

The directory `inputData/` contains all the input data. Which input data is used by the pipeline is stated in `MasterConfig.yaml` (see below).

The directory `results/` contains the outputs of the GIST pipeline. The results from different runs of the pipeline are located in different subdirectories of `results/`. The name of these subdirectories is specified in `MasterConfig.yaml` (see below).

The directory `spectralTemplates/` can contain various spectral template libraries in different subdirectories (e.g. `MILES/`). In addition, if one intends to match a set of measured absorption line strength indices to single stellar populations (SSP), it is necessary to supply a fits-file with the same name as the spectral template library (e.g. `MILES.fits`). This file has to provide the predicted line strength indices for the various SSPs in the library.

The directory `configFiles/` contains all relevant configuration files. These are in particular:

`MasterConfig.yaml`: This file provides the main configuration of the pipeline. It defines which input data and spectral template library is used and which analysis modules will be executed. It further specifies the main parameters for this analysis, for instance the target signal-to-noise ratio of the Voronoi binning and the rest-frame wavelength range in consideration. The main configuration file is passed to the pipeline with the command-line argument --config (see Running the Pipeline).

`defaultDir`: This file defines the absolute paths to the input, output, configuration, and spectral template directories. It must be passed to the pipeline with the command-line argument `--default-dir` (see Running the Pipeline).

`lsf_[Instrument]`: This file defines the line-spread-function of the used instrument.

`lsf_[TemplateLibrary]`: This file defines the line-spread-function of the used template library.

`emissionLinesPHANGS.config`: This file specifies the various emission-lines to be fitted or masked with the emissionLines module. Note that this configuration file only affects the emissionLines module.

`lsBands.config`: This file states the wavelength bands of the line strength indices measured by the lineStrengths module. It further defines which indices to consider for the conversion of line strength indices to stellar population properties. Note that this configuration file only affects the lineStrengths module.

`specMask_KIN`: This files defines spectral regions to be masked during the stellarKinematics analysis. All sky lines and emission lines are masked in this file.

`specMask_SFH`: This files defines spectral regions to be masked during the starFormationHistories analysis. Only sky lines are masked in this file (i.e. emission lines are not masked.)

The files `emissionLinesPHANGS.config`, `lsBands.config`, `specMask_KIN` and `specMask_SFH` can be different for different runs of the pipeline. Therefore, the files provided in `configFiles/` should only be considered as templates. The used versions of these files always need to be defined in the corresponding parameters in `MasterConfig.yaml`.

A detailed description of each configuration file is also provided.

