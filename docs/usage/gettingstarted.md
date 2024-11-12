# Getting started

### Running `nGIST`
Once you have cloned the two github repositories and installed nGIST, you will be able to test the installation using the test cube, NGC 0000. nGIST is initially configured such that your working directory is `ngistTutorial/`. In this working directory, execute the following command:

```
ngistPipeline --config configFiles/MasterConfig.yaml --default-dir configFiles/defaultDir
```

The pipeline will begin by printing the values input by your `MasterConfig.yaml` file, and then run through each module listed in the `MasterConfig.yaml` file in turn. Each step is printed in your terminal, and in the resultant logfile. 

### Output 
Outputs are saved in the `ngistTutorial/results/RUN_NAME` folder, where the `RUN_NAME` is specified in the `Master_Config.yaml`. 

Current output files are:

`_BinSpectra.hdf5` - a table containing the spatially binned log-rebinned spectra <br> 
`_BinSpectra_linear.hdf5` - a table containing the spatially binned lin-binned spectra <br> 
`_table.fits` - a table used to reconstruct 2D maps files based on the Voronoi binning.
`_mask.fits` <br> 
`_kin.fits` - a table containing the stellar kinematics results <br> 
`_kin-bestfit.fits` <br> 
`_kin-optimalTemplates.fits` <br> 
`_kin-SpectralMask.fits` <br> 
`_KIN_maps.fits` <br> 
`_CONTcube.fits` - an emission line-subtracted continuum cube <br> 
`_LINEcube.fits` - a continuum-subtracted line cube <br> 
`_ORIGcube.fits` <br> 
`_gas_BIN/SPX.fits` - a table containing the Voronoi binned (BIN) or spaxel-level (SPX) emission line results <br> 
`_gas_BIN/SPX_maps.fits` <br> 
`_gas-bestfit_BIN/SPX.fits` <br> 
`_gas-cleaned_BIN/SPX.fits` <br> 
`_SFH_maps.fits` <br> 
`_sfh-bestfit.fits` <br> 
`_sfh-weights.fits` <br> 
`_sfh.fits`  - a table containing the stellar populations results <br> 
`_LS_ADAPTED_maps.fits` <br> 
`_LS_ORIGINAL_maps.fits` <br> 
`_ls_AdapRes.fits` <br> 
`_ls_OrigRes.fits` <br> 
`_ls-cleaned_linear.fits` <br> 


### Visualising your results 
While the `MAPS.fits` files can be read in and plotted using e.g. python, the original GIST pipeline also developed the Mapviewer software, which enables a quick-look at your data. To use, run:
```py
Mapviewer
```
in your terminal, making sure that you are using the python environment in which nGIST is installed. 

### Next steps 
Now it's time to try one of your own cubes! Simply edit the `MasterConfig.yaml` file with the details of your own cube, and run!
