# Getting started

### Running `nGIST`
Once you have cloned the two github repositories and installed nGIST, you will be able to test the installation using the test cube, NGC0000. nGIST is initially configured such that your working directory is `ngistTutorial/`. In this working directory, execute the following command:

```
ngistPipeline --config configFiles/MasterConfig.yaml --default-dir configFiles/defaultDir
```

The pipeline will begin by printing the values input by your `MasterConfig.yaml` file, and then run through each module listed in the `MasterConfig.yaml` file in turn. Each step is printed in your terminal, and in the resultant logfile. 

### Output 
Outputs are saved in the `ngistTutorial/results/RUN_NAME` folder, where the `RUN_NAME` is specified in the `Master_Config.yaml`. 

Current output files are:

`_bin_spectra.hdf5` - a table containing the spatially binned log-rebinned spectra. <br> 
`_bin_spectra_linear.hdf5` - a table containing the spatially binned lin-binned spectra. <br> 
`_all_spectra.hdf5` - if GAS is run in SPAXEL mode, a table containing all log-rebinned spectra. <br> 
`_all_spectra_linear.hdf5` - if GAS is run in SPAXEL mode, a table containing all lin-binned spectra. <br> 
`_table.fits` - a table used to reconstruct 2D maps files based on the Voronoi binning.
`_mask.fits` - contains information on the spatial masking. <br> 
`_kin.fits` - a table containing the stellar kinematics results. <br> 
`_kin-bestfit.fits` - the bestfit to the spectra. <br> 
`_kin-optimal_templates.fits` - optimal templates for the fit to each bin. <br> 
`_kin-spectral_mask.fits` - spectral mask. <br> 
`_kin_maps.fits` - fits file containing 2D maps of the output kinematic properties. <br> 
`_cont_cube.fits` - an emission line-subtracted continuum cube. <br> 
`_line_cube.fits` - a continuum-subtracted line cube. <br> 
`_orig_cube.fits` - the orginal cube, trimmed to the wavelength range used for the CONT module. <br> 
`_gas_bin/spx.fits` - a table containing the Voronoi binned (BIN) or spaxel-level (SPX) emission line results. <br> 
`_gas_bin/spx_maps.fits` - fits file containing 2D maps of the output emission line properties. <br> 
`_gas-bestfit_bin/spx.fits` - the best fit to the spectra. <br> 
`_gas-cleaned_bin/spx.fits` - emission line-subtracted spectra. <br> 
`_sfh_maps.fits` - fits file containing 2D maps of the output stellar population properties. <br> 
`_sfh-bestfit.fits` - the bestfit to the spectra. <br> 
`_sfh-weights.fits` - the weights assigned to each template during the fit. <br> 
`_sfh.fits`  - a table containing the stellar populations results <br> 
`_ls_adapted_maps.fits` - fits file containing 2D maps of the output adapted spectral resolution line strength properties. <br> 
`_ls_original_maps.fits` - fits file containing 2D maps of the output original spectral resolution line strength properties. <br> 
`_ls_AdapRes.fits` - line strength indices and their errors as estimated from MC-simulations for adapted sprectral resolution. <br> 
`_ls_OrigRes.fits`  - line strength indices and their errors as estimated from MC-simulations for original sprectral resolution. <br> 
`_ls-cleaned_linear.fits` - emission-subtracted, linearly binned spectra. <br> 


### Visualising your results 
While the `MAPS.fits` files can be read in and plotted using e.g. python, the original GIST pipeline also developed the Mapviewer software, which enables a quick-look at your data. To use, run:
```py
Mapviewer
```
in your terminal, making sure that you are using the python environment in which nGIST is installed. 

### Next steps 
Now it's time to try one of your own cubes! Simply edit the `MasterConfig.yaml` file with the details of your own cube, and run!
