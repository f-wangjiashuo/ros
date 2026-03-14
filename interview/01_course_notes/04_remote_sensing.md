# 课程笔记 04：遥感模块
# Course Notes 04 – Remote Sensing

> 涵盖课程 / Courses covered:  
> Hyperspectral Remote Sensing · Multispectral Remote Sensing ·  
> UAV Remote Sensing · Photogrammetry

---

## 1. Multispectral Remote Sensing（多光谱遥感）

### 1.1 Core Concept / 核心概念

**Definition:** Remote sensing that captures reflected/emitted electromagnetic energy in a limited number of broad spectral bands (typically 3–10), enabling land cover classification, vegetation monitoring, and change detection.  
**定义：** 在有限数量的宽光谱波段（通常3–10个）中捕获反射/发射电磁能量，用于土地覆盖分类、植被监测和变化检测。

### 1.2 Key Sensors & Specifications / 关键传感器与规格

| Sensor | Bands | Resolution | Revisit | Use |
|---|---|---|---|---|
| Landsat 8/9 OLI | 11 | 30 m (15 m pan) | 16 days | Land cover, change |
| Sentinel-2 MSI | 13 | 10/20/60 m | 5 days | Agriculture, forestry |
| SPOT 7 | 4 | 1.5/6 m | 1–3 days | Urban, cadastral |
| Planet SuperDove | 8 | 3.7 m | Daily | Rapid change |
| UAV multispectral | 5–10 | cm-level | On-demand | Precision agriculture |

### 1.3 Spectral Indices / 光谱指数

```
NDVI (归一化植被指数):
    NDVI = (NIR - Red) / (NIR + Red)
    Range: -1 to +1; vegetation > 0.3

NDWI (归一化水体指数):
    NDWI = (Green - NIR) / (Green + NIR)
    Water bodies > 0; vegetation < 0

NDBI (归一化建筑指数):
    NDBI = (SWIR - NIR) / (SWIR + NIR)
    Built-up areas > 0

EVI (增强植被指数):
    EVI = 2.5 × (NIR - Red) / (NIR + 6×Red - 7.5×Blue + 1)
    Less soil/atmosphere sensitive than NDVI
```

### 1.4 Image Classification Workflow / 图像分类工作流

```
1. Pre-processing (预处理)
   ├── Radiometric calibration: DN → Radiance → Reflectance
   ├── Atmospheric correction: FLAASH, Sen2Cor, 6S
   ├── Geometric correction: GCP-based orthorectification
   └── Mosaic & clipping

2. Classification (分类)
   ├── Supervised: SVM, Random Forest, CNN
   │   └── Requires training samples (训练样本)
   └── Unsupervised: k-Means, ISODATA
       └── Requires post-classification labelling

3. Accuracy Assessment (精度评估)
   ├── Confusion matrix (混淆矩阵)
   ├── Overall Accuracy (OA) = correct / total
   ├── Kappa coefficient (Kappa系数)
   └── Producer's / User's Accuracy (生产者/用户精度)
```

### 1.5 Common Pitfalls / 常见错误

- **Training sample bias (训练样本偏差):** Samples not representative of target class variability.  
- **Atmospheric effects uncorrected (未校正大气效应):** Haze adds additive offset, scatters blue band.  
- **Mixed pixels (混合像元):** Subpixel classification (unmixing) needed for coarse resolution data.

### 1.6 Link to Applicant / 与申请人经历的连接

> You used ENVI for spectral analysis in your LiDAR competition. Multispectral RS complements LiDAR: while LiDAR provides precise 3D structure, multispectral imagery provides spectral/radiometric attributes. Fusion of LiDAR + Multispectral is a key approach in urban tree detection, powerline corridor monitoring, and precision agriculture — all directly relevant to your experience.  
> 你在LiDAR竞赛中使用ENVI进行光谱分析。多光谱遥感与LiDAR互补：LiDAR提供精确的三维结构，多光谱影像提供光谱/辐射属性。LiDAR+多光谱融合是城市树木检测、输电线路廊道监测和精准农业的关键方法，与你的经验直接相关。

---

## 2. Hyperspectral Remote Sensing（高光谱遥感）

### 2.1 Core Concept / 核心概念

**Definition:** Remote sensing acquiring contiguous spectral bands (typically 100–400 bands, 5–10 nm bandwidth) forming a "spectral cube" — enabling identification of materials by their unique spectral signatures (like a fingerprint).  
**定义：** 采集连续光谱波段（通常100–400个波段，5–10 nm带宽）形成"光谱立方体"——通过独特的光谱特征（如指纹）实现材料识别。

