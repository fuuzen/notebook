---
title: DLP
date: 2024/12/19
math: true
categories:
  - [数学, 密码学]
tags:
  - 密码学
---

## 大数运算

大数运算实现先前的实验报告已经有过，不再赘述。

但是在 Pollard-Rho 中有大数的模 3 运算，在先前的大数库中没有实现。这里再在 `Field::Int` 类中增加模 3 运算的方法：

```cpp
int mod3() const {
  __uint128_t res = 0;
  for(int i = F.N - 1;i >= 0; i--){
    res = (res * (static_cast<__uint128_t>(1) << 64) + a[i]) % 3;
  }
  return static_cast<int> (res);
}
```

## 大数字符串输入输出处理

本次实验不像先前的实验都是二进制输入输出，而是十进制字符串输入输出。

对于输出，由于输出的离散对数是有限域 $n$ 以内的，而 $n$ 在本次实验限定为 64 位以内，所以可以直接使用 C++ 本身自带的 64 位数输出。

对于输入，包含了大数，我在自己的大数库已经有实现，为了更高效的性能，我使用查表的方式实现。我的大数库最高支持 2048 bits 的大数，使用 `uint64_t[32]` 存储，对应到十进制是 618 位，所以建立的 LookUpTable 可以以 618 个十进制位、1 到 9 这 9 个十进制数作为索引，查询结果就是对应的大数的二进制表示。建表如下：

```cpp
auto gen_TABLE(){
  array<array<array<uint64_t, 32>, 9>, 617> table = {0};
  uint64_t a[32] = {0};
  uint64_t a1[32] = {0};
  a[0] = 1;
  for(int i = 0; i < 617; ++i){
    for(int j = 0; j < 32; ++j){
      a1[j] = a[j];
    }
    int t = i ** 616 ? 3 : 9;
    for(int j = 0; j < t; ++j){
      uint64_t carry = 0;
      for (int k = 0; k < 32; ++k) {
        table[i][j][k] = a[k];
        uint64_t sum = a[k] + a1[k] + carry;
        if (sum < a[k] || sum < a1[k]) {
          carry = 1;
        } else {
          carry = 0;
        }
        a[k] = sum;
      }
    }
  }
  return table;
}
```

这里我之所以使用 `array<array<array<uint64_t, 32>, 9>, 617> ` 存表，是因为我原本希望在编译时 `constexpr` 建表，提升运行效率，但是在 OJ 系统上会导致超出编译内存。

```cpp
/*
not in constexpr, bcz adding it will exceed memory limit on OJ system.
*/
const auto TABLE = gen_TABLE();
```

输入就通过查表实现，根据输入的每一个十进制位查刚刚建立的表，查询结果加到最终结果之中：

```cpp
// return the sum of used limbs
inline int string2limbs(const string& s, uint64_t * a){
  int l = s.length(), k = 1;
  bool flag = l > 19;
  a[0] = stoull(
    s.substr(
      flag ? l - 19 : 0,
      flag ? 19 : l
    )
  );
  for (int i = 19; i < l; ++i) {
    int j = s[l - i - 1] - '0';
    if(j ** 0) continue;
    uint64_t sum, carry = 0;
    k = 0;
    j--;
    while(TABLE[i][j][k] ** 0)k++;
    while(k < 32 && (TABLE[i][j][k] != 0 || carry != 0)){
      sum = a[k] + TABLE[i][j][k] + carry;
      if (sum < a[k] + carry || sum < TABLE[i][j][k] + carry) {
        carry = 1;
      } else {
        carry = 0;
      }
      a[k] = sum;
      k++;
    }
  }
  return k;
}
```

我的大数库采用面向对象的设计，所以输入也封装到了友元 `>>` 之中：

```cpp
// 输入有限域（p）
inline friend std::istream& operator>>(std::istream& is, Field& f) {
  string s;
  is >> s;
  f.N = string2limbs(s, p);
  initField(f.p, f.p_2, f.R2modp, f.g, f.N);  // 有限域预计算处理
  return is;
}

// 输入整数
inline friend std::istream& operator>>(std::istream& is, Int& a) {
  string s;
  is >> s;
  string2limbs(s, a.a);
  return is;
}
```

## Pollard-Rho

Pollard-Rho 的算法我也采用了面向对象的设计，将一个 DLP 问题的条件作为成员变量封装在里面：

- $p$ 定义在 `Field f`，同时也是整个 Pollard-Rho 算法大数运算时所基于的有限域。
- `uint64_t n` 即为 $n$，因为输入限制在 64 位以内，所以不必使用大数。
- `Int alpha = Int(f)` 即为 $\alpha$，属于有限域 $p$。
- `Int beta = Int(f)` 即为 $\beta$，属于有限域 $p$。

