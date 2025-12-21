---
layout: archive
title: "The four fundamental subspaces and rank-nullity"
date: 2021-01-01
---

$\newcommand{\R}{\mathcal R}\newcommand{\C}{\mathcal C}\newcommand{\Rbb}{\mathbb R}\newcommand{\0}{\mathbf 0}\newcommand{\one}{\mathbb 1}\newcommand{\span}{\operatorname{span}}\newcommand{\rank}{\operatorname{rank}}$In this post I'm going to prove the Steinitz exchange lemma, that the row rank and column rank of a matrix are equal, and the rank-nullity theorem. I'll then apply these results to understanding the four fundamental subspaces (a term I first heard in Gilbert Strang's linear algebra course). I'll take $A$ to be an $m\times n$ matrix with rank $r$ corresponding to a linear transformation $T : V\to W$ where $V$ and $W$ are vector spaces over a field $F$ with dimensions of $n$ and $m$ respectively. Typically $F = \Rbb$ but some results will be for a general field.

The four fundamental subspaces are:

1. the column space, denoted $\C(A)$. This is equivalent to the image of $T$.

2. the row space, denoted $\R(A)$. This is equivalent to $\C(A^T)$. I won't be discussing dual spaces in this post but in a future post I'll return to some of these ideas and interpret them in terms of linear functionals and dual spaces.

3. $N(A)$, the null space of $A$. This is equivalent to the kernel of $T$.

4. $N(A^T)$, the null space of $A^T$.

$\C(A)$ and $N(A^T)$ are both subspaces of $W$ while $\R(A)$ and $N(A)$ are subspaces in $V$.

I'll begin by stating and proving the aforementioned results and then I'll then apply them to interpreting these subspaces.

## Proofs

Result 1: Steinitz exchange lemma

Claim: if $U = \{u_1, \dots, u_m\}$ is a linearly independent set of vectors in a vector space $V$, and $W= \{w_1,\dots,w_n\}$ spans $V$, then

(1) $m \leq n$

(2) there is a set $W' \subseteq W$ with $|W'|=n-m$ such that $U\cup W'$ spans $V$.

This result is important because it confirms that I can't form a linearly independent set out of linear combinations of a smaller number of vectors, so e.g. I can't compress $U$ by representing it in terms of combinations of a smaller set. This result also shows that I can augment any linearly independent set with elements of a spanning set until I get a new spanning set. The idea behind this is that $U$ being linearly independent means $\span U$ is a hyperplane of dimension $m$, but this may not cover $V$. But since $W$ does cover $V$ I know there must be some $w_j$ with activity in the dimensions $U$ misses, and since $U$ doesn't have redundant information in it I'll need at most $n-m$ extra vectors to guarantee spanning.

I'll be proving this by induction (and following the proof given [here](https://en.wikipedia.org/wiki/Steinitz_exchange_lemma)) but first I'll give a sense of why an inductive approach makes sense. To begin with, $W$ spanning $V$ means $u_1 = \sum_{i=1}^n \alpha_i w_i$ since $u_1$ is just some element of $V$. If $\alpha_i = 0$ for all $i$ then I'd have $u_1 = \0$ which contradicts $U$ being linearly independent, so WLOG I can take $\alpha_1 \neq 0$. Then
$$
u_1 = \sum_{i=1}^n \alpha_i w_i \implies w_1 = \frac 1{\alpha_1}\left(u_1 - \sum_{i=2}^n \alpha_i w_i\right).
$$
This implies $\{u_1, w_2, \dots, w_n\}$ spans $V$ because any $v\in V$ can be written as $v = \sum_{i=1}^n \beta_i w_i$ and by replacing $w_1$ with the above equation I have every element in $V$ as a linear combination of $\{u_1, w_2, \dots, w_n\}$ instead.

I can now repeat the idea with my new spanning set:
$$
u_2 = \alpha_1 u_1 + \sum_{i=2}^n \alpha_i w_i.
$$
If $\alpha_2 = \dots = \alpha_n = 0$ then $u_2 = \alpha_1 u_1$ which contradicts linear independence, so there must be some $\alpha_i$ with $i>1$ such that $\alpha_i\neq 0$. Again WLOG I'll take this to be $\alpha_2$. I can then rearrange as before so
$$
w_2 = \frac 1{\alpha_2}\left(u_2 - \alpha_1 u_1 - \sum_{i=3}^n \alpha_i w_i\right)
$$
and by an identical argument I know $\{u_1,u_2, w_3,w_4,\dots,w_n\}$ spans $V$. I can then inductively continue this process where at each step I use spanning to swap out a $w_j$ with a new $u_i$ until $U$ has been exhausted and I have a set of the form $\{u_1,\dots,u_m,w_{m+1},\dots,w_n\}$ that still spans $V$. I also know that $m$ can't exceed $n$ since at each step I still need some elements of $W$ in the mix otherwise I'll contradict the linear independence of $U$.

I'll now give the formal proof.

Pf: Suppose I have $U$ and $W$ as described above. I want to show for $k = 0,\dots,m$ that $k \leq n$ and the set $S_k := \{u_1,\dots,u_k, w_{k+1}, \dots, w_n\}$ spans $V$ (with possible reordering of the $w_j$ in each $S_k$).

Base case: $k=0$. Then $0\leq n$ and $S_0 = W$ spans $V$ by assumption.

Inductive case: assume true for some $k<m$. I'll show the result holds for $k+1$. By the inductive hypothesis $S_k$ spans $V$ so, since $u_{k+1} \in V$, I have
$$
u_{k+1} = \sum_{i=1}^k \alpha_i u_i + \sum_{i=k+1}^n \alpha_i w_i.
$$
As before, if $\alpha_{k+1} = \dots = \alpha_n = 0$ then I'd have $u_{k+1} \in \span\{u_1,\dots,u_k\}$ which contradicts the linear independence of $U$. If $k \geq n$ then I'd also violate linear independence since I wouldn't have any $w_i$ in the right hand side. This means that $k < n$ and there is at least one non-zero $\alpha_i$ for $i>k$, and WLOG I can take it to be $\alpha_{k+1}$.

Solving for $w_{k+1}$ I have
$$
w_{k+1} = \frac{1}{\alpha_{k+1}}\left(u_{k+1} - \sum_{j=1}^k \alpha_j u_j - \sum_{j=k+2}^n \alpha_j w_j\right)
$$
so $w_{k+1}$ is in the span of $S_{k+1} = \{u_1,\dots,u_{k+1}, w_{k+2}, \dots, w_n\}$. $V = \span S_k$ by hypothesis so this means $V = \span S_{k+1}$ too.

This holds until $U$ is exhausted when $k=m$, and since the above result holds for any $k \leq m$ I therefore also have $m \leq n$.

$\square$

Result 2: the row rank and column rank of a matrix are equal.

I'll be following the proof [here](https://en.wikipedia.org/wiki/Rank_(linear_algebra)#Proofs_that_column_rank_=_row_rank).

The heart of this proof is that if I have a basis of size $r$ for $\C(A)$ then I can get a spanning set of size $r$ for $\R(A)$, and that means $\dim \R(A) \leq r$. I can then apply the same result to $A^T$ to get the other direction.

Pf: let $A$ be $m\times n$ with $\text{col rank}(A) = r$. I'll prove $\text{row rank}(A) = r$ by first showing $\text{row rank}(A) \leq r$ and then proving the other direction for equality.

$\C(A)$ is a vector space in its own right and by assumption it has dimension $r$ so it has a basis $\{c_1,\dots,c_r\}$. I'll collect these into the columns of an $m\times r$ matrix $C$. Every column of $A$ is in $\C(A)$ so $A$ can be expressed as $A = CR$ for an $r\times n$ matrix $R$ where the $i$th column of $R$ gives the $r$ weights needed to combine the $c_j$ into the $i$th column of $A$.

This factorization also means that each row of $A$ is made by a linear combination of the $r$ rows of $R$, so the rows of $R$ span $\R(A)$. By the Steinitz exchange lemma any linearly independent set in $\R(A)$ cannot exceed $r$ in size, so in particular any basis for $\R(A)$ can have at most $r$ elements. This means $\text{row rank}(A) \leq r$.

I can now apply this same argument to $A^T$ to conclude $\text{row rank}(A^T) \leq \text{col rank}(A^T)$. But $\text{row rank}(A^T) = \text{col rank}(A) = r$ and similarly for the column rank, so $\text{row rank}(A) \geq r$. This establishes $\text{row rank}(A) = r$.

$\square$

Factoring $A$ as $A = CR$ with $C$ being $m\times r$ and $R$ being $r\times n$ is sometimes called a rank factorization.

Result 3: Rank-nullity.

Let $V,W$ be vector spaces over a field $F$ with $\dim V = n < \infty$ and let $T : V\to W$ be a linear transformation. Then $\rank T + \dim N(T) = \dim V$.

I'm using $\rank T$ equivalently with the dimension of the image of $T$, and similarly $\dim N(T)$ equivalently with the dimension of the kernel of $T$.

Pf: $N(T)\subseteq V$ is a subspace so it has a basis. Letting $\dim N(T) = k$ I'll take $K = \{v_1,\dots,v_k\}\subseteq N(T)$ to be one such basis.

I want to extend $K$ to a basis for all of $V$ which means I'll need to add in $n-k$ more vectors. I can accomplish this via the Steinitz exchange lemma: let $W$ be a basis for $V$ so $|W|=n$. Then $W$ spans $V$ and $K$ is linearly independent so the lemma lets me create a spanning set for $V$ that is of the form $B:= \{v_1,v_2,\dots,v_k, w_{k+1}, \dots, w_n\}$. $|B| = \dim V$ and $B$ spans $V$, so if $B$ was linearly dependent then that means there is a set with size strictly smaller than $\dim V$ that is linearly independent and spans $V$. But that by definition would be a basis and that contradicts $\dim V = n$, so I know $B$ is linearly independent and therefore is a basis. I'll let $S = B \backslash K$ be these new vectors I added.

I'll now consider the image of $T$, i.e. the column space of the corresponding matrix. I have
$$
\text{im}(T) = \{T(x) : x \in V\}
$$
but since $B$ is a basis for $V$ I can express $x$ as $\sum_{i=1}^n \alpha_i b_i$ where $b_1,\dots,b_n$ is an enumeration of $B$ and $\alpha_j\in F$. $T$ being a linear transformation means
$$
\text{im}(T) = \left\{T\left(\sum_{i=1}^n \alpha_i b_i\right) : \alpha \in F^n\right\} = \left\{\sum_{i=1}^n \alpha_i T(b_i) : \alpha \in F^n\right\} = \span T(B).
$$
$T$ maps $K$ to $\0_W$ so actually $\span T(B) = \span T(S)$.

This shows that $T(S)$ spans $\text{im}(T)$, and I'll now show that it's linearly independent which means $T(S)$ is in fact a basis for $\text{im}(T)$.

Suppose I have
$$
\sum_{j=1}^{n-k} \alpha_j T(w_j) = \0_W
$$
for $\alpha_j \in F$. Then by the linearity of $T$ I have
$$
T\left(\sum_{j=1}^{n-k} \alpha_j w_j\right) = \0_W \implies \sum_{j=1}^{n-k} \alpha_j w_j \in N(T) = \span K.
$$
If any of the $\alpha_j$ are non-zero then this means I can express that $w_j$ in terms of the other $w_i$ and $K$ which contradicts $B$ being linearly independent. So in fact $\alpha_1 = \dots = \alpha_{n-k} = 0$ which means $T(S)$ is linearly independent and therefore is a basis.

All together I have $\dim N(T) = k$ and $\dim \text{im}(T) = |S| = n-k$ so
$$
\dim N(T) + \rank T = n
$$
as desired.

$\square$

## The four subspaces

I'll now apply these results. Given the dimensions of $V$, $W$, and the rank of $A$, I immediately know the dimensions of the four subspaces. $\dim \C(A) = r$ by assumption and Result 2 gives me $\dim \R(A) = r$ too. The rank-nullity theorem then tells me that $\dim N(A) = n - r$ and $\dim N(A^T) = m-r$.

Next I'll show that $\R(A)$ and $N(A)$ are orthogonal complements. Suppose $x \in \R(A)\cap N(A)$. Then $Ax = \0$ but also $x = A^Ty$ for some $y \in W$. This means
$$
x^Tx = x^TA^Ty = (Ax)^Ty = \0^Ty = 0
$$
implying $x = \0$. This means that $\R(A)$ and $N(A)$ only share $\0$, which is the smallest intersection they could have since every subspace has to contain $\0$. Next, let $x\in N(A)$ and $y \in \R(A)$ so $y = A^Tz$ for some $z$. Then
$$
x^Ty = x^TA^Tz = (Ax)^Tz = 0 \implies x\perp y
$$
so not only is the intersection of $N(A)$ and $\R(A)$ trivial, but they are orthogonal to each other. Finally, let $B_N$ and $B_R$ be bases for $N(A)$ and $\R(A)$ respectively. Each basis by itself is linearly independent by definition and $b_N\perp b_R$ for all $b_N\in B_N$, $b_R \in B_R$. This means $B := B_N\cup B_R$ is a linearly independent set of size $n$ (since $|B_N| = n-r$ and $|B_R| = r$). The dimension of $V$ is $n$ so $|B|=n$ makes $B$ a basis. This shows that $\R(A)$ and $N(A)$ partition $V$ into two orthogonal pieces. $\C(A)$ and $N(A^T)$ analogously partition $W$ into two orthogonal pieces.

These subspaces also determine whether or not $T$ is a bijection. $T$ is a surjection precisely when $\text{im}(T) = \C(A) = W$, and this happens when $\rank A = m$. That can only happen when $m = \min\{n,m\}$ since if $n < m$ I can't have $\rank A = m$ by Result 2. Similarly $T$ is an injection only when $\ker T = N(A) = \{\0\}$, i.e. the null space is trivial. Suppose $x \in N(A)$ is nonzero. Then for any other $y\in V$ I have $T(y) = \0_W + T(y) = T(x) +T(y) = T(x+y)$ yet $y \neq x+y$ so $T$ is not 1-1. For the other direction if $\ker T = \{\0_V\}$ then if $T(x) = T(y)$ I can rearrange to $\0_W = T(x) - T(y) = T(x-y) \implies x-y\in \ker T$. Since the only element of $\ker T$ is $\0_V$ I then have $x-y=\0 \implies x=y$ so $T$ is indeed an injection. By rank-nullity if I have $\dim N(A) = 0$ then that means $\rank A = n$. $T$ is a bijection when both conditions hold, i.e. $\rank A = n = m$, so $A$ needs to be square with a column space spanning $W$ and a trivial null space (and each of those implies the other).

These subspaces also play an important role in solving linear equations. If I have a vector $b$ then I know $Ax = b$ has a solution if and only if $b \in \C(A)$, so there is an $r$-dimensional subspace of vectors $b$ for which I can solve $Ax=b$. And given a solution to $Ax = b$, say $x_*$, I know $A(x_* + y) = Ax_* = b$ for any $y \in N(A)$ so $A$ maps the entire affine space $x_* + N(A)$ to $b$. This means that the dimension of $N(A)$ controls the number of solutions, in the same way that it controls the injectivity of $T$.

I can also use these results to confirm some aspects of the geometry of linear equations. Suppose I am trying to solve the homogeneous equation $Ax = \0$. If I let $a_1,\dots,a_m$ be the rows of $A$, then this is equivalent to seeking a solution to $a_i^Tx = 0$ for $i=1,\dots,m$. Each $a_i$ is rank $1$ when treated as a linear transformation (I'm assuming $a_i \neq \0$) so by the rank-nullity theorem $N(a_i)$ is $n-1$ dimensional, i.e. each equation $a_i^Tx = 0$ has an $n-1$ dimension solution space which is exactly the hyperplane with $a_i$ as its normal vector. Now when I think about solving $Ax = \0$, so I'm looking for simultaneous solutions to all $m$ equations, I can picture this as seeking elements of the intersection of all $m$ of these $(n-1)$-dimension hyperplanes. If $m \ll n$ and I imagine slowly adding more and more equations, then initially I am solving ${a_1 \choose a_2}x = {0\choose 0}$. If $a_1$ and $a_2$ are linearly dependent then their hyperplanes are the same since the normal vectors are parallel, the matrix ${a_1\choose a_2}$ is still just rank $1$, and the solution space stays at $n-1$ dimensions. But if they're linearly independent then I decrease the dimension of the solution space by one, reflecting that the intersection of two hyperplanes with independent normals is an $n-2$ dimension subspace. Each individual hyperplane has a one dimension space not in it, and when the normal vectors are not collinear then these one dimension spaces are not the same between different equations, so the intersection loses all of the vectors not in both of them. Continuing, if I'm always adding linearly independent equations, at each step the new hyperplane I'm adding removes one more dimension from the solution space by the intersection. When I finally hit $m=n$ then there are no dimensions left and the intersection is just $\{\0\}$, reflecting that $A$ is invertible at this point.

As a final point, in earlier math classes I was taught to describe a line in $\Rbb^3$ by a parametric equation, so something of the form $(x(t), y(t), z(t))$ for $t \in \Rbb$, while a plane is given by the solutions to $\mathbf n^Tx = c$ with $\mathbf n$ as the normal vector. I recall finding it odd that a line was given by specifying the points on it while a plane was given by specifying the dimension that is not occupied. Rank-nullity helps to clear this up: if both are through the origin then a line is of the form $\span \{v\}$ while a plane is of the form $\span\{v,w\}$ with $v$ and $w$ linearly independent. Describing $\span\{v\}$ is easy as it's a one dimension subspace and I'm handed a basis in $\{v\}$. The parametric equations for a line exactly correspond to specifying an element of this subspace via $tv$. A plane is more awkward though as I'd need to describe a vector in it by $tv + sw$. I can make this easier by noting that $M := [v \mid w]\in\Rbb^{3\times 2}$ is a rank $2$ matrix so by rank-nullity $\dim N(M^T) = 1$. I can then describe $\C(M)$, i.e. the plane in question, by getting a basis for $N(M^T)$ and taking the vectors that are orthogonal to this basis since $\C(M)$ is the orthogonal complement to $N(M^T)$. This basis is just one vector so this is easier to do. Furthermore since I'm in $\Rbb^3$ I can get this vector via the cross product which is the usual procedure in these type of problems.

I'll finish by following Gilbert Strang's advice ([here](https://web.mit.edu/18.06/www/Essays/newpaper_ver3.pdf)) to explicitly plot these subspaces for an example with $A \in \mathbb R^{2\times 2}$ and $\rank A = 1$. Since $A$ is rank $1$ I'll factor it as $A = xy^T$ with $x,y\neq \0$. In this case I have
$$
\C(A) = \{Av : v \in \Rbb^2\} = \{x \cdot (y^Tv) : v \in \Rbb^2\}.
$$
$y$ is nonzero so WLOG I can assume $y_1\neq 0$. Taking $v = (r/y_1, 0)^T$ I then have $y^Tv = r$ so this shows that the linear functional $y^T : \Rbb^2\to\Rbb$ is onto since for any $r\in\Rbb$ there is at least one $v\in\Rbb^2$ such that $y^Tv = r$. This means that
$$
\C(A) = \span\{x\}
$$
which is one dimensional and confirms that $\rank A = 1$.

Next, for the null space, I need $Av = xy^Tv = \0$. $x\neq \0$ so this happens exactly when $y^Tv = 0$. In other words, the null space is given by the line with $y$ as its normal vector.

For the other two subspaces I can apply the same ideas to $A^T$ to get
$$
\R(A) = \C(A^T) = \span\{y\}
$$
and
$$
N(A^T) = \{v\in\Rbb^2 : v\perp x\}.
$$

These four subspaces are shown in the picture below.

![The four fundamental subspaces for a rank-1 matrix](/assets/archive/four-subspaces/subspaces.png)

I can see how $\C(A)\perp N(A^T)$ and $\R(A)\perp N(A)$ and every subspace is one dimensional.

I can also work out the eigenvalues of $A$: the characteristic polynomial is given by
$$
\begin{aligned}
&p_A(\lambda) = \det(xy^T - \lambda I_2)
\\&= \det(-\lambda I_2)(1 - \lambda^{-1} y^Tx)
\\&= \lambda(\lambda - y^Tx)
\end{aligned}
$$
(I used the matrix determinant lemma here) so the eigenvalues are $\{0, y^Tx\}$. In the picture above I took $x$ and $y$ to be unit vectors, so if I keep that here then $y^Tx = \cos\theta_{xy}$ meaning the non-zero eigenvalue depends on the angle between $x$ and $y$.

As $x$ and $y$ become increasingly collinear $\cos\theta_{xy}\to 1$ so the limit of that is $A = xx^T$ and then the eigenvalues are $\{0,1\}$ and the eigenspace of $1$ is spanned by $x$ since $Ax = x$.

In the other direction as $x$ and $y$ become increasingly orthogonal I'll have $\cos\theta_{xy}\to 0$. If $x\perp y$ then the eigenvalue of $0$ will have an algebraic multiplicity of $2$, but a geometric multiplicity of only $1$ since the null space of $A$ is given by the one dimensional space orthogonal to $y$ (which happens to be spanned by $x$ in this case). That makes $A$ a defective matrix. I'll also have $A^2 = xy^Txy^T = (y^Tx)xy^T = \mathbf O$ so $A$ is nilpotent.
