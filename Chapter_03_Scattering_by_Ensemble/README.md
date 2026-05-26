# Chapter 3: Scattering by Ensemble of Hydrometeors

## 章节简介

本章从集合平均的角度讨论水凝物散射，包括距离权重函数、功率与相关运算、以及极化变量的定义与粒子取向分布效应。

**核心主题：**
- 距离权重函数与雷达体积采样
- 集合平均功率与相关运算（双求和、协方差矩阵）
- 极化变量定义（$Z_{hh}$、$Z_{DR}$、$\rho_{hv}$、$LDR$ 等）
- 粒子取向分布对极化变量的影响

## Notebook 讲义

| 文件 | 主题 | 交互功能 |
|------|------|----------|
| `3_1_Range_Weighting_Function.ipynb` | 3.1 节：距离权重函数 | 脉冲宽度 τ (0.5-2 μs) 可调、WSR-88D 参数图 |
| `3_2_Powers_and_Correlations.ipynb` | 3.2 节：功率与相关运算 | 散射粒子数 $N_s$ 可调、多普勒参数、三种雷达模式对比 |
| `3_3_Polarimetric_Variables.ipynb` | 3.3+3.4 节：极化变量与取向效应 | 取向标准差 σ (0-50°)、轴比 $s_b/s_a$ (1.0-2.0) 可调 |

## 学习目标

1. 理解雷达体积采样的概念和距离权重函数 $|W(r)|^2$
2. 掌握双求和展开区分单求和（永久项）与双求和（起伏项）
3. 理解集合平均与时间平均的关系
4. 掌握 SHV、HSHV、VSVH 三种模式下的协方差方程
5. 理解 $Z_{DR}$、$\rho_{hv}$、$LDR$ 等极化变量的物理意义
6. 分析粒子倾角分布对极化变量的影响

## 关键公式

### 距离权重函数
$$|W(r - r_0)|^2 = \left|p\left[\frac{2(r-r_0)}{c}\right]\right|^2 \tag{3.1}$$

### 集合平均功率
$$\langle P_{hh}\rangle = \frac{C_1}{r_0^2 l_h^2(r_0)} \langle |s_{hh}(r_0)|^2 \rangle \tag{3.23}$$

### 协方差矩阵（上三角形式）
$$\begin{bmatrix} Z_{hh} & \rho_{xh} & \rho_{hv} \\ & Z_{hv} & \rho_{xv} \\ & & Z_{vv} \end{bmatrix} \tag{3.42}$$

### 取向因子
$$F_{orient} = A_1 - A_2 = \frac{1}{2} r_\sigma (1 + r_\sigma), \quad r_\sigma = \exp(-2\sigma^2) \tag{3.53, 3.54}$$

## 内容与 ch3.md 对照

| ch3.md 小节 | Notebook | 主要内容 |
|-------------|----------|---------|
| 3.1 Range Weighting Function | 3_1 | 脉冲几何、匹配滤波器、WSR-88D 实例 |
| 3.2.1 Powers | 3_2 | 双求和、集合平均、天气雷达方程 |
| 3.2.2 Correlations | 3_2 | $R_{hv}$、$R_{hh}(T_s)$、多普勒速度与谱宽 |
| 3.2.2.1 SHV Mode | 3_2 | 式 (3.23)-(3.25) |
| 3.2.2.2 HSHV Mode | 3_2 | 式 (3.26)-(3.27) |
| 3.2.2.3 VSVH Mode | 3_2 | 式 (3.28) |
| 3.3.1 Equivalent Reflectivity | 3_3 | 式 (3.29)-(3.31) |
| 3.3.2 Differential Reflectivity | 3_3 | 式 (3.32)-(3.34) |
| 3.3.3 Cross-Correlation Coefficient | 3_3 | 式 (3.35)-(3.38) |
| 3.3.4 HSHV/VSVH Modes | 3_3 | 式 (3.39)-(3.42) |
| 3.4 Effects of Particle Orientations | 3_3 | 角矩 A₁-A₅、三种取向情形、图 3.4 与图 3.5 |

## 先修知识

- 第 1 章：散射理论基础（米散射、SAS 矩阵）
- 第 2 章：极化雷达原理（SHV/HSHV/VSVH 模式）
- 概率统计基础：集合平均、时间平均、指数分布

## 参考资料

- Doviak & Zrnic (2006) Doppler Radar and Weather Observations, 2nd ed.
- Ryzhkov & Zrnic (2024) Radar Polarimetry for Weather Observations, Ch. 3
- Ryzhkov (2001) J. Atmos. Oceanic Technol., 18, 315-328
- Sachidananda & Zrnic (1985) J. Atmos. Oceanic Technol.