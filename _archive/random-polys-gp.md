---
layout: archive
title: "Random polynomials with Gaussian processes"
date: 2021-06-01
---

$\newcommand{\X}{\mathcal X}\newcommand{\x}{\mathbf x}\newcommand{\0}{\mathbf 0}\newcommand{\one}{\mathbf 1}\newcommand{\y}{\mathbf y}\newcommand{\z}{\mathbf z}$In this post I'm going to look at distributions over polynomials using Gaussian processes. Let $\mathcal X = \mathbb R$ be the index set so I'm just considering univariate polynomials since that captures a lot of the interesting behavior while being much simpler. My goal is to understand the functions that are sampled when using the so-called polynomial kernel $k(x,x') = (x\cdot x' + c)^d$ for $d \in \mathbb N$ and $c \geq 0$.

Using the binomial theorem I have
$$
k(x,x') = \sum_{m=0}^d {d\choose m} c^{d-m} \cdot (x\cdot x')^m .
$$
Let $k_m(x,x') = (xx')^m$. Then $k$ can be viewed as a linear combination of the $k_m$ with coefficients of ${d\choose m} c^{d-m}$.

Claim: each $k_m$ is a PSD kernel and PSD kernels are closed under non-negative linear combinations, so $k$ is itself a PSD kernel.

Pf: Let $x_1, \dots, x_n \in \X$ and set $\x = (x_1,\dots,x_n)^T$. For an arbitrary $m\in\mathbb N$ I need to first show that the $n\times n$ matrix $K$ with $K_{ij} = (x_ix_j)^m$ is PSD. Letting $v\in\mathbb R^n$ and interpreting exponents elementwise, I have
$$
v^T K v = v^T (\x\x^T)^m v = v^T (\x^m)(\x^m)^Tv = (v^T\x^m)^2 \geq 0
$$
so $K$ is indeed PSD (and is rank $1$ no matter $m$). This means each $k_m$ is PSD.

Next, let $M_1,\dots,M_\ell$ be $n\times n$ PSD matrices and consider $M := \sum_{i=1}^\ell a_i M_i$ for $a_i \geq 0$. Then for an arbitrary $v\in\mathbb R^n$ I have
$$
v^T M v= \sum_{i=1}^\ell a_i v^T M_i v \geq 0
$$
so $M$ is also PSD. This shows that the $n\times n$ PSD matrices are closed under non-negative combinations, meaning they form a convex cone. Since ${d\choose m} c^{d-m} > 0$ this means $k = \sum_{m=0}^d {d\choose m} c^{d-m} k_m$ is also PSD.

$\square$

Just as I showed that $k$ is PSD by looking at it in terms of the pieces $k_m$, I'll build up to understanding $\mathcal {GP}(0, k)$ by understanding each $\mathcal {GP}(0, k_m)$ in turn.

Let $f_m \sim \mathcal{GP}(0, k_m)$. When $m=0$ I'll have
$$
\begin{bmatrix}f_0(x_1) \\ \vdots \\ f_0(x_n)\end{bmatrix} \sim \mathcal N(\0, \one\one^T) \stackrel{\text d}= \one Z
$$
with $Z \sim \mathcal N(0,1)$. This shows that every value of $f_0$ is the same, and I can simulate a draw from $\mathcal{GP}(0, k_0)$ by sampling a single $Z\sim\mathcal N(0,1)$ and returning that value for every index point. Another way to look at this is to consider $\begin{bmatrix} f_0(1) \\ f_0(x)\end{bmatrix}$ for an arbitrary $x$. Then I know
$$
\begin{bmatrix} f_0(1) \\ f_0(x)\end{bmatrix} \sim \mathcal N\left(\0, \one\one^T\right) = \begin{bmatrix}1\\1\end{bmatrix} Z
$$
so once I know $f_0(1)$, which is just $Z$, then with probability one I'll have $f_0(x) \mid \{f_0(1) = z\} = z$ so again I can see that $\mathcal{GP}(0,k_0)$ samples from horizontal lines with heights given by $\mathcal N(0,1)$.

I can understand $f_m\sim\mathcal {GP}(0,k_m)$ in a similar way:
$$
\begin{bmatrix} f_m(1) \\ f_m(x)\end{bmatrix} \sim \mathcal N\left(\0, {1\choose x^m}{1\choose x^m}^T\right) \stackrel{\text d}= {1 \choose x^m} Z
$$
so I'll have $f_m(x) \mid \{f_m(1) = z\} = z\cdot x^m$. This shows that $\mathcal {GP}(0, k_m)$ is a distribution over polynomials of the form $f(x) = ax^m$ and it does this by just having the coefficient of $x^m$ be distributed as $\mathcal N(0,1)$. This is a one dimension space with the polynomial $x\mapsto x^m$ as a basis.

For $m>0$ I'll also always have $f_m(0) \sim \mathcal N(0,0) = 0$ almost surely so all of these polynomials go through the origin.

