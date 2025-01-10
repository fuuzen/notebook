---
title: 仿射希尔密码
date: 2024/10/05
math: true
categories:
  - [数学, 密码学]
tags:
  - 密码学
---

## 破解原理

!!! warning
    以下运算均为模 26 意义下

### 转化为方程问题

仿射希尔密码中定义密钥包括一个矩阵 $L$ 和一个向量 $\vec b$:

$$
L=\begin{pmatrix}
l_{11} & l_{12} & \cdots & l_{1m}\\
l_{21} & l_{22} & \cdots & l_{2m}\\
\vdots & \vdots &        & \vdots\\
l_{m1} & l_{m2} & \cdots & l_{mm}
\end{pmatrix}
\ ,\ \
\vec b=\begin{pmatrix}
b_{1}\\
b_{2}\\
\vdots\\
b_{m}
\end{pmatrix}\\
$$

给定的明文首先字母转化为在模 26 下的数，然后每连续 $m$ 个数分为一组，每一组数组成一个长度为 $m$ 的向量 $\vec x$，将其经过如下运算：

$$
\vec x\cdot L+\vec b=\vec y
$$

得到的向量 $\vec y$ 再转化为对应的字母，就是一段属于密文的分组。所有明文分组按顺序运算，得到的密文分组按同样的顺序拼接，就得到了密文。

仿射希尔密码在现代的选择明文、选择密文攻击下是不安全的，只要攻击方：

- 利用统计规律或其他手段得到了分组长度 $m$；
- 获取了足够多的明文密文对；

这些明文密文对实际上就能依据 $m$ 分组得到一系列形如 $\vec x\cdot L+\vec b=\vec y$ 的方程，只要方程足够多（足够好），那么就能解出其中的 $L$ 和 $\vec b$。

接下来的分析在如下**假设**的场景下进行：

- 已知分组长度 $m$；
- 已知明文密文对的长度 $len$ 满足 $len\geq 2m^2$；
  - 换句话说如上两个条件保证了给出刚好 $n=len\div m\geq2m$ 个形如 $\vec x\cdot L+\vec b=\vec y$ 的方程。
- 给出的明文密文对一定是有解的

于是问题转化为已知 $n$ 个方程：

$$
\begin{aligned}
\vec x_{1}\cdot L+\vec b&=\vec y_{1}\\
\vec x_{2}\cdot L+\vec b&=\vec y_{2}\\
&\cdots\\
\vec x_{n}\cdot L+\vec b&=\vec y_{n}
\end{aligned}
$$

求解矩阵 $L$ 和向量 $\vec b$。

也可以写成：

$$
\begin{pmatrix}
x_{11} & x_{12} & \cdots & x_{1m}\\
x_{21} & x_{22} & \cdots & x_{2m}\\
\vdots & \vdots &        & \vdots\\
x_{m1} & x_{m2} & \cdots & x_{mm}\\
\vdots & \vdots &        & \vdots\\
x_{(2m)1} & x_{(2m)2} & \cdots & x_{(2m)m}\\
\vdots & \vdots &        & \vdots\\
x_{n1} & x_{n2} & \cdots & x_{nm}
\end{pmatrix}
\begin{pmatrix}
l_{11} & l_{12} & \cdots & l_{1m}\\
l_{21} & l_{22} & \cdots & l_{2m}\\
\vdots & \vdots &        & \vdots\\
l_{m1} & l_{m2} & \cdots & l_{mm}
\end{pmatrix}
+
\begin{pmatrix}
b_{1} & b_{2} & \cdots & b_{m}\\
b_{1} & b_{2} & \cdots & b_{m}\\
\vdots & \vdots &        & \vdots\\
\vdots & \vdots &        & \vdots\\
\vdots & \vdots &        & \vdots\\
\vdots & \vdots &        & \vdots\\
b_{1} & b_{2} & \cdots & b_{m}
\end{pmatrix}
=
\begin{pmatrix}
y_{11} & y_{12} & \cdots & y_{1m}\\
y_{21} & y_{22} & \cdots & y_{2m}\\
\vdots & \vdots &        & \vdots\\
y_{m1} & y_{m2} & \cdots & y_{mm}\\
\vdots & \vdots &        & \vdots\\
y_{(2m)1} & y_{(2m)2} & \cdots & y_{(2m)m}\\
\vdots & \vdots &        & \vdots\\
y_{n1} & y_{n2} & \cdots & y_{nm}
\end{pmatrix}
$$

### 思路一

