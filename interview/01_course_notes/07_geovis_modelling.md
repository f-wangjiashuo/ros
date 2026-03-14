# 课程笔记 07：地理可视化与建模模块
# Course Notes 07 – Geovisualisation & Modelling

> 涵盖课程 / Courses covered:  
> Maps on WEB · Models in GIS (lecture + practice) · Geovisualisation

---

## 1. Maps on WEB（网络地图）

### 1.1 Core Concept / 核心概念

**Definition:** Design, development, and deployment of interactive web-based cartographic applications — from tile-based slippy maps to dynamic, data-driven visualisations using modern web GIS frameworks.  
**定义：** 设计、开发和部署基于Web的交互式地图应用，从瓦片式滑动地图到使用现代Web GIS框架的动态数据驱动可视化。

### 1.2 Web Map Architecture / 网络地图架构

```
Client (浏览器)                Server (服务器)
    │                              │
    │  HTTP request for tile        │
    │  z/x/y  ──────────────────► │ Tile cache (缓存)
    │  ◄──────────────────  PNG   │  │
    │                              │  ├── Raster tiles (GeoTIFF → MBTiles)
    │  WFS/API request             │  ├── Vector tiles (MVT/PBF)
    │  ──────────────────────────► │  └── Dynamic rendering (GeoServer/MapServer)
    │  ◄──────────────── GeoJSON  │

Tile pyramid (瓦片金字塔):
Zoom 0: 1 tile (whole world)
Zoom 10: 1,048,576 tiles
Zoom 18: 68 billion tiles (城市街道级别)
```

### 1.3 Key Frameworks / 关键框架

| Framework | Language | Strengths | Use Case |
|---|---|---|---|
| Leaflet.js | JS | Lightweight, easy | Simple web maps |
| OpenLayers | JS | OGC standard support | Enterprise web GIS |
| Mapbox GL JS | JS | Vector tiles, 3D | Modern rich maps |
| Deck.gl | JS | Big data visualisation | Millions of points |
| CesiumJS | JS | 3D globe, point clouds | 3D city models, LiDAR viewer |
| Folium (Python) | Python | Quick prototyping | Data science maps |

### 1.4 OGC Web Services / OGC网络服务

```
WMS (Web Map Service 网络地图服务):
→ Returns rendered images (PNG/JPEG)
→ GetCapabilities, GetMap, GetFeatureInfo
→ Example: http://server/wms?SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap
             &LAYERS=lidar_dem&BBOX=...&WIDTH=800&HEIGHT=600&FORMAT=image/png

WFS (Web Feature Service 网络要素服务):
→ Returns vector data (GeoJSON/GML)
→ GetCapabilities, GetFeature, DescribeFeatureType
→ Supports spatial filtering, attribute queries

WCS (Web Coverage Service 网络覆盖服务):
→ Returns raster data (raw values, not rendered)
→ Suitable for DEM, multispectral rasters

OGC API Features (modern REST equivalent of WFS):
→ RESTful, JSON-LD, OpenAPI spec
→ Example: http://server/ogcapi/collections/powerlines/items?bbox=...
```

### 1.5 Cartographic Design Principles / 制图设计原则

```
Bertin's Retinal Variables (贝尔坦视觉变量):
├── Position (位置): most powerful variable
├── Size (大小): proportional symbols
├── Shape (形状): categorical symbols
├── Colour hue (色相): categorical data
├── Colour value (明度): quantitative data (light=low, dark=high)
├── Texture (纹理): qualitative variation
└── Orientation (方向): directional data

Colour rules (色彩规则):
├── Sequential: single hue gradient (quantitative: 单色渐变)
├── Diverging: two hues from centre (signed data: 双色发散)
└── Qualitative: distinct hues (categories: 独特色相)
→ Use ColorBrewer (colorbrewer2.org) for perceptually uniform palettes
```

---

## 2. Models in GIS（GIS模型）

### 2.1 Core Concept / 核心概念

**Definition:** The construction and application of spatial process models — analytical workflows that transform spatial data into outputs representing real-world processes — including terrain, hydrological, network, and environmental models.  
**定义：** 构建和应用空间过程模型——将空间数据转化为代表真实世界过程的输出的分析工作流，包括地形、水文、网络和环境模型。

### 2.2 Model Types / 模型类型

```
GIS Model Taxonomy (GIS模型分类):
│
├── Terrain Models (地形模型)
│   ├── DEM (Digital Elevation Model): bare earth
│   ├── DSM (Digital Surface Model): with features (buildings, trees)
│   ├── CHM (Canopy Height Model): DSM - DEM
│   ├── Slope, Aspect, Curvature: DEM derivatives
│   └── TPI (Topographic Position Index), TRI (Terrain Ruggedness)
│
├── Hydrological Models (水文模型)
│   ├── Flow direction (D8, D-infinity)
│   ├── Flow accumulation → stream network
│   ├── Watershed delineation
│   └── Runoff modelling (SCS Curve Number)
│
├── Network Models (网络模型)
│   ├── Topological network (nodes, edges, connectivity)
│   ├── Directed vs. undirected
│   ├── Attribute-weighted (distance, time, cost)
│   └── Algorithms: Dijkstra, A*, Travelling Salesman
│
├── Suitability Models (适宜性模型)
│   ├── MCE (weighted overlay)
│   └── Boolean / fuzzy membership
│
└── Simulation Models (仿真模型)
    ├── Agent-based models (ABM): pedestrian movement
    ├── Cellular automata: urban growth
    └── Statistical simulation: Monte Carlo DEM uncertainty
```

