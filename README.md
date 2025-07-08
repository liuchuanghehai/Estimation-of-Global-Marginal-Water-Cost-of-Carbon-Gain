# Estimation-of-Global-Marginal-Water-Cost-of-Carbon-Gain
Key procedures involved in the study of marginal water use efficiency for storing global carbon assimilation


# 🌍 Global Estimation of Marginal Water Cost of Carbon Gain (λ)

## 🔍 Project Overview
This project aims to predict high-resolution global transpiration (T) and gross primary production (GPP) based on multi-source meteorological datasets, and to calculate the ecosystem marginal water cost of carbon gain λ (λ = ∂E/∂A). The outputs support modeling, statistical analysis, and mechanistic interpretation of water–carbon interactions at global scales.

## 📘 项目简介
本项目旨在利用多源气象数据预测全球高精度 T 和 GPP，并结合常用数据计算生态系统的边际水分利用效率 λ（λ = ∂E/∂A），服务于后续的模拟建模、统计分析与机制解释。

### 1️⃣ model training.py — Machine Learning Modeling for λ Drivers

This module trains machine learning models (e.g., BNF, Random Forest, XGBoost) to predict T and GPP based on preprocessed ERA5 meteorological drivers.
No functions are defined — the script is executable directly.
**Main workflow includes:**
- Loading `.npy`-formatted drivers and target variables
- Splitting into training and testing sets
- Model training (Ridge, RF, BNF, etc.)
- Model evaluation and saving
:white_check_mark: This script serves as the modeling backbone of the λ system.

### 模块 1️⃣：model training.py —— λ 驱动因子的机器学习建模模块

本模块主要用于使用前期处理好的 ERA5 驱动变量数据，训练用于预测蒸腾（T）和光合碳增益（GPP）的机器学习模型（如 BNF、RF、XGBoost 等）。
该脚本未定义函数，直接运行即可完成模型训练。
**主要流程包括：**
- 加载 `.npy` 格式驱动因子与目标变量
- 进行训练集与测试集划分
- 模型训练（如 Ridge、随机森林、BNF 等）
- 模型评估与保存
:white_check_mark: 是 λ 模型系统的建模核心。

### 2️⃣ Predicting Global T and GPP.py — Prediction & Output of T and GPP

This module loads trained models to predict T and GPP across future years or regions.
**Key functions include:**
- `get_data()` — Load GeoTIFF raster files
- `read_land_use_tif()` — Load land-use masks for spatial filtering
- `delete_files_in_folder()` — Clear previous outputs
- `extract_index()` — Extract pixel-wise time series indices
- `save_to_tif_with_gdal()` — Save GeoTIFF with georeferencing
- `read_model()` — Load saved models
- `process_nc_files_by_time()` — Prepare ERA5 `.nc` files and run predictions
:white_check_mark: This is the forward prediction engine of the system.

### 模块 2️⃣：Predicting Global T and GPP.py —— 基于模型的 GPP 与 T 预测与输出模块

本模块用于加载已训练模型，对未来年份或空间区域的 GPP 与蒸腾进行模拟预测。
**主要函数包括：**
- `get_data()`：读取 GeoTIFF 栅格数据
- `read_land_use_tif()`：加载土地利用图层
- `delete_files_in_folder()`：清空输出目录
- `extract_index()`：提取像元时间序列索引
- `save_to_tif_with_gdal()`：保存为 GeoTIFF 并保留地理参考
- `read_model()`：加载已保存模型
- `process_nc_files_by_time()`：处理 ERA5 `.nc` 并运行预测
:white_check_mark: 本模块是系统的前向预测引擎。

### 3️⃣ basis function.py — Core Function Library for λ and Ecohydrological Variables

Reusable functions to support λ and related computations:
- `compute_GPP()` — Estimate GPP from inputs
- `compute_lambda()` — Calculate λ from GPP and T
- `generate_vpd()` — Compute vapor pressure deficit
- `get_data()` / `get_coords()` — Load TIFF or NetCDF data
- `save_tif()` — Save arrays as GeoTIFF
- `normalize()` / `denormalize()` — For ML preprocessing
- `calc_ET0_FAO()` — Estimate ET₀ using FAO-PM
- `resample_grid_with_empty_row()` — Spatial resolution matching
- `jisuan1_fanwei1()` — Count pixels within masks
:white_check_mark: These utilities support modular and scalable λ computation.

### 模块 3️⃣：basis function.py —— λ 与生态水文变量计算函数库

本模块定义了多个可复用函数，支持 λ 与生态水文过程的计算：
- `compute_GPP()`：估算 GPP
- `compute_lambda()`：根据 GPP 与 T 计算 λ
- `generate_vpd()`：计算 VPD
- `get_data()` / `get_coords()`：读取 TIFF 或 NetCDF 数据
- `save_tif()`：保存为 GeoTIFF
- `normalize()` / `denormalize()`：归一化预处理
- `calc_ET0_FAO()`：FAO-PM 方法估算 ET₀
- `resample_grid_with_empty_row()`：重采样支持
- `jisuan1_fanwei1()`：区域像元计数
:white_check_mark: 为模块化 λ 计算提供全面支撑。

### 4️⃣ Calculation and processing.py — ERA5 Preprocessing & λ Input Generation

This module prepares λ driver matrices and exports inputs:
- `fun1()` — Combine ERA5 variables and export as `.npy`/`.tif`
- `fun2()` — Configure I/O paths
- `fun3()` — Extract values using AI or land-use masks
- `fun4()` — Bin and group data
- `fun5()` — Generate derived variables like Tsum, VPD
- `fun6()` — Multi-variable export with masks
- `sn_statistic()` — Zonal statistics
- `write_tif()` — Save arrays with georeferencing
:white_check_mark: The core data engine for λ modeling.

### 模块 4️⃣：Calculation and processing.py —— ERA5 数据预处理与 λ 驱动因子生成

本模块从 ERA5 数据中构建 λ 输入矩阵并输出变量：
- `fun1()`：拼接变量并导出为 `.npy` 或 `.tif`
- `fun2()`：配置输入输出路径
- `fun3()`：结合掩膜提取像元值
- `fun4()`：变量分 bin 统计
- `fun5()`：生成 Tsum、VPD 等变量
- `fun6()`：多图层掩膜与输出
- `sn_statistic()`：区域平均值计算
- `write_tif()`：输出为带参考坐标的 GeoTIFF
:white_check_mark: 是 λ 建模的数据处理引擎。

## ⚙️ Environment Requirements / 环境依赖

- Python ≥ 3.8
- Required packages: `numpy`, `xarray`, `pandas`, `netCDF4`, `rasterio`, `tqdm`, `gdal`
- GDAL support recommended via `conda` or `OSGeo` environment.