这一方法基于常规的**方阵**矩阵方程解法，对于如下形式的矩阵方程：

$$
X\cdot L = Y
$$

最直接的解法就是对 $X$ 求逆：

$$
L=X^{-1}\cdot Y
$$

回到原来的问题，在 $n\geq 2m$ 的背景下，对 $n$ 个方程暴力枚举的抽出 $2m$ 个方程，排列组成两个 $m$ 阶方阵的方程:

$$
X1\cdot L+B=Y1\\
X2\cdot L+B=Y2
$$

做差消掉 $B$，得到齐次方程:

$$
(X1-X2)\cdot L=Y1-Y2
$$

然后检查些向量组成的矩阵 $(X1-X2)$ 是否可逆，若不可逆就继续枚举下一个，可逆就能算出答案:

$$
L=(X1-X2)^{-1}\cdot(Y1-Y2)\\
$$

$$
B = Y1-X1\cdot L
$$

其中 $B$ 是每一行均为 $\vec b$ 的方阵。

在这个算法的基础上，从 $n$ 个方程里面选 $2m$ 个没什么问题，就是 $C_{n}^{2m}$，但是这 $2m$ 个向量 在 $(X1-X2)$ 中的排列分布对 $(X1-x2)$ 是否可逆是有影响的，这是经过尝试验证的。

具体步骤如下：

1. 暴力枚举 $C_{n}^{m}$ 选, $n$ 个向量不排列组成 $X1$。
2. 暴力枚举 $X_{m}^{n-m}$ 排列 $m$ 个向量组成 $X2$。
3. 检查 $(X1-X2)$ 是否可逆。

> 这个做法不太好，比较慢，这一过程还能优化吗？

### 思路二

这一方法基于**非方阵**矩阵方程的最小二乘法解法，对于如下形式的矩阵方程，$X$，$Y$ 不再是方阵：

$$
X\cdot L = Y
$$

解形式为：

$$
L = (X^{T}X)^{-1}X^{T}Y
$$

这里要求 $(X^{T}X)$ 可逆。这就和上一个思路要求 $(X1-X2)$ 可逆很像。

在这种情况下，求解用的矩阵 $X$ 的行数不再有限制，而且当然越多越好，越能保证最终是可逆的。那么干脆就用上所有 $n$ 个方程，就让 $X$ 的行数为 $n$。但是还需要消去 $\vec b$，经过尝试发现消去 $\vec b$ 所用的方程也会影响最终 $(X^{T}X)$ 的可逆性。那么这里也需要暴力枚举各种排列组合了么？

为了比上一个思路更快，尝试在这一步随机选择消去 $\vec b$ 所用的方程，实践发现随机选取得到的 $(X^{T}X)$ 是可逆的情况是稠密的，总是能够很快求解成功。

### 思路三

> 只是想法，并没有实现

依然是基于**非方阵**矩阵方程的求解。

实际上，目前我已知的**非方阵**矩阵方程的求解就只有两种办法:

- 最小二乘法
- 奇异值分解(SVD)(伪逆)

那么采取奇异值分解，或者说伪逆的办法也是可以的。

对于一个任意的矩阵 $X$（大小为 $n \times m$），可以进行奇异值分解：

$$
X = U \Sigma V^T
$$

其中：

- $U$ 是一个 $n \times n$ 的正交矩阵，列向量是 $XX^T$ 的特征向量
- $\Sigma$ 是一个 $n \times m$ 的对角矩阵，包含 $X$ 的奇异值
- $V^T$ 是一个 $m \times m$ 的正交矩阵，，列向量是 $X^TX$ 的特征向量

伪逆 $X^+$ 可以通过 SVD 得到：

$$
X^+ = V \Sigma^+ U^T
$$

其中 $\Sigma^+$ 是将 $\Sigma$ 中的非零奇异值取倒数并转置后得到的矩阵。在模 26 的意义下应当是取逆。

::: primary
这里实际上有一个问题就是是否所有的奇异值都是可逆的
对于如下形式的矩阵方程，$X$，$Y$ 不再是方阵：

$$
X\cdot L = Y
$$

其解形式为：

$$
L = X^+Y
$$

当 $X$ 的秩 $r$ 小于 $\min(m, n)$ 时，$X^+$ 仍然是有效的，但解可能不唯一。在我们的应用中，保证有解的条件下应该是唯一的。

## 代码实现

### Common

#### 矩阵运算

所有的运算均模 26 意义下，所以可以先定义：

