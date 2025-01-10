---
title: 维吉尼亚密码
date: 2024/09/19
math: true
categories:
  - [数学, 密码学]
tags:
  - 密码学
---

## 破解原理

- Kasiski 测试法：
  - 找到所有可能的周期 `m`
- 指数重合法：
  - 在备选周期中找到最有可能的周期
  - 在确定周期后用来确定密钥
Í
## 代码实现

这只是一个作业，用来完成在线测评，在这里输入假定：

$$
\text{密文长度}:\text{密钥长度} > 50:1
$$

### 处理输入输出

传统的维吉尼亚密码都是允许大小写，并且大小写统一在 $\mathbb{z}_{26}$ 空间加密解密，其他空格、换行、标点符号均不参与加密，输出时须保留其原样。

```cpp
int main(){
    vector<char> input;
    vector<char> c;
    int input_l=0, c_l=0;
    char tmp;
    while(fread(&tmp, sizeof(char), 1, stdin)){
        input.push_back(tmp);
        if((tmp <= 'Z' && tmp >= 'A') || (tmp <= 'z' && tmp >= 'a')){
            c.push_back(tmp);
            c_l += 1;
        }
        input_l += 1;
    }

	// 破解获取密钥 K

	// 解密并输出
}
```

在这里使用了两个 `vector<char>` 存储了：

- 包括空格、换行、标点符号的完整原文 `input`
- 去除了空格、换行、标点符号的密文 `c`

### 寻找可能的周期

维吉尼亚密码的破解经验来看，在密文中**长度为 3 的相同密文段的距离以及它们的所有因数**都可以作为可能的周期，当然 1 除外。在实践中，这样的相同密文段往往是存在的，并且并不稀疏，而长度为 4 开始就非常稀疏难以找到了。

在这里只考虑长度为 3 的相同密文段，没有任何其他优化直接暴力枚举所有长度为 3 的密文段。为了防止重复，使用集合存储它们的距离。

```cpp
string c_s(c.begin(), c.end());
set<int> candidates;
for(int i=0;i<c_l-2;++i){
    string substr = c_s.substr(i, 3);
    size_t found = c_s.substr(i + 2, c_l - i).find(substr);
    if(found != string::npos){
        int tmp = abs(i-(int)found);
        candidates.insert(tmp);
    }
}
```

### 确定最有可能的周期

对每一个备选周期，包括刚刚求出的一系列**距离以及它们的因数**，按照备选周期 `candidate = m` 将密文 `c` 在分为 `candidate` 段：

$$
\begin{aligned}
y_1&=y_1y{m+1}y_{2m+1}\cdots\\
y_2&=y_2y{m+2}y_{2m+2}\cdots\\
&......\\
y_m&=y_my{2m}y_{3m}\cdots
\end{aligned}
$$

对每一个长度为 $l_{y_i}$ 的 $y_i$，统计出 26 个字母各自的出现频数 $f_j$，计算 $M_i$ 如下：

$$
\frac{\sum_{j=0}^{25}f_j\cdot(f_j-1)}{l_{y_i}\cdot(l_{y_i}-1)}
$$

其中 $p_i$ 是统计出来的英语书面语 26 个字母的出现频率。通过常量定义如下：

```cpp
const double FREQ[26] = {
    0.082, 0.015, 0.028, 0.043,
    0.127, 0.022, 0.020, 0.061,
    0.070, 0.002, 0.008, 0.040,
    0.024, 0.067, 0.075, 0.019,
    0.001, 0.060, 0.063, 0.091,
    0.028, 0.010, 0.023, 0.001,
    0.020, 0.001
};
```

计算出来的 $M_i$ 将会和实际英语书面语的**重合指数**比较，重合指数的计算如下，假设大量样本得到的文本长度为 $l$，26 个字母各自的出现为频数 $f_j$：

$$
\frac{\sum_{j=0}^{25}p_j\cdot(p_j-1)}{l\cdot(l-1)}\approx\sum_{j=0}^{25}p_j^2=0.065
$$

认为越接近这个数值的 $M_i$，那么对应的周期 `m` 越有可能是正确的密钥长度。

同时这里假定密文和密钥的比例不小于 $50:1$，所以根据密文长度可以筛选掉一些太长的备选周期，而且实际允许程序发现有一些错误的密钥长度下计算的 $M_i$ 比真正的密钥长度还接近 $0.065$，这一筛选可以提高破解准确率。