Next I'll consider
$$
f = \sum_{m=0}^d \sqrt{a_m} f_m
$$
with $a_m > 0$ and $f_m \stackrel \perp \sim \mathcal {GP}(0,k_m)$. I'll have $\sqrt{a_m} f_m \sim\mathcal {GP}(0, a_m k_m)$ so this means that $\sqrt{a_m} f_m$ samples from the one dimensional space $\{x \mapsto ax^m : a \in \mathbb R\}$ by returning polynomials with a coefficient sampled from $\mathcal N(0, a_m)$. Then since the $f_m$ are independent I'll have
$$
\begin{bmatrix}f(x_1) \\ \vdots \\ f(x_n)\end{bmatrix} \sim \mathcal N\left(\0, \sum_{m=0}^d a_m (\x\x^T)^m\right) \stackrel{\text d}= \mathcal N\left(\0, \sum_{m=0}^d a_m K_m\right)
$$
This means $f$ is a GP with a mean function of zero and a kernel of $k(x,x') = \sum_{m=0}^d a_m k_m$. Taking $a_m = {d\choose m}c^{d-m}$ I have $k(x,x') = \sum_{m=0}^d {d\choose m}c^{d-m}(xx')^m = (xx'+c)^d$ so I can see that samples from this GP are a sum of random polynomials (and $m=0$ gives a constant) so ultimately this GP gives me a distribution over the $d+1$ dimension space of $d$-degree polynomials and the coefficient for the $x^m$ term is sampled from $\mathcal N(0, {d\choose m}c^{d-m})$. This also shows the role of $c$ in either damping high degree terms or focusing on them.

## The rank of the kernel matrix

I want to understand the rank of the kernel matrices produced by the polynomial kernel. I know $K = \sum_{m=0}^d a_m (\x^m)(\x^m)^T$ so if I define a matrix $X$ by
$$
X = (\one \mid \x \mid \x^2 \mid \dots \mid \x^d)
$$
and $D = \text{diag}(a_0, a_1, \dots, a_d)$ then
$$
XDX^T = a_0 \one\one^T + a_1\x\x^T + \dots + a_d(\x^d)(\x^d)^T = K.
$$
$D$ is full rank so the rank of $XDX^T$ comes from the rank of $X$, which is a Vandermonde matrix. If the elements of $\x$ are distinct then $X$ is full rank, so as $n$ grows this means the rank of $K$ is capped at $d+1$.

I can also see from this that if I set $Z = XD^{1/2}$ then $K = ZZ^T$ and the kernel $k$ corresponds to the inner product $\langle \varphi(x), \varphi(x')\rangle$ with feature map
$$
\varphi(x) = (a_0^{1/2}, a_1^{1/2}x , \dots, a_d^{1/2} x^d)
$$
which is an embedding from $\mathbb R$ into $\mathbb R^{d+1}$.

I'll now conclude with a comment on a connection between the rank of the kernel matrix and the ability of a GP to interpolate an arbitrary sequence.

Let $\x := (x_1, \dots, x_n)^T$ with $x_1 < \dots < x_n$ and let $\y := (y_1, \dots, y_n)^T \in \mathbb R^n$. Suppose $f \sim \mathcal{GP}(0, k)$ for an arbitrary kernel $k$.

I'll now assume that the kernel matrix associated with $k$ is full rank (so strictly PD rather than PSD). An example of such a kernel is the squared exponential kernel. I'll model $\y \sim \mathcal N(\0, K)$ and since $K$ is full rank this distribution is supported on all of $\mathbb R^n$ and any $\y$ is a possible output. Now for a new point $x_*\in\X$ with $f_* := f(x_*)$ I'll set $k_* = k(x_*, \x)$ and $k_{**} = k(x_*,x_*)$ so
$$
f_* \mid \y \sim \mathcal N(k_*^TK^{-1}\y, k_{**} - k_*^TK^{-1}k_*).
$$
This is the posterior GP. I now want to see what happens if $x_* = x_i$ for $i\in\{1,\dots,n\}$. I know $KK^{-1} = I$ and
$$
K = \begin{bmatrix} k(x_1, \x) \\ \vdots \\ k(x_n, \x)\end{bmatrix}
$$
so this means $k(x_i, \x)^TK^{-1} = e_i$, the $i$th standard basis vector. I'll also have
$$
\begin{aligned}
&k_{**} - k_*^TK^{-1}k_* = k(x_i,x_i) - k(x_i, \x)^TK^{-1}k(x_i, \x)\\& = k(x_i, x_i) - e_i^Tk(x_i, \x) \\&= 0
\end{aligned}
$$
so all together $f(x_i) \mid \y \sim \mathcal N(y_i, 0) = y_i$ almost surely. This means that the posterior GP interpolates $\y$ over $\x$ with probability one.

If instead $K$ is not full rank then an arbitrary $\y$ likely will not be in the support of $\mathcal N(\0, K)$ so the GP has no hope of hitting these values. It also doesn't make sense to condition on the GP taking values outside of its support.
