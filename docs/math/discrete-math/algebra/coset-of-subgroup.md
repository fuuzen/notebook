---
title: 子群的陪集
date: 2024/03/29
math: true
categories:
  - [数学, 离散数学, 代数结构]
tags:
  - 离散数学
  - 代数
  - 群论
---

## 群子集的乘积运算

### 定义

设 $A$ 和 $B$ 是群 $G$ 的两个非空子集，称集合：

$$
AB = \{ab|a\in A, b\in B \}
$$

为群的子集 $A$ 与 $B$ 的**乘积**(product)

如果 $g\in G$ 为群 $G$ 的一个元素，$A=\{g\}$ ，则：

- $AB$ 简记为 $gB = \{gb|b\in B\}$；
- $BA$ 简记为 $Bg = \{bg|b\in B\}$；

!!! warning
    注意，即使有 $AB=BA$，也不意味着 $\forall a\in A, b\in B,\ \ \ ab =ba$，正确的结论应该是：$\forall a\in A, b\in B,\ \ \ \exist a'\in A, b'\in B,\ \ \ ab = b'a'$

### 交换群的交换律

当 $G$ 是**交换群**时，对 $G$ 的任意子集 $A,B$ ，满足交换律，有 $AB=BA$ ；但对于非交换群，一般没有 $AB=BA$ ，也就是没有交换律成立。

### 满足结合律

群的子集运算满足结合律：$A(BC) = (AB)C$ ；

### 对单元素满足消去律

若 $gA=gB$ 或 $Ag=Bg$，则 $A=B$。

!!! warning
    但对普遍的

$$
AC=BC\not\rArr A=B
$$

---

证明（不妨考虑 $gA=gB$ ）

$\forall a\in A$，有 $ga\in gA$ ；

因为 $gA=gB$ ，所以 $ga\in gB$ ，所以：

$$
\exist b\in B,\ \ \ ga=gb
$$

由群满足消去律可得：$b=a$ ；

所以 $a=b\in B$ ，所以有 $A\sube B$ ；同理可证 $B\sube A$，从而可得 $A=B$。

### 幂运算

若 $H$ 是群 $G$ 的子群，则 $HH=H$。

### 乘积的子群充要条件

如果 $A,B$ 是群 $G$ 的两个子群，则 $AB$ 也是群 $G$ 的子群当且仅当 $AB=BA$。

---

必要性: $AB\leq G\rArr AB=BA$：

对于 $\forall a,b,\ \ ab\in AB$，只需证 $ab\in BA$；

由于 $AB\leq G$ ，所以 $\exist a'b'\in AB,\ \ \ (ab)^{-1}=a'b'$

$$
ab=((ab)^{-1})^{-1}=(a'b')^{-1}=b'^{-1}a'^{-1}\in BA
$$

从而可得 $AB\sube BA$ ；同理可证 $BA\sube AB$，从而可得 $AB=BA$。

---

充分性: $AB=BA\rArr AB\leq G$：

对于 $\forall a_1,b_1,a_2,b_2,\ \ \ a_1b_1,a_2b_2\in AB$，有：$a_1b_1(a_2b_2)^{-1}\in AB$；

$$
a_1b_1(a_2b_2)^{-1}=a_1b_1b_2^{-1}a_2^{-1}=a_1(b_1b_2^{-1})a_2^{-1}\\
\in ABA=A(BA)=A(AB)=(AA)B=AB
$$

从而由子群判定定理二可知 $AB$ 是 $G$ 的子群

### 乘积的子集关系

设 $A,B,C$ 是群 $G$ 的非空子集（不一定是群），若 $B⊆C$ ，则有 $AB⊆AC$ 和 $BA⊆BC$。

---

证明

对任意 $x\in AB$ ，则存在 $a\in A,b\in B$，使得 $x=ab$ 。由 $b\in B$ ，且 $B⊆C$，因此 $b\in C$，因此 $x=ab\in AC$ ，因此 $AB⊆BC$ 。

对任意 $x\in BA$ ，则存在 $a\in A,b\in B$ ，使得 $x=ba$ 。由 $b\in B$ ，且 $B⊆C$ ，因此 $b\in C$ ，因此 $x=ab\in AC$ ，因此 $BA⊆CA$ 。

