# Installation

The pipeline is available via the [GECKOS Survey Github page](https://github.com/geckos-survey). There are two repositories here, the gist-geckos repo, which contains the gist-geckos code itself, and the gist-geckos-supplementary repo, which contains some extra examples to get you started.

### Cloning repositories 
Clone the `gist-geckos` and `gist-geckos-supplementary` repos to your local machine using the SSH tab[^1]
[^1]: If you haven't already, create an ssh key using ssh-keygen using the instructions [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

`git clone git@github.com:geckos-survey/gist-geckos.git`

`git clone git@github.com:geckos-survey/gist-geckos-supplementary.git`

### Setting up your python environment 
(optional, but strongly recommended)
Setup a separate python environment to run gist-geckos in. The `gistenv.yaml` file located in the `gist-geckos-supplementary` repo can be used to install all of the dependencies and specified versions into a conda environment using the following call:

```py
conda env create -f gistenv.yaml
```

If you do not use conda or another package manager, make sure you are running gist-geckos using `python 3.11`, and the latest versions of `astropy`, `numpy`, `scipy`, `matplotlib`, `multiprocess`, `spectral-cube`, `extinction`, `pip`, `latex`, and `pyqt5`.

### Folder setup
The gistTutorial folder contained within the `gist-geckos-supplementary` repo is your working directory. Within it, you will find four folders: `configFiles`, `inputData`, `results`, and `spectralTemplates`. 

- `configFiles`: contains your `MasterConfig.yaml` file, as well as some files describing the LSF of your chosen instrument and SSP template set. Also included are some files describing the emission lines to fit, and the line strengh indices. 

- `inputData`: contains the IFS datacube that you wish to analyse and its associated spatial mask.

- `results`: this is the folder that your results will save in to.

- `spectralTemplates`: Place the SSP models that you wish to use in this folder. 

### Example data 
The gistTutorial folder comes with the default NGC0000.fits and NGC0000_mask.fits files. These are small MUSE data cubes which may be used for initial testing because they will run quicker.

### Running `gist-geckos`
Once you have cloned the repositories (for the first time), or pulled to update (subsequent times) run 
```py
python setup.py install 
```

 in the desired path (where your gist-geckos repo is installed). 

Then, in your gistTutorial/ directory run the gist pipeline with the following call:

```
gistPipeline --config configFiles/MasterConfig_GALAXYNAME.yaml --default-dir configFiles/defaultDir
```

The above assumes that your directory structure is such that your gistTutorial is set up as on the gist-geckos-supplementary repo (i.e. with the results, configFiles, inputData, and spectralTemplates folders inside the gistTutorial directory). 
