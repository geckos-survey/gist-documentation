# Installation[^1]
[^1]: Installation has been tested using OSX and Linux. Windows OS is not currently supported.

The pipeline is available via the [GECKOS Survey Github page](https://github.com/geckos-survey). There are two repositories here, the [nGIST](https://github.com/geckos-survey/nGIST) repo, which contains the nGIST code itself, and the [ngist_supplementary_public](https://github.com/geckos-survey/ngist_supplementary_public) repo, which contains some extra examples to get you started.

### Cloning repositories 
Clone the `ngist` and `ngist-supplementary-public` repos to your local machine using the HTTPS tab.

`git clone https://github.com/geckos-survey/ngist.git`

`git clone https://github.com/geckos-survey/ngist_supplementary_public.git`

### Setting up your Python environment 
(optional, but strongly recommended)
Set up a separate Python environment to run nGIST in. The `ngistenv.yaml` file located in the `ngist_supplementary_public` repo can be used to install all of the dependencies and specified versions into a conda environment using the following call:

```py
conda env create -f ngistenv.yaml
```

If you do not use conda or another package manager, make sure you are running nGIST using `python >3.11`, and the latest versions of `astropy`, `numpy`, `scipy`, `matplotlib`, `spectral-cube`, `extinction`, `pip`, `latex`, `pyqt5`, `joblib`, `h5py`, `multiprocess` and `tqdm` (all available from conda or conda-forge). From pip, you will also need `printStatus`, `ppxf`, `emcee`, `plotbin`, `vorbin`, `plotly`, and `pandas`.

### Installing nGIST 
In your new Python environment navigate to the ngist/ folder and run 
```py
pip install .
```

That's it! nGIST should be installed and ready to use. :lizard:

### Updating nGIST 
You should always check for new updates. before running nGIST. In your ngist/ folder type 
```py
git pull --all
```
and then reinstall any new updates using pip:
```py
pip install .
```

### Using a different branch than main - only recommended for experienced users
If you want to use a different branch than main, for example the dev-branch, go to your ngist/ folder and type 
```py
git checkout dev-branch
```
Then, go to your ngist_supplementary_public, and do the same to make sure your nGIST installation and tutorial are using the same branch.
