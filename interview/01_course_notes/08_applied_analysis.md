# 课程笔记 08：应用分析、毕业论文与实习模块
# Course Notes 08 – Applied Analysis, Thesis & Internship

> 涵盖课程 / Courses covered:  
> GIS Software · Raster Analysis · Point Cloud Processing ·  
> GIS Fieldwork · Thesis I & II · Internship

---

## 1. GIS Software（GIS软件）

### 1.1 Commercial GIS Landscape / 商业GIS全景

| Software | Vendor | Strengths | Common Use |
|---|---|---|---|
| ArcGIS Pro | Esri | Comprehensive, 3D, ML | Government, enterprise |
| ERDAS IMAGINE | Hexagon | Advanced RS image processing | Photogrammetry, RS |
| ENVI | L3Harris | Spectral analysis, hyperspectral | Scientific RS ← you've used this |
| Global Mapper | Blue Marble | LiDAR, fast raster ops | Field surveying, LiDAR |
| AutoCAD Map 3D | Autodesk | CAD-GIS integration | Engineering surveys |

### 1.2 ArcGIS Pro Key Tools for LiDAR / ArcGIS Pro LiDAR关键工具

```
3D Analyst Extension:
├── LAS Dataset Toolbar → Browse/filter LAS files
├── LAS Dataset to Raster → DEM/DSM generation
├── Classify LAS → ground/vegetation/building
├── Extract LAS → by attribute, geometry
└── LAS Point Statistics as Raster → density, intensity maps

Spatial Analyst:
├── Slope/Aspect/Curvature → from DEM
├── Hillshade → visualisation
├── Fill/Flow Direction/Accumulation → hydrology
└── Reclassify + Weighted Overlay → suitability

ModelBuilder (模型构建器):
→ Visual drag-and-drop workflow automation
→ Equivalent to: arcpy.env → gp.tool() chain
```

### 1.3 ArcPy Automation / ArcPy自动化

```python
import arcpy

# Set workspace
arcpy.env.workspace = r"C:\LiDAR_Project\Output"
arcpy.env.overwriteOutput = True

# LAS Dataset → DEM
arcpy.conversion.LasDatasetToRaster(
    in_las_dataset="survey.lasd",
    out_raster="dem_0.5m.tif",
    value_field="ELEVATION",
    interpolation_type="BINNING AVERAGE LINEAR",
    data_type="FLOAT",
    sampling_type="CELLSIZE",
    sampling_value=0.5
)

# Slope from DEM
arcpy.sa.Slope("dem_0.5m.tif", "slope_deg.tif", "DEGREE").save()

print("Processing complete. DEM and slope generated.")
```

---

## 2. Raster Analysis（栅格分析）

### 2.1 Core Concept / 核心概念

**Definition:** Analytical operations on raster (gridded) spatial data — treating each cell as a unit of analysis. Includes map algebra, focal/zonal statistics, terrain analysis, and raster-based modelling.  
**定义：** 对栅格（格网）空间数据的分析操作——将每个像元作为分析单元。包括地图代数、焦点/区域统计、地形分析和基于栅格的建模。

### 2.2 Map Algebra / 地图代数

```
Three operational levels (三个操作层次):

1. Local operations (逐像元操作):
   output[i,j] = f(input₁[i,j], input₂[i,j], ...)
   Example: NDVI = (NIR - Red) / (NIR + Red)

2. Focal operations (邻域操作):
   output[i,j] = f(neighbourhood of input[i,j])
   Example: 3×3 mean filter (smoothing), edge detection
   
3. Zonal operations (区域操作):
   output[zone] = f(all cells in zone)
   Example: mean elevation per watershed, sum area per land cover class

4. Global operations (全局操作):
   output = f(entire raster extent)
   Example: Euclidean distance transform, cost surface
```

### 2.3 DEM Quality & Error Sources / DEM质量与误差来源

```
DEM Error Budget (DEM误差预算):
├── Source data quality: LiDAR pulse density, GPS accuracy
├── Interpolation error: artefacts from TIN triangulation
├── Vegetation penetration: under-canopy not captured by photogrammetry
├── Water surfaces: LiDAR specular reflection → voids
└── Temporal change: DEM age vs. current terrain

Validation (验证):
├── Vertical accuracy: RMSE vs. independent checkpoints
├── NSSDA standard: 95% confidence = 1.96 × σ
│   Example: 1 cm σ → 95% accuracy = ±1.96 cm
└── Vertical datum: AMSL (mean sea level) vs. ellipsoidal height
    Geoid undulation N: H = h - N (orthometric = ellipsoidal - geoid)
```

### 2.4 Raster Formats / 栅格格式