## 子群的陪集及其性质

### 陪集定义

$G$ 是群，$H$ 是 $G$ 的子群

对任意 $a\in G$，群 $G$ 的**子集** $aH=\{ah|h\in H\}$ 与 $Ha=\{ha|h\in H\}$ ，分别称为 $H$ 在 $G$ 中的**左陪集**(left coset)和**右陪集**(right coset)

子群 $H$ 的所有右陪集构成的集合族 $G\backslash H$ 是 $G$ 的一个划分

子群 $H$ 的所有左陪集构成的集合族 $G/H$ 也是 $G$ 的一个划分

!!! warning
    注意陪集不一定是群了

### 陪集举例

对于群 $(U(5),⊗_5)$，子群 $H=\{1,4\}$：

| $⊗_5$ | 1   | 2   | 3   | 4   |
| ----- | --- | --- | --- | --- |
| 1     | 1   | 2   | 3   | 4   |
| 2     | 2   | 4   | 1   | 3   |
| 3     | 3   | 1   | 4   | 2   |
| 4     | 4   | 3   | 2   | 1   |

$$
1H=\{1⊗_5 1,1⊗_5 4\}=\{1,4\}\\
2H=\{2⊗_5 1,2⊗_5 4\}={2,3}\\
3H=\{3⊗_5 1,3⊗_5 4\}=\{3,2\}\\
4H=\{4⊗_5 1,4⊗_5 4\}=\{4,1\}
$$

$H$ 有两个不同的陪集 $\{1, 4\}$ 和 $\{2,3\}$。

---

对于下面的置换群 $(G,\circ)$ ，子群 $H=\{f_1, f_2\}$：

| $\circ$ | $f_1$ | $f_2$ | $f_3$ | $f_4$ | $f_5$ | $f_6$ |
| ------- | ----- | ----- | ----- | ----- | ----- | ----- |
| $f_1$   | $f_1$ | $f_2$ | $f_3$ | $f_4$ | $f_5$ | $f_6$ |
| $f_2$   | $f_2$ | $f_1$ | $f_6$ | $f_5$ | $f_4$ | $f_3$ |
| $f_3$   | $f_3$ | $f_5$ | $f_1$ | $f_6$ | $f_2$ | $f_4$ |
| $f_4$   | $f_4$ | $f_6$ | $f_5$ | $f_1$ | $f_3$ | $f_2$ |
| $f_5$   | $f_5$ | $f_3$ | $f_4$ | $f_2$ | $f_6$ | $f_1$ |
| $f_6$   | $f_6$ | $f_4$ | $f_2$ | $f_3$ | $f_1$ | $f_5$ |

$$
f_1 H=\{f_1\circ f_1, f_1\circ f_2\}=\{f_1, f_2\}\\
f_2 H=\{f_2\circ f_1, f_2\circ f_2\}=\{f_2, f_1\}\\
f_3 H=\{f_3\circ f_1, f_3\circ f_2\}=\{f_3, f_5\}\\
f_4 H=\{f_4\circ f_1, f_4\circ f_2\}=\{f_4, f_6\}\\
f_5 H=\{f_5\circ f_1, f_5\circ f_2\}=\{f_5, f_3\}\\
f_6 H=\{f_6\circ f_1, f_6\circ f_2\}=\{f_6, f_4\}\\
Hf_3=\{f_1\circ f_3, f_2\circ f_3\}=\{f_3, f_6\}\\
Hf_4=\{f_1\circ f_4, f_2\circ f_4\}=\{f_4, f_5\}\\
Hf_5=\{f_1\circ f_5, f_2\circ f_5\}=\{f_5, f_4\}\\
Hf_6=\{f_1\circ f_6, f_2\circ f_6\}=\{f_6, f_3\}
$$

$H$ 有三个不同的陪集，且左右陪集不相等，例如 $f_3H≠Hf_3$。

### 不同元素陪集相同充要条件

$\forall a,b\in G$，

