---
layout: post
title: "The converse of the strong law of large numbers"
date: 2026-06-27
---

$\newcommand{\E}{\operatorname{E}}\newcommand{\convp}{\stackrel{\text p}{\to}}$
$\newcommand{\convas}{\stackrel{\text{as}}{\to}}\newcommand{\e}{\varepsilon}$

Suppose $X_1, X_2, \dots$ is an iid sequence of random variables. The [strong law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers#Strong_law) (SLLN) is the result that

$$
\E\lvert X_1\rvert <\infty \implies \bar X_n \convas \E X_1.
$$

In this post, I will explore the following question: __does the converse of the SLLN hold?__

More precisely:

$$
\text{If } \bar X_n \convas \mu \text{ for any } \mu \in \mathbb R, \text{ then must }\E \lvert X_1\rvert  < \infty \text{ and } \E X_1 = \mu?
$$

It turns out: yes!

__Proof:__

First, I'm going to step back and explain our plan of attack. We need to connect a sequence having an almost-sure limit, to a statement about the heaviness of the tails of the distribution of $X$. The key bridge here will be the second [Borel-Cantelli lemma](https://en.wikipedia.org/wiki/Borel%E2%80%93Cantelli_lemma) (BC2): it states that if we have an independent sequence of events $A_1, A_2, \dots$, with the property that $\sum_{n=1}^\infty P(A_n) = \infty$, then $P(\text{infinitely many }A_n\text{ occur}) = 1$. If our $A_n$ are something like $\{\lvert X_n \rvert > a_n\}$, for some $a_n \to \infty$, then $\sum_{n=1}^\infty P(A_n)$ tells us about the tails of $X$ (since that sum tells us how quickly the probability of large deviations goes to zero). And whether or not infinitely many of the $A_n$ occur tells us about whether or not the sequence $X_1,X_2,\dots$ has a limit after some normalization.

But right now, that is the opposite direction. We will therefore use the contrapositive of BC2, which is that

$$
P(\text{infinitely many }A_n\text{ occur}) < 1 \implies \sum_{n=1}^\infty P(A_n) < \infty.
$$

That is how we will connect the almost-sure convergence of our sequence to the heaviness of the tails.


Now, for the proof.

_Step 1: simplify the sequence._

First, that $\mu$ isn't adding anything, and it will be sufficient to prove this assuming $\mu = 0$. Here's why: by definition,

$$
\bar X_n \convas \mu \iff P\left(\lim \bar X_n = \mu\right) = 1.
$$

Thus by definition there exists an event $E$ with $P(E) = 1$, and $\bar X_n(\omega) \to \mu$ deterministically, just as a sequence in $\mathbb R$, for every $\omega \in E$. This is equivalent to $\frac 1n \sum_{i=1}^n (X_i - \mu)(\omega) \to 0$, so $\bar X_n \convas \mu$ if and only if $\bar X_n' \convas 0$ with $X_i' = X_i - \mu$. So if we prove the result for $\bar X_n' \convas 0$, we have our original result. I will therefore assume $\mu = 0$, so we have $\bar X_n \convas 0$.

Next, we want a sequence where each term is independent, so that this better matches with BC2. Right now, our $n$th term involves $X_1, \dots, X_n$, and so the terms of our sequence are dependent.

Let $S_n = X_1 + \dots + X_n$, so we have $S_n/n \convas 0$. Again let $E$ be our event with prob. 1 where $S_n/n \to 0$ pointwise. Then

$$
\frac{S_n(\omega)}{n} = \underbrace{\frac{S_{n-1}(\omega)}{n-1}}_{\to 0,\text{ by assump.}} \underbrace{\frac{n-1}{n}}_{\to 1} + \frac{X_n(\omega)}{n} \to 0
$$

so it must be that $X_n(\omega)/n \to 0$. This is for any $\omega\in E$, so $X_n/n \convas 0$. We now have our sequence with independent terms.


_Step 2: apply BC2._

On $E$, again, we have $X_n/n \to 0$ pointwise. Fix $\e > 0$. Then, by the definition of convergence, we have

$$
\exists N : n > N \implies \lvert X_n(\omega)/n\rvert < \e.
$$

In other words, $\lvert X_n \rvert  > n \e$ can only happen finitely many times, everywhere on a set with prob. 1. We now can use BC2: let $A_n = \{\lvert X_n \rvert  > n \e\}$. We just showed that

$$
P\left(\lvert X_n \rvert  > n \e \text{ infinitely often}\right) = P\left(A_n \text{ occur infinitely often}\right) = 0.
$$

BC2 then tells us that

$$
\sum_{n=1}^\infty P(A_n) = \sum_{n=1}^\infty P(\lvert X_n \rvert  > n \e) < \infty.
$$

_Step 3: connecting this to the mean._

Our $X_i$ are iid, so $P(\lvert X_n \rvert  > n \e) = P(\lvert X_1 \rvert  > n \e)$. This means we have

$$
 \sum_{n=1}^\infty P(\lvert X_1 \rvert  > n \e) < \infty.
$$

There's a really useful result in math stats: for a non-negative random variable $Y$,

$$
\E Y = \int P(Y > y)\,\text dy.
$$

To see why this is true, I'll sketch it assuming a density:

$$
\E Y = \int_0^\infty y f(y)\,\text dy = \int_0^\infty \int_0^y f(y)\,\text dt\,\text dy = \int_0^\infty \int_t^\infty f(y)\,\text dy\,\text dt = \int_0^\infty P(Y>t)\,\text dt,
$$

with the key step being the exchange of the order of integration. This is justified by [Tonelli's theorem](https://en.wikipedia.org/wiki/Fubini%27s_theorem#Tonelli's_theorem_for_non-negative_measurable_functions), with the added plus that it also still holds even if $\E Y = \infty$. I've seen this referred to by various names but I don't know a canonical one!

Back to the result: we have

$$
\sum_{n=1}^\infty P(\lvert X_1  / \e\rvert > n) < \infty.
$$

Let $Y := \lvert X_1 /\e\rvert$ be our non-negative random variable. $P(Y>y)$ is non-increasing, so $\sum_{n=1}^\infty P(Y>n)$ is an upper bound to $\int_1^\infty P(Y>y)\,\text dy$. And $\int_0^1 P(Y>y)\,\text dy \leq 1$, so this is enough to guarantee $\E Y = \E\lvert X_1 /\e\rvert < \infty$, and therefore $\E\lvert X_1 \rvert  < \infty$.

Having $\E\lvertX_1\rvert < \infty$ means the strong law of large numbers applies to the $X_i$, so $\bar X_n \convas \E X_1$. But we already know this sequence has an almost sure limit of $0$, so $0 = \E X_1$.

Finally, to recover the original result for $\mu \neq 0$, I'll use the notation $X_i' = X_i - \mu$, so we actually just showed $\E\lvert X_1 '\rvert  < \infty$ and $\E X_1' = 0$. For any random variable $Y$, $Y$ is integrable if and only if an arbitrary translation $Y-c$ is integrable via the triangle inequality:

$$
\begin{aligned}
\E \lvert Y \rvert  < \infty &\implies \E\vert Y-c\rvert \leq \E\lvert Y \rvert  + \lvert c \rvert  < \infty \\
\E \lvert Y-c\rvert < \infty &\implies \E\lvert Y \rvert  = \E\lvert Y-c+c\rvert \leq \E \lvert Y-c \rvert + \lvert c \rvert  < \infty.
\end{aligned}
$$

This means our original $X_1$ is also integrable, and $\E[X_1'] = 0 \implies \E[X_1] = \mu$.


$\square$

As a parting thought, we get another insight from BC2 here: suppose our iid sequence $X_1, X_2, \dots$ now is not integrable, i.e. $\E\lvert X_1 \rvert  = \infty$. Then

$$
\sum_{n=1}^\infty P(\lvert X_1 \rvert  > n \e) = \infty,
$$

so by BC2 we have

$$
P(\lvert X_n \rvert  > n\e \text{ infinitely often}) = 1.
$$

So in an iid sequence of RVs with $\E\lvert X_1 \rvert  = \infty$, we will see fluctuations of order $n$ infinitely often (with probability 1). In an integrable sequence, we will see fluctuations of this size only finitely often (with probability 1). We almost surely will not realize sequences that hit values proportional to $n$ infinitely often.

We can easily extend this to other moments: a more general form of the tail integration moment formula is

$$
\E\lvert X \rvert ^p = \int_0^\infty p x^{p-1}P(\lvert X \rvert  > x)\,\text dx = \int_0^\infty P(\lvert X \rvert  > t^{1/p})\,\text dt.
$$

Thus if $\E\lvert X_1 \rvert ^p = \infty$, then

$$
P(\lvert X_n \rvert  > n^{1/p}\e \text{ infinitely often}) = 1.
$$

So, as an example of what this can tell us, suppose the $X_i$ have a finite mean, but an infinite variance (such RVs can be useful for modeling real-world heavy tailed phenomena, like in finance). This means that we will only have fluctuations of order $n$ finitely often, but we will have fluctuations of order $\sqrt n$ infinitely often. I think this is a really useful way to get an intuitive feel for what this modeling choice really means!

