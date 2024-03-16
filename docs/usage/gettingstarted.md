# Getting started

### Running `nGIST`
Once you have cloned the two githab repos and installed nGIST, you will be able to test the installation using the test cube, NGC 0000. nGIST is initially configured such that your working directory is `gistTutorial/`. In this working directory, execute the following command:

```
gistPipeline --config configFiles/MasterConfig.yaml --default-dir configFiles/defaultDir
```

The pipeline will begin by printing the values input by your `MasterConfig.yaml` file, and then run through each module listed in the `MasterConfig.yaml` file in turn. Each step is printed in your terminal, and in the resultant logfile. 

### Output 
Outputs are saved in the `gistTutorial/results/RUN_NAME` folder, where the `RUN_NAME` is specified in the `Matser_Config.yaml`. 

Current output files are:

`_AllSpectra.fits`  <br> 
`_BinSpectra_linear.fits` - a table containing the spatially binned spectra <br> 
`_BinSpectra.fits` - a table containing the spatially binned spectra <br> 
`_gas_BIN_2dmap.fits` <br> 
`_gas_BIN_maps.fits` <br> 
`_gas_BIN.fits` - a table containing the Voronoi binned emission line results <br> 
`_gas-bestfit_BIN.fits` <br> 
`_gas-cleaned_BIN.fits` <br> 
`_kin_2dmap.fits` <br> 
`_KIN_CONTcube.fits` - an emission line-subtracted continuum cube <br> 
`_KIN_LINEcube.fits` - a continuum-subtracted line cube <br> 
`_KIN_maps.fits` <br> 
`_KIN_ORIGcube.fits` <br> 
`_kin-bestfit.fits` <br> 
`_kin-optimalTemplates.fits` <br> 
`_kin-SpectralMask.fits` <br> 
`_kin.fits` - a table containing the stellar kinematics results <br> 
`_ls_AdapRes.fits` <br> 
`_LS_ADAPTED_maps.fits` <br> 
`_LS_ORIGINAL_maps.fits` <br> 
`_ls_OrigRes.fits` <br> 
`_ls-cleaned_linear.fits` <br> 
`_mask.fits` <br> 
`_sfh_2dmap.fits` <br> 
`_SFH_maps.fits` <br> 
`_sfh-bestfit.fits` <br> 
`_sfh-weights.fits` <br> 
`_sfh.fits`  - a table containing the stellar populations results <br> 
`_table_2dmap.fits` <br> 
`_table.fits` - a table used to reconstruct 2D maps files based on the Voronoi binning.

### Visualising your results 
While the 2D maps files can be read in and plotted using python, the original GIST pipeline also developed the Mapviewer software, which enables a quick-look at your data. To use, run:
```py
mapviewer
```
in your terminal. 

### Next steps 
Now it's time to try one of your own cubes! Simply edit the `MasterConfig.yaml` file with the details of your own cube, and run!