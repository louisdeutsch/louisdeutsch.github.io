---
layout: archive
title: "The degree distribution of the stochastic block model"
date: 2021-08-01
---

$\newcommand{\d}{\mathbf d}\newcommand{\one}{\mathbf 1}\newcommand{\E}{\operatorname{E}}\newcommand{\Var}{\operatorname{Var}}$In this post I'm going to look at the degree distribution of stochastic block models. $n$ will denote the total number of nodes. The nodes will be partitioned into $K$ communities with sizes $m_1, \dots, m_K$ so $n = \sum_{i=1}^K m_i$. The edge probabilities will be given by a symmetric $K\times K$ matrix $P$ with $P_{ij}$ giving the probability that a node in community $i$ and $j$ form an edge. The resulting graphs will be undirected and unweighted. I'll also assume no self edges.

I will let $C \in \{1,\dots, K\}^n$ give the cluster index of each node, so e.g. $C_1$ indicates which community node $1$ belongs to. I can equivalently think of $C$ as a map from the vertex set to $\{1,\dots, K\}$ that assigns each node to a cluster, but I'll use subscript notation to avoid extra parentheses. I'll use $A$ for the adjacency matrix and this means for $i\neq j$
$$
P(A_{ij} = 1) = P_{C_iC_j}.
$$
I'll accomplish my requirement of no self edges by just setting $A_{ii} = 0$ for $i=1,\dots,n$.

The degree vector is given by $A\one$ so the degree of node $i$ is just the row sums (or equivalently the column sums) of $A$. The $A_{ij}$ are independent Bernoulli random variables and are also identically distributed within each community, so
$$
d_i = \sum_{j=1}^n A_{ij} \sim \text{Bin}(m_{C_i} - 1, P_{C_iC_i}) + \sum_{j\neq C_i} \text{Bin}(m_j, P_{jC_i}).
$$
which is a Poisson-Binomial. The $m_{C_i}-1$ in the first binomial is because $A_{ii} = 0$ so within cluster $C_i$ there are only $m_{C_i}-1$ Bernoulli RVs (with non-zero success probabilities, at least).

Since the binomials in question are independent I can easily get the mean and variance of each degree:
$$
\E[d_i] = (m_{C_i}-1)P_{C_iC_i} + \sum_{j\neq C_i} m_j P_{jC_i}
$$
and
$$
\Var[d_i] = (m_{C_i}-1)P_{C_iC_i}(1 - P_{C_iC_i}) + \sum_{j\neq C_i} m_j P_{jC_i}(1 - P_{jC_i}).
$$
Typically it is assumed that the graph is assortive so the diagonal entries of $P$ are larger than the off-diagonal. Whether or not the mean and variance of $d_i$ are dominated by the within-cluster connections depends on the cluster sizes and the extent to which $P_{C_iC_i}$ dominates $P_{jC_i}$ for $j\neq C_i$.

I can also work out $\text{Cov}(d_i, d_j)$:
$$
\text{Cov}(d_i, d_j) = \text{Cov}\left(\sum_{k} A_{ik}, \sum_{\ell} A_{\ell j}\right) = \sum_{k\ell}\text{Cov}(A_{ik}, A_{\ell j})
$$
where I used the fact that $A_{uv} = A_{vu}$. The edge indicators are all independent so the only contributions to the covariance will be when it's the same indicator, i.e. $A_{ik}=A_{\ell j}$ which means $i=\ell$ and $k=j$. There is only one such indicator, and that is for the edge between nodes $i$ and $j$. This makes sense because that's the only edge affected both degrees. Thus
$$
\text{Cov}(d_i,d_j) = \text{Var}(A_{ij}) = P_{C_iC_j}(1 - P_{C_iC_j}).
$$
I can combine this to get the correlation:
$$
\text{Corr}(d_i, d_j) = \frac{P_{C_iC_j}(1 - P_{C_iC_j})}{\sqrt{\Var[d_i] \Var[d_j] }}.
$$
I'm not plugging in the variances since they're long expressions, but it's clear from this that $\max_k m_k \to \infty \implies |\text{Corr}(d_i,d_j)|\to 0$. Intuitively, if any of the community sizes grow large then the single possible edge between nodes $i$ and $j$ matters less and less to the degrees so they are increasingly uncorrelated.

A common thing to explore is whether or not a degree distribution is heavy tailed, as this is a property demonstrated empirically in many real networks. To that end I want an upper bound on $P(d_i \geq a)$.

Result 1: Chernoff bound

Let $S = \sum_{i=1}^n X_i$ for independent $X_i$. Then
$$
P(S \geq s) \leq \inf_{t > 0} e^{-ts} \prod_{i=1}^n \E[e^{tX_i}].
$$
Pf: Let $t > 0$. $P(S \geq s) = P(e^{tS} \geq e^{ts})$ since $s \mapsto e^{ts}$ is a bijection. The requirement that $t > 0$ is essential to keeping the direction of the inequality (e.g. $3 < 4$ but $e^{-3} > e^{-4}$). I can then apply Markov's inequality to get
$$
P(S \geq s) \leq e^{-ts} \E[e^{tS}] = e^{-ts} \prod_{i=1}^n \E[e^{tX_i}]
$$
by independence. Since $P(S\geq s)$ is a lower bound for this quantity for all $t > 0$ then it is also a lower bound to the infimum over $t>0$, i.e.
$$
P(S \geq s) \leq \inf_{t > 0} e^{-ts} \prod_{i=1}^n \E[e^{tX_i}]
$$
as desired. This bound still holds if any of the expectations in question are infinite but it's not very helpful in that case.

