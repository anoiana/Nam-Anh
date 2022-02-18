---
title: "Limit of Supremum and Infimum"
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




# $\pmb{\lim\sup}$ and $\pmb{\lim\inf}$ of a sequence of real numbers

Suppose we have a bounded sequence of real numbers $(c_n)^{\infty}_{n=1}$. Also define 
$$
\begin{aligned}
a_n &:= \inf\{c_k: k \ge n\}, \\
b_n &:= \sup\{c_k:k \ge n\}.
\end{aligned}
$$
We deem two cases that likely occur with $(c_n)_{n=1}^{\infty}$

## $\lim\inf$

If $(a_n)$ is bounded and increasing, then it has a limit called $a$. We can define the so-called $\lim\inf$ of such a sequence as follows
$$
\lim_{n}\inf(c_n) = \lim_{n \to \infty}\inf\{c_k: k \ge n\} 
$$

The following figure illustrates for $\lim\inf$: 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>f</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/Vectorize.html'>Vectorize</a></span><span class='op'>(</span><span class='kw'>function</span><span class='op'>(</span><span class='va'>n</span><span class='op'>)</span> <span class='op'>(</span><span class='op'>-</span><span class='fl'>1</span><span class='op'>)</span><span class='op'>^</span><span class='va'>n</span><span class='op'>*</span><span class='op'>(</span><span class='va'>n</span><span class='op'>+</span><span class='fl'>5</span><span class='op'>)</span><span class='op'>/</span><span class='va'>n</span><span class='op'>)</span>
<span class='va'>n</span> <span class='op'>=</span> <span class='fl'>1</span><span class='op'>:</span><span class='fl'>30</span>
<span class='va'>s</span> <span class='op'>=</span> <span class='fu'>f</span><span class='op'>(</span><span class='va'>n</span><span class='op'>)</span>
<span class='va'>d</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/cbind.html'>cbind</a></span><span class='op'>(</span><span class='va'>n</span>,<span class='va'>s</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/base/Random.html'>set.seed</a></span><span class='op'>(</span><span class='fl'>200</span><span class='op'>)</span>
<span class='va'>r</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/stats/Uniform.html'>runif</a></span><span class='op'>(</span><span class='fl'>100</span>,<span class='op'>-</span><span class='fl'>1</span>,<span class='fl'>0</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/plot.default.html'>plot</a></span><span class='op'>(</span><span class='va'>d</span><span class='op'>[</span><span class='va'>d</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span><span class='op'>&lt;</span><span class='fl'>0</span>,<span class='op'>]</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, ylim <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='op'>-</span><span class='fl'>6</span>,<span class='fl'>0</span><span class='op'>)</span>, cex <span class='op'>=</span> <span class='fl'>1.2</span>, ylab <span class='op'>=</span> <span class='st'>"sequence (c)"</span>,type <span class='op'>=</span> <span class='st'>"o"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/points.html'>points</a></span><span class='op'>(</span><span class='va'>r</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, col <span class='op'>=</span> <span class='st'>"red"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/abline.html'>abline</a></span><span class='op'>(</span>h <span class='op'>=</span> <span class='op'>-</span><span class='fl'>1</span>, col <span class='op'>=</span> <span class='st'>"red"</span><span class='op'>)</span>
</code></pre></div>

