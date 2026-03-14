# 课程笔记 06：专项应用模块
# Course Notes 06 – Specific Applications

> 涵盖课程 / Courses covered:  
> Applied Agriculture Informatics · Applied GIS in Regional Development ·  
> Open Source GIS · CAD System · Environmental Informatics

---

## 1. Applied Agriculture Informatics（应用农业信息学）

### 1.1 Core Concept / 核心概念

**Definition:** Integration of geospatial technologies (RS, UAV, GIS), IoT sensors, and data analytics to enable precision agriculture — optimising crop management, resource use, and yield prediction at sub-field resolution.  
**定义：** 整合地理空间技术（遥感、无人机、GIS）、物联网传感器和数据分析以实现精准农业——在亚田块分辨率上优化作物管理、资源利用和产量预测。

### 1.2 Precision Agriculture Workflow / 精准农业工作流

```
1. Field boundary delineation (田块边界划定)
   → GIS polygon digitisation, GPS surveying

2. Soil sampling + analysis (土壤采样与分析)
   → Grid / zone-based sampling
   → Geostatistical interpolation (Kriging) → soil property maps

3. Crop monitoring (作物监测)
   → NDVI time series from Sentinel-2 / UAV multispectral
   → Identify stress zones, growth variability

4. Variable Rate Application (变量施肥/喷药)
   → VRA maps from soil + crop maps
   → Export to precision tractor / sprayer

5. Yield monitoring (产量监测)
   → GPS-enabled combine harvester → yield map
   → Validate against RS predictions
```

### 1.3 Key Indices for Crop Assessment / 作物评估关键指数

| Index | Formula | Sensitivity |
|---|---|---|
| NDVI | (NIR-R)/(NIR+R) | General vegetation vigour |
| NDRE | (RedEdge-R)/(RedEdge+R) | Chlorophyll content (canopy) |
| GNDVI | (NIR-G)/(NIR+G) | Photosynthetic capacity |
| CWSI | (Tc-Ta)/(Tc_dry-Ta) | Crop water stress (thermal) |
| LAI | Leaf Area Index | Canopy structure, biomass |

### 1.4 Link to Applicant / 与申请人经历的连接

> Your competition involved terrain analysis for earthwork — precision agriculture is the same GIS toolbox applied to agronomic decision-making. The UAV workflow you know (flight planning → image processing → DEM/DSM) is identical to UAV-based crop monitoring, just with a multispectral sensor instead of LiDAR.  
> 你的竞赛涉及土方计算的地形分析——精准农业是将同一GIS工具箱应用于农艺决策。你熟悉的无人机工作流（任务规划→影像处理→DEM/DSM）与无人机作物监测完全相同，只是使用多光谱传感器替代了LiDAR。

---

## 2. Applied GIS in Regional Development（区域发展应用GIS）

### 2.1 Core Concept / 核心概念

**Definition:** Application of GIS for spatial planning, regional analysis, urban-rural development, infrastructure planning, and policy evaluation — bridging spatial data with socioeconomic decision-making.  
**定义：** 将GIS应用于空间规划、区域分析、城乡发展、基础设施规划和政策评估——连接空间数据与社会经济决策。

### 2.2 Core Applications / 核心应用

| Application | 应用 | GIS Methods |
|---|---|---|
| Land use change analysis | 土地利用变化分析 | Multi-temporal RS classification |
| Site suitability analysis | 选址适宜性分析 | Multi-criteria evaluation (MCE/AHP) |
| Infrastructure planning | 基础设施规划 | Least-cost path, network analysis |
| Population accessibility | 人口可达性分析 | Service area, travel time isochrones |
| Urban heat island | 城市热岛效应 | Thermal RS + land cover regression |
| Flood risk assessment | 洪水风险评估 | DEM hydrological modelling |

### 2.3 Multi-Criteria Evaluation (MCE) / 多准则评价