| Format | 特点 | Use Case |
|---|---|---|
| GeoTIFF | Universal, lossless | Standard exchange format |
| COG (Cloud-Optimised GeoTIFF) | HTTP range requests | Cloud-native streaming |
| NetCDF / HDF5 | Multi-dimensional | Climate, time series |
| MBTiles | SQLite tile container | Offline mobile maps |
| JPEG 2000 | Lossy/lossless, wavelet | Large image archives |
| LASzip / LAZ | Compressed LAS | Point clouds |

---

## 3. Point Cloud Processing（点云处理）⭐ 重点课程

### 3.1 Core Concept / 核心概念

**Definition:** Processing of unstructured 3D point sets from LiDAR, photogrammetry, or structured light scanning — including filtering, classification, segmentation, feature extraction, and analysis for geographic, engineering, and robotic applications.  
**定义：** 处理来自LiDAR、摄影测量或结构光扫描的非结构化三维点集——包括过滤、分类、分割、特征提取和分析，应用于地理、工程和机器人领域。

### 3.2 Point Cloud Data Formats / 点云数据格式

```
LAS format structure (LAS格式结构):
├── Header: file version, point format, scale/offset, bounds, point count
├── VLRs (Variable Length Records): spatial reference, extra byte definitions
└── Point records (点记录): X, Y, Z, Intensity, Return Number, 
    Number of Returns, Classification, Scan Angle, GPS Time, RGB

LAS Classification codes (ASPRS):
0=Created/unclassified  1=Unclassified  2=Ground  3=Low vegetation
4=Medium vegetation  5=High vegetation  6=Building  7=Low noise
9=Water  10=Rail  11=Road  12=Overlap  14=Wire-guard  15=Wire-conductor
17=Bridge  18=High noise  64-255=User-defined
```

### 3.3 Processing Pipeline / 处理流程

```
Raw LiDAR Point Cloud
        │
1. Import & QC (导入与质控)
   ├── Check density (密度检查): points/m²
   ├── Check vertical accuracy: against known checkpoints
   └── Visualise: intensity, return count, scan angle

2. Noise Removal (噪声去除)
   ├── Statistical Outlier Removal (SOR): mean distance + σ threshold
   └── DBSCAN-based: remove isolated small clusters

3. Ground Filtering (地面滤波)
   ├── Progressive TIN Densification (PTD/ATIN)
   ├── CSF (Cloth Simulation Filter) ← simple, effective for flat terrain
   └── SMRF (Simple Morphological Filter) ← in PDAL/LAStools

4. Classification (分类)
   ├── Vegetation: height above ground thresholds
   ├── Buildings: planarity + height
   ├── Powerlines: linearity + height
   └── ML-based: Random Forest / PointNet for complex scenes

5. Product Generation (成果生成)
   ├── DEM: ground points → TIN/Kriging → raster
   ├── DSM: first/single returns → raster
   ├── CHM: DSM - DEM (canopy height)
   ├── Normalised point cloud: Z = Z - DEM_value (height above ground)
   └── Intensity image: ortho-photo substitute

6. Feature Extraction (要素提取)
   ├── Building footprints (建筑物轮廓)
   ├── Tree crown delineation (树冠分割)
   ├── Powerline extraction (输电线路提取) ← your competition task
   └── Road marking detection (路面标线检测)
```

### 3.4 Powerline Extraction — Your Competition Task / 输电线路提取——你的竞赛任务

```
Algorithm chain for powerline detection (输电线路检测算法链):

1. Height filter: keep points above 5 m (remove ground/low veg)
2. DBSCAN / Euclidean cluster: separate objects
3. Linearity test: PCA on cluster → ratio of eigenvalues λ₁/λ₂ > threshold
   (lines have one dominant eigenvalue → high linearity)
4. Catenary fitting (悬链线拟合): 
   y = a·cosh(x/a) + b (电线弧垂模型)
5. Safety clearance check (安全间距检验):
   Minimum distance from line to vegetation/ground = regulated clearance
6. Cross-section profiling (横断面分析): 
   Slice point cloud at corridor stations → 2D profile → AutoCAD output
```

### 3.5 Deep Learning for Point Clouds / 点云深度学习

```
Key architectures (关键架构):
│
├── PointNet (2017) [Qi et al., Stanford]
│   ├── Input: N×3 (or N×6 with normals)
│   ├── Per-point MLP → max pooling (global feature)
│   └── Output: per-point or global classification
│
├── PointNet++ (2017)
│   ├── Hierarchical feature learning (层次特征学习)
│   ├── Ball query grouping (球查询分组)
│   └── Better for non-uniform density
│
├── VoxelNet / PointPillars (autonomous driving)
│   ├── Voxelise point cloud → 3D/2D convolutions
│   └── Real-time object detection
│
├── KPConv (2019) [Kernel Point Convolution]
│   ├── Flexible convolution on point sets
│   └── State-of-the-art for semantic segmentation
│
└── Point Transformer (2021)
    ├── Self-attention on local point sets
    └── Strong performance on ScanNet, ShapeNet

Datasets (数据集):
├── ModelNet40: 3D shape classification (40 classes)
├── ShapeNet: part segmentation
├── S3DIS: indoor scene segmentation
├── Semantic3D: outdoor LiDAR segmentation ← most relevant to you
└── ISPRS Vaihingen 3D: urban aerial LiDAR benchmark
```

