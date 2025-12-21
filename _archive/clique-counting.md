---
layout: archive
title: "Clique counting and The Twelve Days of Christmas"
date: 2021-12-01
---

Yesterday at the office we played a rousing game of holiday trivia, and one question asked for the total number of gifts given in the song The Twelve Days of Christmas. This requires determining
$$
S(d) := \sum_{n=1}^{d} (1 + 2 + \dots + n)
$$
for $d=12$ since each verse adds a new number of gifts to the total number in the previous verse.

I computed this directly using some standard formulas: letting $T_n^{(k)} = 1^k + 2^k + \dots + n^k$, in an older blog post (link TBD) I proved the classic results that
$$
T_n^{(1)} = \frac{n(n+1)}{2}
$$
and
$$
T_n^{(2)} = \frac{n(n+1)(2n+1)}{6}.
$$
This means
$$
S(d) = \sum_{n=1}^{d} T_n^{(1)} = \sum_n \frac{n(n+1)}{2} = \frac 12 \left(T_d^{(2)} + T_d^{(1)}\right).
$$
This can be simplified to $S(d) = \frac{d(d+1)(d+2)}{6}$ so in particular $S(12) = 364$.

I'll first give an alternate proof that directly derives this from a counting argument, and then I'll generalize this to a clique counting problem.

Result 1: $S(d) = {d+2\choose 3}$.

Pf: One proof of $T_n^{(1)} = \frac{n(n+1)}{2} = {n+1\choose 2}$ uses the fact that if there are $n+1$ people shaking hands, then person $1$ shakes $n$ hands, person $2$ shakes $n-1$ hands, and so on, so overall $1+2+\dots+n$ handshakes occur. That's one handshake per pair of people for ${n+1\choose 2}$ in total. Another way to look at this is $T_n^{(1)}$ counts the number of edges in $K_{n+1}$, the complete graph on $n+1$ vertices.

Suppose now that we have $n+2$ people. If we remove person $1$, the remaining people can complete ${n+1\choose 2}$ handshakes. If we then additionally remove person $2$, there are ${n\choose 2}$ handshakes possible. Continuing on, we have
$$
\sum_{j=1}^{n+1}{j \choose 2}
$$
handshakes in all, and this is equal to $S(n)$, although it's not the form we want. The key insight is as follows: the number of handshakes with person $1$ dropped out is equivalent to the number of triangles in the graph that have person $1$ as a vertex. And then the number of handshakes with person $1$ and $2$ dropped out is equivalent to the number of triangles that have person $2$ as a vertex that we did not already count, i.e. we are excluding the triangles with both persons $1$ and $2$ as vertices. This means that overall this process is counting the number of triangles in $K_{n+2}$, so
$$
S(d) = {d+2\choose 3}.
$$
$\square$

Computing $S(d)$ was fun, but it's begging the following question: $T_n^{(1)}$ gives the sum of the first $n$ integers. $S(d)$ gives the sum of the first $d$ sums of integers. What's next? And can we relate it to a graph counting problem for a tidy formula?

Define $S_0 : \mathbb N\to \mathbb N$ by $S_0(n) = 1$ everywhere. Then for $m\geq 1$ define functions $S_m: \mathbb N\to\mathbb N$ by
$$
S_{m}(d) = \sum_{n=1}^d S_{m-1}(n)
$$
so e.g.
$$
S_1(d) = \sum_{n=1}^d S_0(n) = d,
$$
$$
S_2(d) = \sum_{n=1}^d S_1(n) = T_n^{(1)} = {d+1\choose 2},
$$
and
$$
S_3(d) = \sum_{n=1}^d S_2(n) = \sum_{n=1}^dT_n^{(1)} = {d+2\choose 3}
$$
gives my original $S(d)$. I want an expression for $S_m(d)$ in general, and I can again use a counting argument.

Result 2:
$$
S_m(d) = {d + m - 1 \choose m}
$$
Pf: I'll proceed by induction on $m$. I showed in Result 1 that $S_3(d)$ counts the number of triangles in $K_{d+2}$. In other words, $S_3(d)$ counts the number of cliques of size $3$ that appear in $K_{d+2}$ (and I can think of the edges that $S_2$ counts as cliques of size $2$). Suppose this property holds up to $S_m(d)$ and consider $S_{m+1}(d)$.

The argument used in Result 1 applies directly. Considering a graph with $d+(m+1)-1 = d+m$ nodes, if I drop out node $1$ I will have ${d+m-1\choose m}$ cliques of size $m$ which is $S_m(d)$ by the inductive hypothesis. If I then drop out node $2$ as well I will have ${d+m-2\choose m} = S_m(d-1)$ cliques of size $m$, and so on. This means that I can count the number of cliques of size $m+1$ by either $\sum_{n=1}^d S_m(n)$ or by ${d+m\choose m+1}$, so $S_{m+1}(d) = {d+m\choose m+1}$.

$\square$

So overall it turns out that these recursive sums of sums of integers are equivalent to counting cliques of a certain size in complete graphs.