</details><div class="figure" style="text-align: center">
<img src="z-limit-of-sup-inf_files/figure-html5/unnamed-chunk-1-1.png" alt="example for $\lim\inf$" width="624" />
<p class="caption">(\#fig:unnamed-chunk-1)example for $\lim\inf$</p>
</div>

</div>

as $n$ goes to $\infty$, $(c_n)$ approaches to $-1$ (never reach $-1$). The term *inf* or *infimum* is also called _the greatest lower bound_, this is, the greatest point of lower bound set. We can see that all black filled circle are less than $-1$, so all of them can be deemed lower bound of the collection of red filled circles, and a point corresponding $n$ as $n \to \infty$ is the greatest point. By definition, this is called _infimum_ or _the greatest lower bound_. 

## $\lim\sup$

Similarly, 

If $(b_n)$ is bounded and decreasing, it has a limit called $b$. We can define the so-called $\lim\sup$ of such a sequence as follows
$$
\lim_n\sup(c_n) = \lim_{n \to \infty}\sup\{c_k: k \ge n\}.
$$

The following figure illustrates for _sup_ or _supremum_

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/base/Random.html'>set.seed</a></span><span class='op'>(</span><span class='fl'>200</span><span class='op'>)</span>
<span class='va'>r</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/stats/Uniform.html'>runif</a></span><span class='op'>(</span><span class='fl'>100</span>,<span class='fl'>0</span>,<span class='fl'>1</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/plot.default.html'>plot</a></span><span class='op'>(</span><span class='va'>d</span><span class='op'>[</span><span class='va'>d</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span><span class='op'>&gt;</span><span class='fl'>0</span>,<span class='op'>]</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, cex <span class='op'>=</span> <span class='fl'>1.2</span>, ylab <span class='op'>=</span> <span class='st'>"sequence (c)"</span>, ylim <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>0</span>,<span class='fl'>3.5</span><span class='op'>)</span>,type <span class='op'>=</span> <span class='st'>"o"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/points.html'>points</a></span><span class='op'>(</span><span class='va'>r</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, col <span class='op'>=</span> <span class='st'>"red"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/abline.html'>abline</a></span><span class='op'>(</span>h <span class='op'>=</span> <span class='fl'>1</span>, col <span class='op'>=</span> <span class='st'>"red"</span><span class='op'>)</span>
</code></pre></div>

</details><div class="figure" style="text-align: center">
<img src="z-limit-of-sup-inf_files/figure-html5/unnamed-chunk-2-1.png" alt="example for $\lim\sup$" width="624" />
<p class="caption">(\#fig:unnamed-chunk-2)example for $\lim\sup$</p>
</div>

</div>


In this case, all black filled circles are greater than $1$, the sequence goes to $1$ as $n$ approaches to $\infty$ (never reach $1$). Thus, they are upper bounds of the collection of red filled circles. Also, a point corresponding $n$ as $n \to \infty$ is the least point of the upper bound set. This is called _supremum_ or _the least upper bound_. 

If we combine both figures, we obtain

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/base/Random.html'>set.seed</a></span><span class='op'>(</span><span class='fl'>200</span><span class='op'>)</span>
<span class='va'>r</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/stats/Uniform.html'>runif</a></span><span class='op'>(</span><span class='fl'>500</span>,<span class='op'>-</span><span class='fl'>1</span>,<span class='fl'>1</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/plot.default.html'>plot</a></span><span class='op'>(</span><span class='va'>d</span><span class='op'>[</span><span class='va'>d</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span><span class='op'>&lt;</span><span class='fl'>0</span>,<span class='op'>]</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, ylim <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='op'>-</span><span class='fl'>6</span>,<span class='fl'>6</span><span class='op'>)</span>, cex <span class='op'>=</span> <span class='fl'>0.7</span>, ylab <span class='op'>=</span> <span class='st'>"sequence (c)"</span>, type <span class='op'>=</span> <span class='st'>"o"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/points.html'>points</a></span><span class='op'>(</span><span class='va'>d</span><span class='op'>[</span><span class='va'>d</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span><span class='op'>&gt;</span><span class='fl'>0</span>,<span class='op'>]</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, cex <span class='op'>=</span> <span class='fl'>0.7</span>, ylab <span class='op'>=</span> <span class='st'>"sequence (c)"</span>, ylim <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>0</span>,<span class='fl'>3.5</span><span class='op'>)</span>, type <span class='op'>=</span> <span class='st'>"o"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/points.html'>points</a></span><span class='op'>(</span><span class='va'>r</span>, pch <span class='op'>=</span> <span class='fl'>16</span>, col <span class='op'>=</span> <span class='st'>"red"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/abline.html'>abline</a></span><span class='op'>(</span>h <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='op'>-</span><span class='fl'>1</span>,<span class='fl'>1</span><span class='op'>)</span>, col <span class='op'>=</span> <span class='st'>"red"</span><span class='op'>)</span>
</code></pre></div>

</details><div class="figure" style="text-align: center">
<img src="z-limit-of-sup-inf_files/figure-html5/unnamed-chunk-3-1.png" alt="example for $\lim\sup$" width="624" />
<p class="caption">(\#fig:unnamed-chunk-3)example for $\lim\sup$</p>
</div>

</div>


Two horizontal red lines represent for $a_n$ and $b_n$, and when $n$ goes to $\infty$ they approach to $-1$ and $1$, respectively. If we extend the interval $[-1,1]$ to a general interval $[a,b]$, then  $[a_{n+1},b_{n+1}]$ will be contained in $[a_n,b_n]$ for all $n$. We can say $\lim_{n \to \infty}[a_n,b_n] = [-1,1]$. This is, as $n \to \infty$ it approaches to the shortest length interval of $[a_n,b_n]_{n=1}^{\infty}$. 

We can also express the above idea by intersection, ie., the intersection of all intervals $[a_n,b_n]_{n = 1,2,\dots}$ is $[a,b] \equiv [-1,1]$. In other words, intersection can be utilized to express the "smallest" magnitude of length, area, volume, etc. This idea can be extended to _limits of sets_.

# $\lim\sup$ and $\lim\inf$ of a sequence of sets

Let $A_n \subset \Omega$, we define

$$
\inf_{k \ge n}A_k := \bigcap_{k=n}^{\infty}A_k; \quad \sup_{k \ge n}A_k := \bigcup_{k=n}^{\infty}A_k
(\#eq:eq1)
$$

thus,

\begin{align}
&\lim_{n \to \infty}\inf A_n = \bigcup_{n=1}^{\infty}\bigcap_{k=n}^{\infty}A_k, \\
&\lim_{n \to \infty}\sup A_n = \bigcap_{n=1}^{\infty}\bigcup_{k=n}^{\infty}A_k
(\#eq:eq2)
\end{align}

To get the intuition about $\lim\sup$ and $\lim\inf$ in term of union and intersection, let us consider Eq.\@ref(eq:eq1) in details. We will consider the case of intersection, second case can be explained in the same way.

Suppose $n= 10$, 

$$
\begin{aligned}
&B_1 = \bigcap_{i=1}^{10}A_i \\
&B_2 = \bigcap_{i=2}^{10}A_i \\
&\quad \dots \\
&B_9 = A_9 \cap A_{10} \\
&B_{10} = A_{10}
\end{aligned}
$$

hence we can conclude

$$
B_1 \le B_2 \le \dots \le B_{10}
$$

and hence $B_n$ is an increasing sequence, so it has a limit. What we need is to find a set that corresponds to greatest lower bound. It is NOT akin to the sequence of real numbers, The sequence of sets i.e., the magnitude of sets is determined by the intersection and union of all available sets. Thus, to find the greatest set, we can't only compare among $\{B_i\}_{i=1}^{10}$, but we also compare all intersections and unions among $\{B_i\}_{i=1}^{10}$. To this end, the greatest set is 

$$
\bigcup_{i=1}^{10}B_i = \bigcup_{n=1}^{10}\bigcap_{i=n}^{10}A_i,
$$

which has a general form in Eq.\@ref(eq:eq2). The case of supremum can be explained with an analogous manner. To this end, we have

$$
\begin{aligned}
&B_1 = \bigcup_{i=1}^{10}A_i \\
&B_2 = \bigcup_{i=2}^{10}A_i \\
&\quad \dots \\
&B_9 = A_9 \cup A_{10} \\
&B_{10} = A_{10}
\end{aligned}
$$
and then 
$$
B_1 \ge B_2 \ge \dots \ge B_{10}
$$

hence $B_n$ is an decreasing sequence and it has a limit. With an analogous manner, we can obtain the smallest set that is 
$$
\bigcap_{i=1}^{10}B_i = \bigcap_{n=1}^{10}\bigcup_{i=n}^{10}A_i.
$$
which is second equation in Eq.\@ref(eq:eq2).
```{.r .distill-force-highlighting-css}
```
