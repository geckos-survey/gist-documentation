# Installation[^1]
[^1]: Installation has been tested using OSX and Linux. Windows OS is not currently supported.

The pipeline is available via the [GECKOS Survey Github page](https://github.com/geckos-survey). There are two repositories here, the [gist-geckos](https://github.com/geckos-survey/gist-geckos) repo, which contains the gist-geckos code itself, and the [gist-geckos-supp-public](https://github.com/geckos-survey/gist-geckos-supp-public) repo, which contains some extra examples to get you started.

### Cloning repositories 
Clone the `gist-geckos` and `gist-geckos-supp-public` repos to your local machine using the SSH tab[^2]
[^2]: If you haven't already, create an ssh key using ssh-keygen using the instructions [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

`git clone git@github.com:geckos-survey/gist-geckos.git`

`git clone git@github.com:geckos-survey/gist-geckos-supp-public.git`

### Setting up your python environment 
(optional, but strongly recommended)
Setup a separate python environment to run gist-geckos in. The `gistenv.yaml` file located in the `gist-geckos-supp-public` repo can be used to install all of the dependencies and specified versions into a conda environment using the following call:

```py
conda env create -f gistenv.yaml
```

If you do not use conda or another package manager, make sure you are running gist-geckos using `python 3.11`, and the latest versions of `astropy`, `numpy`, `scipy`, `matplotlib`, `multiprocess`, `spectral-cube`, `extinction`, `pip`, `latex`, and `pyqt5`.

### Installing gist-geckos 
In your new python environment navigate to the gist-geckos/ folder and run 
```py
python setup.py install
```

That's it! gist-geckos should be installed and ready to use. :lizard:



