---
title: 数论入门
tags:
  - 数论
  - 数学
categories:
  - 学习笔记
mathjax: true
date: 2020-08-04 15:02:49
description:
---


前言：本文主要是听了洛谷夏令营的网课后做的一些数论笔记,难度大致在提高左右,想获得最好的观看体验,建议您准备好纸笔,手动推一下公式加深印象。

<!--more-->

## 整除
若 $a = bk$,其中 $a, b, k$ 都是整数,则 $b$ 整除 $a$,记做 $b|a$。

也称 $b$ 是 $a$ 的约数（因数）,$a$ 是 $b$ 的倍数

### 性质：

- $1$ 整除任何数,任何数都整除 $0$

- 若 $a|b, a|c$,则 $a|(b + c), a|(b − c)$ 
(证明：$b = b'a,c = c'a$ 然后就随便弄了)

- 若 $a|b$,则对任意整数 $c$,$a|bc$

- 传递性：若 $a|b, b|c$,则 $a|c$
  
****

## 质数与合数

若大于 $1$ 的正整数 $p$ 仅有两个因子 $1,p$,则称 $p$ 是一个质数（素数）。

否则,若 $p > 1$,则称 $p$ 是一个合数。

**1 不是质数也不是合数**

若 $n$ 是一个合数,则 $n$ 至少有 $1$ 个质因子。因此其中最小的质因子一定不大于 $\sqrt{n}$

质数有无穷多个。不大于 $n$ 的质数约有 $\frac{n} {\ln n}$ 个。

唯一分解定理：把正整数 n 写成质数的乘积

（即 $n = p_1 ^ {c_1} p_2^{c_2}p_3^{c_3} \dots p_k ^ {c_k}$,其中 pi 为质数且单调不减）,

这样的表示是唯一的。

****

## 质因数分解
观察到 $n$ 最多只有 1 个大于 $\sqrt{n}$ 的质因子

直接 $O(\sqrt{n})$ 枚举即可

```cpp
void factor(int x) {
    int tot = 0;
    for (int i = 2;i * i <= x;++i) {
        if (x % i == 0) fac[++tot] = i;
    }
    if (x != 1) fac[++tot] = x;
    return ;
}
```

****

## 带余除法、同余

对于$a,b (a,b \in  \mathbb{Z},b > 0)$ ,存在唯一的 $q,r(q,r \in  \mathbb{Z})$ 

满足 $a = bq + r$,其中 $0 \leq r < b$ 

我们把 $q$ 称为商,$r$ 称为余数

余数可以用 $a \mod b(a \% b)$来表示。

若两数 $a,b$ 除以 $c$ 的余数相等,则称 $a,b$ 模 $c$ 同余,记作 $a \equiv b (\mod c)$

### 性质

$a \equiv b (\mod c)$ 与 $c | (a - b)$ 等价

### 推论

若 $a \equiv b (\mod c),d | c$ 则 $a \equiv b (\mod d)$

****

## 最大公约数(Greatest Common Divisor)

设 $a, b$ 是不都为 0 的整数,$c$ 为满足 $c|a$ 且 $c|b$ 的最大整数,则称 $c$ 是 $a, b$ 的最大公约数

记为 $\gcd(a, b)$ 或 $(a, b)$

类似地可以定义多个数的最大公约数

求 GCD 的一般公式：质因数分解

$$
\begin{aligned}
    &a = p_1^{c_1}p_2^{c_2}\dots p_k^{c_k}\\
    &b = p_1^{d_1}p_2^{d_2}\dots p_k^{d_k}\\
    &(a,b) = p_1^{\min{c_1,d_1}}p_2^{\min{c_2,d_2}} \dots p_k^{\min{c_k,d_k}}
\end{aligned}
$$

### 性质：

$$
\begin{aligned}
    &(a, a) = (0, a) = a \\
    &\text{if } a|b,\text{then }  (a, b) = a \\
    &(a, b) = (a, a + b) = (a, ka + b) \\
    &(ka, kb) = k·(a, b) \\ 
    &(a, b, c) = ((a, b), c) 
\end{aligned}
$$

若 $(a, b) = 1$,则称 $a, b$ 互质（互素）

互质的两个数往往有很好的性质

****

## 欧几里得算法

