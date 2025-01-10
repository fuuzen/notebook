---
title: Spectral Clustering(谱聚类)
date: 2024/04/07
math: true
categories:
  - [数学, 离散数学, 图论]
tags:
  - 离散数学
  - 图论
---

!!! abstract
    This article isn't finished yet...

Graph Partitioning

Bi-partitioning task

### Minmal-cut

### Normalized-cut

Criterion: **Normalized-cut** [Shi-Malik, '97]

Define: Connectivity between groups relative to the density of each group

$$
ncut(\mathbf{A},\mathbf{B})=\frac{cut(\mathbf{A},\mathbf{B})}{vol(\mathbf{A})}+\frac{cut(\mathbf{A},\mathbf{B})}{vol(\mathbf{B})}
$$

$vol(\mathbf{A})$ : total weight of the edges with at least one endpoint in $\mathbf{A}$ :

$$
vol(\mathbf{A})=\sum_{i\in\mathbf{A}}k_i
$$

The purpose of this criterion is to produce more balanced partitions.

Spectral Graph Theory:

Analyze the "spectrum" of matrix(s) representing $G$

Spectrum: Eigenvectors $x_i$ of a graph, ordered by the magnitude(strength) of their corresponding eigenvalues $\lambda_i$ :

$$
\Lambda=\{\lambda_1,\lambda_2,\cdots,\lambda_n\}\\
\lambda_1\leq\lambda_2\leq\cdots\leq\lambda_n
$$

!!! abstract
    $d$-regular : the degree of each vertex in the graph is $d$

If $G$ is not connected, for example, $G$ has 2 components, each $d$-regular.

!!! abstract
    每一个特征向量表现了图的划分方式

What are some eigenvectors?

## Spectral Partitioning Algorithm

### 算法流程

#### Pre-processing

根据图构建拉普拉斯矩阵 $L$。

#### Decomposition

\*分解

求 $L$ 的第 2 小的特征值 $\lambda_1$ ($\lambda_0=0$) 和特征向量 $\vec{q_1}$。

#### Grouping

$n$ 维向量 $\vec{q_1}$ 的 $n$ 个参数对应到 $n$ 个顶点，可以画图，将参数作为纵坐标，顶点标号作为横坐标，结合图像进行分组(grouping)