```cpp
class PollardRho {
public:
  PollardRho(){};

private:
  void func(Int const & x, __uint128_t const & a, __uint128_t const & b, Int & x_, __uint128_t & a_, __uint128_t & b_);

  void func(Int & x, __uint128_t & a, __uint128_t & b);

  __uint128_t gcd(__uint128_t a, __uint128_t b);  // 返回 a,b 的 gcd

  __uint128_t mod_inverse(__uint128_t a, __uint128_t p);  // 返回 a^(-1) mod p

  // 值为 1 的大数
  string _One = string("1");
  const Int One = Int(f, _One);

public:
  Field f;
  Int alpha = Int(f);
  Int beta = Int(f);
  uint64_t n;

  uint64_t solve();
}
```

具体的算法流程如下：

$$
\begin{aligned}
&(x1,a1,b1)\gets f(1,0,0)\\
&(x2,a2,b2)\gets f(x1,a1,b1)\\
&\bold{\text{while}}\ x1\neq x2\\
&\ \ \ \ (x1,a1,b1)\gets f(x1,a1,b1)\\
&\ \ \ \ (x2,a2,b2)\gets f(x2,a2,b2)\\
&\ \ \ \ (x2,a2,b2)\gets f(x2,a2,b2)\\
&\text{解线性同余方程:}\\
&\ \ \ \ (b2-b1)x=a1-a2\ \ \ \ (\bmod n)
\end{aligned}
$$

其中 $f$ 的计算如下：

$$
f(x,a,b)=
\begin{cases}
(\beta x ,& a,& (b+1)\bmod n &), && x\bmod3=1\\
(x^2 ,& 2a ,& 2b\bmod n &), && x\bmod3=0\\
(\alpha x ,& (a+1)\bmod n ,& b &), && x\bmod3=2
\end{cases}
$$

这里我对输入和输出是否为为同一变量分别作了实现避免 C++ 未定义行为（我真的怕极了，不想在遇到 undefined behavior 了）：

```cpp
void func(Int const & x, __uint128_t const & a, __uint128_t const & b, Int & x_, __uint128_t & a_, __uint128_t & b_){
    switch(x.mod3()){
        case 1: {
            x_ = beta * x;
            a_ = a;
            b_ = (b + 1) % n;
        } break;
        case 0: {
            x_ = x * x;
            a_ = (a * 2) % n;
            b_ = (b * 2) % n;
        } break;
        default: {  // 2
            x_ = alpha * x;
            a_ = (a + 1) % n;
            b_ = b;
        }
    }
}

void func(Int & x, __uint128_t & a, __uint128_t & b){
    switch(x.mod3()){
        case 1: {
            x = beta * x;
            a = a;
            b = (b + 1) % n;
        } break;
        case 0: {
            x = x * x;
            a = (a * 2) % n;
            b = (b * 2) % n;
        } break;
        default: {  // 2
            x = alpha * x;
            a = (a + 1) % n;
            b = b;
        }
    }
}
```

然后求最终 $b1,b2,a1,a2$ 的过程如下：

```cpp
uint64_t solve(){
    Int x1(f), x2(f);
    __uint128_t a1, b1, a2, b2;
    func(One, 0, 0, x1, a1, b1);
    func(x1, a1, b1, x2, a2, b2);
    while(x1 != x2){
        func(x1, a1, b1);
        func(x2, a2, b2);
        func(x2, a2, b2);
    }
    // 解线性同余方程...
}
```

