---
title: 图的代数表示
date: 2024/04/07
math: true
categories:
  - [数学, 离散数学, 图论]
tags:
  - 离散数学
  - 图论
  - 代数
---

!!! abstract
    用邻接矩阵或关联矩阵表示图，称为图的代数表示。用矩阵表示图,主要有两个优点:

- (1) 能够把图输入到计算机中
- (2) 可以用代数方法研究图论

[Spectral Graph Theory](https://en.wikipedia.org/wiki/Spectral_graph_theory) 是研究图和代数的一个领域

## 邻接矩阵

Adjaceny matrix is $n\times n$ symmetric matrix

### 定义

设 $G$ 为 $n$ 阶图，$V=\{v_1,z_2,\cdots,v_n\}$ ，邻接矩阵 $A(G)= (a_{ij})$ ，其中：

$$
a_{ij}=\begin{cases}
l,\ \ \ v_i\text{与}v_j\text{间边数}\\
0,\ \ \ v_i\text{与}v_j\text{不邻接}\\
a_i\text{上的环数},\ \ \ i=j
\end{cases}
$$

### 代数性质

非负性——由邻接矩阵定义知 $a_{ij}$ 是非负整数，即邻接矩阵是非负整数矩阵

(无向图) 对称性——显然 $a_{ij}=a_{ji}$ ，所以邻接矩阵是对称矩阵。

同一图的不同形式的邻接矩阵是**相似矩阵**。这是因为，同图的两种不同形式矩阵可以通过交换行和相应的列变成致。

如果 G 为简单图，则 $A(G)$ 为**布尔矩阵**；行和(列和)等于对应顶点的度数；矩阵元素总和为图的总度数，也就是 $G$ 的边数的 2 倍

### 连通的充要条件

$G$ 连通的充要条件是：$A(G)$ 不能与如下矩阵相似：

$$
\begin{pmatrix}
A_{11} & 0\\
0 & A_{22}
\end{pmatrix}
\quad
$$

### 性质(定理)

$A^{k}(G)=(a_{ij}^{(k)})$ ，则 $a_{ij}^{(k)}$ 表示顶点 $v_i$ 到 $v_j$ 的途径长度为 $k$ 的途径数目；

---

证明：对 $k$ 作数学归纳法证明

当 $k=1$ 时，由邻接矩阵定义；

设结论对 $k-1$ 时成立，考虑 $k$ 的情况：

$$
A^k=A^{k-1}A=(a_{i1}^{(k-1)}a_{j1}+a_{i2}^{(k-1)}a_{j2}+\cdots+a_{in}^{(k-1)}a_{jn})_{n\times n}=(a_{ij}^{(k)})
$$

同时，考虑顶点 $v_i$ 到 $v_j$ 的途径长度为 $k$ 的途径，设 $v_m$ 设 $v_i$ 到 $v_j$ 的途径中点，且该点与 $v_j$ 邻接。则 $v_i$ 到 $v_j$ 的经过 $v_m$ 且途径长度为 $k$ 的途径数目为：

$$
a_{im}^{(k-1)}a_{mj}
$$

所以 $v_i$ 到 $v_j$ 的途径长度为 $k$ 的途径数目为：

$$
a_{i1}^{(k-1)}a_{j1}+a_{i2}^{(k-1)}a_{j2}+\cdots+a_{in}^{(k-1)}a_{jn}=a_{ij}^{(k)}
$$

---

推论：

若 $G$ 是连通的，则对于 $i\neq j$ ， $v_i$ 到 $v_j$ 的距离是使 $A^{n}$ 的 $a_{ij}^{(k)}\neq0$ 的最小整数

这是显然的

## 关联矩阵

Incidence matrix is $n\times m$ matrix of a $(n,m)$ graph

### 定义

若 $G$ 是 $(n,m)$ 图。定义 $G$ 的关联矩阵: $M(G)=(a_{ij})_{n\times m}$ ，其中：$a_{ij}$ 是 $v_i$ 与 $e_j$ 关联的次数 (取值为 0,1,或 2(环)).

- 关联矩阵的元素为 0，1 或 2 ；
- 关联矩阵的每列和为 2 ; 每行的和为对应顶点度数；

### 连通的充要条件

无环图 $G$ **连通的充分必要条件**是 $𝑅(𝑀)=n-1$；

---

证明充分性：

若 $G$ 不连通，假设 $G$ 有两个连通分支 $G_1$ 与 $G_2$ , 又设 $G_1$ 与 $G_2$ 的关联矩阵分别为 $𝑀_1$ 与 $𝑀_2$ , 则 $G$ 的关联矩阵可以写为:

$$
M(G)=
\begin{pmatrix}
M_1 & \\
& M_2
\end{pmatrix}
$$

于是 $𝑅(𝑀)=V(G_1)-1+V(G_2)-1=n-2$ ，矛盾！所以 $G$ 一定连通。

---

证明必要性：

令：

$$
M=
\begin{pmatrix}
m_1\\
m_2\\
\vdots\\
m_n
\end{pmatrix}
$$

由于 $𝑀$ 中每列恰有两个 “1” 元素，所有行向量的和为 0 (模 2 加法运算，即为偶数)，所以有 $R(M)\leq n-1$。

另一方面，在 $M$ 中任意去掉一行 $m_k$ ，由于 $G$ 是**连通**的，因此，$m_k$ 中存在元素 “1” ，这样，$M$ 中去掉行 $m_k$ ，后的行按模 2 相加不等于零，即它们是线性无关的。所以有 $R(M)\geq n-1$。

因此，$R(M)=n-1$。

### 基本关联矩阵

在 $G$ 的关联矩阵中删掉任意一行后得到的矩阵可以完全决定 $G$ ，该矩阵称为 $G$ 的**基本关联矩阵**。删掉的行对应的顶点称为该基本关联矩阵的**参考点**。

!!! abstract
    图的关联矩阵及其性质是网络图论的基础，在电路分析中有重要应用

图的关联矩阵比邻接矩阵大得多，不便于计算机存储。但二者都有各自的应用特点

### 定义 2

Incidence matrix $C$ is $m\times n$ matrix of a $(n,m)$ graph

用来定义拉普拉斯矩阵 $L=C^{T}C$ 。

- 每行代表一条边，每列代表一个顶点
- 每行和为 0 ，有一个 1 和 -1 ，分别表示边的起点和终点
- 其余元素全部为 0

$$
C=
\begin{pmatrix}
e_1^{T}\\
e_2^{T}\\
\vdots\\
e_m^{T}
\end{pmatrix}
$$

## 度矩阵

Degree matrix is $n\times n$ diagonal matrix

### 定义

度矩阵是**对角阵**，对角上的元素为各个顶点的度。

:warning:有向图中，可能会有不同的定义。一般度数为各个顶点入度与出度之和，例如拉普拉斯矩阵引入的度矩阵。

## 拉普拉斯矩阵

Laplacian matrix ($L$) is $n\times n$ symmetric matrix

!!! abstract
    拉普拉斯矩阵是对称矩阵，保留了图的“无向”的属性，丢失了“有向”的属性

设 $G$ 是顶点集合为 $V(G)=\{v_1,v_2,\cdots,v_n\}$ 的图， $D$ 为图的**度矩阵**，$A=a_{ij}$ 是 $G$ 的**邻接矩阵**，$C$ 为图的**关联矩阵**。

定义：

$$
L=D-A=C^{T}C
$$

拉普拉斯矩阵 $L=(l_{ij})$ 是 $n$ 阶方阵，其中：

$$
l_{ij}=\begin{cases}
d(v_i), & i=j\\
-a_{ij}, & i\neq j
\end{cases}
$$

- 所有特征值为非负实数: $0\leq\lambda_0\leq\lambda_1\leq\cdots\leq\lambda_{n-1}$；
- 特征向量为实(正交)向量
- 每一行、列之和都等于 0
- $x^{T}Lx\geq0$ 是半正定二次型，或者说拉普拉斯矩阵 $L$ 是半正定矩阵

[Fiedler, 1973] $G$ 有 $k$ 个连通分支当且仅当：

$$
\lambda_0=\lambda_1=\cdots=\lambda_{k-1}=0
$$

$G$ 显然至少有一个连通分支，所以 $\lambda_0=0$。

$G$ 的割边(cut edges)数量等于：

$$
\frac{1}{4}x^{T}Lx=\frac{1}{4}\sum_{(i,j)\in E}(x_i-x_j)^2
$$

(等式右侧推导在下面)

对任意对称矩阵 $M$ 定义 $\lambda_2$：

$$
\lambda_2=\min_{x}\frac{x^{T}Mx}{x^{T}x}
$$

考虑拉普拉斯矩阵 $L$ 对应的二次型 $x^{T}Lx$：

$$
\begin{aligned}
x^{T}Lx
&=\sum_{i,j=1}^{n}L_{ij}x_ix_j\\
&=\sum_{i,j=1}^{n}(D_{ij}-A_{ij})x_ix_j\\
&=\sum_{i=1}^{n}D_{ii}x_i^2-\sum_{(i,j)\in E}2x_ix_j\\
&=\sum_{(i,j)\in E}(x_i^2+x_j^2-2x_ix_j)\\
&=\sum_{(i,j)\in E}(x_i-x_j)^2
\end{aligned}
$$

此时：

$$
\lambda_2=\min_{\text{All labelings of so that }\sum x_i=0}\frac{\sum_{(i,j)\in E}(x_i-x_j)^2}{\sum_ix_i^2}
$$

## 子图空间

### 回路空间

https://lfool.github.io/LFool-Notes/other/%E5%9B%BE%E8%AE%BA.html#%E5%9B%9E%E8%B7%AF%E7%B3%BB%E7%BB%9F%E7%AE%80%E4%BB%8B

### 断集空间