又称辗转相除法

迭代求两数 gcd 的做法

由 $(a, b) = (a, ka + b)$ 的性质：$(a, b) = (b, a \mod b)$

时间复杂度 $O(\log \max{a,b})$

如何证明？每次递归 $b$ 的范围缩小一半,也就是说要证明 $a \mod b \leq \frac{a}{2}$

分类讨论一手：
$$
\begin{aligned}
    &1.b \leq \frac{a}{2}\\
    &\Rightarrow a \mod b < b \leq \frac{a}{2}\\
    &2.a > b > \frac{a}{2} \\
    &\Rightarrow a \mod b = a - b \leq \frac{a}{2}
\end{aligned}
$$

****

## 裴蜀定理

设 $(a, b) = d$,则对任意整数 $x, y$,有 $d|(ax + by)$ 成立

特别地,一定存在 $x, y$ 满足 $ax + by = d$

等价的表述：不定方程 $ax + by = c (a,b,c \in  \mathbb{Z})$有解的充要条件为 $(a, b)|c$

推论：$a, b$ 互质等价于 $ax + by = 1$ 有解

### 证明

显然有 $d | a,d | b$

根据之前的引理有 $d | ax,d | by$

故 $d | (ax+by)$

对于第二个结论（以下转自[OI Wiki](https://oi-wiki.org/math/bezouts/)）

若任何一个等于 $0$ , 则 $\gcd(a,b)=a$ . 这时定理显然成立。

若 $a,b$ 不等于 $0$ .

由于 $\gcd(a,b)=\gcd(a,-b)$ ,

不妨设 $a,b$ 都大于 $0$ , $a\geq b,\gcd(a,b)=d$ .

对 $ax+by=d$ , 两边同时除以 $d$ , 可得 $a_1x+b_1y=1$ , 其中 $(a_1,b_1)=1$ .

转证 $a_1x+b_1y=1$ .

我们先回顾一下辗转相除法是怎么做的,由 $\gcd(a, b) \rightarrow \gcd(b,a\mod b) \rightarrow ...$ 我们把模出来的数据叫做 $r$ 于是,有

$$ \gcd(a_1,b_1)=\gcd(b_1,r_1)=\gcd(r_1,r_2)=\cdots=(r_{n-1},r_n)=1 $$

把辗转相除法中的运算展开,做成带余数的除法,得

$$ 
\begin{aligned}
    a_1 &= q_1b+r_1 &(0\leq r_1<b_1) \\
    b_1 &= q_2r_1+r_2 &(0\leq r_2<r_1) \\ 
    r_1 &= q_3r_2+r_3 &(0\leq r_3<r_2) \\ 
    &\cdots \\
    r_{n-3} &= q_{n-1}r_{n-2}+r_{n-1} \\
    r_{n-2} &= q_nr_{n-1}+r_n \\
    r_{n-1} &= q_{n+1}r_n
\end{aligned} 
$$

不妨令辗转相除法在除到互质的时候退出则 $r_n=1$ 所以有（ $q$ 被换成了 $x$ ,为了符合最终形式）

$$ r_{n-2}=x_nr_{n-1}+1 $$

即

$$ 1=r_{n-2}-x_nr_{n-1} $$

由倒数第三个式子 $r_{n-1}=r_{n-3}-x_{n-1}r_{n-2}$ 代入上式,得

$$ 1=(1+x_nx_{n-1})r_{n-2}-x_nr_{n-3} $$

然后用同样的办法用它上面的等式逐个地消去 $r_{n-2},\cdots,r_1$ ,

可证得 $1=a_1x+b_1y$ . 这样等于是一般式中 $d=1$ 的情况。

****

## Exgcd

由裴蜀定理可得,方程 $ax + by = \gcd(a,b)$ 必有一组解。

所以我们考虑如何求出这样的方程的一组解。

### 证明：
设 
$$
\begin{aligned}
    &ax_1 + by_1 = \gcd(a,b) \\
    &bx_2 + (a \mod b)y_2 = \gcd(b,a \mod b) 
\end{aligned}
$$

则
$$
\begin{aligned}
    &\because \gcd(a,b) = \gcd(b,a \mod b)\\
    &\therefore ax_1 + by_1 = bx_2 + (a \mod b)y_2 \\
    &\because (a \mod b ) = a - (\lfloor \frac{a}{b} \rfloor \times b)\\
    &\therefore ax_1 + by_1 = bx_2 + (a-\lfloor \frac{a}{b} \rfloor \times b) y_2 \\
\end{aligned}
$$

展开得
$$
\begin{aligned}
    &ax_1 + by_1 = ay_2 + bx_2 - \lfloor \frac{a}{b} \rfloor by_2 = &ay_2 + b(x_2 - \lfloor \frac{a}{b} \rfloor y_2)\\
    &\because a = a,b = b \\
    &\therefore x_1 = y_2 , y_1 = x_2 - \lfloor \frac{a}{b} \rfloor y_2
\end{aligned}
$$

所以就可以递归求解了。

### 代码
```cpp
void exgcd(int a,int b,int &x,int &y) {
    if (!b)  {
        x = 1,y = 0;
        return ;
    }
    int q = a / b;
    exgcd(b,a%b,y,x);
    x = y;
    y = y - q * x;
}
```

### 拓展

考虑形如 $ax + by = c (a,b,c \in  \mathbb{Z})$ 的方程的所有解

首先由裴蜀定理得,不定方程 $ax + by = c (a,b,c \in  \mathbb{Z})$有解的充要条件为 $(a, b)|c$ 。

先考虑求一组特殊解的情况：首先将 $a,b,c$ 都除以 $\gcd(a,b)$ 得 $a_0x + b_0y = c_0$

此时 $\gcd(a_0,b_0) = 1$,所以我们可以求出 $a_0x + b_0y = 1$ 的一组解 $x_0,y_0$ ,则原方程的一组特解为

$$x_1 = \frac{x_0c}{\gcd(a,b)},y_1 = \frac{y_0c}{\gcd (a,b)}$$

然后我们考虑构造所有解的形式：

设有 $d (d \in \mathbb{Q})$ 那么必有 $a(x_1 + db) + b(y_1 - da) = c$

同时,这组解需要保证 $db,da \in  \mathbb{Z}$

令当 $d$ 取到最小可能的正值的 $d_x = db,d_y = da$ ,那么任意解中这两个变量与 $x_1,x_2$ 的偏差一定分别是 $d_x,d_y$ 的倍数。

显然,最小的时候 $d_x = \frac{b}{\gcd (a,b)},d_y = \frac{-a}{\gcd (a,b)}$

所有解就是 $x = x_0 + kd_x,y = y_0 + kd_y (k \in  \mathbb{Z})$

****
## 逆元
若 $ax \equiv 1 (\mod b)$,则称 $x$ 是 $a$ 关于模 $b$ 的逆元,

常记做 $a^{-1}$。

回忆同余的性质。上式等价于 $ax + by = 1$ 

如何求逆元？等价于解方程 $ax + by = 1$

因此逆元不一定存在：存在的充要条件为 $\gcd (a, b) = 1$

推论：$p$ 是质数,$p$ 不整除 $a$,则 $a$ 模 $p$ 的逆元存在。

结论：在 $[0,b)$ 的范围内,若存在 $a$ 关于 $b$ 的逆元,则它是唯一的。

证明：反证法,若存在 2 个 $a$ 关于 $b$ 的逆元,设其为 $x_1,x_2$,不妨假设 $0 < x_1 < x_2 < b$ 

也就是说 $ax_1 \equiv ax_2 \equiv 1 (\mod b)$

可以得出 $b | a(x_2 - x_1)$ ,由于 $\gcd(a,b) = 1$,所以 $b | (x_2 - x_1)$

也就是有 $0 \leq (x_2 - x_1) < b$ , 所以不成立。

### 求逆元

我们刚才说求逆元等价于解方程 $ax + by = 1$,所以直接 `exgcd` 求解。

```cpp
int inv(int a,int b) {
    int x,y;
    exgcd(a,b,x,y);
    return x;
}
```

这样求单个逆元的复杂度是 $O(\log n)$。

如何 $O(n)$ 求 $1\dots n$ 模质数 $p$ 的逆元？

- 方法一：递推

    假设现在要求 $i$ 的逆元  

    考虑带余除法,设 $p = iq + r$,则有 $iq + r \equiv 0(\mod p)$

    注意到 $p$ 是质数,因此 $r \not = {0}$,$r$ 的逆元存在

    等式两边乘 $i^{−1}r^{−1}$,得到 $qr^{−1} + i^{−1} \equiv 0(\mod p)$

    因此 $i−1 \equiv −qr^{−1} \equiv -\frac{p}{i}(p \mod i)^{−1}(\mod p)$

    注意这里如果直接算 $-\frac{p}{i}$ 会产生负数,然后 $p -\frac{p}{i} \equiv -\frac{p}{i} (\mod p)$,所以这么算就不会出问题

    代码

    ```cpp
    int inv[N];

    void prework(int n) {
        inv[1] = 1;
        for (int i = 2;i <= n;++i) {
            inv[i] = (p - p / i) * inv[p % i] % p;
        }
    }
    ```
- 方法二：倒推

例题：组合数取模 1

> 回答 $T$ 次询问
> 
> 每次询问 $C(n, k) \mod 998244353$(一个质数)
>
> $T ≤ 10^5$, $0 \leq k \leq n \leq 10^7$


分析：$C(n, k) = n!/(k!(n − k)!)$

线性求逆,预处理 $n!$ 以及 $n! ^ {-1}$ 

$O(1)$ 回答询问

****

## 线性同余方程

形如 $ax \equiv c(\mod b)$ 的方程,称为线性同余方程。

等价于 $ax + by = c$ 因此有解条件为 $\gcd(a, b)|c$

若 $\gcd(a, b) = 1$,则 $x$ 有唯一解 $x ≡ a^{-1}c(\mod b)$。

否则设 $(a, b) = d, a = a^{\prime}d, b = b^{\prime}d, c = c'd$

那么有 $a'x + b'y = c'$,即 $a'x \equiv c'(\mod b')$

