# 第4章：水凝物的微物理与介电特性

## 章节简介

本章介绍降水粒子的微物理特性（尺寸分布、形状、下落速度）和介电特性（复介电常数、混合公式），这些是理解极化雷达变量的基础。

**核心主题：**
- 降水滴谱分布（Marshall-Palmer指数分布、Gamma分布）
- 粒子形状模型（轴比与直径的关系）
- 终端下落速度模型（Atlas-Livesey公式）
- 介电混合公式（Maxwell-Garnett、Bruggeman）

## Notebook 讲义

| 文件 | 主题 | 交互功能 |
|------|------|----------|
| `4_1_Drop_Size_Distribution.ipynb` | 滴谱分布 | 降雨率参数、N0和Lambda滑块 |
| `4_2_Shape_Velocity_Models.ipynb` | 形状与速度模型 | 直径范围可调、轴比曲线 |
| `4_3_Dielectric_Constant_Calculator.ipynb` | 介电常数计算 | 水/冰比例、温度、频率 |

## 学习目标

1. 掌握Marshall-Palmer和Gamma滴谱分布的数学表达式
2. 理解雨滴轴比随直径变化的规律
3. 了解终端下落速度的经验公式
4. 学会计算混合相态粒子的有效介电常数

## 关键公式

### Marshall-Palmer分布
$$N(D) = N_0 \exp(-\Lambda D)$$

其中 $N_0 = 8 \times 10^6$ m⁻⁴，$\Lambda = 4.1 R^{-0.21}$ mm⁻¹，$R$ 是降雨率(mm/h)。

### 终端速度
$$V(D) = V_0 (1 - \exp(-\frac{D}{D_0}))$$

其中 $V_0 \approx 9.65$ m/s，$D_0 \approx 0.9$ mm。

## 先修知识

- 第3章水凝物集合散射基础
- 大气物理基础