```
Example: Wind farm site selection (风电场选址)

Criteria (准则):
├── Wind speed > 6 m/s            (weight: 0.35)
├── Distance from settlements > 500m (weight: 0.25)
├── Slope < 15°                   (weight: 0.20)
├── Accessible by road < 5 km     (weight: 0.15)
└── Not in protected area         (weight: 0.05)

Steps (步骤):
1. Rasterise each criterion
2. Standardise to 0–1 scale
3. Apply weights (AHP or expert-defined)
4. Weighted overlay → suitability map
5. Validate with existing wind farm locations
```

### 2.4 NUTS Classification System / NUTS区域分类系统

> Relevant for Hungary/EU context. NUTS (Nomenclature of Territorial Units for Statistics) is the EU's hierarchical system for regional statistics — NUTS 0 = country, NUTS 1 = major regions, NUTS 2 = basic regions (EU Cohesion Policy), NUTS 3 = small regions. Debrecen is in the NUTS 3 unit of Hajdú-Bihar county (HU321).  
> NUTS（统计领土单元命名系统）是欧盟区域统计的层级体系——NUTS 0=国家，NUTS 1=主要地区，NUTS 2=基本地区（EU凝聚政策），NUTS 3=小区域。德布勒森位于豪伊杜-比豪尔县（HU321）NUTS 3单元。

---

## 3. Open Source GIS（开源GIS）

### 3.1 Core Open Source GIS Ecosystem / 核心开源GIS生态系统

```
OSGeo Stack (开源地理空间基金会软件栈):
│
├── Desktop GIS
│   ├── QGIS            — full-featured desktop GIS (ArcGIS open-source equivalent)
│   └── GRASS GIS       — raster/vector analysis, scripting
│
├── Web GIS
│   ├── GeoServer       — OGC-compliant WMS/WFS/WCS server
│   ├── MapServer       — lightweight map server
│   └── GeoNode         — spatial data portal (Django-based)
│
├── Databases
│   ├── PostgreSQL/PostGIS — spatial RDBMS
│   └── SpatiaLite      — SQLite + spatial extension
│
├── Libraries
│   ├── GDAL/OGR        — universal raster/vector translation
│   ├── PROJ            — coordinate transformations
│   ├── GEOS            — geometric engine
│   └── PDAL            — point cloud processing pipeline
│
└── Point Cloud
    ├── PDAL            — pipeline-based point cloud processing
    ├── LAStools        — fast LAS/LAZ processing (partially open)
    ├── CloudCompare    — 3D point cloud/mesh editor
    └── Open3D          — ML-friendly point cloud library (Python)
```

### 3.2 QGIS vs. ArcGIS / QGIS对比ArcGIS

| Feature | QGIS | ArcGIS Pro |
|---|---|---|
| Cost | Free / open source | Commercial licence |
| Plugin ecosystem | 1000+ plugins | Extensions (paid) |
| Python API | PyQGIS | ArcPy |
| Performance | Adequate for most tasks | Better for very large datasets |
| 3D support | Limited (Qgis2threejs) | Full 3D scenes |
| Point cloud | LAStools/PDAL plugin | Native |

> You used ArcGIS in your competition — in the MSc you'll expand to open-source tools. The concepts are identical; only the interface changes.  
> 你在竞赛中使用了ArcGIS——在硕士学习中你将扩展到开源工具。概念完全相同，只是界面不同。

### 3.3 PDAL Pipeline Example / PDAL管线示例

```json
{
  "pipeline": [
    {"type": "readers.las", "filename": "input.las"},
    {"type": "filters.elm"},
    {"type": "filters.outlier", "method": "statistical", "mean_k": 12, "multiplier": 2.2},
    {"type": "filters.smrf", "window": 18, "slope": 0.15, "threshold": 0.5},
    {"type": "filters.range", "limits": "Classification[2:2]"},
    {
      "type": "writers.gdal",
      "filename": "dem.tif",
      "resolution": 0.5,
      "output_type": "mean",
      "gdaldriver": "GTiff"
    }
  ]
}
```

