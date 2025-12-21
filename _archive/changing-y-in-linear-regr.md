---
layout: archive
title: "Changing beta hat without changing y hat"
date: 2020-01-01
---

$\newcommand{\R}{\mathbb R}\newcommand{\e}{\varepsilon}\newcommand{\0}{\mathbf 0}\newcommand{\E}{\operatorname{E}}\newcommand{\Var}{\operatorname{Var}}$Consider a linear regression model $y = X\beta+\e$ with $X\in\R^{n\times p}$ known and full column rank. Under common assumptions like $\E[\e]=\0$ and $\Var[\e] = \sigma^2 I$, the Gauss-Markov theorem establishes that the best linear unbiased estimator (BLUE) of $\beta$ is $\hat\beta = (X^TX)^{-1}X^Ty$, and $\hat\sigma^2 = \frac{1}{n-p}\|y-\hat y\|^2$ is unbiased for $\sigma^2$. In this post I'm going to characterize the modifications of $y$ that leave $\hat\beta$ and $\hat\sigma^2$ unchanged. I'll let $X=UDV^T$ be the SVD of $X$ and I'll extend $U$ to an orthonormal basis $\bar U := (U\mid U_\perp)$ for all of $\R^n$ (given $U$, one simple way to do this with probability 1 is to take $n-p$ draws from $\mathcal N(0, I_n)$ and orthonormalize via Gram-Schmidt). I'll use $\hat\beta(y)$ and $\hat\sigma^2(y)$ when I want to indicate the response vector used in their computation. The hat matrix will be denoted by $H=X(X^TX)^{-1}X^T$ and I'll use the fact that $H = UU^T$.

I will let $\tilde y$ denote a perturbed version of $y$, and for convenience I'll write $\tilde y$ as $\tilde y = \hat y + z$. If I want to get the vector that perturbs $y$, instead of $\hat y$, I can just do $\tilde y = y + [(\hat y - y) + z]$. My goal is to characterize the $z$ that lead to $\hat\beta(y) = \hat\beta(\tilde y)$ and $\hat\sigma^2(y) = \hat\sigma^2(\tilde y)$.

For the first requirement, I need

$$
(X^TX)^{-1}X^Ty = (X^TX)^{-1}X^T\tilde y \iff (X^TX)^{-1}X^T(y-\hat y - z) = \0.
$$

$(X^TX)^{-1}$ is a bijection so this is satisfied iff $X^T(y-\hat y - z)=\0$. Since $X^T\hat y = X^THy = X^Ty$, $X^T(y-\hat y) = \0$ always so this condition is equivalent to $z \in N(X^T)$ where $N$ denotes the null space of a matrix. By the rank-nullity theorem $N(X^T)$ is a $n-p$ dimension subspace of $\R^n$, so $\hat\beta(y) = \hat\beta(\tilde y)$ is satisfied for any $\tilde y \in \hat y + N(X^T)$, since this affine subspace is precisely the set of vectors that project to $\hat y$. If $n=p$, so $X$ itself is a bijection, then $N(X^T)$ is zero dimensional and this means that there is no non-zero perturbation of $\hat y$ that still projects to $\hat y$. I'll assume now that $n>p$ to keep things interesting. Furthermore, since I know $z \in N(X^T)$, I have $z = U_\perp c$ for some $c \in \R^{n-p}$ since $U_\perp$ is an orthonormal basis for $N(X^T)$, so I will use this going forward.

For the second requirement, let $r := \|y - \hat y\|$. I need $\|\tilde y\| = \|y\|$ and I have

$$
\|\tilde y\|^2 =\|\hat y + U_\perp c\|^2 = \|UU^Ty + U_\perp c\|^2 = \left\|\bar U {U^Ty \choose c}\right\|^2 = y^THy + \|c\|^2.
$$

Setting this equal to $\|y\|^2$ gives me

$$
\|y\|^2 \stackrel{\text{set}}= y^THy + \|c\|^2 \implies \|c\| = \sqrt{y^T(I-H)y} = \|y - \hat y\| = r.
$$

