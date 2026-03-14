# 课程笔记 01：地理学基础模块
# Course Notes 01 – Geographical & Environmental Foundations

> 涵盖课程 / Courses covered:  
> New Geographical Research Methods · Environmental System & Environmental Geography ·  
> Applied Geomathematics, Modelling & Simulation · Geostatistics

---

## 1. New Geographical Research Methods（新地理研究方法）

### 1.1 Core Concept / 核心概念

**Definition:** A meta-discipline course that surveys the *philosophy, design, and toolbox* of modern geographic research — from positivist quantitative methods to qualitative, mixed, and participatory approaches.  
**定义：** 本课程是方法论导论，涵盖现代地理研究的哲学基础、设计框架和工具箱，从实证定量方法到定性、混合及参与式方法均有涉及。

### 1.2 Key Topics / 关键主题

| Topic | 主题 | Memory Hook |
|---|---|---|
| Scientific paradigms | 科学范式 | 实证主义 vs. 诠释主义 |
| Research design | 研究设计 | 假设→数据→分析→结论 |
| Quantitative methods | 定量方法 | 统计、GIS、遥感 |
| Qualitative methods | 定性方法 | 访谈、民族志、GeoHumanities |
| Mixed methods | 混合方法 | 三角验证(Triangulation) |
| Bibliometrics & Scientometrics | 文献计量 | VOSviewer, CiteSpace |
| Research ethics | 研究伦理 | IRB, informed consent |
| Academic writing | 学术写作 | IMRAD结构 |

### 1.3 Typical Research Workflow / 典型研究工作流

```
1. Problem formulation (研究问题)
       ↓
2. Literature review (文献综述)
       ↓
3. Research design (设计：变量/假设/采样)
       ↓
4. Data collection (数据采集)
       ↓
5. Analysis & Modelling (分析与建模)
       ↓
6. Interpretation (解释：结果 → 理论)
       ↓
7. Communication (报告/论文/演示)
       ↓
8. Reproducibility check (可重复性检验)
```

### 1.4 Common Pitfalls / 常见错误

- **Confirmation bias (确认偏误):** Selecting only data that confirms a hypothesis.  
  只选支持假设的数据，应预先注册研究设计。  
- **Spatial autocorrelation ignored (忽视空间自相关):** Treating spatial samples as independent.  
  空间数据不满足独立同分布假设，需用Moran's I检验。  
- **Scale mismatch (尺度不匹配):** Mixing datasets at incompatible spatial/temporal resolutions.  
  混用不同分辨率数据集，需重采样或MAUP校正。

### 1.5 Link to Applicant's Experience / 与申请人经历的连接

> Your LiDAR competition project followed a rigorous workflow: sensor calibration → point cloud filtering → DEM generation → volume computation — each step maps directly to this course's "research design → data collection → analysis" chain. In your interview, frame it as a *scientific workflow*, not just "operating software".  
> 你的LiDAR竞赛项目遵循了严格的工作流：传感器标定→点云滤波→DEM生成→土方计算，每步都对应本课程的"研究设计→数据采集→分析"链条。面试时，将其框定为"科学工作流"，而非仅仅"操作软件"。

---

## 2. Environmental System – Environmental Geography（环境系统与环境地理学）

### 2.1 Core Concept / 核心概念

**Definition:** Study of Earth's interacting subsystems (atmosphere, hydrosphere, lithosphere, biosphere, anthroposphere) and how geographic tools quantify environmental change and human–environment interactions.  
**定义：** 研究地球相互作用的子系统（大气、水圈、岩石圈、生物圈、人类圈），以及如何用地理工具量化环境变化与人地关系。

### 2.2 Key Subsystems & Tools / 关键子系统与工具

| Subsystem | 子系统 | Key Variables | GIS/RS Tools |
|---|---|---|---|
| Atmosphere | 大气圈 | Temperature, precipitation, wind | Meteorological grids, ERA5 |
| Hydrosphere | 水圈 | Runoff, soil moisture, flood extent | DEM-based watershed analysis |
| Lithosphere | 岩石圈 | Slope, aspect, soil type | DEM derivatives |
| Biosphere | 生物圈 | NDVI, LAI, biomass | Multispectral/Hyperspectral RS |
| Anthroposphere | 人类圈 | Land use, urban heat island | Landsat time series |

### 2.3 Systems Thinking / 系统思维

```
Input → Process → Output → Feedback
输入 →  过程  →  输出  →  反馈

Example: LiDAR terrain model as Input
→ Hydrological modelling (Process)
→ Flood risk map (Output)
→ Land-use policy update (Feedback)
```

### 2.4 Common Pitfalls / 常见错误

- **Neglecting feedbacks (忽视反馈):** Linear thinking in inherently cyclic systems.  
- **Boundary definition (边界定义):** Choosing arbitrary system boundaries distorts mass/energy balance.  
- **Scale dependency (尺度依赖):** Processes dominant at micro-scale may be negligible at macro-scale.

### 2.5 Link to Applicant / 与申请人经历的连接

> Powerline inspection and cross-section analysis in your LiDAR competition connect to the lithosphere subsystem: terrain shape (DEM, slope, aspect) determines corridor feasibility. Your earthwork calculation also demonstrates understanding of mass balance — a core systems concept.  
> 你竞赛中的输电线路巡检和横断面分析与岩石圈子系统直接相关：地形形状（DEM、坡度、坡向）决定了廊道可行性。土方计算体现了质量守恒，这是核心的系统概念。

---

## 3. Applied Geomathematics, Modelling & Simulation（应用地理数学、建模与仿真）

### 3.1 Core Concept / 核心概念