---

## 4. CAD System（CAD系统）

### 4.1 Core Concept / 核心概念

**Definition:** Computer-Aided Design software used in geospatial contexts for precise 2D/3D drafting, infrastructure design, and BIM (Building Information Modelling) — with workflows for integrating survey data into engineering designs.  
**定义：** 地理空间情境下用于精确2D/3D制图、基础设施设计和BIM（建筑信息模型）的计算机辅助设计软件，包含将测量数据集成到工程设计中的工作流。

### 4.2 CAD–GIS Integration / CAD与GIS的集成

```
Data flow (数据流):
Survey (total station/GNSS) → CAD (design) → GIS (spatial planning)

Formats (格式):
├── DXF/DWG — AutoCAD native (most common in CAD)
├── DGN    — Bentley MicroStation
└── IFC    — BIM open standard (open for analysis in GIS)

Coordinate systems (坐标系统):
CAD: often local arbitrary system or national grid
GIS: geographic/projected (must georeference CAD data)

CAD → GIS conversion (转换):
QGIS: Layer → Add Layer → Add Vector Layer (DXF)
ArcGIS: CAD to Geodatabase tool
```

### 4.3 Relevance to LiDAR / 与LiDAR的关联

> Cross-section analysis in your competition (powerline corridor profiling) is directly a CAD task: extracting 2D profiles from 3D terrain data. The CAD course will formalise skills you've already partially developed: precise measurement, dimensioning, coordinate transformations, and technical drawing standards.  
> 你竞赛中的横断面分析（输电线路廊道剖面）直接是CAD任务：从三维地形数据中提取二维剖面。CAD课程将正式化你已部分掌握的技能：精确量测、标注、坐标转换和技术制图规范。

---

## 5. Environmental Informatics（环境信息学）

### 5.1 Core Concept / 核心概念

**Definition:** Application of computer science, data science, and geospatial analysis to environmental monitoring, modelling, and management — from air quality sensors to ecosystem service assessment.  
**定义：** 将计算机科学、数据科学和地理空间分析应用于环境监测、建模和管理——从空气质量传感器到生态系统服务评估。

### 5.2 Key Application Domains / 关键应用领域

| Domain | 领域 | Data Sources | Analysis |
|---|---|---|---|
| Air quality | 空气质量 | IoT sensors, satellite (Sentinel-5P) | Interpolation, source attribution |
| Water quality | 水质 | In-situ, hyperspectral RS | Turbidity, chl-a mapping |
| Urban green space | 城市绿地 | LiDAR CHM, multispectral | Tree canopy cover, ecosystem services |
| Carbon accounting | 碳核算 | LiDAR biomass, flux towers | Above-ground carbon stock |
| Noise mapping | 噪声制图 | Traffic counts + propagation model | EU Environmental Noise Directive |

### 5.3 Ecosystem Services Valuation / 生态系统服务评估

```
InVEST (Integrated Valuation of Ecosystem Services and Tradeoffs):
├── Carbon storage: biomass from LiDAR CHM × density factors
├── Habitat quality: threat layers + sensitivity maps
├── Water yield: curve number + precipitation
└── Coastal blue carbon: mangrove + seagrass extent

LiDAR contribution (LiDAR贡献):
├── Canopy Height Model (CHM) → tree height → biomass
├── LAD (Leaf Area Density) → photosynthesis potential
└── Ground surface model → hydrological connectivity
```

### 5.4 Environmental Decision Support Systems (EDSS) / 环境决策支持系统

```
Components (组成):
├── Data layer: sensor networks, RS, field measurements
├── Model layer: process models (hydrological, atmospheric, ecological)
├── GIS layer: spatial integration, visualisation
├── Knowledge base: expert rules, regulatory thresholds
└── Interface: dashboard for decision-makers

Example: Flood EDSS
→ Real-time rain gauge + DEM + hydrological model → flood extent forecast → 
   WebGIS dashboard → evacuation route planning
```
