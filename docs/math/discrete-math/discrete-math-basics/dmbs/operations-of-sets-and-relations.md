---
title: 离散数学基础:集合与关系的运算
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

![集合与关系的运算](../../assets/math/discrete-math/discrete-math-basics/集合与关系的运算.png)

## 并

### 定义

$$
\begin{aligned}& R\cup S =\{<a,b>|<a,b>\in R\ \ \lor <a,b>\in S\}\end{aligned}
$$

### 关系矩阵

$$
\begin{aligned}& M_{R\cup S} = M_R \lor M_S = [r_{ij}\lor s_{ij}]\end{aligned}
$$

### 性质

- 

$$
\begin{aligned}&\text{单位元： 空关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{零元：全关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系并满足交换律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系并满足结合律}\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系并的单调性：关系并保持子集关系}\\
A\subseteq B,C\subseteq D \Rightarrow A\cup B\subseteq C\cup D\\
A\subseteq C,B\subseteq C \Leftrightarrow A\cup B\subseteq  C\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系并对关系交有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系并对关系差不满足分配律}\\
A\cup B-A\cup C\subseteq A\cup(B-C)\end{aligned}
$$

## 交

### 定义

- 

$$
\begin{aligned}& R\cap S =\{<a,b>|<a,b>\in R\ \ \land <a,b>\in S\}\end{aligned}
$$

### 关系矩阵

- 

$$
\begin{aligned}& M_{R\cap S} = M_R \land M_S = [r_{ij}\land s_{ij}]\end{aligned}
$$

### 性质

- 

$$
\begin{aligned}& 单位元：全关系 \end{aligned}
$$

- 

$$
\begin{aligned}&\text{零元： 空关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系交满足交换律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系交满足结合律}\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系交的单调性：关系交保持子集关系}\\
A\subseteq B,C\subseteq D \Rightarrow A\cap B\subseteq C\cap D\\
C\subseteq A,C\subseteq B \Leftrightarrow C\subseteq  A\cup B\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系交对关系并有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系交对关系差有分配律}\end{aligned}
$$

## 差

### 定义

- 

$$
\begin{aligned}& R- S =\{<a,b>|<a,b>\in R\ \ \land <a,b>\notin S\}\end{aligned}
$$

### 关系矩阵

- 

$$
\begin{aligned}& M_{R- S} = M_R \ominus M_S = [r_{ij}\ominus s_{ij}]\end{aligned}
$$

- 

$$
\begin{aligned}&\text{运算}\ominus：0\ominus0=0\ominus1=1\ominus1=0，1\ominus0=1\end{aligned}
$$

### 性质

- 

$$
\begin{aligned}&\text{左零单位： 全关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{右单位元： 空关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{左零元： 空关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{右零元： 全关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系差不满足交换律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系差不满足结合律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系差无单调性：关系差不保持子集关系}\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系差对关系并没有分配律}:\\
A-(B\cup C)
\subseteq
 (A-B)\cup(A-C)\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系差对关系交没有分配律}:\\
(A-B)\cap(A-C)
\subseteq
A-(B\cap C)\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系差对关系交、关系并有反分配律}:\\
A-(B\cup C)
=
(A-B)\cap(A-C)\\
A-(B\cap C)
=
(A-B)\cup(A-C)\end{aligned}
$$

## 复合

### 定义

- 

$$
\begin{aligned}& S\circ R=\{<a,c>|\exists b\in B(<a,b>\in R\land <b,c>\in S)\}\end{aligned}
$$

### 关系矩阵

- 

$$
\begin{aligned}& M_{R\circ S} = M_R \odot M_S\end{aligned}
$$

- 

$$
\begin{aligned}&\text{逻辑积运算}\odot\text{：与矩阵乘法本质相同，}\\&
\text{只是将其中的加法、乘法分别用}\\&
\text{逻辑或}\lor\text{(逻辑加法) 、逻辑与}\land\text{(逻辑减法)替代}\end{aligned}
$$

### 备注

- 

$$
\begin{aligned}&\text{本课程采用逆序复合}\end{aligned}
$$

### 性质

- 

$$
\begin{aligned}&\text{单位元：恒等关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{零元： 空关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系复合不满足交换律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系复合满足结合律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系复合的单调性：关系复合保持子集关系}\\&
R\subseteq S, U\subseteq W \to U\circ R\subseteq W\circ S\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系复合对关系并有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}\text{关系复合对关系交没有分配律}:\\
(R\cap S)\circ T
\subseteq
 (R\circ T)\cap(S\circ T)\end{aligned}
$$

## 补

### 定义

- 

$$
\begin{aligned} \overline R &=\{<a,b>\in A\times B|<a,b>\notin R\}\\&
=A\times B-R\end{aligned}
$$

### 关系矩阵

- 

$$
\begin{aligned}& M_{\overline R}=\overline{M_R}=[\overline{r_{ij}}]\end{aligned}
$$

### 性质

- 

$$
\begin{aligned}&\text{关系补的单调性：关系补运算保持相反的子集关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系补对关系并没有分配律}:
\overline{A\cup B}
\subseteq
\overline{A}\cup\overline{B}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系补对关系交有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系补对关系差没有分配律}:
\overline{A}-\overline{B}\subseteq\overline{A-B}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系补对关系复合有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系逆与关系补有交换律(两个一元运算)}\end{aligned}
$$

## 逆

### 定义

- 

$$
\begin{aligned}& R^{-1}=\{<b,a>|<a,b>\in R\}\end{aligned}
$$

### 关系矩阵

- 

$$
\begin{aligned}& M_{R^{-1}}=(M_R)^T\end{aligned}
$$

### 性质

- 

$$
\begin{aligned}&\text{关系逆的单调性：关系逆运算保持子集关系}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系逆对关系并有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系逆对关系交有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系逆对关系差有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系逆对关系复合有分配律}\end{aligned}
$$

- 

$$
\begin{aligned}&\text{关系逆与关系补有交换律(两个一元运算)}\end{aligned}
$$