$\square$

In my case I have $d_i = \sum_{j=1}^K X_j$ for $X_j \sim \text{Bin}(m_j - \delta_{jC_i}, P_{jC_i})$ with $\delta_{jC_i}$ denoting the Kronecker delta. I'll simplify this notationally by using $X_j \sim \text{Bin}(n_j, p_j)$. This means
$$
P(d_i \geq s) \leq e^{-ts} \prod_{i=1}^K \E[e^{tX_i}].
$$
I now need the MGF of a binomial RV. I also will need a couple other tools.

Result 2: if $X \sim \text{Bin}(n,p)$ then $\E[e^{tX}] = (1 - p + pe^t)^n$.

Pf: I can decompose $X$ as $X = \sum_{i=1}^n Y_i$ for $Y_i \stackrel{\text{iid}}\sim \text{Bern}(p)$. $\E[e^{tY_1}] = e^{t \cdot 0} (1-p) + e^{t\cdot 1} p = 1 - p + pe^t$. This means
$$
\E[e^{tX}]= \prod_{i=1}^n \E[e^{tY_i}] = (1 - p +pe^t)^n.
$$
$\square$

I also will want the following famous inequality:

Result 3: $e^x \geq 1 + x$.

Pf: I will prove this using the definition $e^x = \sum_{k=0}^\infty \frac{x^k}{k!}$. I'll consider three cases: (1) $x \geq 0$; (2) $x \leq -1$; and (3) $-1 < x < 0$.

Case 1:
$$
e^x = 1 + x + \sum_{k=2}^\infty\frac{x^k}{k!} \geq 1 + x
$$
since every term in the series is non-negative (and equality happens only when $x=0$).

Case 2: I want to show that $e^x > 0$ because then the result will follow from $x \leq -1 \implies 1 + x \leq 0 < e^x$. I'll do this by showing that $e^{-x} = \frac 1{e^x} > 0$, which I don't think is at all obvious just from the power series definition. I can do this by expanding the series for $e^x \cdot e^{-x}$:
$$
e^x \cdot e^{-x} = \left(\sum_{k=0}^\infty \frac{x^k}{k!}\right)\left(\sum_{\ell=0}^\infty \frac{(-x)^\ell}{\ell!}\right) = \sum_{k,\ell=0}^\infty \frac{x^k(-x)^\ell}{k!\ell!}
$$
where I'm freely rearranging the series due to absolute convergence. When $k=\ell=0$ I get a term of $+1$. But for each $c \in \mathbb N_+$ I'll have the terms corresponding to $k+\ell = c$ summing to zero. This can be seen by noting that
$$
\sum_{k+\ell = c}\frac{x^k(-x)^\ell}{k!\ell!} = x^c\sum_{j=0}^c \frac{(-1)^j}{j!(c-j)!} = \frac{x^c}{c!} \sum_{j=0}^c {c\choose j} (-1)^j = \frac{x^c}{c!}(1-1)^c= 0
$$
where the second to last equality uses the binomial theorem in reverse. This verifies that $e^x\cdot e^{-x} = 1$, so for $x \leq -1$ I'll have $e^{x} = \frac{1}{e^{-x}}$ and I know the denominator there is positive because every term in the series is positive (since $-x \geq 1$). That shows that $e^x > 0$ for all $x\in\mathbb R$, and then $x \leq -1 \implies 1+x \leq 0 < e^x$ as desired.

