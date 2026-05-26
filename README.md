# 雷达极化气象观测互动讲义

**基于教材：** *Radar Polarimetry for Weather Observations*（雷达极化气象观测），Alexander V. Ryzhkov and Dusan S. Zrnic，2019

本仓库包含 **33 个交互式 Jupyter Notebook 讲义**，将这本经典教材转化为动手实践型的计算学习材料。每个 Notebook 具有以下特色：

- **交互式控件**（滑块、下拉菜单）用于参数探索
- **数学基础**配有 LaTeX 公式渲染
- **3D 可视化**展示电磁波传播与散射
- **实时计算**极化变量
- **练习题**供自我评估

---

## 课程总览

| 章节 | 标题 | Notebook 数量 |
|------|------|-------------|
| 1 | 电磁波极化、散射与传播 | 3 |
| 2 | 极化多普勒雷达 | 3 |
| 3 | 水凝物集合的散射 | 3 |
| 4 | 水凝物的微物理与介电特性 | 3 |
| 5 | 雷达极化变量 | 3 |
| 6 | 数据质量与测量误差 | 3 |
| 7 | 微物理过程的极化"指纹" | 3 |
| 8 | 深对流风暴的极化特征 | 3 |
| 9 | 雷达回波的分类 | 3 |
| 10 | 降水的极化测量 | 3 |
| 11 | 极化微物理参数反演 | 2 |

---

## 快速开始

### 1. 安装依赖

```bash
pip install -r requirements.txt
```

### 2. 启动 JupyterLab

```bash
jupyter lab
```

### 3. 导航至目标章节

打开对应章节文件夹，先阅读 `README.md` 了解主题概览。

---

## 章节索引

### [第1章：电磁波极化、散射与传播](Chapter_01_Polarization_Scattering_Propagation/)

- **1_1** — 电磁波偏振态3D可视化（线偏振、圆偏振、椭圆偏振）
- **1_2** — 单粒子散射行为：形状、倾角与后向散射矩阵
- **1_3** — 传播效应：大气介质中的相移与衰减

### [第2章：极化多普勒雷达](Chapter_02_Polarimetric_Doppler_Radar/)

- **2_1** — 脉冲多普勒雷达原理：I/Q信号生成与时序
- **2_2** — SHV与HSHV模式对比：同时收发 vs 交替收发
- **2_3** — 多普勒频移与差分相移：H/V通道关系

### [第3章：水凝物集合的散射](Chapter_03_Scattering_by_Ensemble/)

- **3_1** — 距离权重函数：脉冲宽度对分辨率的影响
- **3_2** — 粒子取向分布：高斯倾角分布的影响
- **3_3** — 极化变量统计：功率与通道间相关性

### [第4章：水凝物的微物理与介电特性](Chapter_04_Microphysical_Dielectric_Properties/)

- **4_1** — 降水滴谱分布（DSD）：指数分布与Gamma分布的交互式滑块
- **4_2** — 形状与终端下落速度模型：轴比与落速曲线
- **4_3** — 介电常数计算器：干雪/湿雪/融化冰雹的混合公式

### [第5章：雷达极化变量](Chapter_05_Polarimetric_Variables/)

- **5_1** — 瑞利近似下的反射率：Z与ZDR计算
- **5_2** — KDP的波长依赖性：S波段 vs C波段 vs X波段对比
- **5_3** — 退极化比与交叉相关系数：LDR与ρhv分析

### [第6章：数据质量与测量误差](Chapter_06_Data_Quality_Measurement_Errors/)

- **6_1** — 雷达定标模拟：天顶观测法与干雪法
- **6_2** — 衰减校正：恢复衰减前的真实反射率
- **6_3** — 杂波识别：地物杂波极化特征与波束遮挡

### [第7章：微物理过程的极化"指纹"](Chapter_07_Polarimetric_Fingerprints/)

- **7_1** — 碰并与破碎：DSD变化对极化变量的影响
- **7_2** — 尺寸分选：风切变与差分落速效应
- **7_3** — 冰相与融化层：混合相态降水的亮带特征

### [第8章：深对流风暴的极化特征](Chapter_08_Deep_Convective_Storms/)

- **8_1** — 冰雹信号识别：冰雹风暴中的极化变量典型范围
- **8_2** — 龙卷碎片特征（TDS）：识别非气象散射体
- **8_3** — 超级单体与上升气流：ZDR柱状结构与上升气流强度

### [第9章：雷达回波的极化分类](Chapter_09_Classification_of_Radar_Echo/)

- **9_1** — 模糊逻辑分类器：水凝物隶属度函数可调
- **9_2** — 融化层自动检测：极化变量垂直廓线特征
- **9_3** — 非气象目标识别：地物、海杂波、生物、扬沙

### [第10章：降水的极化测量](Chapter_10_Polarimetric_Measurements_of_Precipitation/)

- **10_1** — 降雨率估计模型对比：R(Z)、R(ZDR)、R(KDP)方法
- **10_2** — R(A)算法：比衰减反演雨强的抗误差优势
- **10_3** — 降雪量测量：雷达反射率与KDP估算等效液态降雪量

### [第11章：极化微物理参数反演](Chapter_11_Polarimetric_Microphysical_Retrievals/)

- **11_1** — 液态水含量反演：纯雨条件下LWC与DSD参数反演
- **11_2** — 冰相微物理反演：高空IWC与雪粒子尺寸分布
- **11_3** — 误差敏感性分析：测量噪声下的反演稳定性

---

## 核心物理概念

### 电磁波理论
- 偏振态的Jones矢量表示
- Stokes参数与Poincaré球
- 复折射率与介电常数

### 散射物理
- 瑞利散射与米散射 regime
- 后向散射矩阵与散射矩阵
- 非球形粒子的T矩阵方法
- 瑞利-甘斯近似

### 雷达气象学
- 反射率因子 Z（水平等效）
- 差分反射率 ZDR
- 比差分相移 KDP
- 同极化相关系数 ρhv
- 线性退极化比 LDR
- 差分相移 ΦDP

### 微物理
- Marshall-Palmer滴谱分布
- Gamma分布参数（N0, Λ, μ）
- 终端速度（Atlas-Livesey模型）
- 雨/雪/冰雹的轴比形状
- 介电混合公式

---

## Notebook 结构

每个 Notebook 遵循标准化模板：

```markdown
# 第X章.Y：标题

## 学习目标
- 目标1
- 目标2

## 理论背景
[LaTeX公式]

## 交互演示
[ipywidgets滑块]

## 可视化
[matplotlib/plotly图表]

## 练习题
1. 题目描述
2. 题目描述

## 参考文献
- Ryzhkov & Zrnic，第X章，第YY-ZZ页
```

---

## 依赖包

| 包名 | 用途 |
|------|------|
| `numpy` | 数值计算 |
| `scipy` | 特殊函数、积分 |
| `matplotlib` | 2D绘图与动画 |
| `plotly` | 交互式3D可视化 |
| `ipywidgets` | 交互式滑块与下拉菜单 |
| `sympy` | 符号数学 |
| `pandas` | 表格数据处理 |

---

## 引用

如在教学或研究中使用这些讲义，请引用：

```
Ryzhkov, A.V., and D.S. Zrnic, 2019: Radar Polarimetry for Weather Observations.
   Springer, 526 pp.
```

---

## 许可

仅供教育使用。基于 Springer 出版的教材《Radar Polarimetry for Weather Observations》，
作者：Alexander V. Ryzhkov and Dusan S. Zrnic。