All together, I have shown that $\tilde y$ preserves $\hat \beta$ and $\sigma^2$ exactly when $\tilde y = \hat y + U_\perp c$ with $\|c\| = r$. Let $S_t^k := \{x \in \R^k : \|x\| = t\}$ so $S_t^k$ is a sphere of radius $t$ in $\R^k$. Geometrically I can picture my findings as follows: I start with the sphere $S_r^{n-p}$ and then embed it in $\R^n$ via $U_\perp S_r^{n-p} := \{U_\perp s : s \in S_r^{n-p}\}$. Next I shift it over by $\hat y$. The resulting set $\hat y + U_\perp S_r^{n-p}$, which is a low dimension sphere in an affine space, is exactly the set of valid $\tilde y$. Another way to picture this is the set of valid $z$ with which I perturb $\hat y$ is $N(X^T) \cap S_r^n$, which is a $n-p$ dimensional slice through a sphere and results in a lower dimension sphere.

There is one special case I'll comment on: suppose $n=p+1$ so $N(X^T)$ is one dimensional. In this case I'll have $c = \pm r$ so there are only two perturbations possible. I can express $y$ w.r.t. the basis $\bar U$ via $y = \bar U a$ for $a = {U^Ty \choose U_\perp ^T y}$ meaning

$$
y = UU^Ty + U_\perp U_\perp^T y = \hat y + U_\perp (U^T_\perp y)
$$

is the orthogonal decomposition of $y$. I know

$$
\|U^T_\perp y \|^2 = y^TU_\perp U_\perp^T y = y^T(I-H)y = r^2,
$$

so in the case where $c=U_\perp^T y$ (which is a scalar in this case) the perturbation is actually just $y$. In the other case with $c=-U^T_\perp y$ I have

$$
\tilde y = \hat y + U_\perp(-U_\perp^Ty) = (UU^T - U_\perp U_\perp^T)y.
$$

I can rewrite this as

$$
UU^T - U_\perp U_\perp^T = UU^T + U_\perp U_\perp^T - 2 U_\perp U_\perp^T = I - 2 U_\perp U_\perp^T
$$

and this operator is known as the Householder reflection of $y$. Intuitively, this means that (for $n>p$) there will always be at least one perturbation possible, and that's when $y$ is reflected to the other side of the column space of $X$. When $N(X^T)$ is one dimensional this is the only perturbation possible which corresponds to the intersection of a line and a sphere just being two points (i.e. a sphere in $\R$).

A final perspective on this is to consider the function $T : \R^n \to \R^{p+1}$ defined by $T(y) = (X^Ty \mid y^Ty)$, and to note that this is like a sufficient statistic for $(\beta, \sigma^2)$. I have just shown that the preimage $T^{-1}(\{y\})$ is a $n-p$ dimension sphere centered at $\hat y$ and lying in $\hat y + N(X^T)$, so that gives me a sense of how much of a reduction it is to consider just $T(y)$ versus all of $y$.

## Example in $\R^3$

I'll conclude with a particular example. I'll fix $n=3$ and $p=1$ so the column space of $X$ is 1-dimensional and $N(X^T)$ is 2-dimensional. I will use $\bar U$ as my basis, so

$$
y = \begin{bmatrix}a\\b\\c\end{bmatrix}, \;\;\hat y = \begin{bmatrix}a \\ 0 \\ 0\end{bmatrix},\;\; y-\hat y = \begin{bmatrix}0 \\ b \\ c\end{bmatrix}.
$$

By this choice of basis I'll have

$$
N(X^T) = \left\{\begin{bmatrix}0\\s\\t\end{bmatrix} : s,t \in \R\right\}
$$

so vectors of the form $\begin{bmatrix}a\\s\\t\end{bmatrix}$ will project to $\hat y$, and these are all elements of the 2-dimensional affine subspace $\hat y + N(X^T)$. Next I need $\|\tilde y\|$ preserved which means my $z$ have norm $r$, so if I place a sphere of radius $r$ centered at $\hat y$ and look for the intersection with $\hat y + N(X^T)$ I will find all my valid perturbations. This is shown in the picture below.

![In this picture y projects to y-hat and N(X^T) is the span of the 2nd and 3rd basis vectors. The orange circle is the set of vectors that all lead to the same beta-hat and sigma-hat-squared.](/assets/archive/changing-y-in-linear-regr/set-of-valid-perturbations-v2.png)

In this picture $\color{blue}y$ projects to $\color{teal}{\hat y}$ and $N(X^T)$ is the span of the 2nd and 3rd basis vectors. The orange circle is the set of vectors that all lead to the same $\hat\beta$ and $\hat\sigma^2$. The dotted blue line going to the bottom of the orange circle is the Householder reflection of $y$ around the column space of $X$.