- $Ha=Hb$ 当且仅当 $a\in Hb$ 当且仅当 $ab^{-1}\in H$；
- $aH=bH$ 当且仅当 $a\in bH$ 当且仅当 $b^{-1}a\in H$；

### 陪集不变的充要条件

设 $H$ 是群 $G$ 的子群，对任意的 $a\in G$：

- $Ha＝H$ 的充分必要条件是 $a\in H$；

---

引理

因 $e\in H$ ，所以 $a＝ea\in Ha$ 。

---

证明

必要性：若 $Ha＝H$ ，则因为 $a\in Ha$ ，从而 $a\in H$ 。

充分性：反之若 $a\in H$ ，则对任意 $ha\in Ha$ ，由 $H$ 对群运算封闭，也有 $ha\in H$ ，从而有 $Ha\subseteq H$；

而这时对任意 $h\in H$ ，由 $a\in H$ 有 $a^{-1}\in H$ ，从而 $ha^{-1}\in H$ ，从而有 $h＝ha^{-1}a\in Ha$ ，即有 $H\subseteq Ha$ 。

于是又 $Ha\subseteq H,H\subseteq Ha$ 得到 $Ha=H$ 。

### 陪集的子群充要条件

设 $H$ 是群 $G$ 的子群，对任意的 $a\in G$：

- 右陪集 $Ha$ 是 $G$ 的子群当且仅当 $Ha=H$ 当且仅当 $a\in H$；
- 左陪集 $aH$ 是 $G$ 的子群当且仅当 $aH=H$ 当且仅当 $a\in H$；

---

证明以右陪集为例：

必要性：若 $Ha\leq G$ ，所以 $e\in Ha$ ，从而存在 $h\in H$ 使得 $e＝ha$ ，从而 $a＝h^{-1}e=h^{-1}\in H$ ；

充分性：若 $a\in H$ ，则 $Ha＝H$ 是子群。

### 子群陪集运算性质

设 $H$ 是群 $G$ 的子群，则 $\forall a,b\in G$ 有：

$$
a\in Hb\Longleftrightarrow ab^{-1}\in H\Longleftrightarrow Ha=Hb
$$

---

循环论证：

---

（1）$a\in Hb\Longrightarrow ab^{-1}\in H$：

若 $a\in Hb$ ，即存在 $h\in H$ 使得 $a=hb$ 。从而 $ab^{-1}＝h\in H$ 。

---

（2）$ab^{-1}\in H\Longrightarrow Ha=Hb$：

对于任意的 $ha\in Ha$ ，这里 $h\in H$ ，因此 $hab^{-1}\in H$，从而：

$$
ha=hab^{-1}b\in Hb
$$

这表明 $Ha\subseteq Hb$ 。类似地，对任意的 $hb\in Ha$ ，这里 $h\in H$ 。由于 $ab^{-1}\in H$ ，则也有：

$$
ba^{-1}=(b^{-1})^{-1}a^{-1}=(ab^{-1})^{-1}\in H
$$

因此 $ha=hba^{-1}a\in Ha$ ，这表明 $Hb\subseteq Ha$ 。综合起来就有 $Ha=Bb$ 。

---

（3）$Ha=Hb\Longrightarrow a\in Hb$：

若 $Ha=Hb$ ，则由 $a\in Ha$ （$H$ 是子群）可得 $a\in Hb$ 。

### 子群陪集构成等价关系

设 $H$ 是群 $G$ 的子群，在 $G$ 上定义二元关系 $R\in G\times G$：

$$
\forall a,b\in G,\ \ \ a\text{R}b\Longleftrightarrow ab^{-1}\in H
$$

则 $R$ 是 $G$ 上的等价关系，且 $[a]_R=Ha$ 。

---

证明

上面已经证明了 $ab^{-1}\in H\Longleftrightarrow Ha=Hb$，因此 $a\text{R}b\Longleftrightarrow Ha= Hb$ 。

从而 $R$ 确实是自反、对称和传递的，即 $R$ 是等价关系。

对任意 $b\in G$ ，我们有：

$$
b\in Ha\Longleftrightarrow Hb=Ha\Longleftrightarrow b\text{R}a\Longleftrightarrow b\in[a]_R
$$

