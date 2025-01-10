---
title: 离散数学基础:代数系统
date: 2024/01/09
math: true
categories:
 - [数学, 离散数学, 离散数学基础]
tags:
 - 离散数学
 - 代数
---

:::primary

[离散数学基础-整理:](/math/discrete-math/discrete-math-basics/discrete-math/discrete-math-basics/) --- [排列与组合](/math/discrete-math/discrete-math-basics/dmbs/permutaion-and-combination/) --- [组合计数基本原理](/math/discrete-math/discrete-math-basics/dmbs/principles-of-counting/) --- [排列组合计数问题](/math/discrete-math/discrete-math-basics/dmbs/counting/) --- [递推关系式](/math/discrete-math/discrete-math-basics/dmbs/difference-equations/) --- [关系](/math/discrete-math/discrete-math-basics/dmbs/relations/) --- [集合与关系的运算](/math/discrete-math/discrete-math-basics/dmbs/operations-of-sets-and-relations/) --- [函数](/math/discrete-math/discrete-math-basics/dmbs/functions/) --- [代数系统](/math/discrete-math/discrete-math-basics/dmbs/algebra-system/)
## 思维导图

![代数系统](../../assets/math/discrete-math/discrete-math-basics/代数系统.png)

## 二元运算

封闭性

### 定义

- 

$$
\begin{aligned}& f:S\times S\to S 是集合S的运算，\\&
则称集合S对运算f封闭
\end{aligned}
$$

- 

$$
\begin{aligned}& T\subseteq S是S的子集，若\forall t_1,t_2\in T(f(t_1,t_2)\in T)，\\&
则称集合T对S的运算f封闭
\end{aligned}
$$

### 经典例子

- 

$$
\begin{aligned}& 自然数集、整数集和实数集对加法、乘法运算封闭
\end{aligned}
$$

- 

$$
\begin{aligned}& 自然数集和整数集对实数集的加法、乘法运算封闭
\end{aligned}
$$

- 

$$
\begin{aligned}& 自然数集对整数集的加法、乘法运算封闭
\end{aligned}
$$

- 

$$
\begin{aligned}& 自然数集对实数集和整数集的减法运算不封闭
\end{aligned}
$$

- 

$$
\begin{aligned}& 自然数集、整数集和实数集对除法运算不封闭
\end{aligned}
$$

- 

$$
\begin{aligned}& 关系复合\circ 是P(A\times B)\times P(C\times D)\to P(A\times D)的函数\\&
一般情况下\circ 不是某个集合的运算\\&
关系复合\circ 是运算\\
\Leftrightarrow & A=B=C=D\\
\Leftrightarrow &
关系复合\circ 是P(A\times A)\times P(A\times A)\to P(A\times A)的函数
\end{aligned}
$$

- 

$$
\begin{aligned}& 函数族A^A关于关系复合运算\circ 封闭，A^A\subseteq P(A\times A)
\end{aligned}
$$

## 特殊元

### 左单位元

- 

$$
\begin{aligned}& e_l是运算\circ的左单位元\Leftrightarrow\forall x\in S, e_l\circ x=x
\end{aligned}
$$

### 右单位元

- 

$$
\begin{aligned}& e_r是运算\circ的右单位元\Leftrightarrow\forall x\in S, x\circ e_r=x
\end{aligned}
$$

### 单位元

(幺元)

- 

$$
\begin{aligned}& e是运算\circ的单位元\Leftrightarrow\forall x\in S, x\circ e=e\circ x=x
\end{aligned}
$$

- 左逆元
  （左逆）

  - 

$$
\begin{aligned}& y_l
是x(关于运算\circ)的左逆元\\\Leftrightarrow&
y_l\circ x=e\\\Leftrightarrow&
x左可逆
\end{aligned}
$$

- 右逆元
  （右逆）

  - 

$$
\begin{aligned}& y_r
是x(关于运算\circ)的右逆元\\\Leftrightarrow&
x\circ y_r=e\\\Leftrightarrow&x右可逆
\end{aligned}
$$

- 逆元
  （逆）

  - 

$$
\begin{aligned}& y是x(关于运算\circ)的逆元\\\Leftrightarrow&
y既是x的左逆又是x的右逆\\\Leftrightarrow&
x可逆
\end{aligned}
$$