这里 $(a', b') = 1$,因此有 $x ≡ (a')^{−1}c'(\mod b')$

综上,任意的线性同余方程总可以判定为无解,或化为 $x \equiv a(\mod m)$ 的形式。

****

## 中国剩余定理

对于同余方程组 $x \equiv a_i(\mod m_i)(i = 1\dots n)$,

若 $m_i$ 两两互质,则 $x$ 在 $\mod M$ 下有唯一解。这里 $M = m_1m_2\dots m_n$

### 计算方法

1. 计算所有模数的积 $M$
2. 对于第 $i$ 个方程
   1. 计算 $n_i = \frac{M}{m_i}$
   2. 因为 $\gcd(n_i,m_i) = 1$,所以 $n_i$ 关于 $m_i$ 的逆元 $t_i$ 存在,求出 $t_i$
   3. 计算 $c_i = n_i \times t_i$ 
3. 方程组的唯一解为 $a = \sum_{i = 1}^k a_ic_i (\mod n)$

### 代码

```cpp
int CRT(const int a[],const int m[],int n) {
    int M = 1,ans = 0;
    for (int i = 1;i <= n;++i) M *= m[i];
    for (int i = 1;i <= n;++i) {
        int ni = M / m[i],ti = inv(ni,m[i]);
        ans = (ans + a[i] *  ni * ti) % M;
    }
    return ans;
}
```

### 应用

某些计数问题或数论问题给出的模数不是质数 

但是对其质因数分解会发现它没有平方因子,也就是该模数是由一些不重复的质数相乘得到。

那么我们可以分别对这些模数进行计算,最后用 CRT 合并答案。

**** 

## Lucas 定理

$$ \binom{n}{m}\bmod p = \binom{\left\lfloor n/p \right\rfloor}{\left\lfloor m/p\right\rfloor}\cdot\binom{n\bmod p}{m\bmod p}\bmod p $$

****

## 欧拉函数 $\varphi$

欧拉函数 $\varphi$ (Euler’s totient function)

$\varphi (n)$ 定义为 $[1, n]$ 中与 $n$ 互质的数的个数

例：$\varphi (1) = 1, \varphi (2) = 1, \varphi (6) = 2, \varphi (8) = 4$

若 $p$ 为质数,显然 $\varphi (p) = p - 1$

### 性质

-  欧拉函数是**积性函数**

    若 $\gcd (a,b) = 1$, $\varphi(ab) = \varphi(a) \times \varphi(b)$

    证明：

    首先令 $S(n)$ 为 $[1,n]$ 中与 $n$ 互质的数的集合

    任取 $S(a),S(b)$ 中的数 $a_0,b_0$

    考虑一个线性同余方程组

    $$
    \begin{cases}
        \ x \equiv a_0 (\mod a)\\
        \ x \equiv b_0 (\mod b)
    \end{cases}
    $$

    则根据中国剩余定理有 $x \equiv c_0(\mod ab)$ 

    可以证明 $\gcd(c_0,a_0) = \gcd(c_0,b_0) = 1$

    所以 $\gcd(c_0,ab) = 1,c_0 \in S(ab)$

    如果反过来,任取 $S(ab)$ 中一个数 $c_0$,那么设 $a_0 = c_0 \mod a,b_0 = c_0 \mod b$

    这样可以证明有 $a_0 \in S(a),b_0 \in S(b)$

    因此,$|S(ab)| = |S(a)| \times |S(b)|$ 

    也就是 $\varphi(ab) = \varphi(a) \times \varphi(b)$

- 若 $n = p ^ k$,则有 $\varphi(n) = n(1-\frac{1}{p})$

    证明：设 $x$ 为满足 $\gcd(x,p^k) > 1$ 的整数,此时 $p | x$

    则这样的 $x$ 有$\frac{n}{p}$ 个,所以 $\varphi(n) = n - \frac{n}{p} = n(1-\frac{1}{p})$

- 若 $n$ 所有**不同**的质因子为 $p_1,p_2 \dots p_n$

    则 $\varphi(n) = n(1 - \frac{1}{p_1}) \times (1 - \frac{1}{p_2}) \dots (1 - \frac{1}{p_n})$

    证明：把 $n$ 拆成 $p_i ^{a_i}$ 用上面那个就整完了。

根据这些性质,我们可以通过质因子分解求出 $\varphi(n)$,时间复杂度 $O(\sqrt{n})$

### 代码

```cpp
int phi(int x) {
    int ans = x;
    for (int i = 2;i * i <= x;++i) {
        if (x % i == 0) {
            while (x % i == 0) x *= i;
            ret = ret / i * (i - 1);//这里等价于刚才的1 - 1/p
        }
    }
    if (x > 1) ret = ret / x * (x - 1);
    return ret;
}
```

****

## 欧拉定理

若 $\gcd(a,n) = 1$, 则 $a ^ {\varphi(n)} \equiv 1(\mod n)$

### 证明

取 $\varphi(n)$ 个与 $n$ 互质且互不同余的整数构成一个集合 $S = \{a_1,a_2\dots a_{\varphi(n)}\}$ 又称模 $n$ 的简化剩余系。

考虑集合 $S' = {pa_1,pa_2,\dots,pa_{\varphi(n)}}$, 显然 $S'$ 也是一个模 $n$ 的简化剩余系

这样如果把每个元素都对 $n$ 取模,则有 $S = S'$

所以可以得 $a_1,a_2\dots a_{\varphi(n)} \equiv pa_1,pa_2,\dots,pa_{\varphi(n)}(\mod n)$

将 $a_1,a_2\dots a_{\varphi(n)}$ 约掉就有 $p ^ {\varphi} \equiv 1 (\mod n)$

### 应用

#### 利用欧拉定理求逆元

$a \times a^{\varphi(n)-1}\equiv 1(\mod n)$ 

如果是质数,则有 $a \times a^{n-2} \equiv 1(\mod n)$

快速幂实现。

#### 快速幂缩小指数

例：求 $7 ^ {222} (\mod 10)$

解：

$$
\begin{aligned}
    &\because \gcd(7,10) = 1\\
    &\therefore 7^{\varphi(10)} = 7 ^ 5\equiv 1 (\mod 10)\\
    &\therefore 7^{222} \equiv 7 ^ {222 \mod 5} = 49 \equiv 9 (\mod 10)
\end{aligned}
$$

当 $\gcd(a,n) > 1$ 时,我们有 $a^{\varphi(p)} \equiv a^{2\varphi(p)} (\mod n)$

推论：当 $b \geq \varphi(n)$ 时,$a ^ b \equiv a ^ {b \mod \varphi(n) + \varphi(n)} (\mod n)$