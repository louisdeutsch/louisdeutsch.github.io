---
layout: post
title: "The converse to the (standard) weak law of large numbers"
date: 2026-06-28
---

$\newcommand{\E}{\operatorname{E}}\newcommand{\Var}{\operatorname{Var}}$
$\newcommand{\convp}{\stackrel{\text p}{\to}}\newcommand{\convas}{\stackrel{\text{as}}{\to}}$
$\newcommand{\e}{\varepsilon}\newcommand{\1}{\mathbf 1}$

Suppose $X_1, X_2, \dots$ is an iid sequence of random variables. In my [previous post](https://louisdeutsch.github.io/2026/06/27/converse_to_the_strong_law_of_large_numbers.html) on the converse of the strong law of large numbers (SLLN), I showed that 

$$
\E \lvert X_1 \rvert < \infty \iff \bar X_n \text{ has an almost sure limit}.
$$
In this post, I will explore a natural companion question: __does the converse of the standard [weak law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers#Weak_law) (WLLN) hold?__ 

More precisely:

$$
\text{If } \bar X_n \convp \mu \text{ for some } \mu \in \mathbb R, \text{ then must }\E \lvert X_1\rvert  < \infty?
$$

It turns out: no! $\bar X_n$ only having a limit in probability is a strictly weaker assumption. 

For the converse of the SLLN, the key insight was that $\bar X_n$ having an a.s. limit means that our iid sequence $(X_n)$ stays within $\pm n \e$ eventually, for any $\e > 0$ (with probability 1). Via [Borel-Cantelli 2](https://en.wikipedia.org/wiki/Borel%E2%80%93Cantelli_lemma) (BC2), and the independence of our $X_i$, this is equivalent to a bound on the decay of $P(\lvert X_1\rvert > x)$, a bound giving the integrability of $X_1$. 

For the converse of the WLLN, we only have the weaker assumption that $\bar X_n$ has a limit in probability, i.e. for any $\e > 0$ we'll have $P(\vert \bar X_n - \mu \vert > \e) \to 0$ for some $\mu$. This tells us that $\bar X_n$ concentrates near $\mu$, but we don't know anything about specific sample paths $(X_1(\omega), X_2(\omega), \dots)$, so we can't use this BC2 argument. 

As a final aside before beginning, I am referring to this as the "standard" WLLN because it assumes $\E \vert X_1 \vert < \infty$. This could be relaxed slightly, and I discuss this briefly at the very end of this post.

Now, for the result.

__Claim:__ there exists an iid sequence $X_1, X_2, \dots$ of random variables such that $\bar X_n \convp 0$ yet $\E \vert X_1 \vert = \infty$.

__Proof:__

This is a proof of existence, so we need to find a specific distribution with $\E \vert X_1\vert = \infty$ for which we can prove that $\bar X_n \convp 0$.

I will only consider symmetric distributions, so that $0$ is the natural limit of $\bar X_n$, and I'll restrict myself to continuous distributions so that I can use standard calculus tools.

Let $f$ be my density. Integrability is determined by the tails of the distribution, so I will assume that my density is well-defined around $0$ and just consider upper tail probabilities. Suppose $f(x)$ decays like $x^{-\alpha-2}$ for any $\alpha > 0$. Then $\int_c^\infty x \cdot x^{-\alpha-2}\,\text dx < \infty$, so this distribution has a finite mean. That means I need $f$ to decay slower than $x^{-\alpha-2}$ for any $\alpha > 0$.

But $f$ can't decay _too_ slowly: the Cauchy density has tails that decay exactly at the rate $x^{-2}$, and it is a well-known fact that if $X_1, X_2, \dots$ are iid standard Cauchy, then $\bar X_n$ is also standard Cauchy. That means that tails like $x^{-2}$ are too slow to allow $\bar X_n$ to get squashed to something.

So I need tails slower than $x^{-\alpha-2}$ for any $\alpha > 0$, but faster than $x^{-2}$. In other words, I need tails like $1 / (x^2 h(x))$ for some $h(x) \to \infty$, but slower than any $x^\alpha$ with $\alpha > 0$. The natural choice in such situations is $h(x) = \log x$, and that turns out to work!

I will therefore take

$$
f(x) = \frac 1c \frac{1}{x^2 \log \vert x\vert } \mathbf 1_{\vert x\vert \geq 2}
$$

with normalizing constant $c = 2 \int_2^\infty \frac{1}{x^2\log x}\,\text dx$. 

Verifying that $\E \vert X_1\vert =\infty$:

$$
\E \vert X_1\vert = \frac 2c \int_2^\infty \frac{\text dx}{x\log x} = \frac 2c \int_{\log 2}^\infty \frac {\text dt}t = \infty.
$$

Now I can restate my goal more specifically:

__Claim: if $X_1, X_2, \dots \stackrel{\text{iid}}\sim f$, then $\bar X_n \convp 0$.__

I will take inspiration from the standard proof of the WLLN using triangular arrays (I learned this from Durrett (see [chapter 2](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf))), and use truncations. A truncated random variable $Y$ is something like $Y \mathbf 1_{\vert Y\vert \leq c}$, so it is equal to $Y$ on $[-c,c]$ and $0$ outside of this finite interval. This ensures that all moments of $Y$ are finite, so it will be much easier to work with. We then do our asymptotics by letting $c$ get bigger and bigger, so in the limit we are reasoning about $Y$ itself. The key steps are to show that the truncated sum has the desired limit, and that the error (the difference between the actual sum and the truncated sum) goes to $0$ in probability.



_Step 1: the truncated cumulative mean approaches $0$ in probability._

Let $S_n = \sum_{k=1}^n X_k$ be our actual sum, so we are trying to show $\frac 1n S_n \convp 0$. Then define $X_{nk} = X_k \mathbf 1_{\vert X_k\vert \leq n}$ (this definition follows the triangular array WLLN setup). We could consider truncating to a sequence $b_n \to \infty$ that grows faster or slower than $n$, but we are analyzing a sample mean, and so $n$ is the natural choice to start with. And it ends up working. We will therefore analyze the behavior of our truncated sum

$$
S_n' := \sum_{k=1}^n X_{nk} = \sum_{k=1}^n X_k \mathbf 1_{\vert X_k\vert \leq n}.
$$

Fix $\e > 0$. By definition, we need to show $P(\vert \frac 1n S_n' \vert > \e ) \to 0$. Even though our $X_i$ don't have a finite mean or variance, our truncations $X_{nk}$ do (and $\E X_{nk} = 0$ by symmetry), so we can use Chebyshev's inequality:

$$
P\left (\left \vert \frac 1n S_n' \right \vert > \e \right ) \leq \frac{\Var[S_n'] }{n^2 \e^2} = \frac{\Var[X_{n1}]}{n \e^2} \leq \frac{\E[X_{n1}^2]}{n \e^2}.
$$

We need to show that this goes to zero. We have

$$
\frac{\E[X_{n1}^2]}{n} \propto \frac 1n \int_2^n \frac{\text dx}{\log x}. 
$$

That integral is a translation of the special function $\operatorname{li}(x)$, which comes up a lot ([especially in number theory](https://en.wikipedia.org/wiki/Logarithmic_integral_function#Number_theoretic_significance)), but we don't actually need any of those fancy properties. Both $n$ and $\E[X^2_{n1}]$ go to $\infty$ as $n$ increases, so all we need here is L'Hôpital's rule: 

$$
\lim_{n\to\infty}  \frac 1n \int_2^n \frac{\text dx}{\log x} = \lim_{n\to\infty} \frac{\text d}{\text dn}\int_2^n \frac{\text dx}{\log x} = \lim_{n\to\infty}\frac 1{\log n} = 0.
$$

Easy as that! We have therefore shown that $\frac 1n S_n' \convp 0$.

_Step 2: the truncation error goes to $0$ in probability._


Let $X_{nk}^c = X_k -  X_{nk} = X_k \1_{\vert X_k\vert > n}$, and define $S_n^c = \sum_{k=1}^n X_{nk}^c$ (I'm using a superscript $c$ to denote "complement"). We have $S_n = S_n' + S_n^c$, so if we can show that $S_n^c \convp 0$, then we are done by [Slutsky's theorem](https://en.wikipedia.org/wiki/Slutsky%27s_theorem).

Goal: show that $P(\vert \frac 1n S_n^c \vert > \e) \to 0$.

It turns out we can show something even stronger: that $S_n^c \convp 0$, without even needing the division by $n$. The key idea here is that $\vert S_n^c \vert > \e$ is only possible if at least one $X_{nk}^c$ is not zero, so the event $\\{\vert S_n^c \vert > \e\\}$ is contained in the event $\\{\text{at least one of } X_{n1}^c, \dots, X_{nn}^c \text{ is not zero}\\}$. This means

$$
P(\vert  S_n^c \vert > \e) \leq P(\vert X_{1} \vert > n \cup \dots \cup \vert X_n \vert > n) \leq n P(\vert X_1 \vert > n)
$$

by the union bound and the $X_i$ being iid. $n$ is large, so 

$$
P(\vert X_1 \vert > n) = \frac 2c \int_{n}^\infty \frac{\text dx}{x^2 \log x}.
$$

Integrating by parts, 

$$
\int_{n}^\infty \frac{\text dx}{x^2 \log x} = -\frac{1}{x\log x} \bigg\vert_{n}^\infty - \int_n^\infty \frac{\text dx}{x^2 (\log x)^2} \leq \frac{1}{n\log n}.
$$
This means $n P(\vert X_1 \vert > n) \to 0$, and we are done!


$\square$

To put a cap on this, we found that if we have a density with tails that decay like $\frac{1}{x^2 \log x}$, then this random variable is not integrable, but the tails are light enough that the cumulative average still gets squashed to zero in probability. This shows that $\E \vert X_1 \vert <\infty$ is a strictly stronger assumption than $\bar X_n$ having a limit in probability. This ultimately isn't too surprising, because convergence a.s. is strictly stronger than convergence in probability, and my previous post showed that $\E \vert X_1 \vert <\infty$ is equivalent to $\bar X_n$ having an a.s. limit. 

What we found here also is no accident: Theorem 2.2.12 in Durrett says the following:

Let $X_1, X_2, \dots$ be iid with $x P(\vert X_1 \vert > x) \to 0$ as $x \to \infty$. Let $S_n = \sum_{k=1}^n X_k$ and let $\mu_n = \E[X_1 \1_{\vert X_1 \vert \leq n}]$. Then $S_n/n - \mu_n \convp 0$.

The particular distribution that I used in my proof satisfies this condition, with $\mu_n = 0$, so we could have just checked this and then been done!

If we had assumed that $x P(\vert X_1 \vert > x) \to 0$ as $x \to \infty$, then step 2 of my proof would work exactly as written for arbitrary random variables satisfying our assumptions, since the only time I used the specifics of my $f$ was to arrive at this conclusion. The main difference between my proof for this one example, and the full proof of Theorem 2.2.12, is showing that step 1 also goes through, i.e. that $n^{-1} \E[X_1^2 \1_{\vert X_1\vert \leq n}] \to 0$ is also still true. This one is more work, and Durrett's proof of Theorem 2.2.12 has the details. We'd also need to add back in the centerings $\mu_n$.

Durrett also mentions that the assumption of $x P(\vert X_1 \vert > x) \to 0$ as $x \to \infty$ cannot be further relaxed, citing Feller, so it really is no coincidence that my example satisfied this! The union bound gives us

$$
P(\vert X_1 \vert > n \cup \dots \cup \vert X_n\vert > n) \leq n P(\vert X_1 \vert > n),
$$

so intuitively $x P(\vert X_1 \vert > x) \to 0$ is like requiring that we eventually do not see any order $n$ fluctuations in our sample, since fluctuations on that order will cause fluctuations in the cumulative mean and prevent it from converging. And since we are talking about large deviations, it's unlikely that we will see lots of them, so the union bound isn't overcounting by much. 


