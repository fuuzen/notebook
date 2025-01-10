---
title: 离散数学基础:排列与组合
date: 2024/01/09
math: true
categories:
 - [数学, 离散数学, 离散数学基础]
tags:
 - 离散数学
 - 组合数学
 - 数论
---

:::primary

[离散数学基础-整理:](/math/discrete-math/discrete-math-basics/discrete-math/discrete-math-basics/) --- [排列与组合](/math/discrete-math/discrete-math-basics/dmbs/permutaion-and-combination/) --- [组合计数基本原理](/math/discrete-math/discrete-math-basics/dmbs/principles-of-counting/) --- [排列组合计数问题](/math/discrete-math/discrete-math-basics/dmbs/counting/) --- [递推关系式](/math/discrete-math/discrete-math-basics/dmbs/difference-equations/) --- [关系](/math/discrete-math/discrete-math-basics/dmbs/relations/) --- [集合与关系的运算](/math/discrete-math/discrete-math-basics/dmbs/operations-of-sets-and-relations/) --- [函数](/math/discrete-math/discrete-math-basics/dmbs/functions/) --- [代数系统](/math/discrete-math/discrete-math-basics/dmbs/algebra-system/)
## 思维导图

![排列组合计数问题](../../assets/math/discrete-math/discrete-math-basics/排列与组合.png)

## 排列

### 记号

- 

$$
P(n,r)=P_n^r=A_n^r
$$

### 描述

- n个(可区别的)物体

  - n个物体的r-排列

    - n个物体的n-排列，或n个物体的全排列

- 集合S，|S|=n

  - S的r-排列

    - S的n-排列，或S的全排列

### 公式

- 

  - 

$$
P(n,r)=\frac{n!}{(n-r)!}
$$

$$
P(n,n)=n!
$$

$$
P(n,r)=\left \{ \begin{aligned}
& \frac{n!}{(n-r)!} &&,& 0\leq r\lt n \\
&n! &&,&r=n\\
& 0 &&,& r\gt n
\end{aligned}
\right.
$$

## 组合

### 记号

- 通常将第三个称为二项式系数

$$
C(n,r)=C_n^r=\begin{pmatrix}n\\r \end{pmatrix}
$$

### 描述

- n个(可区别的)物体

  - n个物体的r-组合

- 集合S，|S|=n

  - S的r-组合

- 长度为n的二进制串

  - 长度为n且含r个1(或0)的二进制串数

### 公式

- 

$$
C(n,r)=C(r,n)=\frac{P(n,r)}{P(r,r)}=\left \{ \begin{aligned}
& \frac{n!}{r!(n-r)!} &,& 0\leq r\leq n \\
& 0 &,& r\gt n
\end{aligned}
\right.
$$

## 组合证明

（不严谨）

### 双计数证明

- 论证等式两边是针对同一集合元素的不同计数方法

### 双函数证明

- 论证等式两边虽然是针对两个不同的集合的元素进行计数，但这两个集合之间存在双函数

## 代数证明

（严谨）

### 利用数学归纳法、组合数、排列数等计算公式的证明

## 二项式定理

### 

$$
(x+y)^n=\sum_{k=0}^{n} \begin{pmatrix}n\\k\end{pmatrix}x^k y^{n-k}
$$

## 二项式定理的组合数推论

### 

$$
\sum_{k=0}^{n}\begin{pmatrix}n\\k\end{pmatrix} =2^n
$$

## 帕斯卡等式

### 

$$
\begin{pmatrix}n\\k \end{pmatrix} =\begin{pmatrix}n-1\\k \end{pmatrix}
+\begin{pmatrix}n-1\\k-1 \end{pmatrix}
$$

## 递推式

### 

$$
r\begin{pmatrix}n\\r\end{pmatrix} =(n-r+1)\begin{pmatrix}n\\r-1 \end{pmatrix}
，1\leq r\lt n
$$

### 

$$
r\begin{pmatrix}n\\r\end{pmatrix} =n\begin{pmatrix}n-1\\r-1 \end{pmatrix}
，1\leq r\lt n
$$

## 乘积化简式

### 

$$
\begin{pmatrix}n\\m\end{pmatrix} \begin{pmatrix}m\\k\end{pmatrix}
=\begin{pmatrix}n\\k\end{pmatrix}
\begin{pmatrix}n-k\\m-k\end{pmatrix}
，k\leq m\leq n
$$

## 上下标求和式

朱世杰恒等式

### Hockeystick等式

$$
\begin{aligned} &\begin{pmatrix}m+r+1\\r\end{pmatrix}\\
=&\sum_{k=0}^{r}
\begin{pmatrix}m+k\\m\end{pmatrix}
=\begin{pmatrix}m\\m\end{pmatrix}
+\begin{pmatrix}m+1\\m\end{pmatrix}
+...
+\begin{pmatrix}m+r\\m\end{pmatrix}\\
=&\sum_{k=0}^{r}
\begin{pmatrix}m+k\\k\end{pmatrix}
=\begin{pmatrix}m\\0\end{pmatrix}
+\begin{pmatrix}m+1\\1\end{pmatrix}
+...
+\begin{pmatrix}m+r\\r\end{pmatrix}
\end{aligned}
$$

### 令 n=r+1 可得到上面Hockeystick等式

$$
\sum_{k=0}^{n} \begin{pmatrix}k\\m\end{pmatrix}
=\begin{pmatrix}0\\m\end{pmatrix}
+\begin{pmatrix}1\\m\end{pmatrix}
+...
+\begin{pmatrix}n\\m\end{pmatrix}
=\begin{pmatrix}n+1\\m+1\end{pmatrix}
$$

## 朱世杰-范德蒙等式

### 

$$
\begin{aligned} &
\sum_{k=0}^{r}
\begin{pmatrix}m\\r-k\end{pmatrix}
\begin{pmatrix}n\\k\end{pmatrix}
=\begin{pmatrix}m+n\\r\end{pmatrix}
\\&
=\begin{pmatrix}m\\r\end{pmatrix}
\begin{pmatrix}n\\0\end{pmatrix}
+\begin{pmatrix}m\\r-1\end{pmatrix}
\begin{pmatrix}n\\1\end{pmatrix}
+...
+\begin{pmatrix}m\\1\end{pmatrix}
\begin{pmatrix}n\\r-1\end{pmatrix}
+\begin{pmatrix}m\\0\end{pmatrix}
\begin{pmatrix}n\\r\end{pmatrix}
\end{aligned}
$$

