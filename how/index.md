---
layout: post
title: how
permalink: /how/
---

{:.first}
# How Verdict works

* TOC
{:toc}

This page gives a brief description on two things: (1) how Verdict can bring massive speedups and (2) how Verdict works with existing database systems without requiring any modifications on them.


## Verdict's approximate query processing

Verdict exploits the fact that commonly-used analytic queries can be accurately answered using a tiny fraction of the entire data, i.e., samples. One basic example is the use of the [central limit theorem](https://en.wikipedia.org/wiki/Central_limit_theorem). That is, the mean of a uniform random sample is an unbiased estimator of the population mean, and the expected error of the sample mean decreases by a factor of $$1 / \sqrt{n}$$, where $$n$$ is the number of the values aggregated for computing the sample mean.

Verdict uses more advanced statistical techniques to answer real-world SQL queries that include filtering predicates, joins, grouping, derived tables, comparison operators against the answers from subqueries, and so on. Estimating the error of the query answers become also complicated as the projections (i.e., select expressions) include arbitrary combinations of `sum`, `count`, `avg`, and `distinct-count`. Verdict handles such complex cases in a principled way by applying our original research contributions in the area of approximate query processing (AQP).


## How Verdict connects to existing SQL-engines?
