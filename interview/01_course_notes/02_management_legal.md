# 课程笔记 02：管理、法律与人文模块
# Course Notes 02 – Management, Legal & Human Sciences

> 涵盖课程 / Courses covered:  
> Management Skills · Data Protection & Data Security · Space and Society ·  
> Project Management (lecture + practice)

---

## 1. Management Skills（管理技能）

### 1.1 Core Concept / 核心概念

**Definition:** The study of organisational theory, leadership styles, team dynamics, decision-making, and resource allocation as applied to geospatial companies, governmental bodies, and research institutions.  
**定义：** 研究组织理论、领导风格、团队动态、决策制定和资源分配，应用于地理空间企业、政府机构和科研院所。

### 1.2 Key Management Frameworks / 关键管理框架

| Framework | 框架 | Core Idea | GIS Context |
|---|---|---|---|
| SWOT Analysis | 优劣势分析 | Strengths, Weaknesses, Opportunities, Threats | Evaluating a GIS department's capacity |
| PDCA Cycle | 戴明循环 | Plan→Do→Check→Act | Iterative geodata quality improvement |
| Maslow's Hierarchy | 马斯洛需求层次 | Physiological → Self-actualisation | Motivating survey field teams |
| Situational Leadership | 情境领导 | Adapt style to team maturity | Leading novice vs. expert GIS analysts |
| Balanced Scorecard | 平衡计分卡 | Financial/Customer/Process/Learning | KPIs for a geospatial consultancy |

### 1.3 Conflict Resolution in Teams / 团队冲突解决

```
Thomas–Kilmann Model:
├── Competing (竞争): High assertive, low cooperative — use in emergencies
├── Collaborating (协作): High–High — best for complex, interdependent GIS projects
├── Compromising (妥协): Mid–Mid — quick resolution, partial satisfaction
├── Avoiding (回避): Low–Low — for trivial issues
└── Accommodating (迁就): Low assertive — when relationship > outcome
```

### 1.4 Link to Applicant / 与申请人经历的连接

> As a competition team leader, you applied situational leadership: guiding less-experienced teammates through LiDAR workflows while delegating autonomous tasks to skilled members. Describe how you used a collaborative style when integrating ArcGIS and SouthMap outputs from different team members.  
> 作为竞赛队长，你运用了情境领导力：指导经验较少的队友完成LiDAR工作流程，同时将自主任务委派给有经验的成员。描述在整合不同队员的ArcGIS和SouthMap成果时如何运用协作风格。

---

## 2. Data Protection & Data Security（数据保护与数据安全）

### 2.1 Core Concept / 核心概念

**Definition:** Legal, technical, and organisational measures to ensure the confidentiality, integrity, and availability (CIA triad) of geospatial data, with specific attention to personal data under GDPR and national regulations.  
**定义：** 确保地理空间数据保密性、完整性和可用性（CIA三要素）的法律、技术和组织措施，特别关注GDPR及各国法规下的个人数据保护。

### 2.2 The CIA Triad / CIA三要素

```
┌─────────────────────────────────────────┐
│           DATA SECURITY TRIAD           │
│                                         │
│   Confidentiality ←→ Integrity          │
│        (保密性)         (完整性)         │
│              ↕                          │
│         Availability                    │
│           (可用性)                      │
└─────────────────────────────────────────┘
```

### 2.3 GDPR Essentials for Geospatial Data / GDPR中地理空间数据要点

| Principle | 原则 | Geospatial Implication |
|---|---|---|
| Lawful basis | 合法依据 | Location data requires explicit consent or legitimate interest |
| Data minimisation | 最小化原则 | Collect only necessary precision (neighbourhood vs. exact GPS) |
| Purpose limitation | 目的限制 | Survey data collected for routing ≠ surveillance |
| Accuracy | 准确性 | Maintain metadata on data age, source, precision |
| Storage limitation | 存储限制 | Define retention policy for drone imagery |
| Data subject rights | 数据主体权利 | Right to access/erase location history |

### 2.4 Geospatial Data Security Measures / 地理空间数据安全措施

```
Technical Controls (技术控制):
├── Encryption at rest (静态加密): AES-256 for raster/vector archives
├── Encryption in transit (传输加密): TLS 1.3 for WMS/WFS services
├── Access control (访问控制): Role-based access in PostGIS/GeoServer
└── Anonymisation (匿名化): Spatial k-anonymity for personal location data

Organisational Controls (组织控制):
├── Data classification policy (数据分级政策)
├── Incident response plan (事件响应计划)
└── Staff awareness training (员工意识培训)
```

### 2.5 Common Pitfalls / 常见错误

- **Publishing raw GPS tracks (发布原始GPS轨迹):** Can reveal home/work locations of individuals.  
- **Over-precise coordinates (过精确坐标):** 6 decimal places (~0.1 m) often qualifies as personal data under GDPR.  
- **Open drone imagery (开放无人机影像):** Faces/licence plates in imagery require blurring before publication.

### 2.6 Link to Applicant / 与申请人经历的连接

> In your GNSS and LiDAR projects, consider what data governance you applied: Were measurement files stored securely? Were project deliverables shared via authenticated channels? Mention that you are aware of national secret mapping regulations (涉密测绘数据管理规定) in China, which parallels GDPR in spirit.  
> 在你的GNSS和LiDAR项目中，思考你采用了哪些数据治理措施：测量文件是否安全存储？项目成果是否通过认证渠道共享？提到你了解中国涉密测绘数据管理规定，这在精神上与GDPR相似。

