# 课程笔记 05：计算机科学与编程模块
# Course Notes 05 – CS & Programming

> 涵盖课程 / Courses covered:  
> GIS-specific Programming · Technical Informatics (lecture + practice)

---

## 1. GIS-specific Programming（GIS专项编程）

### 1.1 Core Concept / 核心概念

**Definition:** Programming geospatial workflows using Python (the dominant language for GIS automation), including spatial data I/O, geometric operations, raster/vector analysis, web service integration, and spatial machine learning pipelines.  
**定义：** 使用Python（GIS自动化主流语言）编写地理空间工作流，包括空间数据I/O、几何运算、栅格/矢量分析、Web服务集成和空间机器学习管线。

### 1.2 Core GIS Python Libraries / 核心GIS Python库

```
GIS Python Ecosystem (GIS Python生态系统):
│
├── Data I/O
│   ├── GDAL/OGR       — raster + vector (C++ binding, universal)
│   ├── Fiona          — vector read/write (pythonic OGR wrapper)
│   ├── Rasterio       — raster I/O (pythonic GDAL wrapper)
│   └── laspy/pylas    — LAS/LAZ point cloud I/O ← key for your interests
│
├── Geometry & Analysis
│   ├── Shapely        — geometric operations (buffer, union, intersection)
│   ├── GeoPandas      — spatial DataFrame (Pandas + Shapely + Fiona)
│   ├── PyProj         — coordinate transformations
│   └── PySAL          — spatial statistics, econometrics
│
├── Raster Analysis
│   ├── Numpy          — array operations on raster bands
│   ├── SciPy          — signal processing, interpolation
│   ├── Scikit-image   — image processing, morphology
│   └── WhiteboxTools  — terrain, hydrology (Python wrapper)
│
├── Machine Learning
│   ├── Scikit-learn   — classical ML (RF, SVM, k-Means)
│   ├── PyTorch/TF     — deep learning (CNN, PointNet)
│   └── Open3D / PDAL  — point cloud processing ← key library
│
├── Web & Visualisation
│   ├── Folium         — interactive Leaflet maps in Python
│   ├── Plotly/Dash    — web dashboards
│   └── Matplotlib/Cartopy — static maps
│
└── Spatial Database
    ├── SQLAlchemy + GeoAlchemy2 — ORM for PostGIS
    └── psycopg2       — direct PostgreSQL/PostGIS access
```

### 1.3 Common Patterns / 常用代码模式

```python
# --- Vector overlay (矢量叠加) ---
import geopandas as gpd

parcels = gpd.read_file("parcels.shp")
flood_zone = gpd.read_file("flood_100yr.gpkg")
at_risk = gpd.overlay(parcels, flood_zone, how="intersection")
at_risk.to_file("at_risk_parcels.gpkg", driver="GPKG")

# --- Raster band math (栅格波段运算) ---
import rasterio
import numpy as np

with rasterio.open("sentinel2.tif") as src:
    nir = src.read(4).astype(float)   # Band 4 = NIR
    red = src.read(3).astype(float)   # Band 3 = Red
    ndvi = (nir - red) / (nir + red + 1e-10)
    profile = src.profile.copy()

profile.update(count=1, dtype=rasterio.float32)
with rasterio.open("ndvi.tif", "w", **profile) as dst:
    dst.write(ndvi.astype(np.float32), 1)

# --- Point cloud processing with PDAL (点云处理) ---
import pdal
import json

pipeline_json = {
    "pipeline": [
        {"type": "readers.las", "filename": "input.las"},
        {"type": "filters.smrf"},           # Ground classification
        {"type": "filters.range",
         "limits": "Classification[2:2]"},  # Keep ground only
        {"type": "writers.las", "filename": "ground.las"}
    ]
}
pipeline = pdal.Pipeline(json.dumps(pipeline_json))
pipeline.execute()
```

### 1.4 Algorithm Complexity for Spatial Operations / 空间操作算法复杂度

```
Operation              Naive         Optimised (R-tree)
Point-in-polygon       O(n×m)        O(n log m)  ← use STRtree
Nearest neighbour      O(n×m)        O(n log m)  ← use KDTree/BallTree
Spatial join           O(n×m)        O((n+m) log m)
Buffer + dissolve      O(n²)         O(n log n)  ← use spatial index

→ Always build spatial indices on large datasets!
→ 大数据集始终建立空间索引！
```

### 1.5 Reproducibility Best Practices / 可重复性最佳实践

```
1. Version control (版本控制): Git for code + DVC for datasets
2. Environment management: conda env + requirements.txt / pyproject.toml
3. Configuration files: YAML/JSON for parameters (not hardcoded)
4. Logging: structured logging with timestamps and parameter values
5. Unit tests: test spatial functions with known-answer test cases
6. Documentation: docstrings + README with data sources
```

### 1.6 Your GNSS Code → GIS Context / 你的GNSS代码→GIS语境

```python
# Your Kalman filter implementation (Python) adapts directly to:

# 1. GNSS trajectory smoothing → maps to spatial trajectory analysis
# 2. Least squares adjustment → maps to GCP-based georeferencing
# 3. Error propagation → maps to uncertainty quantification in GIS

# Example: converting your GNSS result to GIS-ready format
from pyproj import Transformer

transformer = Transformer.from_crs("EPSG:4978", "EPSG:4326")  # ECEF → WGS84
lat, lon, h = transformer.transform(X_ecef, Y_ecef, Z_ecef)
```