### 2.3 DEM Analysis — Core Tools / DEM分析核心工具

```python
# Slope from DEM (GDAL/Numpy approach)
import numpy as np
from scipy.ndimage import generic_filter

def slope_degrees(dem, cell_size):
    """Calculate slope in degrees from DEM array."""
    # Sobel operators for gradient
    dz_dx = generic_filter(dem, lambda x: (x[5]-x[3])/(2*cell_size), size=3)
    dz_dy = generic_filter(dem, lambda x: (x[7]-x[1])/(2*cell_size), size=3)
    slope = np.degrees(np.arctan(np.sqrt(dz_dx**2 + dz_dy**2)))
    return slope
```

### 2.4 Spatial Model Validation / 空间模型验证

```
Validation strategy (验证策略):
├── Split validation: 70% train, 30% test (spatial blocks!)
├── Cross-validation: k-fold spatial CV
├── Known-answer test: run model on synthetic data with known output
└── Sensitivity analysis: vary parameters → assess output uncertainty

Key metrics (关键指标):
├── RMSE (Root Mean Square Error): overall magnitude of errors
├── MAE (Mean Absolute Error): robust to outliers
├── Bias: systematic over/under-estimation
├── R² (Nash-Sutcliffe for hydrological models)
└── Spatial pattern metrics: fractal dimension, connectivity
```

---

## 3. Geovisualisation（地理可视化）

### 3.1 Core Concept / 核心概念

**Definition:** The visual exploration, analysis, and communication of geospatial data — encompassing static maps, interactive cartography, 3D visualisation, animation, and geostatistical displays that support human cognition and decision-making.  
**定义：** 地理空间数据的视觉探索、分析和传达——涵盖支持人类认知和决策的静态地图、交互式制图、三维可视化、动画和地统计显示。

### 3.2 Visualisation Types / 可视化类型

| Type | 类型 | Best For | Tools |
|---|---|---|---|
| Choropleth | 分级统色图 | Aggregated area data | QGIS, D3.js |
| Proportional symbols | 比例符号图 | Point data magnitudes | Leaflet, Mapbox |
| Heat map / KDE | 热力图 | Point density | QGIS, Kepler.gl |
| 3D terrain + draping | 三维地形叠加 | Landscape visualisation | CesiumJS, QGIS2threejs |
| Point cloud viewer | 点云浏览器 | LiDAR data | CloudCompare, Potree |
| Space-time cube | 时空立方体 | Temporal patterns | ArcGIS Pro, D3 |
| Sankey diagram | 桑基图 | Flow data | Plotly |
| Bivariate choropleth | 双变量分级图 | Two simultaneous variables | R ggplot2 |

### 3.3 Potree — Web-based Point Cloud Visualisation / Potree网络点云可视化

```
Potree is the standard tool for sharing massive point clouds on the web:
Potree是在网络上共享大规模点云的标准工具：

Workflow (工作流):
1. Convert LAS/LAZ → Potree format (PotreeConverter)
2. Host on web server (any static file server)
3. Embed in HTML with Potree viewer

<html>
  <script src="libs/potree/potree.js"></script>
  <div id="potree_render_area"></div>
  <script>
    const viewer = new Potree.Viewer(document.getElementById("potree_render_area"));
    Potree.loadPointCloud("pointcloud/metadata.json", "LiDAR", e => {
        viewer.scene.addPointCloud(e.pointcloud);
        viewer.fitToScreen();
    });
  </script>
</html>

→ Your LiDAR competition output could be published this way for your portfolio!
→ 你的LiDAR竞赛成果可以通过这种方式发布到你的作品集！
```

### 3.4 Perceptual Considerations / 感知注意事项

```
Lie factor (谎言因子) [Tufte]:
Lie factor = (size of effect in graphic) / (size of effect in data)
Good visualisation: Lie factor ≈ 1.0

Common misrepresentations (常见误导):
├── 3D pie charts: distort relative proportions (避免3D饼图)
├── Truncated Y-axis: exaggerates differences (避免截断Y轴)
├── Rainbow colour maps: not perceptually linear (使用viridis/plasma)
└── Not accounting for MAUP in choropleth (注意MAUP效应)

Accessibility (无障碍性):
├── Colour-blind safe palettes (色盲友好色板): ColorBrewer
├── Sufficient contrast ratio: WCAG 2.1 ≥ 4.5:1
└── Alt text for embedded maps
```

### 3.5 3D City Models & Digital Twins / 三维城市模型与数字孪生

```
CityGML LOD Levels (细节层次):
├── LOD 0: 2.5D footprint
├── LOD 1: block model (box buildings)
├── LOD 2: roof structure, facades
├── LOD 3: architectural details, windows
└── LOD 4: interior rooms

LiDAR → 3D City Model pipeline:
Point cloud → Building segmentation → Rooftop reconstruction → 
LOD 2 mesh → CityGML export → CesiumJS / 3D Tiles web view

→ This is directly your research interest area!
→ 这直接就是你的研究兴趣领域！
```
