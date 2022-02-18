---
title: "The Proof of MAIC"
author:
  - name: Nam-Anh
date: "2022-02-17"
bibliography: citation.bib
csl: citation.csl
output:
  distill::distill_article:
    toc: true
    toc_depth: 3
    pandoc_args: ["--number-sections"]
    code_folding: hide
editor_options: 
  chunk_output_type: console
---

\newcommand{\tp}[1]{{#1}^{\top}}
\newcommand{\bf}[1]{\boldsymbol{#1}}




<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>

```css
.makebox{
border: 2px solid #566573;
border-radius: 5px; 
margin-bottom: 15px;
}
```


</details>
<style type="text/css">
.makebox{
border: 2px solid #566573;
border-radius: 5px; 
margin-bottom: 15px;
}
</style>

</div>



Let $\bf{p} = \tp{(p_1,p_2,\dots,p_n)}$ be propensity score and $\bf X = \tp{(\bf{x}_1,\bf{x}_2, \dots, \bf{x}_n)}$ baseline characteristics of subjects in IPD. Also, $\bf{\mu}$ denotes average vector of baseline characteristics found in SLD. We aim to estimate $\bf p$ such that $\tp{\bf{p}}\bf{X} = \tp{\bf{\mu}}$, i.e. we want to minimize *Kullback Leibler divergence* of $\bf p$ and empirical function with some constraints. Specifically, we want to minimize 

$$
-\tp{\bf{p}}\ln(\frac{1}{n\bf{p}}),
$$

with two constraints:

$$
\tp{\bf{p}}\bf{1} = 1, \quad and \quad \tp{\bf{p}}\bf{X} = \tp{\bf{\mu}}
$$ 

We shall use *Lagrange Multiplier* to solve such an optimization problem as follows.

$$
\begin{aligned}
\mathcal{L} &= -\tp{\bf{p}}\ln(\frac{1}{n\bf{p}})\bf{1} - \lambda\tp{(\tp{\bf{p}}\bf{X} - \tp{\bf{\mu}})} \\
&= \tp{\bf{p}}\ln(n\bf{p})\bf{1} - \lambda[( \tp{\bf{X}} - \bf{\mu}\tp{\bf{1}} )\bf{p}].
\end{aligned}
(\#eq:eq1)
$$

Above equation could be solved by taking derivative w.r.t $\bf{p}$ and $\lambda$, we then equate both derivations to zero and solve such equations. First, Taking derivative $\mathcal{L}$ w.r.t $\lambda$ we obtain 

$$
\tp{\bf{p}}\bf{X} = \tp{\bf{\mu}}.
(\#eq:eq2)
$$

Next, taking derivative w.r.t $p_i$:

$$
\begin{aligned}
&\quad \frac{\partial\mathcal{L}}{\partial p_i} = 0 \\
&\Rightarrow \ln(np_i) +n -\lambda(x_i-\mu) = 0 \\
&\Rightarrow \ln(np_i) = \lambda(x_i-\mu)-n \\
&\Rightarrow p_i = \frac{\exp[\lambda(x_i -\mu)-n]}{n} \\
&\Rightarrow p_i = \frac{e^{-n}}{n}\exp[\lambda(x_i-\mu)]
\end{aligned}
(\#eq:eq3)
$$

thus,

$$
\begin{aligned}
&\quad 1 = \sum_{i=1}^np_i = \frac{e^{-n}}{n}\sum_{i=1}^n\exp[\lambda(x_i-\mu)] \\
& \Rightarrow \frac{n}{e^{-n}} = \sum_{i=1}^n\exp[\lambda(x_i-\mu)]
\end{aligned}
(\#eq:eq4)
$$

substitute \@ref(eq:eq4) to \@ref(eq:eq3)`, we obtain 

::: {.makebox}
\begin{equation}
p_i = \frac{\exp[\lambda(x_i - \mu)]}{\sum_{i=1}^n\exp[\lambda(x_i-\mu)]}
(\#eq:eq5)
\end{equation}
:::

\@ref(eq:eq5) indicates that $p_i$ can be estimated by fitting logistic model in which $z_i= x_i-\mu$ is one covariate, and $z_i$ must be statisfying \@ref(eq:eq2).





```{.r .distill-force-highlighting-css}
```
