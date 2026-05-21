---
title: "几种常用的QLDPC码"
date: 2026-05-11T00:00:00+08:00
draft: false
tags: ["quantum"]
categories: ["personal"]
summary: "用于量子系统设计的几种常用的纠错码, 及相关性质."
math: true
ShowToc: true
TocOpen: false
---

用于量子系统设计的几种常用的纠错码, 及相关性质.

从体系结构的角度，我们重点关注code的构造；对于code的性质，大多直接给出，不加证明，只求直观理解。

## From Stabilizer Code to CSS Code

**Def. (CSS Code)** 若某个stabilizer code, 其生成元要么都是$I$和$X$，要么都是$I$和$Z$, 则称其为CSS code.

**Remark.** CSS code是stabilizer code的子集.

**Remark.** Shor code和Steane code是CSS code. 而下图中的5-qubit code不是CSS code.

> [!NOTE]
>
> 这个5-qubit code是: **最小的**可以检测(detect) 并纠正 (correct) 任意单个比特上错误的code.

> [!NOTE]
>
> 生成元可以看作校验矩阵中的行，生成元构成的**稳定子群**（stabilizer group），就“校验”出来了码空间（code space）。

<img src="assets/image-20260512110455269.png" alt="image-20260512110455269" style="zoom: 67%;" />

CSS code可以由两个对偶 (dual) 的classical error correction code来组成. (这是Gottesman讲义中引入CSS的方法, 但不直接) (实际上这是个定理, 下文详述)

我们为什么喜欢CSS code呢? 因其支持一些**横截门** (transversal gate).

**Def. (Transversal Gate, informal)** 若逻辑门$G_L$ = 对每个物理比特作相同的物理门$G$, 则称其为横截的.

**Remark.** 考虑硬件平台的特性 (如中性原子平台的原子移动等), 可能虽然满足不了上面的横截要求, 但其实也有类似的风格, 能够简化逻辑操作的实现. 稍稍放松上述定义, 或许可以去作一些探讨 (思路类似于近似算法).

下面考虑几种常用门在CSS code上的表现. 不详细证明.

**Prop.** 对所有的CSS code, CNOT都是横截的.

**Prop.** 若CSS code的$X$部分和$Z$部分是**对偶**的, 则Hadamard是横截的.

**Remark.** 此处的*对偶*可以直接认为是校验矩阵相等, 那么更直观地: 把所有$X$稳定子中的$X$替换成$Z$, 就是所有$Z$稳定子. Steane code即满足这一条定义.

**Remark.** 这一条性质由Hardmard门的性质导出, Hadamard做了这种交换.

**Prop.** 若CSS code是double even的，则Phase Gate是横截的. Double even指每个generator中$X$的数目和$Z$的数目均为4的倍数.

Steane Code满足上面的三条属性! 但是由于Gottesman-Knill定理, 我们知道Clifford电路是可以被多项式时间经典模拟的, 因而如果只是用这里的CNOT Hadamard和Phase Gate, 这个电路是可以被经典快速模拟的.

另一个好的想法是, 如果存在QECC, 其对应的横截门是**完备** (**universal**) 的 (如果只用Clifford, 则不完备), 那我们一定会在FTQC里用! 但是, 这个实现不了:

**Thm. (Eastin-Knill)** 对任意可以检测1-qubit错误的QECC, 由横截门生成的幺正变换集合是一个离散的集合 (discrete set), 因此, 不是完备的.


> [!NOTE]
>
> Eastin-Knill并没有否定"横截门可以被快速经典模拟" (Gottesman-Knill重视的). 其在说: 不能有连续的 (continuous) 横截门集合.

## Check Matrix & Tanner Graph

考虑$n$-bit的经典线性纠错码, 首先回顾校验矩阵的定义.

**Def. (Parity-Check Matrix)** $m \times n$矩阵$H$, 其第$i$行定义了一个奇偶校验.

