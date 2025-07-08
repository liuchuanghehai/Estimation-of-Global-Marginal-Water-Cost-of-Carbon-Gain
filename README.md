# Estimation-of-Global-Marginal-Water-Cost-of-Carbon-Gain
Key procedures involved in the study of marginal water use efficiency for storing global carbon assimilation
🔍 Project Overview
This project aims to predict high-resolution global transpiration (T) and gross primary production (GPP) based on multi-source meteorological datasets, and to calculate the ecosystem marginal water cost of carbon gain λ (λ = ∂E/∂A). The outputs support modeling, statistical analysis, and mechanistic interpretation of water–carbon interactions at global scales.

📂 Module Structure & Functional Overview
1️⃣ model training.py — Machine Learning Modeling for λ Drivers
This module trains machine learning models (e.g., BNF, Random Forest, XGBoost) to predict T and GPP based on preprocessed ERA5 meteorological drivers.

No functions are defined — the script is executable directly.

Main workflow includes:

Loading .npy-formatted drivers and target variables;

Splitting into training and testing sets;

Model training (Ridge, RF, BNF, etc.);

Model evaluation and saving.

✅ This script serves as the modeling backbone of the λ system, bridging seamlessly with upstream data preparation and enabling independent regression for both GPP and T.

2️⃣ Predicting Global T and GPP.py — Prediction & Output of T and GPP
This module loads trained models to predict T and GPP across future years or regions.

Key functions include:

get_data() — Load GeoTIFF raster files;

read_land_use_tif() — Load land-use masks for spatial filtering;

delete_files_in_folder() — Clear previous outputs from directories;

extract_index() — Extract pixel-wise time series indices from arrays;

save_to_tif_with_gdal() — Save predicted results as GeoTIFFs with georeferencing;

read_model() — Load saved models (PyTorch, joblib, etc.);

process_nc_files_by_time() — Process ERA5 .nc files chronologically into model inputs and perform prediction.

✅ This is the forward prediction engine of the system, coupling machine learning models with historical ERA5 inputs to simulate ecosystem GPP and T dynamics for λ estimation.

3️⃣ basis function.py — Core Function Library for λ and Ecohydrological Variables
This module defines reusable functions to support λ and related hydrometeorological variable computation:

compute_GPP() — Estimate GPP from radiation, VPD, and temperature;

compute_lambda() — Calculate λ using GPP and transpiration T;

generate_vpd() — Derive vapor pressure deficit (VPD) from temperature and humidity;

get_data() — Load .tif files as NumPy arrays;

get_geoinfo() — Extract projection and spatial metadata from reference rasters;

save_tif() — Save 2D/3D arrays to GeoTIFF with geolocation;

normalize() / denormalize() — Scale/unscale variables for machine learning;

get_coords() — Extract lat/lon arrays from NetCDF files;

calc_ET0_FAO() — Estimate reference ET₀ via FAO Penman–Monteith method (extensible);

resample_grid_with_empty_row() — Resample rasters to new resolution (e.g., 0.25°);

jisuan1_fanwei1() — Count pixels within regional masks for zonal analysis.

✅ These utilities offer full-process support for λ calculation and can be reused across spatial domains and modeling tasks.

4️⃣ Calculation and processing.py — ERA5 Preprocessing & λ Input Generation
This module builds input matrices for λ estimation from raw ERA5 variables, with spatial masking, statistical grouping, and raster export.

Key functions include:

fun1() — Merge ERA5 drivers (e.g., TA, VPD, radiation, SWC), resample and save as .npy or .tif files;

fun2() — Configure input/output paths for ERA5, GPP masks, land-use layers;

fun3(site, outputpath, ras_2) — Extract raster values (e.g., λ) using masks like AI or land use for regional statistics;

fun4(var='new_lambda', interval=300) — Bin data by intervals for AI gradient or zonal summaries;

fun5() — Convert ERA5 humidity, radiation, and temperature to VPD, Tsum, etc. and save in .npy format;

fun6() — Load processed variables (e.g., λ), apply spatial filters, and generate multilayer outputs;

sn_statistic(x) — Compute zonal averages/variances across land-use types or aridity zones;

write_tif(array, reference, filename) — Save array as GeoTIFF with inherited geolocation from reference layer.

✅ This is the data engine of the λ estimation framework, supporting variable transformation, regional filtering, and spatial statistics for modeling and diagnostic analysis.

⚙️ Environment Requirements
Python version: ≥ 3.8 recommended

Key dependencies:

numpy, xarray, pandas, netCDF4, rasterio, tqdm, gdal

GDAL-based functions require conda or OSGeo environment setup.
