---
title: 解题笔记——德国联邦数竞2025年第二轮第四题
description:
date: 2026-06-17 00:00:00+0000
categories:
    - Math
tags:
    - 解题笔记
    - BWM
    - BWM2025
math: true
toc: true
---

{{< auto-number >}}

在我做过的所有德国数学国赛（Bundeswettbewerb Mathematik，简称 BWM，直译为联邦数学竞赛）题目中，2025 年第二轮的第四题无疑是最难也是最有意思的一道。
虽然在解题的过程中数次有过放弃的念头——因为必须承认这种难度的题目我确实无法在一两天内攻克，但在真正找到方向后逐步抽丝剥茧，得出与题干等价但简单许多的另一种表述方式后，
所得到的成就感也是在曾经的竞赛经历中从未有过的。

本篇解题笔记主要回顾本人的完整思路，整理全部解题过程并结合标答修补原证明中的部分漏洞。因此解题过程中提到的部分定理将不给出具体证明，部分简单证明则为方便排版默认折叠。

## 题干

德语题干原文如下：

> Eine ganze Zahl $d > 1$ heiße steinig, wenn man d Steine auf die Felder eines $d \times d$-Schachbretts so legen kann,
dass sich auf keinem Feld mehr als ein Stein befindet und auf dem Feld in Zeile $x$ und Spalte $y$ genau dann ein Stein liegt,
wenn die ganze Zahl $x^3 - y^2$ durch $d$ teilbar ist ($1 \le x, y \le d$). 
>
> Bestimme die kleinste und die größte Anzahl an steinigen Zahlen, die unter zehn aufeinanderfolgenden 
positiven ganzen Zahlen vorkommen können. 

中文译文（非直译）如下：

> 有整数 $d > 1$ 和一个 $d \times d$ 的棋盘。棋盘的每个方格内至多放一枚石子，且仅当第 $x$ 行第 $y$ 列的方格满足 $x^3 - y^2$ 能被 $d$ 整除（其中 $1 \le x, y \le d$）时，
才会在该方格内放一枚石子。若能恰将 $d$ 枚石子放在这个棋盘内，则称该整数为“石性数”。
>
> 试求在任意十个连续正整数中，可能出现的“石性数”的最小个数与最大个数。

## 初步思路

题目中对“石性数”的定义显然是很难直接用数学方法处理的。因此我们需要对这个定义进行简化，写出其等价的代数表达形式。

### 题干的等价代数表达

利用集合可以很好的描述那些应该放石头的格子，也就是那些满足条件的 $(x, y)$ 对。

我们定义集合 $S_d$，有：

$$
S_d = \left\{ (x,y) \;|\; 1 \leq x,y \leq d, \; d \;|\; (x^3-y^2) \right\}
$$

既然 $x^3 - y^2$ 能被 $d$ 整除，则易见 $x^3$ 与 $y^2$ 除以 $d$ 同余。因此集合定义可简化为：

$$
S_d = \left\{ (x,y) \;|\; 1 \leq x,y \leq d, \; x^3 \equiv y^2 \Mod{d} \right\}
$$

显然当 $|S_d| = d$ 时，整数 $d$ 是“石性数”。

### 确定满足条件的 $(x, y)$ 对的数量

在将题干转换为等价代数表达的过程中我们不难发现，原本贴近于生活表达的题干已经被抽象成了一个同余问题。这不难让人想到中国剩余定理。

中国剩余定理的一个简单表达是：

> 有任意两两互质的整数 $m_1, m_2, \cdots, m_n$。则对任意整数 $r_1, r_2, \cdots, r_n$，以下一元同余方程组 $(S)$：
> $$
(S): \begin{cases}
    x \bmod m_1 = r_1 \\
    x \bmod m_2 = r_2 \\
    \quad\vdots \\
    x \bmod m_n = r_n
\end{cases}
$$
> 在模 $M = m_1m_2\cdots m_n$ 的剩余系中有唯一解。

[$$ 解决vscode代码高亮失灵问题]: #

#### 用余数对替代 $(x, y)$ 对

依照中国剩余定理的思路，我们设 $d$ 为两互质的正整数 $m$ 和 $n$ 的乘积。由于任何正整数皆可被质因数分解且不同质数之间显然互质，因此对任意 $d$ 总能找到对应的 $m$，$n$。

则显然有：

{{< span "eq-1" >}}
$$
x^3 \equiv y^2 \Mod{d} \Longleftrightarrow \begin{cases}
    x^3 \equiv y^2 \Mod{m} \\
    x^3 \equiv y^2 \Mod{n}
\end{cases} \tag{1}
$$

不妨将 $x$ 与 $y$ 模 $m$ 的结果构造为余数对 $(x_m, y_m)$。不难得到推论：

> 若 $x_m^3 \equiv y_m^2 \Mod{m}$ 成立，则所有对应的 $x$，$y$ 皆满足 $x^3 \equiv y^2 \Mod{m}$。

<details>
<summary>证明如下：</summary>

$$
\begin{align*}
    \because\quad        & x \equiv x_m \Mod{m}, \; y \equiv y_m \Mod{m} \\
    \therefore\quad      & x = x_m+km, \; y = y_m+lm \; (k,l \in \mathbb{Z}) \\
                         &
    \begin{aligned}
                           x^3 - y^2 &= (x_m+km)^3 - (y_m+lm)^2 \\
                                     &= x_m^3 + 3x_m^2 \cdot km + 3x_m \cdot k^2m^2 + k^3m^3 - y_m^2 - 2y_m \cdot lm - l^2m^2 \\
                                     &= x_m^3 - y_m^2 + m \cdot (\underbrace{3x_m^2 \cdot k + 3x_m \cdot k^2m + k^3m^2 - 2y_m \cdot l - l^2m}_{\in \Z}) = A
    \end{aligned} \\
    \because\quad        & x_m^3 \equiv y_m^2 \Mod{m} \\
    \therefore\quad      & x^3 - y^2 \equiv A \equiv x_m^3 - y_m^2 \equiv 0 \Mod{m} \\
    \Longrightarrow\quad & x^3 \equiv y^2 \Mod{m}
\end{align*}
$$
</details>

通过这种方式，我们不再需要关注所有 $(x, y)$ 对，只需在模 $m$ 的剩余系中选出满足条件的余数对 $(x_m, y_m)$ 即可。这大大削减了我们需要判定的值对的数量。

由于显然有 $0 \leq x_m, y_m \leq m$，我们将这些余数对归入集合 $S_m$ 中。对 $n$ 进行同样的操作，可以得到集合 $S_n$。

#### 根据集合 $S_m$ 与 $S_n$ 计算集合 $S_d$ 的基数

分别从集合 $S_m$，$S_n$ 中取出余数对 $(x_m, y_m)$，$(x_n, y_n)$。

由中国剩余定理推导易知，对任意正整数，若其对两个互质的正整数 $m$ 和 $n$ 取模的余数确定，则其对 $m \cdot n$ 取模的余数也唯一确定。
这说明我们可以由余数对$(x_m, y_m)$ 和 $(x_n, y_n)$ 得到一个唯一的余数对 $(x_d, y_d)$。这个余数对代表了 $x$ 和 $y$ 对 $m \cdot n$（即 $d$）取模的结果。