- 性质

  - 单位元若存在一定是唯一的
  - 若满足结合律，且单位元存在，则可逆元素有唯一逆元

### 左零元

- 

$$
\begin{aligned}& \theta_l
是运算\circ的左单位元\Leftrightarrow\forall x\in S, \theta_l
\circ x=\theta_l
\end{aligned}
$$

### 左零元

- 

$$
\begin{aligned}& \theta_r
是运算\circ的左单位元\Leftrightarrow\forall x\in S, x\circ\theta_r
=\theta_r
\end{aligned}
$$

### 零元

- 

$$
\begin{aligned}& \theta
是运算\circ的左单位元\Leftrightarrow\forall x\in S, x\circ\theta=\theta\circ x
=\theta
\end{aligned}
$$

### 幂等元

- 

$$
\begin{aligned}& x\circ x=x
\end{aligned}
$$

## 二元运算性质

### 交换律

- 描述

  - 

$$
\begin{aligned}& 运算\circ 在S上是可交换的，或满足交换律的
\end{aligned}
$$

- 定义

  - 

$$
\begin{aligned}& \forall x,y\in S,\ \ \ x\circ y=y\circ x
\end{aligned}
$$

### 结合律

- 描述

  - 

$$
\begin{aligned}& 运算\circ 在S上是可结合的，或满足结合律的
\end{aligned}
$$

- 定义

  - 

$$
\begin{aligned}& \forall x,y,z\in S,\ \ \ (x\circ y)\circ z=x\circ (y\circ x)
\end{aligned}
$$

- 满足结合律的运算的指数运算(幂运算)

  - 定义

    - 

$$
\begin{aligned}& x^n=x\circ x\circ...\circ x
\end{aligned}
$$

