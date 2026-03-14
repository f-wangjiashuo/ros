# 课程笔记 03：数据采集与分析模块
# Course Notes 03 – Data Collection & Analysis

> 涵盖课程 / Courses covered:  
> Data Mining in Geosciences · Spatial Data Analysis ·  
> Data Collection Techniques · Database Management (lecture + practice)

---

## 1. Data Mining in Geosciences（地球科学数据挖掘）

### 1.1 Core Concept / 核心概念

**Definition:** The process of discovering non-obvious patterns, relationships, and knowledge from large geoscientific datasets using machine learning, statistical, and spatial analysis techniques.  
**定义：** 从大型地球科学数据集中，运用机器学习、统计学和空间分析技术，发现非显而易见的模式、关系和知识的过程。

### 1.2 KDD Pipeline / 知识发现流程

```
Raw Geo-data
    ↓
1. Selection (选择): Choose relevant datasets, attributes, time windows
    ↓
2. Pre-processing (预处理): Handle missing values, outliers, coordinate systems
    ↓
3. Transformation (变换): Feature engineering, normalisation, PCA, spectral indices
    ↓
4. Data Mining (挖掘): Apply algorithms (below)
    ↓
5. Interpretation & Evaluation (解释与评估): Accuracy metrics, spatial validation
    ↓
Knowledge / Insight
```

### 1.3 Key Algorithms for Geospatial Mining / 地理空间挖掘关键算法

| Algorithm | 类型 | Geospatial Use Case |
|---|---|---|
| k-Means clustering | Unsupervised | Land cover segmentation, point cloud clustering |
| DBSCAN | Unsupervised | Outlier detection in GPS tracks, object detection in point clouds |
| Random Forest | Supervised ensemble | Land use classification, species distribution |
| SVM | Supervised | Hyperspectral image classification |
| CNN / PointNet | Deep learning | Point cloud semantic segmentation ← **your interest** |
| LSTM / Transformer | Deep learning | GNSS trajectory anomaly detection |
| Isolation Forest | Anomaly detection | LiDAR noise point removal |
| Association rules | Pattern | Correlation between soil type and crop yield |

### 1.4 Spatial Cross-Validation / 空间交叉验证

> Standard k-fold CV leaks spatial information (adjacent points share autocorrelation). **Spatial cross-validation** (e.g., spatial blocking) ensures test folds are geographically separated from training folds.  
> 标准k折交叉验证会泄露空间信息。空间交叉验证（如空间块划分）确保测试折与训练折在地理上分离。

```
Random CV (错误用于空间数据):  [train|test|train|test] (interleaved)
Spatial block CV (正确):       [====train====] [=test=] (spatially separated blocks)
```

### 1.5 Common Pitfalls / 常见错误

- **Spatial leakage (空间泄露):** Random CV on spatial data gives over-optimistic accuracy — always use spatial CV.  
- **Class imbalance (类别不平衡):** Rare land cover classes underrepresented — use SMOTE or class weights.  
- **Feature multicollinearity (特征多重共线性):** Correlated spectral bands inflate feature importance in tree models — use VIF.

### 1.6 Link to Applicant / 与申请人经历的连接

> Your airborne LiDAR workflow included point cloud classification (ground/vegetation/powerline). This is unsupervised/supervised data mining on 3D geospatial data — exactly what this course covers. You can mention that you used rule-based classification in SouthMap and are keen to extend it with ML-based approaches like PointNet++ in your MSc research.  
> 你的机载LiDAR工作流包括点云分类（地面/植被/输电线路），这正是对三维地理空间数据的无监督/有监督数据挖掘。你可以提到在SouthMap中使用了规则分类，并希望在硕士研究中将其扩展为基于PointNet++的ML方法。

---

## 2. Spatial Data Analysis（空间数据分析）

### 2.1 Core Concept / 核心概念

**Definition:** A suite of quantitative methods that explicitly account for the spatial dimension of data — location, distance, neighbourhood, connectivity — to reveal spatial patterns, relationships, and anomalies.  
**定义：** 一套明确考虑数据空间维度（位置、距离、邻域、连通性）的定量方法，用于揭示空间模式、关系和异常。

### 2.2 Core Analytical Categories / 核心分析类别

