# Installation[^1]
[^1]: Installation has been tested using OSX and Linux. Windows OS is not currently supported.

The pipeline is available via the [GECKOS Survey Github page](https://github.com/geckos-survey). There are two repositories here, the [nGIST](https://github.com/geckos-survey/nGIST) repo, which contains the nGIST code itself, and the [ngist_supplementary_public](https://github.com/geckos-survey/ngist_supplementary_public) repo, which contains some extra examples to get you started.

### Cloning repositories 
Clone the `ngist` and `ngist-supplementary-public` repos to your local machine using the HTTPS tab.

`git clone https://github.com/geckos-survey/ngist.git`

`git clone https://github.com/geckos-survey/ngist_supplementary_public.git`

### Setting up your python environment 
(optional, but strongly recommended)
Setup a separate python environment to run nGIST in. The `gistenv.yaml` file located in the `ngist-supplementary-public` repo can be used to install all of the dependencies and specified versions into a conda environment using the following call:

```py
conda env create -f gistenv.yaml
```

If you do not use conda or another package manager, make sure you are running nGIST using `python >3.11`, and the latest versions of `astropy`, `numpy`, `scipy`, `matplotlib`, `spectral-cube`, `extinction`, `pip`, `latex`, `pyqt5`, `joblib`, `h5py`, and `tqdm`.

### Installing nGIST 
In your new python environment navigate to the ngist/ folder and run 
```py
pip install .
```

That's it! nGIST should be installed and ready to use. :lizard:

### Updating nGIST 
You should always check for new updates. before running nGIST. In your the ngist/ folder type 
```py
git pull --all
```
and then reisntall any new updates using pip:
```py
pip install .
```
If you have any issues with the code not updating, try 
```py
pip uninstall ngistPipeline
pip install .
```
If even that doesn't work, remove the .egg directory
```py
rem -r ngistPipeline.egg-info
pip install .
```