最后就是求解线性同余方程了，主要参考 [OIWiki](https://oi-wiki.org/math/number-theory/linear-equation/) 中用逆元求解的思路，这里我的方程如下：

$$
(b2-b1)x=a1-a2\ \ \ \ (\bmod n)
$$

当然这里我先要保证 $b2-b1$ 和 $a1-a2$ 都是模 $n$ 下的正数，上述 Pollard-Rho 得到的结果并不保证 $b2>b1$ 和 $a1>a2$，所以稍微处理一下：

```cpp
while(b2 < b1) b2 += static_cast<__uint128_t>(n);
while(a1 < a2) a1 += static_cast<__uint128_t>(n);
```

接下来先求 $g=gcd(b1-b2,n)$，通过如下函数实现：

```cpp
__uint128_t gcd(__uint128_t a, __uint128_t b){
    while (b != 0) {
        __uint128_t temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

当且仅当 $(b2-b1)\bmod g\neq0$ 时是无解的，反之则可以在 $(b2-b1)x=a1-a2$ 两侧同时除以 $g$，得到新方程：

$$
b'x'=a'\ \ \ \ (\bmod n')\ \ \ \ \begin{cases}
b'=(b2-b1)\div g\\
a'=(a1-a2)\div g\\
n'=n\div g\\
\end{cases}
$$

就可以通过求逆得到一个基础解 $x'=a'\cdot b'^{-1}\bmod n$。求逆代码实现如下：

```cpp
__uint128_t mod_inverse(__uint128_t a, __uint128_t p) {
    __uint128_t u = a, v = p, x1 = 1, x2 = 0, q, r, x;
    while(u != 1){
        q = v / u;
        r = v - q * u;
        while(x2 < q * x1) x2 += p;
        x = x2 - q * x1;
        v = u;
        u = r;
        x2 = x1;
        x1 = x;
    }
    return x1 % p;
}
```

但这个解不一定是 DLP 的解，因为线性同余方程有 $g=gcd(b1-b2,n)$ 个解，只有一个才是我们想要的，那就只好尝试 $g$ 次试出最终解了，这 $g$ 个解通过 $x'$ 构造如下：

$$
x_i=(x'+i\cdot n')\bmod n\ ,\ \ \ 0\leq i\lt g
$$

最终求解原方程代码如下：

```cpp
if ((b2 - b1) % g != 0){
    // 无解
} else {
    __uint128_t n_ = n / g;
    __uint128_t a_ = (a1 - a2) / g;
    __uint128_t b_ = (b2 - b1) / g;
    __uint128_t b_inv = mod_inverse(b_, n_);
    __uint128_t x_ = (a_ * b_inv) % n_;
    Int t(f);
    for(uint64_t i = 0; i < g; ++i){
        uint64_t tmp = static_cast<uint64_t>(x_);
        t.a[0] = tmp;
        if((alpha ^ t) ** beta){
            return tmp;
        }
        x_ = (x_ + n_) % n;
    }
}
return 0xFFFF'FFFF'FFFF'FFFF;  // 无解，我返回一个特殊的数
```

## 我学号的 DLP

相关代码进行计时，输入输出：

```cpp
#ifndef ONLINE_JUDGE
#include <chrono>
#include <iomanip>
FILE *output, *input;
chrono::time_point<chrono::high_resolution_clock> start_time, end_time;
#endif

int main(){
  cin.tie(0);
  ios::sync_with_stdio(false);
  cout.tie(0);

  #ifndef ONLINE_JUDGE
  output = freopen("output", "w", stdout);
  input = freopen("input", "r", stdin);
  start_time = chrono::high_resolution_clock::now();
  #endif

  PollardRho solution;
  string s;
  cin >> solution.f >> solution.n >> solution.alpha >> solution.beta;
  cout << solution.solve();
  cout.flush();

  #ifndef ONLINE_JUDGE
  output = freopen("time", "w", stdout);
  end_time = chrono::high_resolution_clock::now();
  auto ns = chrono::duration_cast<chrono::nanoseconds>(end_time - start_time);
  auto us = chrono::duration_cast<chrono::microseconds>(end_time - start_time);
  auto ms = chrono::duration_cast<chrono::milliseconds>(end_time - start_time);
  cout << ns.count() << " ns" << endl
  fclose(input);
  fclose(output);
  #endif
}
```

我的学号 $22342043$ 所确定的 DLP 问题：

$$
\begin{aligned}
p&=3768901521908407201157691198029711972876087647970824596533\\
(&p−1=2×2×23×8783×2419781956425763×192888768642311611×9993115456385501509)\\
n&=9993115456385501509\\
\alpha&=3107382411142271813235322646657672922264748410711464860476^{2×2×23×8783×2419781956425763×192888768642311611×22342043}\\
&=1672705575588793411750559287020484713881877693514202546508\\
\beta&=2120553873612439845419858696451540936395844505496867133711
\end{aligned}
$$

输入文件 `input` 内容如下：

```raw
3768901521908407201157691198029711972876087647970824596533
9993115456385501509
1672705575588793411750559287020484713881877693514202546508
2120553873612439845419858696451540936395844505496867133711
```

我在 Linux 虚拟机环境下计算，配置：

- CPU：Intel(R) Xeon(R) CPU E7-4830 v3 @ 2.10GHz 总共 8 核
- 内存： 16.00 GiB
- OS：Ubuntu 22.04

(fish) 开始计算：

```shell
nohup bash ./my_program > output.log 2>&1 & set pid $last_pid; echo $pid > pid
```

计算结果：

$$
4823944491583471581
$$

计算时间：