### 2.2 Spectral Cube & Data Volume / 光谱立方体与数据量

```
Hyperspectral data cube: [rows × cols × bands]
e.g., 1000 × 1000 × 200 bands × 4 bytes (float32) = 800 MB per image

Challenges (挑战):
├── Hughes phenomenon (休斯现象): accuracy degrades as bands increase with fixed samples
├── High dimensionality → curse of dimensionality
└── Redundant bands (adjacent bands highly correlated)

Dimensionality Reduction (降维):
├── PCA: linear, decorrelation (线性去相关)
├── MNF (Minimum Noise Fraction): noise-adjusted PCA
└── t-SNE / UMAP: non-linear, for visualisation
```

### 2.3 Spectral Analysis Techniques / 光谱分析技术

| Technique | 技术 | Purpose |
|---|---|---|
| Spectral Angle Mapper (SAM) | 光谱角制图 | Material mapping by angle to reference spectrum |
| Matched Filtering | 匹配滤波 | Sub-pixel target detection |
| Linear Spectral Unmixing | 线性光谱解混 | End-member abundance estimation |
| Continuum Removal | 连续统去除 | Absorption feature depth comparison |
| SVM-RBF | 支持向量机 | Classification of high-dim spectral space |

### 2.4 Applications / 应用领域

- **Vegetation stress detection (植被胁迫检测):** Chlorophyll absorption at 680 nm
- **Mineral mapping (矿物填图):** Characteristic absorption at 2.1–2.4 μm
- **Water quality (水质监测):** Chlorophyll-a, CDOM, turbidity in visible/NIR
- **Urban material mapping (城市材料制图):** Roof type, pavement classification
- **Soil property estimation (土壤属性估算):** Organic carbon, moisture content

---

## 3. UAV Remote Sensing（无人机遥感）

### 3.1 Core Concept / 核心概念

**Definition:** Use of Unmanned Aerial Vehicles (UAVs/drones) carrying optical, multispectral, LiDAR, or thermal sensors to acquire high-resolution spatial data at flexible temporal and spatial scales.  
**定义：** 使用搭载光学、多光谱、LiDAR或热红外传感器的无人机，以灵活的时间和空间尺度采集高分辨率空间数据。

### 3.2 UAV Types & Payload Suitability / 无人机类型与载荷适用性

| Platform | 平台 | Endurance | Payload | Best For |
|---|---|---|---|---|
| Fixed-wing | 固定翼 | 60–120 min | Light (camera) | Large area mapping |
| Multi-rotor | 多旋翼 | 20–40 min | Medium (LiDAR/multi) | Detailed structure, hovering |
| VTOL hybrid | 垂直起降固定翼 | 60–90 min | Medium | Combines both advantages |

### 3.3 Mission Planning / 任务规划

```
Key parameters (关键参数):
├── GSD (Ground Sampling Distance) 地面采样距离:
│   GSD = (sensor_width × flight_altitude) / (focal_length × image_width)
├── Overlap (重叠率): 
│   Front lap: 80%, Side lap: 70% (standard photogrammetry)
│   LiDAR: 30-50% strip overlap
├── Flight altitude (飞行高度): balance GSD vs. battery life
└── GCP placement (控制点布设): 4-5 minimum, well-distributed

Regulatory considerations (法规注意事项):
├── BVLOS authorisation (超视距飞行授权)
├── No-fly zones (禁飞区): airports, military, urban
└── Maximum altitude: typically 120 m AGL (国内120 m以下无需审批)
```

### 3.4 UAV LiDAR vs. Photogrammetry / 无人机LiDAR与摄影测量对比

| Aspect | UAV LiDAR | UAV Photogrammetry |
|---|---|---|
| Under-canopy 林下穿透 | ✓ (laser penetrates gaps) | ✗ (optical blocked) |
| Texture/colour 纹理/颜色 | ✗ (no colour) | ✓ (RGB imagery) |
| Cost 成本 | High (高) | Low (低) |
| Processing time 处理时间 | Fast | Slow (dense matching) |
| Night operation 夜间作业 | ✓ | ✗ |
| Accuracy (vertical) 垂直精度 | ~1–3 cm | ~3–5 cm |

### 3.5 Common Pitfalls / 常见错误

