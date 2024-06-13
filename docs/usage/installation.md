# Installation[^1]
[^1]: Installation has been tested using OSX and Linux. Windows OS is not currently supported.

The pipeline is available via the [GECKOS Survey Github page](https://github.com/geckos-survey). There are two repositories here, the [nGIST](https://github.com/geckos-survey/nGIST) repo, which contains the nGIST code itself, and the [nGIST_supplementary_public](https://github.com/geckos-survey/ngist_supplementary_public) repo, which contains some extra examples to get you started.

### Cloning repositories 
Clone the `ngist` and `ngist_supplementary_public` repos to your local machine using the SSH tab[^2]
[^2]: If you haven't already, create an ssh key using ssh-keygen using the instructions [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

`git clone git@github.com:geckos-survey/ngist.git`

`git clone git@github.com:geckos-survey/ngist_supplementary_public.git`

### Setting up your python environment 
(optional, but strongly recommended)
Setup a separate python environment to run nGIST in. The `gistenv.yaml` file located in the `ngist_supplementary_public` repo can be used to install all of the dependencies and specified versions into a conda environment using the following call:

```py
conda env create -f gistenv.yaml
```

If you do not use conda or another package manager, make sure you are running gist-geckos using `python 3.11`, and the latest versions of `astropy`, `numpy`, `scipy`, `matplotlib`, `multiprocess`, `spectral-cube`, `extinction`, `pip`, `latex`, and `pyqt5`.

### Installing nGIST 
In your new python environment navigate to the nGIST/ folder and run 
```py
pip install .
```

That's it! nGIST should be installed and ready to use. :lizard:

### Updating gist-geckos 
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
pip uninstall gistPipeline
pip install .
```
If even that doesn't work, remove the .egg directory
```py
rem -r gistPipeline.egg-info
pip install .
```