---

## 3. Space and Society（空间与社会）

### 3.1 Core Concept / 核心概念

**Definition:** An interdisciplinary course at the intersection of human geography, sociology, and critical GIS — examining how space is socially produced, how power relations shape spatial inequalities, and how geospatial technologies are embedded in social contexts.  
**定义：** 人文地理、社会学与批判性GIS的交叉课程——研究空间如何被社会建构、权力关系如何塑造空间不平等，以及地理空间技术如何嵌入社会情境。

### 3.2 Key Theoretical Concepts / 关键理论概念

| Concept | 概念 | Key Author | Geospatial Application |
|---|---|---|---|
| Social production of space | 空间的社会生产 | Lefebvre | Urban morphology analysis |
| Spatial justice | 空间正义 | Soja | Equity mapping, accessibility GIS |
| Critical GIS | 批判性GIS | Harley, Pickles | Power in cartography, surveillance |
| Sense of place | 地方感 | Tuan | Participatory mapping |
| Geosurveillance | 地理监控 | Lyon | Location data ethics |
| Digital divide | 数字鸿沟 | Castells | GIS access inequality |

### 3.3 Practical Skills / 实践技能

- **Choropleth mapping** for socioeconomic inequalities  
- **Accessibility analysis** (network analysis + demographic data)  
- **Participatory GIS (PGIS):** Community-based spatial knowledge collection  
- **Discourse analysis** of maps as political artefacts

### 3.4 Common Pitfalls / 常见错误

- **Treating maps as neutral (将地图视为中立):** Every map embeds choices that reflect power.  
- **Ecological fallacy (生态谬误):** Inferring individual behaviour from aggregate spatial statistics.

---

## 4. Project Management（项目管理）

### 4.1 Core Concept / 核心概念

**Definition:** The application of structured methodologies (traditional waterfall, agile, and hybrid) to plan, execute, monitor, and close geospatial projects within defined scope, schedule, cost, and quality constraints.  
**定义：** 将结构化方法论（传统瀑布、敏捷及混合方法）应用于在限定范围、进度、成本和质量约束内规划、执行、监控和收尾地理空间项目。

### 4.2 Project Management Triangle / 项目管理三角

```
            Scope (范围)
               /\
              /  \
             /    \
            /      \
Time ______/________\ Cost
(时间)                 (成本)

Quality (质量) sits at the centre
```

### 4.3 Key Methodologies / 关键方法论

| Method | 方法 | Best For | GIS Project Type |
|---|---|---|---|
| Waterfall (瀑布) | Sequential phases | Fixed-scope surveys | Cadastral mapping project |
| Agile/Scrum | Iterative sprints | Evolving requirements | Web GIS application dev |
| PRINCE2 | Structured governance | Large public-sector | National SDI project |
| Kanban | Visual workflow | Ongoing operations | GIS support desk |

### 4.4 Scrum Framework for GIS Projects / Scrum框架在GIS项目中的应用

```
Product Backlog → Sprint Planning → Sprint (1-4 weeks) → Sprint Review → Retrospective
产品待办列表  →    迭代计划     →    冲刺(1-4周)    →    迭代评审   →   回顾改进

GIS examples of user stories (用户故事示例):
"As a flood manager, I want a daily-updated inundation raster so that I can 
 dispatch resources to affected areas."
"作为防洪管理员，我需要每日更新的洪水淹没栅格，以便向受影响地区调配资源。"
```

### 4.5 Risk Management in Geospatial Projects / 地理空间项目风险管理

| Risk | 风险 | Probability | Impact | Mitigation |
|---|---|---|---|---|
| Weather delay for UAV survey | 无人机测量因天气延误 | High | Medium | Schedule buffer days |
| GNSS signal obstruction | GNSS信号遮挡 | Medium | High | RTK backup, static mode |
| Data loss | 数据丢失 | Low | Critical | 3-2-1 backup rule |
| Software licence issue | 软件许可问题 | Medium | Medium | Open source fallback |

### 4.6 Earned Value Management (EVM) / 挣值管理

```
PV (Planned Value)  = BCWS  计划值
EV (Earned Value)   = BCWP  挣值
AC (Actual Cost)    = ACWP  实际成本

SPI (Schedule Performance Index) = EV/PV  进度绩效指数
CPI (Cost Performance Index)     = EV/AC  成本绩效指数

SPI > 1: ahead of schedule (提前)
CPI > 1: under budget (节约)
```

### 4.7 Link to Applicant / 与申请人经历的连接

> Your LiDAR competition was effectively a miniature project: defined deliverables (DEM, earthwork report, powerline inspection map), a deadline (competition day), and a multi-person team. Frame this as: "I applied a waterfall approach — defining deliverables upfront — but used daily stand-up style check-ins to keep the team aligned." Also, your GNSS projects required risk-aware planning: what happened when satellite geometry was poor (high PDOP)? How did you respond?  
> 你的LiDAR竞赛本质上是一个微型项目：明确的交付物（DEM、土方报告、输电线路巡检图）、截止日期（竞赛当天）和多人团队。将其框定为："我采用了瀑布方法——预先定义交付物——但使用每日站会式沟通保持团队对齐。"你的GNSS项目也需要风险感知规划：当卫星几何构型差（高PDOP）时发生了什么？你如何应对？