由[式 $(1)$](#eq-1) 和 $x$，$y$ 的取值范围易见，此处的余数对 $(x_d, y_d)$ 实际上就是符合条件的 $(x, y)$ 对。上文也提到这样得到的对是唯一的。

这意味着，集合 $S_m$ 中的所有余数对可以一一和集合 $S_n$ 中的余数对配对得到 $(x, y)$ 对，且这些值对不会重复。因此有：

{{< span "eq-2" >}}
$$
|S_d| = |S_m| \cdot |S_n| \; (m,n \leq d, \; \gcd(m,n)=1) \tag{2}
$$

## 哪些数是“石性数”？{#Steinig_Nummer_Bedingung}

本节将讨论“石性数” $d$ 需要满足的必要条件。

### 所有质数都是“石性数” {#Primzahl_sind_steinig}

为方便理解，本小节中均用 $p$ 代替 $d$，以标明 $d$ 是一个质数。

首先讨论唯一的偶质数 2，易见其是石性数。因此在本小节的剩余部分我们只需要讨论奇质数的情况。

我们首先假定一个前提：$x^3 \equiv y^2 \equiv 0 \Mod{p}$。

由于 $p$ 是质数，所以显然只有一个 $(x, y)$ 对能满足这个条件，即 $x = y = p$。
所以接下来，我们只需继续讨论 $x^3$ 与 $y^2$ 模 $p$ 不为 0 的情况。

假设 $x^3 \equiv y^2 \equiv a \Mod{p}$，则易知 $a \in \\{ 1, 2, \cdots, p-1 \\}$。
因此我们的任务可以被简化为寻找那些使 $x^3 \equiv a \Mod{p}$ 和 $y^2 \equiv a \Mod{p}$ 同时有解的 $a$。

我们引入一个引理（证明见{{< ref-section "Beweis_Lamma_1" >}}）：

> 引理一：对于质数 $p$ 和非零余数 $a$，若高次同余方程 $x^n \equiv a \Mod{p}$ 有解，则它在模 $p$ 的剩余系中恰有 $\gcd(n, p-1)$ 个不同的解。

因此：

* 对于 $x^3 \equiv a \Mod{p}$，我们可知每个使得 $x$ 有解的 $a$ 都能提供 $\gcd(3, p-1)$ 个解。
* 对于 $y^2 \equiv a \Mod{p}$，我们可知每个使得 $y$ 有解的 $a$ 都能提供 $\gcd(2, p-1)$ 个解。
  由于我们正在讨论奇质数，所以 $p-1$ 定为偶数，由此可知 $\gcd(2, p-1) = 2$。

我们将 $X(a)$ 定义为满足 $x^3 \equiv a \Mod{p}$ 的 $x$ 的个数，$Y(a)$ 定义为满足 $y^2 \equiv a \Mod{p}$ 的 $y$ 的个数。
易见对给定余数 $a$，对应的满足同余条件的 $(x, y)$ 对有 $X(a) \cdot Y(a)$ 个。

由此可得：

$$
|S_p| = \sum^{p-1}_{a=0} X(a) \cdot Y(a)
$$

当 $a$ 为 0 时，上文已经提到仅有一组解 $(p, p)$。由此可知 $X(0) \cdot Y(0) = 1$。

而对于所有 $a > 0$ 的情况，要使得该 $a$ 能够贡献解对，则显然需要满足 $X(a) > 0, Y(a) > 0$。
这意味着 $a$ 必须同时是模 $p$ 的二次剩余和三次剩余，由此可知 $a$ 必须是模 $p$ 的六次剩余，即一定存在正整数 $u$ 使得 $u^6 \equiv a \Mod{p}$ 成立。

<details>
<summary>证明如下：</summary>

由原根存在性定理可知，模 $p$ 的简化剩余系构成一个乘法循环群 $(\Z/p\Z)^\times$。

设原根为 $g$，则有 $a \equiv g^k \Mod{p}$。已知余数 $a$ 既是二次剩余也是三次剩余，故指数 $k$ 必须同时满足 $\Divisiable{k}{2}$ 和 $\Divisiable{k}{3}$。
由于 2 与 3 互质，所以必有 $\Divisiable{k}{6}$，即 $a$ 也必定是六次剩余。得证。
</details>

由引理一易证，在模 $p$ 的剩余系中存在 $\frac{p-1}{\gcd(6, p-1)}$ 个六次剩余。而由于当 $X(a)$ 和 $Y(a)$ 均不为 0 时它们的值确定，则易得：

$$
|S_p| = \frac{p-1}{\gcd(6, p-1)} \cdot 2\gcd(3, p-1) + 1
$$

由于上文提到此处我们仅讨论奇质数，所以 $\Divisiable{p-1}{2}$ 恒成立。因此有：

$$
\begin{align*}
    |S_p| &= \frac{p-1}{\gcd(6, p-1)} \cdot \gcd(6, p-1) + 1 \\
          &= p - 1 + 1 \\
          &= p
\end{align*}
$$

综合本小节一开始对偶质数 2 的讨论，得到结论：

> 所有质数都是“石性数”。

### 任意质数的多次幂都不是“石性数” {#Primzahlpotenz_sind_nicht_steinig}

在本节中我们将讨论所有具有 $p^e$ 形式的合数，即任意质数的多次幂。其中 $e > 1$。

#### 情况：$x$，$y$ 均能被 $p^e$ 整除

这种情况要求有 $x^3 \equiv y^2 \equiv 0 \Mod{p^e}$。

我们引入一个引理（证明见{{< ref-section "Beweis_Lamma_2" >}}）：

> 引理二：有正整数 $x$，$n$ 和一质数的多次幂 $p^e$。若 $x^n$ 能被 $p^e$ 整除，则 $x$ 能被 $p^{\lceil e/n \rceil}$ 整除。

由此得到推论：

$$
x^3 \equiv y^2 \equiv 0 \Mod{p^e} \Longrightarrow \begin{cases}
    \Divisiable{x}{p^{\lceil e/3 \rceil}} \\
    \Divisiable{y}{p^{\lceil e/2 \rceil}}
\end{cases}
$$

易得：

$$
\begin{align*}
    \begin{cases}
        x = k \cdot p^{\lceil e/3 \rceil}, &k \in \left\{1,2,\dots,p^{e-\lceil e/3 \rceil}\right\} \\
        y = l \cdot p^{\lceil e/2 \rceil}, &l \in \left\{1,2,\dots,p^{e-\lceil e/2 \rceil}\right\}
    \end{cases}
\end{align*}
$$

可见 $k$ 和 $l$ 的可能取值分别有 $p^{e-\lceil e/3 \rceil}$ 和 $p^{e-\lceil e/2 \rceil}$ 个，从而能构造出相同数量的 $x$ 和 $y$。
由于 $x$ 和 $y$ 可以两两配对，所以在本小节条件下贡献的满足题目条件的 $(x, y)$ 对的数量 $N\'$ 为：

$$
\begin{align*}
    N' &= p^{e-\lceil e/3 \rceil} \cdot p^{e-\lceil e/2 \rceil} \\
       &= p^{2e-\lceil e/3 \rceil - \lceil e/2 \rceil}
\end{align*}
$$

易见：

$$
N' \geq p^e
$$

<details>
<summary>证明如下：</summary>

需证明：$p^{2e-\lceil e/3 \rceil - \lceil e/2 \rceil} \geq p^e$。

显然命题可以被等价转换为：

$$
\left\lceil \frac{e}{3} \right\rceil + \left\lceil \frac{e}{2} \right\rceil \leq e \; (e > 1)
$$

设 $e = 6k + r$，其中 $k$，$r$ 均为整数且 $k \geq 0$，$0 \leq r \leq 5$。

则有：

$$
\frac{e}{3} = \frac{6k+r}{3} = 2k + \frac{r}{3} \Longrightarrow \left\lceil \frac{e}{3} \right\rceil = 2k + \left\lceil \frac{r}{3} \right\rceil
$$

且：

$$
\frac{e}{2} = \frac{6k+r}{2} = 3k + \frac{r}{2} \Longrightarrow \left\lceil \frac{e}{2} \right\rceil = 3k + \left\lceil \frac{r}{2} \right\rceil
$$

由此可推出：

$$
\begin{align*}
    \left\lceil \frac{e}{3} \right\rceil + \left\lceil \frac{e}{2} \right\rceil &= 2k + \left\lceil \frac{r}{3} \right\rceil + 3k + \left\lceil \frac{r}{2} \right\rceil \\
                                                                                &= 5k + \left\lceil \frac{r}{3} \right\rceil + \left\lceil \frac{r}{2} \right\rceil
\end{align*}
$$

由于我们规定了 $r$ 的取值范围，我们可以用表格列出所有情况：

|                   $r$                     |   0   |   1   |   2   |   3   |   4   |   5   |
| :--------------------------------------:  | :---: | :---: | :---: | :---: | :---: | :---: |
|            $\lceil r/3 \rceil$            |   0   |   1   |   1   |   1   |   2   |   2   |
|            $\lceil r/2 \rceil$            |   0   |   1   |   1   |   2   |   2   |   3   |
| $\lceil r/3 \rceil$ + $\lceil r/2 \rceil$ |   0   |   2   |   2   |   3   |   4   |   5   |

观察到除了 $r$ 为 1 的情况外均有 $\lceil r/3 \rceil$ + $\lceil r/2 \rceil = r$。将该条件代回，得到：

$$
\left\lceil \frac{e}{3} \right\rceil + \left\lceil \frac{e}{2} \right\rceil =
\begin{cases}
    5k+r+1 &\If r=1 \\
    5k+r   &\If r \neq 1
\end{cases}
$$

易见当且仅当 $k = 0, r = 1$ 时有：

$$
\left\lceil \frac{e}{3} \right\rceil + \left\lceil \frac{e}{2} \right\rceil = 5k+r+1 > 6k+r = e
$$

但由此得到的 $e$ 的值为 1，这与我们的条件相悖。由此推导后易得证。
</details>

#### 情况：$x$，$y$ 不能被 $p^e$ 整除

据我观察，没有一个简单的公式能够用来计算 $x^3 \equiv y^2 \not\equiv 0 \Mod{p^e}$ 这种情况贡献的满足题目条件的 $(x, y)$ 对的数量 $N\'\'$。
但这其实无关紧要，因为我们需要讨论的仅是全部解对的数量与 $p^e$ 之间的大小关系。

易见在本小节条件下，对任意 $p^e$ 都有一对满足题目条件的 $(x, y)$ 对 $(1, 1)$。因此我们得到：

$$
N'' > 0
$$

#### 小结

综上所述，易得：

$$
|S_{p^e}| = N' + N'' > p^e
$$

由此我们得到结论：

> 任意质数的多次幂都不是“石性数”。

### 合数成为“石性数”的必要条件

我们知道任何正整数 $d$ 均有其质因数分解形式：

$$
d = \prod \limits_{i=1}^{\omega(d)} p_i^{e_i} \; (d > 1)
$$

其中 $\omega(d)$ 代表 $d$ 所有不同质因数的个数。

综合[式 $(2)$](#eq-2) 易得：

$$
|S_d| = \prod \limits_{i=1}^{\omega(d)} \left|S_{p_i^{e_i}}\right|
$$

结合{{< ref-section "Primzahl_sind_steinig" >}}和{{< ref-section "Primzahlpotenz_sind_nicht_steinig" >}}不难看出对连乘中的每一项均有：

$$
\left|S_{p_i^{e_i}}\right| \geq p_i^{e_i}
$$

因此可知若要使题目条件 $|S_d| = d$ 成立，则在 $d$ 的质因数分解式中的每一个 $p_i^{e_i}$ 均需满足 $e_i = 1$。
由此不难得到结论：

> 当合数 $d$ 的质因数分解式中的每一项的指数都为 1 时，则该数是“石性数”。

## 原题的简化等价表达和解答

综合{{< ref-section "Steinig_Nummer_Bedingung" >}}中的所有内容我们很容易就能得到原题的另一种更加简单的等价表达方式：

> 在十个大于 1 的连续正整数中，满足“其质因数分解式中每个质因数的指数均为 1”这一条件的正整数至多和至少有多少个？

这样整个解题思路也彻底清晰了。我们不再需要处理任何复杂的同余问题，只需要关注质因数指数这一相当简单的概念即可。

### “石性数”的最大数量

我们希望在连续十个正整数中，出现尽可能多满足“质因数分解后每个质因数的指数均为 1”这一条件的数。我们可以观察较小的质数（如 2、3）及其幂次（如平方、立方）在连续十个正整数中至少出现的频率。

我们知道：要使某个数的倍数必然出现在连续十个正整数中，该数最大只能为 10，这样其倍数的周期才最多为 10。因此，周期至多为 10 的质数幂有：

$$
2^2=4, \; 2^3=8, \; 3^2=9
$$

此外，还有以下较小的质数幂：

$$
\begin{align*}
    &2^4=16, \; 2^5=32, \; 2^6=64 \\
    &3^3=27, \; 3^4=81 \\
    &5^2=25 \\
    &7^2=49
\end{align*}
$$

由于我们希望这十个数中出现尽可能多的“石性数”，因此必须避免上述质数幂出现在我们所选的区间内。同时，我们可以让 4 的倍数与 9 的倍数重叠，这样就能在该区间内减少一个非“石性数”（即节省出一个位置）。
理论上，这能使连续十个正整数中非“石性数”的数量降至两个，从而最多可能有八个数为“石性数”。

因此，我们可以将 4 与 9 的最小公倍数（即 36）作为锚点来扩展区间，并检验我们的理论是否成立。

在 36 的左侧，我们希望避开 $3^3=27$ 与 $2^2 \cdot 7=28$。因此，我们选择 29 作为区间的左边界；相应的 38 即为右边界。我们现在逐个验证这些数字：

$$
\begin{align*}
    \begin{aligned}
        29 &= 29^1                    \qquad&& 34 = 2^1 \cdot 17^1 \\
        30 &= 2^1 \cdot 3^1 \cdot 5^1 \qquad&& 35 = 5^1 \cdot 7^1 \\
        31 &= 31^1                    \qquad&& 36 = 2^2 \cdot 3^2 \\
        32 &= 2^5                     \qquad&& 37 = 37^1 \\
        33 &= 3^1 \cdot 11^1          \qquad&& 38 = 2^1 \cdot 19^1 \\
    \end{aligned}
\end{align*}
$$

我们发现，仅有 32 和 36 的质因数分解中含有指数大于 1 的质因数；其余八个数均满足石性数的条件。这确实印证了上文推导出的结论。
同时上文也解释了这已是连续十个正整数中可能存在的“石性数”的数量上限，所以我们可以得到结论：

> 连续十个大于 1 的正整数中至多有八个“石性数”。

### “石性数”的最小数量

我们可以采用类似的方法，尝试构造一个连续正整数序列，使得其中的每一个数都能被某个质数的平方整除。

建立如下的线性同余方程组：

$$
\begin{cases}
    x \equiv -1 \Mod{p_1^2} \\
    x \equiv -2 \Mod{p_2^2} \\
    \quad \vdots \\
    x \equiv -10 \Mod{p_{10}^2}
\end{cases}
\; (p_i \in \mathbb{P})
$$

由于任意两不同质数的平方必然互质，所以根据中国剩余定理，该线性同余方程组必有解。

因此可得：

$$
\begin{cases}
    \,\Divisiable{(x+1)}{p_1^2} \\
    \,\Divisiable{(x+2)}{p_2^2} \\
    \,\quad \vdots \\
    \,\Divisiable{(x+10)}{p_{10}^2}
\end{cases}
$$

这意味着：在区间 $[x+1, x+10]$ 内的任何一个整数都不是石性数。也就是说：

> 连续十个大于 1 的正整数中最少有零个“石性数”。

## 总结

总的来说，本题的主要难点在于将原题转化为易于处理的等价表达的过程。不难看出，在真的得出那个等价命题后，整道题的难度便断崖式下降，但在那之前的过程对数学思维能力确实有一定的要求。

从原题写出第一种等价代数表达并不困难，但如何从这个代数表达中看出应该从质数下手呢？我显然是没有那个能力立即联想到（笑）。
这也就是为什么需要数学软件辅助，通过列出海量数据来观察，从而得到一个可能可行的思路去推进。
在本题中也就是先注意到所有质数都是“石性数”，得到那一部分证明后，再逐步推导得到最终判定某一正整数是否满足条件的方法。

这其实也是本人认为 BWM 这类竞赛最有益处的地方：不像国内的数学竞赛要求在给定时间内解决多么复杂（但实际上有一定套路）的题目，BWM 更注重于考察参赛者的独立数学研究能力。
这从其开卷，一轮仅有四题且给足两三个月的时间便能看出。这在我看来其实远比国内所谓的“解题能力”更加重要，因为它是在真的培养一个人对数学的兴趣，而不只是功利地为了一纸奖状而如机器一般刷题。

## 补充证明

### 引理一 {#Beweis_Lamma_1}

我们需证：

> 对于质数 $p$ 和非零余数 $a$，若高次同余方程 $x^n \equiv a \Mod{p}$ 有解，则它在模 $p$ 的剩余系中恰有 $\gcd(n, p-1)$ 个不同的解。

由原根存在性定理可知模质数 $p$ 的（简化）剩余系构成一个乘法循环群 $(\Z/p\Z)^\times$。

设循环群的原根为 $g$。我们令：

{{< span "eq-3" >}}
$$
x \equiv g^k, a \equiv g^l \Mod{p} \tag{3}
$$

因此在当 $x^n \equiv a \Mod{p}$ 有解时可以得到：

$$
\left( g^k \right)^n = g^{nk} \equiv g^l \Mod{p}
$$

由循环群性质可知上式成立的充要条件是它们的指数在模群的阶下同余。即有：

{{< span "eq-4" >}}
$$
nk \equiv l \Mod{p-1} \tag{4}
$$

不难得出：

$$
nk - (p-1) \cdot t = l \; (t \in \Z)
$$

由于线性丢番图方程 $ax + by = c$ 有解的充分必要条件是 $\Divisiable{c}{\gcd(a, b)}$。同时由命题条件已知该方程有解，因此得出：

$$
\Divisiable{l}{\gcd(n, p-1)}
$$

令 $d=\gcd(n,p-1)$。那么我们可以将 $n$、$p-1$ 和 $l$ 分别替换为：

$$
n=dn', \; p-1=dm', \; l=dl'
$$

代入[式 $(4)$](#eq-4) 中可得：

$$
\begin{align*}
                    dn'k &\equiv dl' \Mod{dm'} \\
\Longrightarrow\quad n'k &\equiv l' \Mod{m'}
\end{align*}
$$

由于 $\gcd(n\',m\') = 1$，$n\'$ 在模 $m\'$ 意义下存在乘法逆元，我们将其记作 $(n\')^{-1}$。从而得到在模 $m\'$ 意义下的唯一解 $k_0$：

$$
k_0 \equiv l' \cdot (n')^{-1} \Mod{m'}
$$

由此可得解 $k$ 的通式：

$$
k = k_0 + tm' \; (t \in \mathbb{Z})
$$

容易看出，在模 $dm\'$ 的情况下，$t$ 可以取 $0,1,\dots,d-1$ 这些值，从而为 $k$ 提供不同的解。
由于易知 $k_0<m\'$ 成立，因此这样得到的 $k$ 并不会超过 $dm\'$。

将该结果代入[式 $(3)$](#eq-3) 中，由于 $k$ 没有超过长度为 $p-1$ 的周期，所以所有 $g^k \bmod p$ 的值均两两不同。
因此，$x$ 的解的个数等于 $t$ 的可能取值的个数，即 $d$，亦即 $\gcd(n,p-1)$。得证。

### 引理二 {#Beweis_Lamma_2}

我们需证：

> 有正整数 $x$，$n$ 和一质数的多次幂 $p^e$。若 $x^n$ 能被 $p^e$ 整除，则 $x$ 能被 $p^{\lceil e/n \rceil}$ 整除。

引理的表达为：

$$
\Divisiable{x^n}{p^e} \Longrightarrow \Divisiable{x}{p^{\left\lceil e/n \right\rceil}}
$$

我们设 $x$ 恰好能被 $p$ 整除 $m$ 次。由此可知：

$$
x = p^m \cdot r \; (m,r \in \mathbb{Z}^+, \; p \nmid r)
$$

得到：

$$
x^n=(p^mr)^n=p^{mn}r^n
$$

由于已知 $r$ 无法被 $p$ 整除，可知 $x^n$ 恰巧能被 $p$ 整除 $mn$ 次。因此有：

$$
mn \geq e \Longrightarrow m \geq \frac{e}{n}
$$

由于 $m$ 是正整数，因此可向上取整得到：

$$
m \geq \left\lceil \frac{e}{n} \right\rceil
$$

得证。