Case 3: the final case is for $-1 < x < 0$. For this I can use a rearranging argument that I saw in an answer coming from Noam Elkies [here](https://math.stackexchange.com/questions/504663/simplest-or-nicest-proof-that-1x-le-ex). He rewrote the problem to be showing
$$
e^x - (1 + x) = \sum_{k=2}^\infty \frac{x^k}{k!}\geq 0.
$$
In the series I can group the terms as follows (again relying on absolute convergence):
$$
\sum_{k=2}^\infty \frac{x^k}{k!} = \sum_{k=1}^\infty \left(\frac{x^{2k}}{(2k)!} + \frac{x^{2k+1}}{(2k+1)!}\right) = \sum_{k=1}^\infty \frac{x^{2k}}{(2k+1)!}\left(2k + 1 + x\right ).
$$
$x^{2k} > 0$ and $2k+1+x > 0$ since $x\in(-1,0)$ while $k\geq 1$. This means that every term here is positive so indeed $e^{x} - (1+x) \geq 0$.

These three cases prove the result.

$\square$

As a final comment on this, I can use this result to quickly prove the convexity of $\exp$. I'll just prove midpoint convexity since continuity then finishes it.

Result 4: $\exp$ is midpoint convex.

Pf: let $x,y\in\mathbb R$. I need to show $e^{(x+y)/2} \stackrel?\leq \frac 12(e^x + e^y)$. I know $\exp$ is always positive so I can multiply both sides by $2e^{-(x+y)/2}$ without risking multiplication by zero or changing the direction of the inequality. This means I can equivalently show
$$
2 \stackrel?\leq e^{(x-y)/2} + e^{(y-x)/2}.
$$
I can show this by applying the previous result to both exponentials separately:
$$
e^{(x-y)/2} + e^{(y-x)/2} \geq 1 + \frac{x-y}{2} + 1 + \frac{y-x}{2} = 2.
$$
$\square$

Many of the proofs I saw online for $e^x \geq 1 + x$ used convexity and the fact that $y=1+x$ is a tangent line, but I like this proof of convexity using $e^x\geq 1+x$ so I wanted to prove both starting with the power series definition so I could avoid circularity.

Equipped with this, I can finally get my tail bound. I have
$$
P(d_i \geq s) \leq e^{-ts} \prod_{i=1}^K (1 - p_i + p_ie^t)^{n_i} = \exp\left(-ts + \sum_i n_i \log[1 + p_i(e^t - 1)]\right).
$$
$\exp$ is monotonically increasing so $a \leq b \implies e^a \leq e^b$. This means I can use $1+ x \leq e^x$ on the term inside the log to conclude
$$
P(d_i \geq s) \leq \exp\left(-ts + \sum_i n_i \log[\exp(p_i(e^t - 1))]\right) = \exp\left(-ts + (e^t - 1)\sum_i n_i p_i\right).
$$
If I take $t=1$ this still holds, and in that case I have $P(d_i \geq s) \leq c\cdot e^{-s}$ with $c = \exp((e-1)\mu)$ for $\mu = \sum_i n_ip_i$ (which is just $\E[d_i]$). This shows that the upper tail probabilities decrease exponentially in $s$, so $d_i$ does not have a heavy tail (in the sense of being a power law). $d_i$ is also bounded above by $n-1$ so the tail could only be so heavy because of that, but I think this is still good to confirm.

## Applying the Poisson approximation

I'll finish by using the Poisson approximation to the binomial distribution to get a simpler asymptotic form for $d_i$.

Result 6: if $X \sim \text{Bin}(n,p_n)$ with $np_n\to\lambda$ and $p_n\to 0$ as $n\to\infty$, then $P(X=k) \to \frac{\lambda^ke^{-\lambda}}{k!}$. This is the pmf of a $\text{Pois}(\lambda)$ random variable so this shows $X \stackrel{\text d}\to \text{Pois}(\lambda)$.

Pf: I'll omit the dependence on $n$ for $p$ for simpler notation. I have
$$
P(X=k) = {n\choose k}p^k(1-p)^{n-k} = \frac{n!}{k!(n-k)!}p^k\left(1-\frac{np}{n}\right)^n\left(1-p\right)^{-k}.
$$
I want to keep the $(k!)^{-1}$, $(1-p)^{-k}\to 1$, and since $np\to\lambda$ I'll have $\left(1-\frac{np}{n}\right)^n\to e^{-\lambda}$. The final piece is the binomial coefficient, and for that I'll need to use Stirling's approximation. This is $x! \approx \sqrt{2\pi x}(x/e)^x$, where "$\approx$" here means their ratio tends to $1$ as $x\to\infty$. Plugging this in for $n!$ and $(n-k)!$ I get
$$
\frac{n!}{(n-k)!}\approx \frac{\sqrt{2\pi n}\cdot n^ne^{-n}}{\sqrt{2\pi(n-k)}(n-k)^{n-k}e^{-n+k}} = \sqrt{\frac{n}{n-k}}\left(\frac{n}{n-k}\right)^{n-k}\frac{n^k}{e^k}.
$$
I'll have
$$
\left(\frac{n}{n-k}\right)^{n-k} = \left(1 + \frac k{n-k}\right)^{n-k} \to e^{k}
$$
so
$$
\frac{n!p^k}{(n-k)!}\approx (np)^k \to \lambda^k.
$$
All together this shows the desired convergence.

$\square$

In order to be able to use this approximation I must have $m_{j}P_{jC_i} \to \lambda_{j}$ while $m_{j} \to \infty$ and $P_{jC_i}\to 0$ for $j=1,\dots,K$. This condition here means that the edge probability decreases proportionately as the number of nodes in each community increases, so the expected number of edges stays fixed. This isn't a terribly artificial setting because in real friend networks it seems plausible that the number of expected friends doesn't grow as the population around the person increases.

Making these assumptions, I have
$$
d_i \stackrel{\text d}\to \text{Pois}\left(\sum_{j=1}^K \lambda_j\right)
$$
since these are independent Poisson distributions so their parameters add. This shows that, under these asymptotic conditions, we still do not get heavy tailed behavior from $d_i$, although that's not at all surprising since the required assumptions prevented $d_i$ from blowing up with $m_j\to\infty$.