---

## 2. Technical Informatics（技术信息学）

### 2.1 Core Concept / 核心概念

**Definition:** Principles of computer systems, data communication, embedded systems, and real-time processing as applied to geospatial instrumentation — covering hardware-software interfaces, sensor networks, IoT, and edge computing for field data collection.  
**定义：** 计算机系统、数据通信、嵌入式系统和实时处理原理在地理空间仪器中的应用，涵盖硬件-软件接口、传感器网络、物联网和边缘计算用于野外数据采集。

### 2.2 Computer Architecture Essentials / 计算机体系结构要点

```
CPU Pipeline: Fetch → Decode → Execute → Memory → Write-back
Memory hierarchy (内存层次): Registers → L1/L2/L3 Cache → RAM → SSD → HDD
Parallelism (并行化):
├── SIMD (Single Instruction Multiple Data): vectorised raster operations
├── Multi-threading: concurrent point cloud tile processing
└── GPU (CUDA/OpenCL): deep learning on point clouds (PointNet, VoxelNet)
```

### 2.3 Data Communication Protocols / 数据通信协议

| Protocol | 层级 | Geospatial Application |
|---|---|---|
| NMEA 0183 / 2000 | Sensor | GNSS receiver output ← you've used this |
| NTRIP | Application | RTK correction stream over internet |
| HTTP/REST | Application | WMS, WFS, OGC API Features |
| MQTT | Application | IoT sensor data (soil moisture, weather) |
| gRPC | Application | High-performance ML inference services |
| CAN Bus | Field | UAV autopilot–sensor communication |

### 2.4 Real-Time Systems Concepts / 实时系统概念

```
Hard real-time (硬实时): deadline miss = system failure (e.g., autopilot)
Soft real-time (软实时): deadline miss = degraded performance (e.g., map tile rendering)

GNSS Real-time chain:
Satellite → L1/L2 signal → Receiver (signal processing) → 
NMEA output → IMU fusion (Kalman filter) → 
RTK correction (NTRIP) → Position fix → Application

Latency budget (延迟预算):
Signal travel: ~67 ms (20,000 km at c)
Processing:    ~10–50 ms
Total E2E:     ~100–200 ms for RTK fix
```

### 2.5 Embedded Systems for LiDAR / LiDAR嵌入式系统

```
Typical LiDAR + IMU + GNSS integration board:
├── MCU/SoC (e.g., NVIDIA Jetson, Raspberry Pi CM4)
├── PPS (Pulse Per Second) — GNSS time synchronisation
├── Serial/SPI/I2C — sensor data acquisition
└── ROS 2 — middleware for sensor data fusion (directly relevant to this repo!)

ROS 2 topics for LiDAR:
/velodyne_points   — raw PointCloud2 message
/imu/data          — IMU acceleration + angular velocity
/gnss/fix          — NavSatFix message
/tf                — coordinate frame transforms
```

> **Note:** The repository you're in is named "ros" — suggesting ROS (Robot Operating System) context. Your future research in autonomous LiDAR understanding directly maps to ROS 2 sensor fusion architectures.  
> **注：** 你所在的仓库名为"ros"，暗示ROS（机器人操作系统）语境。你未来的自主LiDAR理解研究直接对应ROS 2传感器融合架构。

### 2.6 Cloud & HPC for Geospatial / 云计算与HPC

```
Cloud platforms (云平台):
├── Google Earth Engine (GEE): petabyte-scale RS analysis
├── AWS S3 + Lambda: serverless raster processing
├── Azure Maps: spatial analytics + ML
└── STAC (SpatioTemporal Asset Catalog): cloud-native data discovery

HPC for point cloud (高性能计算):
├── MPI: distributed processing across nodes
├── OpenMP: shared-memory parallelism
└── CUDA: GPU-accelerated voxelisation, k-NN search
```

### 2.7 Common Pitfalls / 常见错误

- **Byte order mismatch (字节序错误):** Big-endian vs. little-endian in binary sensor data protocols.  
- **Time synchronisation (时间同步):** Without PPS sync, GNSS-IMU time offset causes localisation error.  
- **Buffer overflow (缓冲区溢出):** Real-time sensor data queues must be sized for worst-case latency spikes.

### 2.8 Link to Applicant / 与申请人经历的连接

> Your GNSS implementation in Python/MATLAB/C++ is textbook technical informatics: you designed a real-time-capable pipeline (Kalman filter has strict time-ordering requirements), interfaced with hardware (iRTK20), and processed NMEA/raw observation data. In the interview, describe the pipeline end-to-end and mention any timing or precision challenges you encountered.  
> 你用Python/MATLAB/C++实现的GNSS系统是典型的技术信息学：你设计了具备实时能力的管线（卡尔曼滤波有严格的时序要求），与硬件（iRTK20）进行了接口，并处理了NMEA/原始观测数据。面试时，从头到尾描述该管线，并提及你遇到的任何时序或精度挑战。