这就表明 $Ha=[a]_R$ 。

### 子群陪集构成划分

设 $H≤G$，则：

- （i）$\forall a,b\in G$ ,
  - $Ha=Hb$ ；
  - 或 $Ha\cap Hb=\varnothing$；
- （ii）$\mathbb U\{Ha|a\in G\}=G$。

证明——由等价类的性质立即可得。

### 正规子群

若对任意元素 $a\in G$ 都有 $aH=Ha$，即**左右陪集相等**，则称 $H$ 是 $G$ 的正规子群，

### 左右陪集关系

一个子群的左陪集的所有元素的逆元素组成这个子群的一个右陪集

符号化描述：

$H\leq G$，$a\in G$，

$$
S=\{x^{-1}|x\in{aH}\}
$$

## 拉格朗日定理

### 引理

$H$ 是 $G$ 的子群，对任意 $a\in G$ ，有 $|H|=|aH|=|Ha|$，即这三个集合**等势**

!!! info 
    集合等势：若两个集合前存在双函数，即称它们等势。对于有限集，就是这两个集合有相同元素个数

---

证明

定义 $\varphi:H\to Ha$ 为：

$$
\forall h\in H,\ \ \ \varphi(h)=ha\in Ha
$$

由群的消去律我们有：$\forall h_1,h_2\in H$

$$
\varphi(h_1)=\varphi(h_2)\Longrightarrow h_1a=h_2a\Longrightarrow h_1=h_2
$$

从而 $\varphi$ 是双函数，即 $|Ha|=|H|$。

类似定义 $\varphi':H\to a$ 为

$$
\forall h\in H,\ \ \ \varphi'(h)=ah\in aH
$$

不难证明 $|aH|=|H|$。

### 指标

设 $H$ 是群 $G$ 的子群。称子群 $H$ 在群 $G$ 中的左陪集或右陪集的个数（有限或无限）为 $H$ 在 $G$ 中的**指标**(index)，记为 $[G:H]$ 。

也称 $[G:H]$：$G$ 关于 $H$ 的陪集个数。

### 拉格朗日定理

$H$ 是有限群 $G$ 的子群，则：

$$
|G|=|H|\cdot[G:H]
$$

---

证明

设 $[G:H]=r$ ，则子群 $H$​ 有 $r$ 个不同的右陪集，设为 $H_{a_1},H_{a_2},\cdots,H_{a_r}$ 。由于 $G=U\{Ha|a\in G\}$ ，且对任意的 $a,b\in G$ ，$Ha=Hb$ 或者 $Ha\cap Hb=\varnothing$ ，因此：

$$
G=H_{a_1}\cup H_{a_2}\cup \cdots\cup H_{a_r}\\
|G|=|H_{a_1}|+|H_{a_2}|+\cdots+|H_{a_r}|
$$

而对任意的 $a\in G$ ，$|Ha|=|H|$ ，因此 $|G|=r|H|=|H|\cdot[G:E]$ 。

### 推论 1(元素阶与群阶的关系)

设 $G$ 是 $n$ 阶有限群，单位元是 $e$ ，则对 $G$ 的任意元素 $a$ ，$a$ 的阶 $|a|$ 是 $n$ 的因子，且 $a^{n}=e$ 。

---

证明

对任意的 $a\in G$ ，$<a>$ 是 $G$ 的子群，因此由拉格朗日定理 $|<a>|$ 是 $n$ 的因子;

但另一方面，若 $|a|=r$ ，则因 $<a>$ 是 $a$ 生成的子群，于是

$$
<a>=\{a^{0}=e,a^{1},\cdots,a^{r-1}\}
$$

即 $|<a>|=|a|=r$ ，从而 $|a|$ 是群 G 的阶 n 的因子，进而由**定理 1.6?**有 $a^{n}=e$ ，因为对任意的 $m\in \mathbb{Z}$ , $a^{m}=e$ 当且仅当 $r|m$ 。

!!! abstract
    元素的阶是群的阶的因子；

但并不是群的阶的每个因子，群都存在元素的阶恰好等于这个因子！

### 推论 2(费马小定理)