能用校验矩阵写出来的, 自然是线性码 (Linear Code).

**Def. (Tanner Graph)** 记$H$的Tanner Graph为$\mathcal{G}_H$. 其包含$n$个比特节点, $m$个校验节点, 两组节点构成二分图. 当且仅当$H_{ij} = 1$, 比特$j$和校验$i$之间存在一条边.

可以简单理解成, **校验**和其在校验的**位**之间有边.

CSS code的"校验矩阵"是用两个经典的校验矩阵拼出来的 (当然, 需要引入 $(\cdot | \cdot)$的记号). 

现在考虑CSS code的Tanner Graph. 如图所示:

<img src="assets/image-20260514120656496.png" alt="image-20260514120656496" style="zoom: 37%;" />



<img src="assets/image-20260513212217888.png" alt="image-20260513212217888" style="zoom:50%;" />

从二分图到了"三分图".

我们现在希望回顾一下"拼接"的可行性, 这个要求前面用"对偶"搪塞了. 其实, 拼接法相较CSS定义, 缺少的是"验证"其拼完之后是stabilizer code!

> [!NOTE]
>
> Stabilizer code的充要条件 (已默认在Pauli Group中讨论):
>
> 1. $-I \notin S$
> 2. $S$ 是群
> 3. $S$ 是Abel的
>
> 甚至有些讲义中直接将这个作为定义. 从"如何合理定义"的角度去思考, 可能还更方便, 参考[UCB讲义](https://people.eecs.berkeley.edu/~jswright/quantumcodingtheory24/scribe%20notes/lecture08.pdf)

实际上, 我们发现条件1和2已经满足, 我们只需满足条件3. 我们把用于拼接的两个校验矩阵记作$H_X$和$H_Z$, 则我们有
$$
H_XH_Z^{\top} \equiv 0 \ \text{mod} \ 2
$$
直觉: 如果同一个位置上有X和Z, 由于反对易性, 就会出来一个-1.

推知: 任何$X$校验和$Z$校验均共享偶数个qubit. 

## Go for Quantum LDPC

low density,即**稀疏**

定义"稀疏", 即定义了qLDPC code.

**Def.** **qLDPC code**是由"稀疏"的校验矩阵定义的stabilizer code. "稀疏"指满足如下两个条件: 1. 每个check作用于$O(1)$个qubit. 2. 每个qubit在$O(1)$个check中.

## Another Language: Chain Complex

尽可能少地引入链复形 (Chain Complex) 的语言, 以方便下文关于qLDPC的讨论.

考虑经典线性码. 考虑校验矩阵$H$. 其码字是$Hx = 0$约束出的向量, 即$\ker H$.

即有

$$
\underbrace{\mathbb{F}_2^n}_{\text{比特 (bits)}} \;\xrightarrow{\;H\;}\; \underbrace{\mathbb{F}_2^m}_{\text{校验 (checks)}}
$$

从链复形的角度看:
$$
C_1 \;\xrightarrow{\;\partial\;}\; C_0
$$

> [!TIP]
>
> 何故"重命名"? 笔者的两个想法:
>
> 1. 视角从"矩阵相乘"到"码相乘"
> 2. 理论上, 将其转换到流形上作研究

现在 (不正式地) 定义链复形:

**Def.** 链复形就是下面的链, 且满足$\partial_i \circ \partial_{i+1} = 0$.
$$
\cdots \to C_2 \xrightarrow{\partial_2} C_1 \xrightarrow{\partial_1} C_0 \to \cdots
$$

定义完了.

**Remark.**  $\partial_i \circ \partial_{i+1} = 0 \iff \text{Im} \partial_{i+1} = \ker \partial_i$

现在考虑如下的链: 
$$
C_2 \xrightarrow{\partial_2} C_1 \xrightarrow{\partial_1} C_0
$$

实际上, 给一个CSS code, 让$\partial_1$代表$H_X$, $\partial_2$代表$H_Z^{\top}$, 则这样构造出来的链满足定义条件.

之后, 每构造一个新的码, 只要我们希望去用链复形的语言, 就需要去验证上述性质. 换句话讲, 这个性质对于code也是重要的.

## HP Code

HP = HGP = Hypergraph Product Code.

> 或许有帮助: [UC Berkeley关于Product Code的讲义](https://people.eecs.berkeley.edu/~jswright/quantumcodingtheory24/scribe%20notes/lecture15.pdf)
>

现在考虑有两个经典码, 我们用单步的复形来写出:

$$
A_\bullet:\quad A_1 \xrightarrow{\partial_A} A_0, \qquad B_\bullet:\quad B_1 \xrightarrow{\partial_B} B_0
$$

类比多项式乘法 (不严谨, 但先乘了再说; 你也可以说是Leibniz Rule得出来的, 或许你是对的),  得到下表

| **阶数 (Degree)** | **组成部分 (Pieces)**                        |
| ----------------- | -------------------------------------------- |
| **2**             | $A_1 \otimes B_1$                            |
| **1**             | $(A_1 \otimes B_0) \oplus (A_0 \otimes B_1)$ |
| **0**             | $A_0 \otimes B_0$                            |




这可以写成链:

$$
A_1\!\otimes\! B_1 \;\xrightarrow{\;\partial_2\;}\; (A_1\!\otimes\! B_0)\oplus(A_0\!\otimes\! B_1) \;\xrightarrow{\;\partial_1\;}\; A_0\!\otimes\! B_0
$$

如下的边界 (Boundary) 算子可以写成如下形式:
$$
\partial_2 \;=\; \begin{bmatrix} I\otimes\partial_B \\[2pt] \partial_A\otimes I \end{bmatrix}, \qquad \partial_1 \;=\; \begin{bmatrix}\,\partial_A\otimes I \ \ \big|\ \ I\otimes\partial_B\,\end{bmatrix}.
$$
验算条件:
$$
\partial_1\partial_2 \;=\; (\partial_A\!\otimes\! I)(I\!\otimes\!\partial_B) + (I\!\otimes\!\partial_B)(\partial_A\!\otimes\! I) \;=\; (\partial_A\!\otimes\!\partial_B) + (\partial_A\!\otimes\!\partial_B) \;=\; 0 \pmod 2.
$$
知其为链复形.

> [!TIP]
>
> 为什么可以分块?
>
> 带直和 (direct sum) 的线性映射:
>
>
> -  $X \to Y_1 \oplus Y_2$ 是 $X \to Y_1$ 和 $X \to Y_2$的组合. 纵向分块:
>   $$
    \begin{pmatrix} y_1 \\ y_2 \end{pmatrix} \;=\; \begin{bmatrix} M_1 \\ M_2 \end{bmatrix} x.
    $$
> - $X_1 \oplus X_2 \to Y$ 是 $X_1 \to Y$ 和 $X_2 \to Y$ 的组合, 横向分块: 
>   $$
    y \;=\; \begin{bmatrix} M_1 \ \big|\ M_2 \end{bmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} \;=\; M_1 x_1 + M_2 x_2.
    $$
>

当然, 如果只是讨论HP code, 也可以不引入链复形直接看矩阵. 往回换, 只需找到对应: $\partial_1 \sim H_X$, $\partial_2 \sim H_Z^{\top}$, $\partial_A \sim H_A$, $\partial_B \sim H_B^{T}$ , 则有
$$
\boxed{\;H_X \;=\; \partial_1 \;=\; \big[\,H_A \otimes I_{n_B} \ \;\big|\;\ I_{m_A} \otimes H_B^{\top}\,\big]\;}\qquad m_A n_B \times (n_A n_B + m_A m_B),
$$

$$
\boxed{\;H_Z \;=\; \partial_2^{\top} \;=\; \big[\,I_{n_A} \otimes H_B \ \;\big|\;\ H_A^{\top} \otimes I_{m_B}\,\big]\;}\qquad n_A m_B \times (n_A n_B + m_A m_B).
$$

此处令$\partial_B \sim H_B^{T}$, 可以认为是把Tanner Graph倒过来, 以求校验 $\rightarrow$ 比特 $\rightarrow$ 校验的自然流向.

$H_XH_Z^{\top} \equiv 0$容易检验, 不再证.



这个时候, 有必要回顾一下, 为什么要搞出这样一个Code呢? 

因为在此之前, 对code的研究聚焦于二维拓扑量子码 (2D topological quantum code), 其parity check是geometrically local的, 即其校验仅作用于空间上有限的邻域 (finite spatial neighborhood). 其**码距**极受限. Bravyi-Poulin-Terhal (BPT) bound指出了这一点:

**Thm. (BPT Bound)** 对任意 $[[n, k, d]]$ 二维拓扑量子码,  有$kd^2 \leq O(n)$. 

推知$d \leq O(\sqrt{n})$. 并且, 如果希望码能够容纳更多逻辑比特 (增加$k$), 则$d$很有可能需要减小.

为克服BPT bound, 接近线性的码距 (linear distance), 必须抛弃严格的二维局域校验. 

HP code是第一个从这些角度克服BPT bound的code, 其本身也可以视为toric code的推广.

<img src="assets/image-20260514220403436.png" alt="image-20260514220403436" style="zoom: 33%;" />

<img src="assets/image-20260514220456942.png" alt="image-20260514220456942" style="zoom:33%;" />

## GB Code

GB = Generalized Bicycle

直接介绍构造. 仍然考虑两个码, 但是不再直接作两个码的张量积, 而是将其校验矩阵拼接:
$$
H_X = [\,A \mid B\,], \qquad H_Z = [\,B^{\top} \mid A^{\top}\,].
$$
如果其能满足$H_X H_Z^{\top} \equiv 0 \pmod 2$, 那么这个码就是CSS code.

展开得
$$
H_X H_Z^{\top} = [\,A \mid B\,]\begin{bmatrix} B \\ A \end{bmatrix} = AB + BA \equiv 0 \pmod 2
$$
这也即在$\mathbb{F}_2$上满足$AB = BA$即可.

一个可以满足对易性的常用好东西是循环 (轮换) 矩阵!

比如$l \times l$矩阵$S$:
$$
S = \begin{bmatrix}  0 & 1 & 0 & \cdots & 0 \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & 0 & \cdots & 1 \\ 1 & 0 & 0 & \cdots & 0  \end{bmatrix}
$$
自然有$S^l = I$. 将其作为"生成元", 可以带入到多项式中: $a(x) = \sum_i a_i x^i \ \to \ A = \sum_i a_i S^i$ .

实际上, 这些矩阵构成了一个交换环, 其同构于
$$
R = \mathbb{F}_2[x]\,/\,(x^{\ell} - 1).
$$
注意: $(x^l - 1)$ 为 $x^l - 1$ 生成的主理想.

这个关于商环的观察是重要的, 因为其与BB code所同构的商环比较, 即立刻知二者区别.

回过头来构造GB code. 选定长度$l$和两个多项式$a(x), b(x) \in R$, 将循环矩阵$S$代入即可得$A, B$. 

> [!NOTE]
> 
> 双块布局使得对易性水到渠成. 不再需要费力寻找"对偶"的经典码.

**Why LDPC?**  观察$A, B$, 其矩阵内部**每一行** (校验) 的非零元数目是相同的, 即$w_a, w_b$. 保持$w_a, w_b$为常数, 即每个check对应$O(1)$个qubit. **列同理**. 则每个qubit对应$O(1)$个check.

**参数.** 该码使用 $n = 2\ell$ 个量子比特, 并编码了
$$
k = 2 \deg \gcd\!\big(a(x),\, b(x),\, x^{\ell} - 1\big)
$$
个逻辑量子比特 (gcd 在 $\mathbb{F}_2$ 上计算). 距离通常需要通过数值方法求解. 不作证明. 参考: https://arxiv.org/pdf/1904.02703

> [!NOTE]
> 
> $H_X$ 的各行可以不是线性独立的——也即退化 (degenerate) 的, 包含冗余的生成元. 这在校验矩阵中是允许的; 真正决定逻辑比特数的是独立生成元的数量, 即 $k = 2\ell - \mathrm{rank}(H_X) - \mathrm{rank}(H_Z)$.

## BB Code

BB = Bivariate Bicycle = 双元自行车码

BB code使用了两个循环变量, 将一元环替换为
$$
R = \mathbb{F}_2[x, y]\,/\,(x^{\ell} - 1,\; y^{m} - 1).
$$
采用$$x = S_{\ell} \otimes I_{m}, \ y = I_{\ell} \otimes S_{m}.$$ $S$是同上的循环矩阵, 只是矩阵维数有变.

其满足$xy = yx$. (这是显然的, 但是如果$x$, $y$不这样平凡地取, 会有影响吗?)

推知$R$仍然是交换环.

考虑构造, 同上.
$$
H_X = [\,A \mid B\,], \qquad H_Z = [\,B^{\top} \mid A^{\top}\,].
$$
其中$A = a(x, y), B = b(x, y)$. $a(x,y), b(x, y)$是多项式. 

$H_X H_Z^{\top} \equiv 0 \pmod 2$ 同上, 易证.

**参数.** $A, B$均为$lm \times lm$的矩阵. 则物理比特数$n = 2lm$.

> [!NOTE]
> 
> 对于stabilizer code, 其自然有$n = k - \text{rank}(H)$. 由于CSS code由两部分拼成, 则推知其有$n = k - \text{rank}({H_X}) - \text{rank}(H_Z)$. 且不论复杂度, $\text{rank}$至少有高斯消元法支持求出.

### IBM: Gross Code

取$l = 12, m = 6, A = x^3 + y + y^2, B = y^3 + x + x^2$, 则得到了$[[144, 12, 12]]$ 的BB code.

每个块贡献了3个单项式, 则校验的weight是6, 且每个qubit对应6个校验. 其确为qLDPC code.

调整$l, m$, 还可以得到$[[72,12,6]]$ , $[[288,12,18]]$等code.

**硬件适配.** TODO. 接下来重点看.

### Summary: Two Branches

问题: HP code和BB code中都有张量积. 其可以被归为一种构造吗?

不能.

HP code中, 张量积用于经典码的相乘.

BB code中, 张量积用于构建"二维环面".



将其视为Toric code的推广, 考虑两种方式.

*HP code*. 将两个最基本的repetition code作张量积, 可得

<img src="assets/image-20260521204844034.png" alt="image-20260521204844034" style="zoom:50%;" />

如果这两个repetition code首位相接, 呈循环, 则tensor之后即正好在torus上面.

*BB code*. 直觉简单: 环面上的点是$\mathbb{Z}_\ell\times\mathbb{Z}_m$.

考虑$x = S_\ell\otimes I_m$，$y = I_\ell\otimes S_m, A = 1 + x, B = 1 + y$. 那么, 这个是local的.

## Thoughts

将code放到网络上?

code的三角

## Ref

[Introduction to Quantum Information Science II](https://www.scottaaronson.com/qisii.pdf) by Scott Aaronson

https://www.youtube.com/watch?v=MqGBwQjS4CI&list=PLgKuh-lKre11aM00IH-iPtNoLh9Q9O03t Simons Institute组织的讨论

https://people.eecs.berkeley.edu/~jswright/quantumcodingtheory24/scribe%20notes/lecture08.pdf Stabilizer Code充要条件参考

https://people.eecs.berkeley.edu/~jswright/quantumcodingtheory24/scribe%20notes/lecture15.pdf Product Code