```
Spatial Data Analysis
├── Point Pattern Analysis (点模式分析)
│   ├── Quadrat analysis
│   ├── Kernel Density Estimation (KDE)
│   └── Nearest-neighbour distance
│
├── Area/Polygon Analysis (面状分析)
│   ├── Spatial autocorrelation (Moran's I, Geary's C)
│   ├── Local Indicators of Spatial Association (LISA/Local Moran)
│   ├── Spatial regression (SLM, SEM, GWR)
│   └── Choropleth mapping & classification
│
├── Network Analysis (网络分析)
│   ├── Shortest path (Dijkstra, A*)
│   ├── Service area / Isochrone
│   └── Flow analysis
│
└── Surface Analysis (表面分析)
    ├── Interpolation (IDW, Kriging, Spline)
    ├── Slope / Aspect / Curvature
    └── Viewshed / Watershed
```

### 2.3 Geographically Weighted Regression (GWR) / 地理加权回归

```
Standard OLS:  y = β₀ + β₁x₁ + β₂x₂ + ε          (global coefficients)
GWR:           yᵢ = β₀(uᵢ,vᵢ) + β₁(uᵢ,vᵢ)x₁ᵢ + ε  (local coefficients)

(uᵢ, vᵢ) = location of observation i
Bandwidth: controls spatial smoothing (like kernel bandwidth)
Interpretation: coefficients vary spatially → reveals local relationships
```

### 2.4 MAUP — Modifiable Areal Unit Problem / 可变面元问题

> The statistical results from aggregated spatial data (counts, means) change depending on how areas are defined. **Always report the spatial scale of your analysis.**  
> 聚合空间数据的统计结果会随区域划分方式的变化而改变。始终报告分析的空间尺度。

### 2.5 Common Pitfalls / 常见错误

- **Ignoring spatial non-stationarity (忽视空间非平稳性):** Global regression obscures local relationships — use GWR.  
- **Edge effects (边缘效应):** Points near study area boundary have fewer neighbours — use guard zones.  
- **Projection distortion (投影失真):** Distance-based analysis requires equal-area or equidistant projections.

---

## 3. Data Collection Techniques（数据采集技术）

### 3.1 Core Concept / 核心概念

**Definition:** Principles and practices of acquiring primary geospatial data through ground survey, GNSS, total station, LiDAR scanning, photogrammetry, and remote sensing — including sensor calibration, error budgeting, and quality assurance.  
**定义：** 通过地面测量、GNSS、全站仪、LiDAR扫描、摄影测量和遥感获取一手地理空间数据的原理与实践，包括传感器标定、误差预算和质量保证。

### 3.2 GNSS Positioning Modes / GNSS定位模式

| Mode | 模式 | Accuracy | Use Case |
|---|---|---|---|
| Autonomous / SBAS | 自主/星基增强 | 1–5 m | Navigation |
| DGNSS (DGPS) | 差分GNSS | 0.5–2 m | Marine, GIS mapping |
| RTK | 实时动态 | 1–3 cm | Surveying, UAV ground control |
| PPP | 精密单点定位 | 2–10 cm | Remote areas, offshore |
| Post-processed (PPK) | 后处理差分 | 1–3 cm | UAV, airborne LiDAR |
| Static network adjustment | 静态网平差 | mm-level | Geodetic control |

> **iRTK20 + HGO** — Your fieldwork experience with this RTK system is directly relevant here. Be ready to explain: initialization (fixing ambiguities), baseline length limits, multipath mitigation.  
> **iRTK20 + HGO** — 你使用RTK系统的野外经验在此直接相关。准备好解释：初始化（整周模糊度固定）、基线长度限制、多路径误差抑制。

### 3.3 LiDAR System Components / LiDAR系统组成

```
Airborne LiDAR System:
├── Laser scanner (激光扫描仪): pulse generation, range measurement (ToF/phase)
├── IMU (惯性测量单元): roll, pitch, yaw — synchronised with laser
├── GNSS receiver (GNSS接收机): absolute positioning
└── Boresight calibration (视轴校正): angular offset between laser, IMU, GNSS

Point cloud accuracy budget:
σ_xyz² = σ_range² + σ_scan_angle² + σ_GNSS² + σ_IMU² + σ_boresight²
```

### 3.4 Ground Control Points (GCPs) / 地面控制点

```
Rule of thumb:
- Minimum 3 GCPs for affine transformation (平面)
- 4+ GCPs for photogrammetric block (3D)
- Checkpoints (验证点, not used in adjustment): assess absolute accuracy
- Distribution: corners + centre, avoid collinear arrangements
- Precision: GCP precision ≥ 3× desired final product accuracy
```

