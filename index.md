---
layout: home
title: Home
---

Hello, I am Louis Deutsch, and welcome to my personal website.

I am broadly interested in creating useful and actionable solutions to real-world statistical challenges. My favorite problems are the messy ones where there is no hope of a tidy, simple answer. 

---

In August 2026, I graduated with a PhD in statistics from Wharton, at the University of Pennsylvania, where I was advised by [Eugene Katsevich](https://ekatsevi.github.io/). You can find my dissertation [here](https://repository.upenn.edu/server/api/core/bitstreams/fea87d91-cf80-490a-909a-36ed4c715757/content).

My PhD work focused on one particular messy real-world problem: the statistical and computational challenges that arise when analyzing single-cell CRISPR experiments. Researchers have developed many approaches to analyzing these datasets in the 10 years since their inception, but it can be very hard to know which method to choose for a particular dataset (and which method to trust if they disagree). These datasets are very expensive to generate, and new experiments are planned using the results of past ones, so it is a high stakes statistical situation. In my main project I extensively benchmarked these competing methods, measuring their statistical performance in a wide range of settings, plus their runtime and memory scaling. This paper will be submitted very shortly.

Benchmarking sounds simple ("hey [LLM of choice], run each method on each dataset"), but it's really hard to do well. These datasets are very intricate and require a lot of biology to understand. We generally don't have a ground truth, so if two methods disagree it's not clear which is better. Simulations can help, but those results are only interesting if the simulations are biologically plausible, and the data are so complicated that any simulation will have meaningful departures from reality. It can also be quite difficult to use each method correctly, as they are developed by small academic teams and the code often does not do what the paper says it does. Even measuring quantities like runtime has nuance: for example, one problem I encountered was that some methods set different limits on the multithreading of their underlying linear algebra libraries. This meant that methods could have differing amounts of parallelization even when run sequentially, leading to invalid runtime comparisons. At its core, benchmarking is all about getting the fairest possible comparison between fundamentally different things, and that's tricky!

Another problem I studied is the _variable perturbation effectiveness problem_: the genomic perturbations done by CRISPR are not always fully effective, causing a loss in statistical power by essentially mixing control group cells into the treatment group. This project led to _Location tests with noisy proxies for latent variables_ by Louis Deutsch and Eugene Katsevich (2026), published [here](https://www.sciencedirect.com/science/article/pii/S0167715225002354).

I am also a core developer of [SCEPTRE](https://timothy-barry.github.io/sceptre-book/), our lab's toolkit for analyzing these datasets.

---

Before beginning this PhD in August 2022, I spent 6 years as a data scientist where I worked on projects including machine learning for targeted advertising; fraud detection; social network analysis; and survey design with inferential analyses. I obtained a BS in statistics and mathematics, and a MS in statistics, all from the University of Wisconsin-Madison. My undergrad thesis _Investigating the 2-stage ratio estimator: relative performance under model and design_ was advised by [Erik Nordheim](https://stat.wisc.edu/staff/nordheim-rick/). 

In my pre-PhD days, I also used to be very active on the statistics Q&A website Cross Validated. My profile can be found here:

[![jld](https://stats.stackexchange.com/users/flair/30005.png?theme=clean)](https://stats.stackexchange.com/users/30005/jld)

If you are interested in collaborating or discussing anything statistical, feel free to contact me at `louisdeutsch1023 at gmail dot com`. My full CV is available upon request.
