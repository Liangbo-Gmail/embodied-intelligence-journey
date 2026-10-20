# Diffusion Policy 阅读笔记（一）：摘要 + 引言

> 论文：Diffusion Policy: Visuomotor Policy Learning via Action Diffusion
> 作者：Cheng Chi, Shuran Song (Columbia), Russ Tedrake (MIT) 等
> 日期：2026.10.20 | 状态：摘要+引言已读

## 1. 这篇论文要解决什么问题？

机器人从示范中学习（行为克隆），本质上是学"观测→动作"的映射。
但机器人动作有三个特殊性质让这件事变难：
- **多模态分布（multimodal）**：同一任务有多种合理做法
  （如抓杯子可左抓可右抓），直接回归会学成"平均动作"，两边都失败
- **时序相关性**：前后动作必须连贯，不能跳变
- **高精度要求**：差一毫米就抓空

## 2. 核心方法：条件去噪扩散过程

不直接输出动作，而是把动作生成变成一个**条件去噪扩散过程**
（conditional denoising diffusion process）：
从纯噪声出发，沿着学好的梯度场（score function）一步步"去噪"，
最终在噪声中雕刻出完整动作。图像观测作为条件（conditioning）输入。
在15个任务、4个benchmark上平均超越此前SOTA 46.9%。

## 3. Figure 1：三种策略表示的对比

| 策略 | 做法 | 问题 |
|---|---|---|
| 显式策略 Explicit | 直接回归/分类输出动作 | 表达不了多模态，多种做法被"平均"掉 |
| 隐式策略 Implicit | 学能量函数，找能量最低的动作 | 需要 negative sampling 估计归一化常数，训练不稳定 |
| **扩散策略（本文）** | 从噪声沿梯度场逐步去噪生成动作 | 多模态✓ 高维动作序列✓ 训练稳定✓ |

## 待深入（下次读）
- [ ] Score function 的数学推导
- [ ] Receding Horizon Control 闭环执行细节
- [ ] Time-series Diffusion Transformer 架构