### 3.5 Error Propagation / 误差传播

```
If z = f(x, y), then:
σ_z² = (∂f/∂x)² σ_x² + (∂f/∂y)² σ_y²   (linear propagation)

Example: Volume = Area × Height
σ_V² = Height² × σ_A² + Area² × σ_h²

→ Your earthwork calculation should include an uncertainty estimate!
→ 你的土方计算应包含不确定性估计！
```

### 3.6 South Lidar Pro Workflow / South Lidar Pro工作流

```
1. Raw data import (原始数据导入): .las / .laz files
2. Trajectory processing (轨迹处理): GNSS/IMU integration
3. Point cloud generation (点云生成): boresight correction
4. Noise removal (噪声去除): statistical outlier removal, range filter
5. Ground filtering (地面滤波): CSF / Progressive TIN
6. DEM generation (DEM生成): TIN interpolation
7. Feature extraction (要素提取): powerline, vegetation, building
8. Product output (成果输出): DEM, DSM, intensity image, cross-sections
```

---

## 4. Database Management（数据库管理）

### 4.1 Core Concept / 核心概念

**Definition:** Design, implementation, and administration of spatial and non-spatial databases — covering relational models (SQL), spatial extensions (PostGIS), data modelling (ER diagrams), and NoSQL approaches for geospatial big data.  
**定义：** 空间和非空间数据库的设计、实施和管理——涵盖关系模型（SQL）、空间扩展（PostGIS）、数据建模（ER图）及地理空间大数据的NoSQL方法。

### 4.2 Relational Database Fundamentals / 关系数据库基础

```
Key concepts (关键概念):
├── Tables / Relations (表/关系): rows (records) + columns (attributes)
├── Primary Key (主键): unique identifier per row
├── Foreign Key (外键): reference to another table's PK → referential integrity
├── Normalisation (规范化):
│   ├── 1NF: no repeating groups (无重复组)
│   ├── 2NF: no partial dependencies (无部分依赖)
│   └── 3NF: no transitive dependencies (无传递依赖)
└── ACID transactions (事务):
    ├── Atomicity (原子性)
    ├── Consistency (一致性)
    ├── Isolation (隔离性)
    └── Durability (持久性)
```

### 4.3 PostGIS — Spatial SQL / PostGIS空间SQL

```sql
-- Create a spatial table (创建空间表)
CREATE TABLE lidar_points (
    id SERIAL PRIMARY KEY,
    classification INTEGER,
    intensity FLOAT,
    geom GEOMETRY(PointZ, 4326)
);

-- Spatial index (空间索引)
CREATE INDEX lidar_geom_idx ON lidar_points USING GIST(geom);

-- Nearest neighbour query (最近邻查询)
SELECT id, ST_Distance(geom, ST_MakePoint(117.2, 32.1, 0)::GEOGRAPHY) AS dist
FROM lidar_points
ORDER BY dist LIMIT 10;

-- Buffer intersection (缓冲区相交)
SELECT * FROM lidar_points
WHERE ST_DWithin(geom::GEOGRAPHY, 
                 ST_MakePoint(117.2, 32.1)::GEOGRAPHY, 100);
```

### 4.4 OGC Simple Features Standard / OGC简单要素标准

| Geometry Type | 几何类型 | Example |
|---|---|---|
| Point / PointZ | 点 / 三维点 | Survey station, LiDAR point |
| LineString | 线串 | Road, powerline |
| Polygon | 多边形 | Land parcel, building footprint |
| MultiPoint | 多点集 | GNSS epoch positions |
| GeometryCollection | 几何集合 | Mixed-type layers |

### 4.5 NoSQL for Big Geospatial Data / 大地理空间数据的NoSQL

| Type | 类型 | Product | Use Case |
|---|---|---|---|
| Document | 文档型 | MongoDB + GeoJSON | Sensor metadata |
| Key-value | 键值型 | Redis Geo | Real-time location |
| Column | 列存储 | Apache Cassandra | Time-series trajectory |
| Graph | 图数据库 | Neo4j | Road network topology |

### 4.6 Common Pitfalls / 常见错误

- **Wrong SRID (坐标系错误):** Mixing EPSG:4326 (geographic) with EPSG:32633 (projected) without reprojection.  
- **Missing spatial index (缺少空间索引):** Full table scan on millions of points — orders of magnitude slower.  
- **No backup strategy (无备份策略):** Always implement pg_dump + off-site backup for production geodatabases.
