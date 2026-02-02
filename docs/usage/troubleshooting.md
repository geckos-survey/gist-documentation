# Trouble shooting

Issues with installation and running? Below we list a couple of common problems encountered.

### Issues with updating gist-geckos 

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

### Issues with dependency versioning

Numpy has recently been updated to version 2 and introduced a new C API/ABI. This has caused issues with several other nGIST pipeline dependencies. While we wait for some wheels to catch up, we explicitly recommend that you use nGIST with a Numpy version < 1.27. 

### environment YAML taking too long to resolve?
This behaviour has been reported on a handful of servers. If this happens to you, we suggest installing packages manually with the only package requirements of python=3.11 and numpy<1.27. 
If you are using conda, first install the packages listed in the environment YAML that are available on conda or conda forge, then install the rest with pip.

Alternatively, you could try [mamba](https://github.com/mamba-org/mamba). It’s built on conda so fully compatible but designed to be faster and more robust: 

```py
## prioritize 'conda-forge' channel
conda config --add channels conda-forge

## update existing packages to use 'conda-forge' channel
conda update -n base --all

## install 'mamba'
conda install -n base mamba

```