设 $p$ 是素数，$a$ 是与 $p$ 互素的整数，则有：

$$
a^{p-1}\equiv1(\bmod p)
$$

---

证明

因为在群 $U(p)$ ，即群 $\mathbb{Z}_p^*$ 中，由于 $a$ 与 $p$ 互素，$a$（准确地说，$a$ 整除 $p$ 的余数）属于 $\mathbb{Z}_p^*$ ，

而 $\mathbb{Z}_p^*$ 的阶是 $p-1$ ，因此 $a^{p-1}$ 等于单位元 1 ，对于 $U(p)$ 的模 $p$ 乘运算而言，就是 $a^{p-1}\equiv 1(\bmod p)$ 。

### 推论 3

$H,K$ 都是 $G$ 的有限子群，证明：

- （1） 记 $S=\{hK|h\in H\}$，则 $S$ 是 $H$ 的**划分**，从而 $|HK|=|S|\cdot|K|$
- （2）$|HK|=\frac{|H|\cdot|K|}{|H\cap K|}$；

---

（1）证明

即证 $h_1K$ ，$h_2K$ 要么相等，要么不相交

$$
h_1K=h_2K\lrArr h_1^{-1}h_2\in K\\
h_1K\cap h_2K=\varnothing
$$

---

（2）证明

为方便起见，记 $M=H\cap K$ ，由于 $H$ 和 $K$ 都是 $G$ 的子群,因此 $M$ 也是 $G$ 的子群且也是 $H$的子群。

定义函数 $\varphi:H/M\to S$，对任意 $h\in H$ ，$\varphi(hM)=hK$

下面证明 $\varphi$ 的定义是合适的，且是双函数，从而结合拉格朗日定理有 $|S|=|H/M|=|H|/|M|$ ，从而得到 $|HK|=\frac{|H|\cdot|K|}{|M|}$ 。

(a) 证明 $\varphi$ 的定义是合适的：

对任意 $h_1,h_2\in H$ ，若 $h_1M=h_2M$ ，则 $h_1^{-1}h_2\in M\subseteq K$ ，从而 $\varphi(h_1M)=\varphi(h_2M)$，这说明 $\varphi(hM)$ 的值不会因为选取的代表 $h$ 不同面不同，即 $\varphi$ 的定义是合适的。

(b) 证明 $\varphi$ 是单函数：

对任意 $h_1,h_2\in H$，若 $\varphi(h_1M)=\varphi(h_2M)$，即 $h_1K=h_2K$ ，从而 $h_1^{-1}h_2\in K$ ，

但 $h_1,h_2\in H$ ，因此也有 $h_1^{-1}h_2\in H$ ，

从而 $h_1^{-1}h_2\in H\cap K=M$ ，

从而 $h_1M=h_2M$ ，这就表明 $\varphi$ 是单函数。

(c) 显然 $\varphi$ 是满函数，对任意 $hK\in S$ ，都有 $\varphi(hM)=hk$。

因此 $|S|=|H/M|$ ，由拉格朗日定理有 $|S|=|H/M|=|H|/|M|$ ，最后得到:

$$
|HK|=\frac{|H|}{|M|}|K|=\frac{|H|\cdot|K|}{|H\cap K|}
$$

#### 举例

不容易找到两个子群的交不等于 $\{e\}$ 的例子，利用计算机程序可找到如下的例子：$U(33)=\{1,2,4,5,7,8,10,13,14,16,17,19,20,23,25,26,28,29,31,32\}$。

- 子群 $\{1,2,4,8,16,17,25,29,31,32\}$ 是循环子群，生成元为：$2,8,17,29$ ；
- 子群 $\{1,4,5,14,16,20,23,25,26,31\}$ 是循环子群，生成元为：$5,14,20,26$ ；

上述两个子群交集为 $\{1,4,16,25,31\}$ ；

## 西罗定理

可了解，不考

$G$ 是 $n$ 阶循环群, $a$ 是生成元.对于 $n$ 的每个正因子 $k$ , 有且仅有一个 $k$ 阶循环子群.并且由 $a^{\frac{n}{k}}$ 生成.
