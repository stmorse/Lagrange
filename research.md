---
layout: page
title: Research
---

My current research interests are in complex systems, machine learning, and optimization.  Specifically, I am doing work on information spread through social networks, human mobility, and competitive influence maximization.  I am affiliated with [Draper Laboratory](http://www.draper.com) and [HuMNet Lab](http://humnetlab.mit.edu).

<hr>

## Information spread and influence structure

<img align="left" width="50%" src="{{ site.baseurl }}/images/persistent.png" alt="persistent cascades">

A challenge in large-scale passive-collection communication datasets is inferring the true influence structure, or latent network.   We know A called B, but not why, or whether this influences B to call C.  One method to extract meaningful structure is to look for recurring patterns: imagine if we see A call B and C, who call D and E, and then the same pattern (or something very similar) occurs again every week or two over the course of several months.  Using methods of inexact tree matching and hierarchical clustering, we define, find, and analyze these group conversations (termed "persistent cascades") and show they reveal new roles in network topology and spreading dynamics.  <a href="https://github.com/stmorse/cascades">Repo here.</a>.

<a href="{{ site.baseurl }}/docs/BigD348.pdf">Paper</a>&nbsp;&nbsp;&#8226;&nbsp;
<a href="{{ site.baseurl }}/docs/persistent-cascades-ieee.pdf">Slides</a>

Another approach is to define a probabilistic model of the network interaction structure and learn its parameters.  We can model communication events as arrivals in a stochastic process that is dependent on its immediate history (e.g. A calls B increases the probability B calls A back) and other processes (e.g. A to B increases the probability of B to C); in particular the <i>Hawkes process</i>.  In <a href="{{ site.baseurl }}/docs/6-867-final-writeup.pdf">this project</a> we explore the predictive power of univariate Hawkes processes applied to the persistent cascades from above, learning the parameters with a regularized MAP EM scheme.  <a href="https://github.com/stmorse/hawkes">Repo here</a>, and a <a href="https://stmorse.github.io/journal/Hawkes-python.html">blog post</a> about it.

In my (recently completed!) <a href="{{ site.baseurl }}/docs/orc-thesis.pdf">masters thesis</a>, I expand on both of these topics.  E.g. we can extend techniques from percolation theory to model the idea of "persistence" in cascading patterns, and we apply the method to the Hillary Clinton emails and find hidden influencers.  We also extend the MAP EM approach for parameter estimation of a Hawkes process to the multivariate case.  

<hr>

## Trace diagrams

<img align="left" width="50%" src="{{ site.baseurl }}/images/diagrams.png" alt="trace diagrams">

My undergraduate work was with so-called *trace diagrams* (also *birdtracks*, *spin networks*, *tensor diagrams*, ...), which are structured graphs representing multilinear functions.  They are essentially a notational invention which provide a simple, graphical, intuitive way of performing otherwise complicated tensor or matrix calculations.  Versions of this notation show up in physics, and the current incarnation has gained some traction (<a href="http://arxiv.org/pdf/1102.0316.pdf">normal factor</a> <a href="http://arxiv.org/pdf/1004.3833.pdf">graphs</a>, generalization to commutative <a href="http://dl.acm.org/citation.cfm?id=1596553">monads</a>), but it is still a niche area of research.  The paper below defines them in a rigorous way and shows the power of the notation with several extremely short proofs of classic results in linear algebra.

<a href="{{ site.baseurl }}/docs/tracediagrams.pdf">Paper (<i>Involve</i>)</a>&nbsp;&nbsp;&#8226;&nbsp;
<a href="{{ site.baseurl }}/docs/mainthesis.pdf">Undergrad thesis</a>