```cpp
const int MOD = 26;
```

为了方便和加快求逆，可以直接定义一个数组作为取逆表，使得下标作为输入就能得到结果，若结果为 0 就表示不可逆：

```cpp
const int inversetable[26] = {
    0, 1, 0, 9, 0, 21, 0, 15, 0, 3, 0, 19, 0,
    0, 0, 7, 0, 23, 0, 11, 0, 5, 0, 17, 0, 25
};
```

计算行列式：

```cpp
int determinant(const vector<vector<int>>& matrix) {
    int n = matrix.size();
    if (n ** 1) return matrix[0][0] % MOD;

    int det = 0;
    for (int p = 0; p < n; p++) {
        vector<vector<int>> submatrix(n - 1, vector<int>(n - 1));
        for (int i = 1; i < n; i++) {
            int sub_j = 0;
            for (int j = 0; j < n; j++) {
                if (j ** p) continue;
                submatrix[i - 1][sub_j++] = matrix[i][j];
            }
        }
        det += (p % 2 ** 0 ? 1 : -1) * matrix[0][p] * determinant(submatrix);
    }
    return (det % MOD + MOD) % MOD; // 确保是正数
}
```

计算伴随矩阵：

```cpp
vector<vector<int>> adjugate(const vector<vector<int>>& matrix) {
    int n = matrix.size();
    vector<vector<int>> adj(n, vector<int>(n));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            vector<vector<int>> submatrix(n - 1, vector<int>(n - 1));
            int sub_i = 0;
            for (int row = 0; row < n; row++) {
                if (row ** i) continue;
                int sub_j = 0;
                for (int col = 0; col < n; col++) {
                    if (col ** j) continue;
                    submatrix[sub_i][sub_j++] = matrix[row][col];
                }
                sub_i++;
            }
            adj[j][i] = ((i + j) % 2 ** 0 ? 1 : -1) * determinant(submatrix) % MOD;
            adj[j][i] = (adj[j][i] + MOD) % MOD; // 确保是正数
        }
    }
    return adj;
}
```

以行列式的逆和原矩阵作为输入计算逆矩阵。这里将行列式的逆解耦出来是为了方便先判断方阵是否可逆，再计算逆：

```cpp
vector<vector<int>> inverse(const vector<vector<int>>& matrix, int det_inv) {
    int n = matrix.size();
    vector<vector<int>> adj = adjugate(matrix);
    vector<vector<int>> inv(n, vector<int>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            inv[i][j] = (adj[i][j] * det_inv) % MOD;
            inv[i][j] = (inv[i][j] + MOD) % MOD;
        }
    }
    return inv;
}
```

矩阵乘法：

```cpp
vector<vector<int>> multiply(const vector<vector<int>>& A, const vector<vector<int>>& B) {
    int rowsA = A.size();
    int colsA = A[0].size();
    int colsB = B[0].size();
    vector<vector<int>> C(rowsA, vector<int>(colsB, 0));
    for (int i = 0; i < rowsA; i++) {
        for (int j = 0; j < colsB; j++) {
            for (int k = 0; k < colsA; k++) {
                C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % MOD;
            }
        }
    }
    return C;
}
```

矩阵减法：

```cpp
vector<vector<int>> sub(const vector<vector<int>>& A, const vector<vector<int>>& B) {
    int rowsA = A.size();
    int colsB = B[0].size();
    vector<vector<int>> C(rowsA, vector<int>(colsB, 0));
    for (int i = 0; i < rowsA; i++) {
        for (int j = 0; j < colsB; j++) {
            C[i][j] = (A[i][j] - B[i][j] + MOD) % MOD;
        }
    }
    return C;
}
```

#### 处理输入输出

为了方便观察算法效率，在输出的时候增加输出算法运行时间。同时还希望以文件流进行输入输出方便测试和观察。

假定在线测评平台使用 `-DONLINE_JUDGE` 参数，可以在源代码中这样处理：

```cpp
#ifndef ONLINE_JUDGE
#include <chrono>
#include <iomanip>
FILE *output, *input;
chrono::time_point<std::chrono::high_resolution_clock> start_time, end_time;
#endif

// ......

int main(){
    #ifndef ONLINE_JUDGE
    output = freopen("output", "w", stdout);  // 重定向标准输出流
    input = freopen("input", "r", stdin);     // 重定向标准输入流
    start_time = chrono::high_resolution_clock::now();
    #endif

    //  代码主体

    #ifndef ONLINE_JUDGE
    fclose(input);
    end_time = chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::milliseconds>(end_time - start_time);
    cout << endl << endl
            << "time: "
            << fixed << setprecision(3)
            << duration.count() << " ms";
    fclose(output);
    #endif

    return 0;
}
```