$$
\begin{aligned}     x^n=\left\{\begin{aligned}
            &x\\
            &x^{n-1}\circ x
        \end{aligned}\right.
        &&
        \begin{aligned}
        &n=1\\
        &n\gt 1
        \end{aligned}
    \end{aligned}
$$

- 性质
	
$$
\begin{aligned}& x^nx^m=x^{n+m}
\end{aligned}
$$

$$
\begin{aligned}& (x^n)x^m=x^{nm}
\end{aligned}
$$

### 幂等律

- 定义

  - 

$$
\begin{aligned}& S的运算\circ 满足幂等律\\\Leftrightarrow&
\forall x\in S, x\circ x=x\\\Leftrightarrow&
S的所有元素都是幂等元
\end{aligned}
$$

### 分配律

- 描述

  - 

$$
\begin{aligned}& 运算*对运算\circ 是可分配的，或满足分配律的
\end{aligned}
$$

- 定义

  - 

$$
\begin{aligned} \forall x,y,z\in S\ ,\ &
x*(y\circ z)=(x*y)\circ(x*z)\\&
(y\circ z)*x=(y*x)\circ(z*x)
\end{aligned}
$$

### 吸收律

- 描述

  - 

$$
\begin{aligned}& 运算*对运算\circ 是可分配的，或满足分配律的
\end{aligned}
$$

- 定义

  - 

$$
\begin{aligned} \forall x,y\in S\ ,\ &x*(x\circ y)=x\\&x\circ(x*y)=x
\end{aligned}
$$

- 典例

  - 

$$
\begin{aligned}& 逻辑与、或满足吸收律
\end{aligned}
$$

$$
\begin{aligned}& 集合交、并满足吸收律
\end{aligned}
$$

### 消去律

- 定义

  - 

$$
\begin{aligned}& \forall x,y,z\in S\ ,\ x不是零元\\&
x\circ y=x\circ z\to y=z\\&
y\circ x=z\circ x\to y=z
\end{aligned}
$$

- 典例

  - 

$$
\begin{aligned}& 数集加法、乘法满足消去律
\end{aligned}
$$

$$
\begin{aligned}& 集合交、并不满足消去律
\end{aligned}
$$

## 群

### 定义

- 一个群包含

  - 

$$
\begin{aligned}& 一个二元运算\circ
\end{aligned}
$$

$$
\begin{aligned}& \circ 满足结合律
\end{aligned}
$$

$$
\begin{aligned}& 一个零元运算即单位元e
\end{aligned}
$$

$$
\begin{aligned}& e是\circ 的单位元
\end{aligned}
$$

$$
\begin{aligned}& 一个一元运算(-)^{-1}
\end{aligned}
$$

$$
\begin{aligned}& (-)^{-1}给出每个元素关于\circ 的逆元
\end{aligned}
$$

- 一个独异点包含

  - 

$$
\begin{aligned}& 一个二元运算\circ
\end{aligned}
$$

$$
\begin{aligned}& \circ 满足结合律
\end{aligned}
$$

$$
\begin{aligned}& 一个零元运算即单位元e
\end{aligned}
$$

$$
\begin{aligned}& e是\circ 的单位元
\end{aligned}
$$

- 一个半
  群包含

  - 

$$
\begin{aligned}& 一个二元运算\circ
\end{aligned}
$$

$$
\begin{aligned}& \circ 满足结合律
\end{aligned}
$$

### 性质

- 群有唯一单位元，没有零元，且所有元素有唯一逆元
- 群的二元运算满足消去律

### 阿贝尔群

(可交换群)

- 

$$
运算满足交换律的群
$$

### 群的阶

- 群的元素个数

### 群元素的阶

- 

$$
\begin{aligned}& 定义群元素a的n次幂：\\&
\begin{aligned}
    a^n=\left\{\begin{aligned}
            &e\\
            &a^{n-1}a\\
            &(a^{-1})^{|n|}
        \end{aligned}\right.
        &&&&
        \begin{aligned}
        &n=0\\
        &n\gt 0\\
        &n\lt 0
        \end{aligned}
    \end{aligned}\\&
a的阶|a|是使a^k=e成立的最小正整数k，当k不存在时a是无限阶元
\end{aligned}
$$

- 性质

  - 

$$
\begin{aligned}& a^m=e\iff n|m,\ \ m\in \mathbb Z
\end{aligned}
$$

$$
\begin{aligned}& (a^{-1})^k=(a^k)^{-1},\ k\in\mathbb N
\end{aligned}
$$

$$
\begin{aligned}& |a^{-1}|=|a|
\end{aligned}
$$

$$
\begin{aligned}& |a|=|a^m|gcd(|a|,m),\ \ m\in \mathbb Z
\end{aligned}
$$

## 模m单位群

### 定义

- 

$$
\begin{aligned}& \mathbb{Z}_m=\{0,1,2,...,m-1\}\\&
U(m)是\mathbb{Z}_m中所有与m互质的数构成的集合：\\&
U_m=\{a\in\mathbb{Z}_m|gcd(a,m)=1\}\\&
模m乘运算\otimes与U_m构成群
\end{aligned}
$$

## 正规子群

or
不变子群

### 定义

- 

$$
\begin{aligned}& H是G的子群，
\forall a\in G,\ Ha=aH
\end{aligned}
$$

### 平凡正规子群

- 

$$
G的单位元子群\{e\}
$$

- 

$$
G本身
$$

### 单群

- 

$$
\begin{aligned}& G的正规子群只有平凡正规子群，且G\neq \{e\}
\end{aligned}
$$

### 正规子群

经典例子

- 交换群

  - 

$$
\begin{aligned}& A交换群每个元素左陪集等于右陪集\\&
故交换群每个子群都是正规子群
\end{aligned}
$$

- 只有2个
  相异陪集

  - 

$$
待补
$$

## 商群

### 描述

- 

$$
G关于正规子群H的商群
$$

### 定义

- 

$$
\begin{aligned}& N是G的正规子群，N的全体陪集构成集合G/N\\&
定义G/N上的运算\circ 为\forall a,b\in G,\ \ Na\circ Nb=Nab\\&
从而G/N关于运算\circ构成群,单位元为N
\end{aligned}
$$

## 群同态

### 描述

- 

$$
群G到G'的一个同态f是一个函数f:G\to G'
$$

### 定义

- 

$$
\begin{aligned}& \forall a,b\in G,\ \ \ f(ab)=f(a)f(b)
\end{aligned}
$$

### 自同态

- 

$$
群G到自己的同态f:G\to G
$$

### 满同态

- 

$$
f是满函数
$$

### 单同态

- 

$$
f是单函数
$$

### 同构

- 

$$
f是双函数
$$

- 

$$
G\cong G'
$$