### 3.6 Accuracy Metrics for Point Cloud Classification / 点云分类精度指标

```
Confusion matrix → per-class metrics:
├── Overall Accuracy (OA) = Σ correctly classified / total
├── IoU (Intersection over Union) per class:
│   IoU_c = TP_c / (TP_c + FP_c + FN_c)
├── mIoU (mean IoU): average across all classes ← standard metric
└── F1 = 2×Precision×Recall / (Precision+Recall)

Point cloud specific:
├── Completeness: % reference points correctly detected
├── Correctness: % detected points that are correct
└── Quality: TP / (TP + FP + FN)
```

---

## 4. GIS Fieldwork（GIS野外实习）

### 4.1 Overview / 概述

**Duration:** 1 week (4 credits)  
**Purpose:** Integrate classroom knowledge in real field conditions — topographic surveying, GPS data collection, aerial data acquisition, and on-site GIS analysis.

### 4.2 Typical Fieldwork Activities / 典型野外实习活动

```
Day 1: Equipment setup & safety briefing (仪器安装与安全交底)
Day 2: GNSS control survey (控制测量): static GPS, network adjustment
Day 3: Total station traverse (全站仪导线测量): detail survey
Day 4: UAV mission (无人机飞行): flight planning, acquisition, QC
Day 5: Data processing & report (数据处理与报告): DEM, orthophoto, area calculations
(+ optional: LiDAR acquisition on a dedicated fieldwork day)
```

### 4.3 Field Safety & Regulation / 野外安全与法规

- **UAV regulations (无人机法规):** Register drone, respect no-fly zones, maximum altitude.
- **GNSS in forest/canyon (森林/峡谷GNSS):** Signal multipath and masking — use static mode, reoccupy points.
- **Weather window (气象窗口):** UAV flights require wind < 10 m/s, no precipitation, visibility > 5 km.

---

## 5. Thesis I & II（毕业论文一、二）

### 5.1 Structure / 结构

| Stage | Credits | Content |
|---|---|---|
| Thesis I | 10 cr | Literature review, research design, data collection/preliminary analysis |
| Thesis II | 10 cr | Full analysis, results, discussion, defence |

### 5.2 Thesis Writing Framework / 论文撰写框架

```
IMRaD Structure (标准科学论文结构):
├── Introduction (引言)
│   ├── Background & motivation
│   ├── Research gap (研究缺口)
│   ├── Research questions / objectives
│   └── Chapter outline
├── Methods (方法)
│   ├── Study area
│   ├── Data sources (with metadata)
│   ├── Processing workflow (reproducible)
│   └── Validation approach
├── Results (结果) — facts only, no interpretation
├── Discussion (讨论) — interpretation, limitations, future work
└── Conclusion (结论) — concise answers to research questions
```

### 5.3 Research Quality Checklist / 研究质量检查清单

- [ ] Is the research question SMART (Specific, Measurable, Achievable, Relevant, Time-bound)?  
- [ ] Is the dataset clearly documented (source, date, resolution, license)?  
- [ ] Is the workflow reproducible (code/scripts available)?  
- [ ] Are accuracy/uncertainty estimates reported?  
- [ ] Are limitations honestly discussed?  
- [ ] Are results validated against independent reference data?

---

## 6. Internship（实习）

### 6.1 Overview / 概述

**Duration:** 6 weeks (5 credits)  
**Settings:** GIS consultancy, surveying company, research institute, governmental organisation, NGO.

### 6.2 Competencies to Demonstrate / 需展示的能力

```
Technical (技术能力):
├── Apply GIS/RS analysis in a professional context
├── Handle real client data (messy, incomplete, multi-format)
└── Produce professional deliverables (maps, reports, code)

Professional (职业能力):
├── Meet deadlines and client specifications
├── Communicate technical results to non-technical audiences
└── Work within data governance frameworks
```

### 6.3 For International Students / 对国际学生

> The 6-week internship is an opportunity to build your Hungarian/European professional network. Target: research labs at University of Debrecen, Esri Hungary, or environmental consultancies in Eastern Europe. Your LiDAR + GNSS background makes you competitive for surveying-focused placements.  
> 6周实习是建立匈牙利/欧洲职业网络的机会。目标：德布勒森大学研究实验室、Esri匈牙利或东欧环境咨询公司。你的LiDAR+GNSS背景使你在测量类岗位中具有竞争力。