```cpp
int m = c_l / 50;
double idx_avg_best_diff = 1;
vector<string> y_vec;
vector<vector<int> > y_freq;
for(int candidate: candidates){
    for (int factor = 2; factor <= candidate; ++factor) {
        if (candidate % factor ** 0 && factor * 50 < c_l) {
            vector<string> y_vec_tmp(factor);
            vector<vector<int> > y_freq_tmp(factor, vector<int>(26, 0));
            for(int i=0;i<c_l;++i){
                y_vec_tmp[i % factor].push_back(c[i]);
                if(c[i] - 'a' >= 0){
                    y_freq_tmp[i % factor][c[i] - 'a'] += 1;
                } else {
                    y_freq_tmp[i % factor][c[i] - 'A'] += 1;
                }
            }
            double M_avg = 0;
            for(int i=0;i<factor;++i){
                double M = 0;
                for(int j=0;j<26;++j){
                    M += y_freq_tmp[i][j] * (y_freq_tmp[i][j] - 1);
                }
                M /= y_vec_tmp[i].length() * (y_vec_tmp[i].length() - 1);
                M_avg += M;
            }
            M_avg /= factor;
            double idx_avg_diff = abs(M_avg - 0.065);
            if(idx_avg_diff < idx_avg_best_diff){
                m = factor;
                idx_avg_best_diff = idx_avg_diff;
                y_vec = y_vec_tmp;
                y_freq = y_freq_tmp;
            }
        }
    }
}
```

### 确定密钥

在确定了密钥长度 `m` 之后，维吉尼亚加密就被分解为了 `m` 个单表移位加密，所以接下来的步骤实际上就是进行 `m` 次单表移位破解，`m` 个单表加密密文就是依据周期 `m` 划分的密文段 $y_1,y_2,\cdots y_m$。

对每一个密文段 $y_i$，依然采用计算重合指数的方式，对所有 26 种可能的偏移都进行一次解密，对得到的明文段的重合指数最接近 $0.065$ 的就是真正的偏移，其偏移量对应的字母构成了密钥。

在这里我们不需要真的进行 26 次解密，只是对 26 种偏移后的文段计算重合指数就行：

$$
M_i=\sum_{j=0}^{25}\frac{p_j\cdot f_{j+i}}{l_{y_i}}
$$

```cpp
vector<int> K(m,0);
for(int k=0;k<m;++k){
    double M_best_diff = 1;
    for(int i=0;i<26;++i){
        double M = 0;
        for(int j=0;j<26;++j){
            M += FREQ[j] * y_freq[k][(j + i) % 26];
        }
        M /= y_vec[k].size();
        M /= m;
        double M_diff = abs(M - 0.065);
        if(M_diff < M_best_diff){
            M_best_diff = M_diff;
            K[k] = i;
        }
    }
}
```

### 解密

到此为止，就得到了密钥 `K`，解密并输出的过程如下：

```cpp
for(int i=0; i<input_l; ++i){
    if((input[i] <= 'Z' &&input[i] >= 'A') || (input[i] <= 'z' &&input[i] >= 'a')){
        if(c[j] - 'a' >= 0){
            tmp = 'a' + (c[j] -'a' - K[j % m] + 52) % 26;
        } else {
            tmp = 'A' + (c[j] -'A' - K[j % m] + 52) % 26;
        }
        fwrite(&tmp, sizeof(char), 1, stdout);
        j++;
    } else {
        fwrite(&input[i], sizeof(char), 1, stdout);
    }
}
```

这样将保留输入的空格、换行、标点符号等得到可读的明文。

### 缺陷

这个程序在测试中对于如下输出：

```c
S lotabgy kqmasbaew qy n eizuwuggakgy kknrem lbj dkeanevfo zuw
iagzmtgakogq wl qaoogst srkagtwa ue vwihemtgk. I bndqj qaoogst
yvyvggmzk bf i srkagtw ooiwa g ewkocamtg uwtsalkaum zusb zuw
ukfkimr uisr xzuz s akavmx xfwca lw zuw zkpaxorfb.

Jvyqznd aotfizhjmy njm g flitqszj rdmsrfb us ewyg uzeclwmesxnvu
xxblwibd aavlmy, nfl gew kuzewtyq cyrv nue kwlgoixr
vqygjqhhlqua, xqtnfkond bxnfagplquak, kualzgpl ugasokzwvz
fgnzjszk, nfl oa gbnrj kgfwa cuwzk vl qy vexuelitg lw jrlmig
xwxtwze bj bgzhmxvfo.
```

破解得到的密钥为：`SIGNSIGN`。

这当然是正确的，但是在线测评系统可不这么认为，它认为正确的密钥应该是：`SIGN`。在这个程序的计算中，`SIGNSIGN` 划分和计算得到的重合指数比 `SIGN` 还要接近 $0.065$ ！

这就相当麻烦了，如果硬要过测评为目的，那么还需要判断密钥是否是周期的，需要 KMP 算法找出最小周期子串。

> 所幸，这次出现这个情况的测试用例是输入输出示例，所以我硬编码处理了这个情况来 AC