要求输入为 3 行：

- 第一行为数字代表 $m$
- 第二行为明文
- 第三行为密文

明文密文全部都是大写字母，长度 $len$ 相同且满足 $len\geq 2m^2$；

因此可以先将明文，密文分别存到字符串中，后续再依据算法处理。

```cpp
string m_s, s, c;
getline(cin, m_s);
getline(cin, c);
getline(cin, s);

int len = s.size();
int m = stoi(m_s);
```

打印矩阵，从而输出密钥 $L$：

```cpp
void printMatrix(const vector<vector<int>>& A){
    int m = A[0].size();
    for (const auto& row : A) {
        for (int i = 0; i < m - 1; ++i) {
            cout << row[i] << " ";
        }
        cout << row[m - 1] << endl;
    }
}
```

输出 $\vec b$ 需要单独输出一个向量。假定已经通过 $B = Y1-X1\cdot L$ 得到了矩阵 $B$：

```cpp
for (int i = 0; i < m - 1; ++i) {
    cout << B[0][i] << " ";
}
cout << B[0][m - 1];
```

### 思路一

#### 存储向量和矩阵

定义全局变量：

```cpp
vector<vector<int>> x;
vector<vector<int>> y;
vector<vector<int>> X;
vector<vector<int>> Y;
vector<vector<int>> X1;
vector<vector<int>> Y1;
vector<vector<int>> X2;
vector<vector<int>> Y2;
int m = 0, n=0, det_inv = 0;
```

存储的形式是全局变量，这样递归的时候更方便。

这里 `x` 和 `y` 分别存储了 $n$ 个向量，每一对向量分别代表一个 $\vec x\cdot L + \vec b=\vec y$ 中的向量。这是为了排列组合的时候方便选取指定的方程。

这里 `m`, `n`, `det_inv` 也是全局变量，也是为了方便递归。

另外几个矩阵的关系是：

$$
Y=Y1-Y2\\
X=X1-X2
$$

通过如下方式从明文和密文中提取 $n$ 对向量：

```cpp
for(int i=0; i < len; i+=m){
    x.push_back(vector<int>());
    y.push_back(vector<int>());
    for(int j=0; j<m; ++j){
        x[i / m].push_back(c[i + j] - 'A');
        y[i / m].push_back(s[i + j] - 'A');
    }
}
n = x.size();
```

#### 递归

$n$ 个 里面选 $m$ 对向量组合作为 `X1` 和 `Y1`，定义返回值：

- `0` 表示找到了可逆的 $(X1-X2)$，递归结束。
- `1` 表示还没有找到

后面的函数返回值都是与此相同的定义。

```cpp
int combi(int n, int m, int start, vector<int>& combination) {
    if (combination.size() ** (long unsigned int)m) {
        return after_combi(combination);
    }
    for (int i = start; i < n; ++i) {
        combination.push_back(i);
        if(combi(n, m, i + 1, combination) ** 0)return 0;
        combination.pop_back();
    }
    return 1;
}
```

对每一个组合，执行如下函数，存储 `X1` 和 `Y1`，并开始递归寻找剩余 $n-m$ 对向量的 $m$ 排列来组成 `X2` 和 `Y2`

```cpp
int after_combi(vector<int>& combination){
    vector<int> remain(n);
    iota(remain.begin(), remain.end(), 0);
    for(int i=0; i < m; ++i){
        X1[i] = x[combination[i]];
        Y1[i] = y[combination[i]];
        remain.erase(remove(remain.begin(), remain.end(), combination[i]), remain.end());
    }
    vector<bool> used(n, false);
    vector<int> current;
    return permu(remain, current, used, m);
}
```

$n$ 个数 $k$ 排序的递归实现

```cpp
int permu(vector<int>& nums, vector<int>& current, vector<bool>& used, int k) {
    if ((int)current.size() ** k) {
        return after_permu(nums);
    }
    for (size_t i = 0; i < nums.size(); ++i) {
        if (!used[i]) {
            used[i] = true;
            current.push_back(nums[i]);
            if(permu(nums, current, used, m) ** 0)return 0;
            used[i] = false;
            current.pop_back();
        }
    }
    return 1;
}
```

在递归的最后，存储 `X2` 和 `Y2`，判断 $(X1-X2)$ 是否可逆：

