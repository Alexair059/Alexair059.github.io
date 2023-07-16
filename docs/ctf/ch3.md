---
comments: true
---

# 数论笔记

本人非数学系专业，此笔记仅作为个人记录用

## 整除

$a \mid b$ 表示 $a$ 整除 $b$，$a$ 是 $b$ 的因数，$b$ 是 $a$ 的倍数

### 性质

- 0 是所有非零数的倍数，即 $b\mid 0\ (b\neq 0)$
- 传递性，$a\mid b, b\mid c\Rightarrow a\mid c$

## 质数 | 算数基本定理

### 质数

整数 $p > 1$ 是质数，当且仅当其因子（因数）只有 $\pm 1$ 与 $\pm p$ （通常忽略负数）

### 算术基本定理

任意整数 $a > 1$ 都能**唯一**分解为下面形式

$$
a = p_1^{\alpha_1} \times p_2^{\alpha_2} \times ··· \times p_n^{\alpha_n} \quad (p_1<p_2<···<p_n, \ \alpha_i \in N)
$$

$p_1, p_2, ···, p_n$ 均为质数

## 最大公因数 | 互质 | 欧几里得算法

### 最大公因数

最大公因数即整数 $a$ 和 $b$ 所共有的最大因数（因子），记作 $gcd(a,b)$

### 互质

$$
gcd(a,b)=1 \Longleftrightarrow a,b互质
$$

### 欧几里得算法

求最大公因数常用欧几里得算法（辗转相除法）

对于整数 $a$ 和 $b$

#### 思路上

我们假设 $a > b$，此时被除数为 $a$ ，除数为 $b$ 。循环执行除法操作，每次将除数作为下一次操作的被除数、余数作为下一次操作的除数，当某次操作余数为 0，即整除时，停止循环，取该次操作除数作为最大公因数 $gcd(a,b)$

#### 实际使用中

我们常借助代码或者工具库，例如

```python
def gcd(a, b):
    if b == 0:
        return a
    return gcd(b, a%b)
```

## 模运算

$a$ 除以 $m$ 所得的余数记作 $a\bmod m$

若 $a\bmod m = b\bmod m$，即 $a$ ，$b$ 除以 $m$ 所得的余数相等，我们可以记作

$$
a \equiv b \pmod{m}
$$

> 同余式中的模为`\pmod{m}`，模运算中的模为`\bmod`

### 基本性质

#### 传递性

$$
a\equiv b \pmod{m}, b\equiv c \pmod{m} \Longrightarrow a\equiv c \pmod{m}
$$

#### 基本运算

$$
a\equiv b \pmod{m} \\
c\equiv d \pmod{m} \\
\Downarrow \\
a\pm c\equiv b\pm d \pmod{m} \\
ac\equiv bd \pmod{m}
$$

#### 等量加减法

$$
a\equiv b \pmod{m} \Longrightarrow a \pm c\equiv b\pm c \pmod{m}
$$

#### 等量乘法

$$
a\equiv b \pmod{m} \Longrightarrow ka\equiv kb \pmod{m},\quad \forall k \in Z \\
a\equiv b \pmod{m} \Longrightarrow a^n\equiv b^n \pmod{m}, \quad \forall n \in N \\
a\equiv b \pmod{m} \Longrightarrow P(a)\equiv P(b) \pmod{m},\quad P(x)为任意整系数多项式
$$

#### 放大缩小底数

$$
(km\pm a)^n\equiv (\pm a)^n \pmod{m} \quad (k \in Z, n \in N)
$$

### 剩余系

#### 完全剩余系

比 $n$ 小的非负数集合为 $Z_n=\{0,1,…,(n−1)\}$
，称为模 $n$ 的完全剩余系，记作 $Z_n$

#### 简化剩余系

完全剩余系中与 $n$ 互质的元素的集合，记作$Z_n^*$

## 欧拉函数

欧拉函数 $\varphi(x)$ 指小于等于 $x$ 的正整数中，与 $x$ 互质的数的个数

即 $x$ 的简化剩余系中的元素个数

### 基本性质

- $\varphi(1) = 1$
- 若 $p$ 为质数，$\varphi(p) = p-1$
- 若 $p$ 为质数，$\varphi(p^{\alpha})=p^{\alpha} - p^{\alpha - 1} = p^{\alpha-1}(p-1)$
- 若 $a$ 与 $b$ 互质，$\varphi(ab) = \varphi(a) \times \varphi(b)$

结合算术基本定理，我们有

$$
n = p_1^{k_1}p_2^{k_2}···p_r^{k_r} \\
\Downarrow \\
\varphi(n) = \prod_{i=1}^rp_i^{k_{i-1}}(p_i-1)
$$

## 欧拉定理 | 费马小定理

### 欧拉定理

数论中，若正整数 $n$，$a$ 互质，有

$$
a^{\varphi(n)} \equiv 1 \pmod{n}
$$

### 费马小定理

若 $a$ 为整数，$p$ 为质数，有

$$
a^p \equiv a \pmod{p}
$$

在此基础上若 $a$，$p$ 互质，有

$$
a^{p-1} \equiv 1 \pmod{p}
$$

可以看到符合欧拉定理的情况，因为 $\varphi(p)=p-1$

如果我们做一个变形

$$
a \cdot a^{p-2} \equiv 1 \pmod{p}
$$

我们可以知道 $a^{p-2}$ 即是 $a$ 对于模 $p$ 的模逆元，但注意成立条件