- **Insufficient GCPs (控制点不足):** Systematic doming / bowl distortion in photogrammetric models.  
- **Poor lighting (光照条件差):** Shadows cause feature matching failure; fly 2h after/before solar noon.  
- **IMU drift for LiDAR (IMU漂移):** Perform figure-8 manoeuvres for IMU initialisation before flight.

---

## 4. Photogrammetry（摄影测量）

### 4.1 Core Concept / 核心概念

**Definition:** The science and technology of extracting reliable geometric information (3D coordinates, shapes, measurements) from photographs or other 2D sensor data through geometric modelling and image matching.  
**定义：** 通过几何建模和图像匹配，从照片或其他二维传感器数据中提取可靠几何信息（三维坐标、形状、量测值）的科学与技术。

### 4.2 Collinearity Equation / 共线条件方程

```
The fundamental photogrammetric equation:
(物点、像点、投影中心共线)

x - x₀ = -f × [r₁₁(X-Xₛ) + r₂₁(Y-Yₛ) + r₃₁(Z-Zₛ)] /
                [r₁₃(X-Xₛ) + r₂₃(Y-Yₛ) + r₃₃(Z-Zₛ)]

y - y₀ = -f × [r₁₂(X-Xₛ) + r₂₂(Y-Yₛ) + r₃₂(Z-Zₛ)] /
                [r₁₃(X-Xₛ) + r₂₃(Y-Yₛ) + r₃₃(Z-Zₛ)]

Where:
(x, y) = image coordinates (像点坐标)
(x₀, y₀, f) = interior orientation (内方位元素)
(Xₛ, Yₛ, Zₛ) = exposure station (投影中心坐标)
R = rotation matrix from rᵢⱼ (外方位角元素旋转矩阵)
(X, Y, Z) = object coordinates (物点坐标)
```

### 4.3 SfM–MVS Pipeline / SfM–MVS流程

```
Structure from Motion + Multi-View Stereo:

1. Feature detection (特征提取): SIFT / SURF / ORB
2. Feature matching (特征匹配): FLANN, ratio test
3. Relative orientation (相对定向): Essential/Fundamental matrix, RANSAC
4. Bundle adjustment (光束法平差): simultaneous refinement of all cameras + points
5. Dense matching (密集匹配): MVS → dense point cloud
6. Surface reconstruction (表面重建): Delaunay mesh / Poisson
7. Orthorectification (正射纠正) → Orthophoto
8. DSM / DEM generation

Software: Agisoft Metashape, OpenDroneMap (open source), Pix4D, RealityCapture
```

### 4.4 Accuracy vs. Precision in Photogrammetry / 精度与准确度

```
Geometric accuracy (几何精度):
├── GCP RMSE (控制点均方根误差): internal accuracy
├── Checkpoint RMSE (检查点): external/absolute accuracy
└── Rule of thumb: GSD/2 vertical accuracy achievable with good GCPs

Radiometric accuracy (辐射精度):
├── Colour consistency across strips
└── Requires Radiometric Calibration Target (辐射定标板)
```

### 4.5 Common Pitfalls / 常见错误

- **Nadir-only flights on tall buildings (仅垂直拍摄高层建筑):** Building lean / "bowling effect" — add oblique images (倾斜影像).  
- **Reflective/featureless surfaces (反光/无纹理表面):** Water, glass fail feature matching — spray chalk or use ground targets.  
- **Bundle adjustment not converging (光束法不收敛):** Insufficient image overlap, blurry images, or poor initial camera calibration.

### 4.6 Link to Applicant / 与申请人经历的连接

> Your competition workflow (South Lidar Pro for airborne data) is a specialised form of photogrammetric processing. The same bundle adjustment principle applied in SfM also underlies LiDAR boresight calibration. In your interview, you can link: "My LiDAR processing experience taught me that both photogrammetry and LiDAR depend on the same fundamental principle — collinear geometry and rigorous error propagation — which I applied using least squares adjustment in my GNSS projects as well."  
> 你的竞赛工作流（使用South Lidar Pro处理机载数据）是摄影测量处理的一种特化形式。SfM中的光束法平差原理与LiDAR视轴校正底层相同。面试时可以联系："我的LiDAR处理经验让我认识到，摄影测量和LiDAR都依赖同一基本原理——共线几何和严格的误差传播——我在GNSS项目中也通过最小二乘平差应用了这一原理。"
