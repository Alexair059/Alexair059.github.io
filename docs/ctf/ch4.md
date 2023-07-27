---
comments: true
---

# RSA

RSA 是一种非对称加密算法，在公开密钥加密和电子商业中被广泛使用，主要用于信息加密以及签名消息

## 来源

RSA是由罗纳德·李维斯特（Ron Rivest）、阿迪·萨莫尔（Adi Shamir）和伦纳德·阿德曼（Leonard Adleman）在1977年一起提出。当时他们三人都在麻省理工学院工作，RSA 就是他们三人姓氏开头字母拼在一起组成的

1973年，在英国政府通讯总部工作的数学家克利福德·柯克斯（Clifford Cocks）在一个内部文件中提出了一个与之等效的算法，但该算法被列入机密，直到1997年才得到公开

## 设计 | 已知的攻击情况

对极大整数做因数分解的难度决定了 RSA 算法的可靠性。换言之，对一极大整数做因数分解愈困难，RSA 算法愈可靠。假如有人找到一种快速因数分解的算法的话，那么用 RSA 加密的信息的可靠性就会极度下降。但找到这样的算法的可能性是非常小的。目前只有短的 RSA 密钥才可能被强力方式破解

到2020年为止，世界上还没有任何可靠的攻击RSA算法的方式。wiki 上已知的攻击情况为 RSA-768（768bits，232digits）数被成功分解[^1]

目前典型的 RSA 密钥长度为2048位

[^1]:https://eprint.iacr.org/2010/006.pdf

## 基本原理

### 公钥与私钥的产生

1. 选取两个大质数 $p$ 和 $q$，计算 $N = p \times q$
2. 根据欧拉函数，计算 $\varphi(N)=\varphi(p)\varphi(q)=(p-1)(q-1)$
3. 选取一个小于 $\varphi(N)$ 的整数 $e$，使 $e$ 和 $\varphi(N)$ 互质。求得 $e$ 关于模数 $\varphi(N)$ 的模逆元。令其为 $d$，有 $ed\equiv 1 \pmod{\varphi(N)}$
4. 将 $p$，$q$ 的记录销毁

此时，$(N,e)$ 是公钥，$(N,d)$ 是私钥

### 信息加密

首先需要将消息以一个双方约定好的格式转化为一个小于 $N$ 的整数 $m$。如果消息太长，可以将消息分为几段，即块加密，后对于每一部分进行如下操作

$$
m^{e}\equiv c\pmod N
$$

得到密文 $c$

### 信息解密

得到密文 $c$，可利用密钥 $d​$ 进行解密

$$
c^{d}\equiv m\pmod N
$$

### 私钥解密证明

证明用私钥 $d$ 解密，一定能正确得到 $m$，即

$$
c^d\equiv m \pmod{N}
$$

首先由 RSA 原理与同余性质

$$
m^e \equiv c \pmod{N} \Rightarrow m^{ed} \equiv c^d \pmod{N}
$$

再由传递性，等同于需要证

$$
c^d\equiv m \pmod{N} \Rightarrow m^{ed} \equiv m \pmod{N}
$$

又因为

$$
ed \equiv 1 \pmod{\varphi(N)} \Rightarrow ed=h\varphi(N)+1
$$

因此有如下等式

$$
\begin{aligned}
m^{ed} &\equiv m \pmod{N} \\
&\Updownarrow \\
m^{h\varphi(N)+1} &\equiv m \pmod{N} \\
&\Updownarrow \\
m^{h(p-1)(q-1)+1} &\equiv m \pmod{N}
\end{aligned}
$$

此时分为两种情况

**$m$ 与 $N$ 互质**

由欧拉定理有

$$
m^{\varphi(N)} \equiv 1 \pmod{N}
$$

再由同余性质可得

$$
m^{h\varphi(N)+1} \equiv m \pmod{N}
$$

**$m$ 与 $N$ 不互质**

由于 $N=p\times q$，故 $m$ 必然为 $kp$ 或者 $kq$

以 $m = kp$ 为例（$p$ $q$ 对称，只考虑一种），此时 $m = kp$ 与 $q$ 必然互质，由欧拉定理

$$
(kp)^{q-1}\equiv 1\pmod{q}
$$

由同余性质有

$$
(kp)^{h(p-1)(q-1)+1}\equiv kp\pmod{q}
$$

即

$$
(kp)^{ed} \equiv kp\pmod{q} \Rightarrow (kp)^{ed} = tq + kp
$$

此时等式左边一定为 $p$ 的倍数，故右边的 $tq$ 也一定为 $p$ 的倍数，又因为 $p$ 与 $q$ 互质，故 $t$ 一定为 $p$ 的倍数，即 $t = t'p$

又因为 $N = p\times q$，$m = kp$，所以有

$$
\begin{aligned}
(kp)^{ed} &= t'pq+kp \\
&\Updownarrow \\
(kp)^{ed} &\equiv kp \pmod{N} \\
&\Updownarrow \\
m^{ed} &\equiv m\pmod{N}
\end{aligned}
$$

## 常用工具

### 在线工具

- 整数分解（100 digits 左右）：[factor.db](http://factordb.com/)
- 求模逆元：任意搜索网站
- 质数查询：[haomeili.net](https://www.haomeili.net/ZhiShu)

### Python库

- gmpy2