**Definition:** Translation of geographic phenomena into mathematical structures — differential equations, matrix algebra, optimisation, and numerical simulation — to predict, explain, or control spatial processes.  
**定义：** 将地理现象转化为数学结构（微分方程、矩阵代数、优化、数值仿真），以预测、解释或控制空间过程。

### 3.2 Mathematical Toolkit / 数学工具箱

| Tool | 应用场景 | Example |
|---|---|---|
| Linear Algebra | 坐标变换、最小二乘 | Georeferencing, GNSS adjustment |
| Differential Equations | 扩散/流体模型 | Groundwater flow, heat island |
| Optimisation (LP/NLP) | 路径/资源分配 | Network analysis, site selection |
| Monte Carlo Simulation | 不确定性传播 | Error propagation in DEM |
| Finite Element Method | 连续场建模 | Soil erosion, stress analysis |
| Numerical Integration | 面积/体积计算 | Earthwork volumes from point clouds |

### 3.3 Least Squares — Core Formula / 最小二乘核心公式

```
Model:  y = Ax + ε         (observation = design_matrix × unknowns + residual)
Solution: x̂ = (AᵀA)⁻¹Aᵀy  (Normal equations)
Residuals: v = Ax̂ - y
Quality: σ₀² = vᵀv / (n - u)   (unit weight variance; n=obs, u=unknowns)
```

> This is the same least squares you applied in your GNSS adjustment project!  
> 这正是你GNSS平差项目中使用的最小二乘！

### 3.4 Kalman Filter — Memory Model / 卡尔曼滤波记忆模型

```
Predict:  x̂ₖ⁻ = Fₖx̂ₖ₋₁        (状态预测)
          Pₖ⁻  = FₖPₖ₋₁Fₖᵀ + Qₖ (协方差预测)

Update:   Kₖ   = Pₖ⁻Hₖᵀ(HₖPₖ⁻Hₖᵀ + Rₖ)⁻¹  (卡尔曼增益)
          x̂ₖ  = x̂ₖ⁻ + Kₖ(zₖ - Hₖx̂ₖ⁻)     (状态更新)
          Pₖ   = (I - KₖHₖ)Pₖ⁻              (协方差更新)
```

> You implemented this in your GNSS trajectory smoothing — a perfect interview story.  
> 你在GNSS轨迹平滑中实现了这个——完美的面试素材。

### 3.5 Common Pitfalls / 常见错误

- **Model identifiability (模型可识别性):** Too many parameters relative to observations → over-fitting.  
- **Numerical instability (数值不稳定):** Ill-conditioned normal equation matrix; use regularisation (Tikhonov/ridge regression).  
- **Propagating errors (误差传播):** Always propagate uncertainties through model chains; don't report only final values.

---

## 4. Geostatistics（地统计学）

### 4.1 Core Concept / 核心概念

**Definition:** A branch of statistics that treats spatial (and spatiotemporal) data as realisations of random fields, quantifying spatial correlation structures and providing optimal spatial interpolation with uncertainty estimates.  
**定义：** 将空间（或时空）数据视为随机场的实现，量化空间相关结构，并提供带不确定性估计的最优空间插值。

### 4.2 The Geostatistics Pipeline / 地统计学流程

```
1. Exploratory Spatial Data Analysis (ESDA)
   → Histogram, spatial trend, spatial lag plots

2. Variogram Analysis (变差函数分析)
   → Experimental variogram: γ(h) = ½ Var[Z(x) − Z(x+h)]
   → Fit model: Spherical / Exponential / Gaussian / Nugget

3. Kriging Interpolation (克里金插值)
   → Simple Kriging (已知均值)
   → Ordinary Kriging (未知均值) ← most common
   → Universal Kriging (含趋势项)
   → Co-Kriging (多变量协同)

4. Cross-Validation (交叉验证)
   → Leave-one-out: RMSE, ME, MSDR ≈ 1 if model is correct

5. Uncertainty Mapping (不确定性制图)
   → Kriging variance → confidence intervals
```

### 4.3 Variogram Parameters — Memory Table / 变差函数参数记忆表

| Parameter | 参数 | Meaning |
|---|---|---|
| Nugget (C₀) | 块金值 | Measurement error + micro-scale variation |
| Sill (C₀+C) | 基台值 | Total variance of the process |
| Range (a) | 变程 | Distance beyond which points are uncorrelated |
| Anisotropy | 各向异性 | Directional variation (e.g., wind-driven) |

### 4.4 Moran's I — Spatial Autocorrelation / Moran's I 空间自相关

```
I = (n / W) × [Σᵢ Σⱼ wᵢⱼ(xᵢ-x̄)(xⱼ-x̄)] / Σᵢ(xᵢ-x̄)²

I ≈ +1: clustered (聚集)
I ≈  0: random (随机)
I ≈ -1: dispersed (分散)
```

### 4.5 Application in LiDAR / 在LiDAR中的应用

> Point cloud density and elevation residuals exhibit spatial autocorrelation. Kriging can be used to fill LiDAR voids (e.g., under-canopy gaps) and to estimate DEM uncertainty. Variogram range tells you the effective spatial resolution of your sensor.  
> 点云密度和高程残差表现出空间自相关。克里金插值可用于填充LiDAR空洞（如林下缺失区域）并估算DEM不确定性。变程告诉你传感器的有效空间分辨率。

### 4.6 Common Pitfalls / 常见错误

- **Assuming stationarity (假设平稳性):** Real data often has spatial trends; use detrending or Universal Kriging.  
- **Too few sample pairs (样本对过少):** Unreliable variogram at short lag distances; need ≥ 30 pairs per lag bin.  
- **Ignoring anisotropy (忽视各向异性):** Isotropic model on anisotropic data → poor cross-validation scores.