- 可逆则可以输出结果并返回 `0` 以中断结束递归
- 否则返回 `0` 继续递归

```cpp
int after_permu(vector<int>& permutation){
    for(int i=0; i < m; ++i){
        X2[i] = x[permutation[i]];
        Y2[i] = y[permutation[i]];
    }
    X = sub(X2, X1);
    det_inv = inversetable[determinant(X)];
    if (det_inv ** 0 ) return 1;
    Y = sub(Y2, Y1);
    vector<vector<int>> L = multiply(inverse(X, det_inv), Y);
    vector<vector<int>> B = sub(Y1, multiply(X1, L));

    // 输出结果......

    return 0;
}
```

在 `main` 函数最后调用 `combi` 开始递归。

### 思路二

#### 存储矩阵

定义如下变量：

```cpp
vector<vector<int>> X = vector<vector<int>>(n, vector<int>(m));
vector<vector<int>> Y = vector<vector<int>>(n, vector<int>(m));
vector<vector<int>> X_T = vector<vector<int>>(m, vector<int>(n));
vector<vector<int>> X1 = vector<vector<int>>(m, vector<int>(m));
vector<vector<int>> Y1 = vector<vector<int>>(m, vector<int>(m));
vector<vector<int>> XTX;
int m = 0, n=0, det_inv = 0;
```

- `X` 和 `Y` 均为 $n\times m$ 的矩阵。
- `X_T`是 $m\times n$ 的矩阵，代表 $X^{T}$。
- `XTX`是 $m\times m$ 的矩阵，代表 $X^{T}X$。
- `X1` 和 `Y1` 仍然为 $m\times m$ 的矩阵，用来计算 $B$。

#### 计算

循环反复地随机生成 `X` 和 `Y`，直到得到的 $X^{T}X$ 可逆为止。

```cpp
// #include <random>

random_device rd;
mt19937 gen(rd());
uniform_int_distribution<> dis(0, n - 1);

while(det_inv ** 0){
    for(int i = 0; i < n; ++i){
        int d = dis(gen);
        for(int j = 0; j < m; ++j){
            X[i][j] = (c[i * m + j] - c[ d * m + j ] + MOD) % MOD;
            Y[i][j] = (s[i * m + j] - s[ d * m + j ] + MOD) % MOD;
            X_T[j][i] = X[i][j];
        }
    }
    XTX = multiply(X_T, X);
    det_inv = inversetable[determinant(XTX)];
}

vector<vector<int>> L = multiply(multiply(inverse(XTX, det_inv), X_T), Y);
vector<vector<int>> B = sub(Y1, multiply(X1, L));

// 输出结果......
```

## 测试

### 随机生成测试数据

借用 SageMath 中的 `HillCryptosystem` 类可以方便地生成随机的希尔密码测试数据，但是 SageMath 不支持仿射希尔密码。

```python
from sage.all import *
import random
import string

m = 5        ## 分组长度 m
n = 12       ## 分组数量 n
A = AlphabeticStrings()
H = HillCryptosystem(A, Integer(m))
K = H.random_key()
e = H(K)
print(K)     ## 输出随机密钥矩阵

c = A(''.join(random.choices(string.ascii_uppercase, k = m * n)))

print(m)     ## 输出一行分组长度 m
print(c)     ## 输出一行随机明文 c
print(e(c))  ## 输出一行对应密文 s
```

python 的 `cryptography` 库也一样，虽然支持希尔密码但是不支持仿射希尔密码。

### 评价

只考虑 $m\leq5$ 的情况，再大的情况可能耗时更长暂不考虑。

> 这个实验作业的输入条件如此

在 $n$ 和 $2m^2$ 的差值小于等于 $1$ 时，思路一和思路二实现的破解速度几乎没有差别。

当 $n$ 和 $2m^2$ 的差值提升时，思路一寻找组合排列的时间复杂度大幅提升，这时候思路二的代码可以比思路一快上万倍，考虑到思路一的实现是递归，实际上函数调用栈较深也会影响性能。

然而，当 $n$ 和 $2m^2$ 的差值提升时，思路一的时间复杂度大幅降低，二者就又变得一样快了，猜测是因为这种情况下更容易找到线性无关的向量对 $\vec x$ 和 $\vec y$，使得所需的矩阵可逆。

思路二的速度几乎不受输入的情况影响，总的来说更稳定效果更好。但是思路一输入条件特别简单的时候速度还是比思路二要好的。
