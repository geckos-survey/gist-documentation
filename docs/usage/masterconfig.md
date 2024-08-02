# The `MasterConfig.yaml` file

The input for nGIST is a convineient, human-readable python YAML file. This file is tab-sensitive, so please conserve the structure when editing. An example MasterConfig.yaml file is shown below. 

```py
# This is a test config file
GENERAL :
  RUN_ID : 'NGC0000Example' # Name of the analysis run. A subdirectory of this name within the output directory 
  # will be created. This identifier further serves as a prefix to all output files.
  INPUT : 'NGC0000.fits' # Input file for this analysis run. The specified path 
  # is relative to the input path given in defaultDir.
  OUTPUT : . # Output directory. The output of this run will be collected in a subdirectory named RUN_ID. 
  # The specified path is relative to the output path given in defaultDir.
  REDSHIFT : 0.008764 # Initial guess on the redshift of the system [in z]. Spectra are shifted to rest-frame, 
  # according to this redshift.
  PARALLEL: True # Use multiprocessing [True/False]
  NCPU : 3 # Number of cores to use for multiprocessing.
  LSF_DATA : 'lsf_MUSE-WFM' # Path of the file specifying the line-spread-function of the observational data. 
  # The specified path is relative to the configDir path in defaultDir.
  OW_CONFIG : False #  Ignore configurations from previous runs which are saved in the CONFIG file in the 
  # output directory [True/False]
  OW_OUTPUT : True # Overwrite any output files already present in the current output directory [True/False]

# Read data module
READ_DATA :
  METHOD : 'MUSE_WFM' # Name of the routine in readData/ (without .py) to be used to read-in the input data.
  DEBUG : FALSE # Switch to activate debug mode [True/False]: Pipeline runs on one, central line of pixels. 
  # Keep in mind to clean output directory after running in DEBUG mode!
  ORIGIN : 14,14 # Origin of the coordinate system in pixel coordinates: x,y (Indexing starts at 0).
  LMIN_TOT : 4750 # Spectra are shortened to the rest-frame wavelength range defined by LMIN_TOT and LMAX_TOT. 
  # Note that this wavelength range should be longer than all other wavelength ranges supplied to the modules 
  # [in Angst.]
  LMAX_TOT : 7100
  LMIN_SNR : 4750 # Rest-frame wavelength range used for the signal-to-noise calculation [in Angst.]
  LMAX_SNR : 7100
  EBmV : 0.0367 # Galactic foreground dust redenning. Set to null for no dust correction  [value/null]

# Spatial masking module
SPATIAL_MASKING :
  METHOD : 'default' # Name of the routine in spatialMasking/ (without .py) to perform the tasks. 
  # Set 'False' to turn off module. Set 'default' to use the standard GIST implementation.
  MIN_SNR : 20.0 # Spaxels below the isophote level which has this mean signal-to-noise level are masked.
  MASK : 'NGC0000_mask.fits' # File containing a spatial mask (Set 'False' to not include a file).

# Spatial binning module
SPATIAL_BINNING :
  METHOD : 'voronoi' # Name of the routine in spatialBinning/ (without .py) to perform the tasks. 
  # Set 'False' to turn off module. Set 'voronoi' to use the standard GIST implementation, exploiting the 
  # Voronoi tesselation routine of Cappellari & Copin (2003).
  TARGET_SNR : 500.0 # Target signal-to-noise ratio for the Voronoi binning
  COVARIANCE : 0.0 # Correct for spatial correlations of the noise during the Voronoi binning process according 
  # to the empirical equation SNR /= 1 + COVAR_VOR * np.log10(NSPAXEL) with NSPAXEL being the number of spaxels 
  # per bin (see e.g. Garcia-Benito et al. 2015).

# Prepare spectra module
PREPARE_SPECTRA :
  METHOD : 'default'
  VELSCALE : 41 # km/s

# Stellar kinematics module
KIN :
  METHOD : 'ppxf' # Name of the routine in stellarKinematics/ (without .py) to perform the tasks. Set 'False' 
  # to turn off module. Set 'ppxf' to use the standard nGIST implementation, exploiting the pPXF routine of 
  # Cappellari & Emsellem (2004).
  SPEC_MASK : 'specMask_KIN' # File to define wavelength ranges to be masked during the stellar kinematics fit. 
  # The specified path is relative to the configDir path in defaultDir.
  LMIN : 4750 # Rest-frame wavelength range used for the stellar kinematics analysis [in Angst.]
  LMAX : 7100
  SIGMA : 100 # Initial guess of the velocity dispersion of the system [in km/s]
  MOM : 4 # Number of kinematic moments to be extracted
  ADEG : 12 # Degree of the additive Legendre polynomial. Set '-1' to not include any additive polynomials
  MDEG : 0 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials
  REDDENING : null # Initial guess on the stellar reddening E(B-V), in order to measure the 
  # stellar reddening. Note: This cannot be used together with multiplicative polynomials.
  MC_PPXF : 0 # Number of Monte-Carlo simulations to extract errors on the stellar kinematics. Formal errors are 
  # saved in any case.
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. 
  # The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS' or 'walcher'
  LIBRARY : 'MILES/' # options are 'MILES/', 'miles_ssp_ch/', 'IndoUS', and 'Walcher/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
  DOCLEAN : True # Keyword to turn on/off the sigma clipping. Set to 'False' for testing.

# Continuum Cube module
CONT :
  METHOD : 'ppxf' # Name of the routine in stellarKinematics/ (without .py) to perform the tasks. Set 'False' to turn off module. Set 'ppxf' to use the standard nGIST implementation, exploiting the pPXF routine of Cappellari & Emsellem (2004).
  SPEC_MASK : 'specMask_KIN' # File to define wavelength ranges to be masked during the stellar kinematics fit. The specified path is relative to the configDir path in defaultDir.
  LMIN : 4800 # Rest-frame wavelength range used for the stellar kinematics analysis [in Angst.]
  LMAX : 7000
  SIGMA : 100 # Initial guess of the velocity dispersion of the system [in km/s]
  MOM : 4 # Number of kinematic moments to be extracted
  ADEG : 0 # Degree of the additive Legendre polynomial. Set '-1' to not include any additive polynomials
  MDEG : 8 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials
  REDDENING : null # As opposed to None. Initial guess on the stellar reddening E(B-V), in order to measure the stellar reddening. Note: This cannot be used together with multiplicative polynomials.
  MC_PPXF : 0 # Number of Monte-Carlo simulations to extract errors on the stellar kinematics. Formal errors are saved in any case.
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS' or 'walcher'
  LIBRARY : 'MILES/' # options are 'MILES/', 'miles_ssp_ch/', 'IndoUS', and 'Walcher/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]
  DOCLEAN : True # Keyword to turn on/off the sigma clipping. Set to 'False' for testing.

# Emission line fitting module
GAS :
  METHOD : 'ppxf' # options are 'ppxf', 'gandalf' or 'MAGPI_gandalf' Name of the routine in emissionLines/ (without .py) 
  #to perform the tasks. Set 'False' to turn off module. 
  SPEC_MASK : 'specMask_GAS'
  LEVEL : 'BIN' # Set 'BIN' to perform the analysis bin-by-bin, 'SPAXEL' for a spaxel-by-spaxel analysis, and 'BOTH' 
  # to perform a spaxel-by-spaxel analysis that is informed by a previous bin-by-bin analysis.
  LMIN : 4750 # Rest-frame wavelength range used for the emission-line analysis [in Angst.]
  LMAX : 7100
  ERRORS : 0 # Derive errors on the emission-line analysis (0 No / 1 Yes). Note: Due to limitations in pyGandALF, 
  # this is currently not possible. We expect a new pyGandALF version to be published soon.
  REDDENING : False # Include the effect of reddening by dust in the pyGandALF fit. Put in the form 0.1,0.1 without 
  # any spaces. If you intend to use multiplicative polynomials instead, set REDDENING to 'False' and add a 
  # MDEG keyword in the GAS section to set the polynomial order. Additive polynomials cannot be used with pyGandALF.
  MDEG : 8 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative polynomials.
  FIXED : True # Fix the stellar kinematics to the results obtained with the stellar kinematics module [True / False]
  MOM : 2 # Gas moments. Set to 2 for V and sigma. Higher orders not yet tested
  EBmV : null # De-redden the spectra for the Galactic extinction in the direction of the 
  # target previously to the analysis. Use e.g. EBmV = A_v / 3.2
  EMI_FILE : 'emissionLinesPHANGS.config' # Emission line set-up file for emline fittet. The specified path is 
  # relative to the configDir path in defaultDir.
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. 
  # The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS'
  LIBRARY : 'MILES_EMLINES/' # options are 'MILES_EMLINES/' (contains asmaller subset of templates sim 
  # to that used by PHANGS) 'MILES/', 'miles_ssp_ch', or 'IndoUS'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]

# Star formation histories module
SFH :
  METHOD : 'ppxf' # Name of the routine in starFormationHistories/ (without .py) to perform the tasks. 
  # Set 'False' to turn off module. Set 'ppxf' to use the standard GIST implementation, exploiting the 
  # pPXF routine of Cappellari & Emsellem (2004).
  LMIN : 4750 # Rest-frame wavelength range used for the star formation histories analysis [in Angst.]
  LMAX : 5500
  SPEC_MASK : 'specMask_SFH' # File to define wavelength ranges to be masked during the star formation 
  # histories analysis. The specified path is relative to the configDir path in defaultDir.
  MOM : 4 # Number of kinematic moments to be extracted. If you use FIXED = True, this should be same 
  # number of moments used to extract the stellar kinematics before. Otherwise the parameter can be set independently.
  MDEG : 12 # Degree of the multiplicative Legendre polynomial. Set '0' to not include any multiplicative 
  # polynomials. Note that additive Legendre polynomials cannot be used for this module.
  REGUL_ERR : 1000000. # Regularisation error for the regularised run of pPXF. Note: Regularisation = 1 / REGUL_ERR
  NOISE : 1. # Set a wavelength independent noise vector to be passed to pPXF.
  FIXED : True # Fix the stellar kinematics to the results obtained with the stellar kinematics module 
  # [True / False]. If 'False', please provide an initial guess on the velocity dispersion of the systems 
  # [in km/s] by adding the parameter SIGMA.
  MC_PPXF : 0 # Added to add the option of running MC on the SFH module
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. 
  # The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS (not recc.)'
  LIBRARY : 'MILES_SFH/' # options are 'MILES/', 'miles_ssp_ch/', or 'IndoUS/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results 
  # [LIGHT / MASS]
  DOCLEAN : True # Keyword to turn on/off the sigma clipping. Set to 'False' for testing.

# Line strenghts module
LS :
  METHOD : False #'default' # Name of the routine in lineStrengths/ (without .py) to perform the tasks. 
  # Set 'False' to turn off module. Set 'default' to use the standard GIST implementation, exploiting the 
  # routines of Kuntschner et al. (2006) and Martin-Navarro et al. (2018).
  TYPE : 'SPP' # Set 'LS' to only measure line strength indices, or 'SPP' to also match these indices 
  # to stellar population properties.
  LS_FILE : 'lsBands.config' # File to define the wavelength band of the line strength indices to 
  # be measured. The specified path is relative to the configDir path in defaultDir.
  CONV_COR : 8.4 # Spectral resolution [in Angst.] at which the line strength indices are measured.
  SPP_FILE : 'MILES_KB_LIS8.4.fits' # File providing predictions on line strength indices for a 
  # set of single stellar population models
  MC_LS : 30 # Number of Monte-Carlo simulations in order to obtain errors on the line strength indices. 
  # Note: This must be turned on.
  NWALKER : 10 # Number of walkers for the MCMC algorithm (used for the conversion of indices to population properties)
  NCHAIN : 100 # Number of iterations in the MCMC algorithm (used for the conversion of indices to population properties)
  LSF_TEMP : 'lsf_MILES' # Path of the file specifying the line-spread-function of the spectral templates. 
  # The specified path is relative to the configDir path in defaultDir.
  TEMPLATE_SET : 'miles' # options are 'miles' or 'IndoUS'
  LIBRARY : 'MILES/' # options are 'MILES/', 'miles_ssp_ch/', or 'IndoUS/'
  NORM_TEMP : 'LIGHT' # Normalise the spectral template library to obtain light- or mass-weighted results [LIGHT / MASS]

# User modules
UMOD :
  METHOD : False
```
