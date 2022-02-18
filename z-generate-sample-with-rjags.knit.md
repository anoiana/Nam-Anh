---
title: "Generate Samples with `Rjags`"
description: "This note summarizes methods to generate samples and calculate measures of interest through such generated samples."
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




# Sampling with `rjags`

## Generalized t distribution

Suppose we want to generate samples from generalized $t$ defined as follows

\begin{equation}
f(x) = \frac{c\sigma^{-1}}{\bigg[n+\Big(\frac{x-\theta}{\sigma}\Big)^2\bigg]^{0.5(n+1)}},
\end{equation}

we say $X \sim t(\theta,\sigma,n)$, where $\theta, \sigma$ and $n$ are location, scale parameter and degree of freedom.

To generate samples of $X$, we could use `rt` function in R to simulate from standard $t$ distribution and then transform such samples. We, however, now wants to simulate generalized $t$ distribution directly, which does not require an extra step of transformation. To this end, we employ `rjags` which is popular for obtaining samples from posterior distribution in Bayesian framework. 

It is worth noting that the generalized $t$ distribution function `dt` in `rjags` requires *mean* and *precision* rather than location and scale as its parameters. For example, we simulate samples from a generalized $t$ distribution whose mean and variance are 10 and 0.5, we shall need to plug in values of 10 and 1/0.5 as its first two parameters (third parameter is degree of freedom which equals 4). 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
 Y ~ dt(10,2,4)
}"</span><span class='op'>)</span>
<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, n.chains <span class='op'>=</span> <span class='fl'>1</span>, quiet <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>
<span class='co'>#update(model, 1000, progress.bar="none")</span>
<span class='va'>params</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"Y"</span><span class='op'>)</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>1000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='co'>##################</span>
<span class='va'>x</span> <span class='op'>=</span> <span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>1</span><span class='op'>]</span> <span class='op'>%&gt;%</span> <span class='fu'><a href='https://rdrr.io/r/base/sort.html'>sort</a></span><span class='op'>(</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>


Let us compare with samples obtained by R function

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/graphics/curve.html'>curve</a></span><span class='op'>(</span><span class='fu'>wiqid</span><span class='fu'>::</span><span class='fu'><a href='https://rdrr.io/pkg/wiqid/man/TDist.html'>dt3</a></span><span class='op'>(</span><span class='va'>x</span>,<span class='fl'>10</span>, <span class='fu'><a href='https://rdrr.io/r/base/MathFun.html'>sqrt</a></span><span class='op'>(</span><span class='fl'>0.5</span><span class='op'>)</span>,<span class='fl'>4</span><span class='op'>)</span>, xlim <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>0</span>,<span class='fl'>20</span><span class='op'>)</span>, lty <span class='op'>=</span> <span class='fl'>3</span>,, ylab <span class='op'>=</span> <span class='st'>"density"</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/graphics/points.html'>points</a></span><span class='op'>(</span><span class='va'>x</span>,<span class='fu'>wiqid</span><span class='fu'>::</span><span class='fu'><a href='https://rdrr.io/pkg/wiqid/man/TDist.html'>dt3</a></span><span class='op'>(</span><span class='va'>x</span>,<span class='fl'>10</span>,<span class='fu'><a href='https://rdrr.io/r/base/MathFun.html'>sqrt</a></span><span class='op'>(</span><span class='fl'>0.5</span><span class='op'>)</span>,<span class='fl'>4</span><span class='op'>)</span>, col <span class='op'>=</span> <span class='st'>"red"</span>, type <span class='op'>=</span> <span class='st'>"l"</span>, lty <span class='op'>=</span> <span class='fl'>2</span><span class='op'>)</span>
</code></pre></div>

</details><img src="z-generate-sample-with-rjags_files/figure-html5/unnamed-chunk-2-1.png" width="624" />

</div>


## Transformation

We sometimes require simulating samples from a transformed random variable, i.e we generate $Y = (2Z+1)^3$, where $Z \sim \mathcal{N}(0,1)$ and calculate $\mathbb{E}(Y)$ and $\mathbb{P}(Y \ge10)$. Such measures can be obtained analytically, more specifically $\mathbb{E}(Y) = 13$ and $\mathbb{P}(Y \ge 10) = 0.28$. @fouley2013

We now use `rjags` to simulate samples 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
 Z ~ dnorm(0,1)
 Y &lt;- pow(2*Z+1,3)
 P&lt;- step(Y-10)
}"</span><span class='op'>)</span>
<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, n.chains <span class='op'>=</span> <span class='fl'>1</span>, quiet <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/stats/update.html'>update</a></span><span class='op'>(</span><span class='va'>model</span>, <span class='fl'>1000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>
<span class='va'>params</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"Y"</span>,<span class='st'>"P"</span><span class='op'>)</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>2000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='co'>##################</span>

<span class='va'>exp_val</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span><span class='op'>)</span>
<span class='va'>prob</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>1</span><span class='op'>]</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>


and we obtained $\mathbb{E}(Y) =$ 13.1189579 and $\mathbb{P}(Y\ge 10) =$ 0.2935.

## A more complicated problem

Suppose we are required calculating how many items can be fixed with \$1000 given the cost to repair an item follows gamma distribution with mean $\mu = 100$ and standard deviation $\sigma = 50$, i.e

\begin{equation}
X \sim \mathcal{G}am(4, 0.04)
\end{equation}

we implement as follows

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
  for(i in 1:n){Y[i] ~ dgamma(4,0.04)}
  sim[1]&lt;- Y[1]
  for(i in 2:n){sim[i] &lt;- sim[i-1]+Y[i]}
  for(i in 1:n){order[i]&lt;- step(1000-sim[i])}
  item&lt;- sum(order[])
  check &lt;- step(sim[n]-1000)
  
}"</span><span class='op'>)</span>
<span class='va'>data</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>n<span class='op'>=</span><span class='fl'>20</span><span class='op'>)</span>
<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, n.chains <span class='op'>=</span> <span class='fl'>1</span>, data <span class='op'>=</span> <span class='va'>data</span>, quiet <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/stats/update.html'>update</a></span><span class='op'>(</span><span class='va'>model</span>, <span class='fl'>1000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>
<span class='va'>params</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"item"</span>,<span class='st'>"check"</span><span class='op'>)</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>10000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='co'>##################</span>

<span class='va'>item</span> <span class='op'>=</span> <span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span> <span class='op'>%&gt;%</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='op'>)</span>
<span class='va'>check</span> <span class='op'>=</span> <span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>1</span><span class='op'>]</span> <span class='op'>%&gt;%</span> <span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>


Mean of `item` is 9.6375 indicates there are about 10 items which can be repaired, Mean of `check` equals 1 indicates $n=20$ is sufficient. 

# Prediction with unknown parameters

for the sake of ease, we shall consider a mixed distribution as follows

\begin{equation}
X = K\mathcal{N}(0,1) + (1-K)\mathcal{N}(5,1),
\end{equation}

where $K \sim \mathcal{B}er(0.3)$. Alternatively, above distribution can be rewritten as follows

\begin{equation}
f(x) = \int f(y|k)f(k)dk,
(\#eq:1)
\end{equation}

which is predictive prior distribution, where $k$ was defined above, and $Y|K=1 \sim \mathcal{N}(0,1)$ while $Y|K=0 \sim \mathcal{N}(5,1)$. To simulate samples of above distribution, we employ Bernoulli and Normal distributions as follows

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
alpha ~ dbin(0.3,1)
x1 ~ dnorm(0,1)
x2 ~ dnorm(5,1)
# x = alpha*x1 + (1-alpha)*x2
x = ifelse(alpha==1,x1,x2)
}"</span><span class='op'>)</span>

<span class='va'>data</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>n<span class='op'>=</span><span class='fl'>20</span><span class='op'>)</span>
<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, n.chains <span class='op'>=</span> <span class='fl'>1</span>, quiet <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/stats/update.html'>update</a></span><span class='op'>(</span><span class='va'>model</span>, <span class='fl'>1000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>
<span class='va'>params</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"x"</span>,<span class='st'>"alpha"</span><span class='op'>)</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>10000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>2</span><span class='op'>]</span> <span class='op'>%&gt;%</span> <span class='fu'><a href='https://rdrr.io/r/graphics/hist.html'>hist</a></span><span class='op'>(</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>


\@ref(eq:1) can be expended on distribution of $K$ that is known as prior distribution. For example, we want to estimate, of 20 new students, how many ones will join painting club given the number of people joining the painting club follows Binomial distribution whose the second parameter follows Beta distribution. i.e

\begin{equation}
X \sim \mathcal{B}in(20,\pi), \quad \pi \sim \mathcal{B}eta(3,2).
\end{equation}

Thus, in `rjags` code one has 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
  p ~ dbeta(3,10)
  x ~ dbin(p,20)}"</span>
  
  <span class='op'>)</span>

<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, n.chains <span class='op'>=</span> <span class='fl'>1</span>, quiet <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/stats/update.html'>update</a></span><span class='op'>(</span><span class='va'>model</span>, <span class='fl'>1000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>
<span class='va'>params</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"x"</span><span class='op'>)</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>10000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='fu'><a href='https://rdrr.io/r/base/summary.html'>summary</a></span><span class='op'>(</span><span class='va'>samples</span><span class='op'>)</span>
</code></pre></div>

</details>

```

Iterations = 1001:11000
Thinning interval = 1 
Number of chains = 1 
Sample size per chain = 10000 

1. Empirical mean and standard deviation for each variable,
   plus standard error of the mean:

          Mean             SD       Naive SE Time-series SE 
       4.57440        2.87418        0.02874        0.02874 

2. Quantiles for each variable:

 2.5%   25%   50%   75% 97.5% 
    0     2     4     6    11 
```

</div>


hence, 5 of 20 new students will join the club. If we'd like to calculate probability that there is at least 2 students who will join the club, the result is

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/base/mean.html'>mean</a></span><span class='op'>(</span><span class='va'>samples</span><span class='op'>[[</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>]</span><span class='op'>[</span>,<span class='fl'>1</span><span class='op'>]</span><span class='op'>&gt;=</span><span class='fl'>5</span><span class='op'>)</span>
</code></pre></div>

</details>

```
[1] 0.4607
```

</div>


# Running MCMC using `rjags`

We shall consider the following setup of a linear model:

$$
\begin{aligned}
y_i &\sim \mathcal{N}(\mu_i,\sigma^2), \text{ and} \\
\mu_i &= \alpha+\beta X_i, \text{ where } i= 1,\dots,N \\
\alpha,\beta &\sim \mathcal{N}(0,h^2) \\
\ln{\sigma} &\sim \mathcal{U}nif(-k,k) \\
\text{we choose } h &= 100, \text{ and } k = 5000
\end{aligned}
$$

(this example refers to @baio2013.)

We shall generate fake data for the model with $\alpha = 150$ and $\beta=-4$. 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
  # likelihood
  for(i in 1:N){
  y[i] ~ dnorm(mu[i],tau)
  mu[i] &lt;- alpha + beta*x[i]
  }
  
  # redundant parameter 
  lsigma ~ dunif(-k,k)
  tau &lt;- pow(exp(lsigma),-2)
  #sigma = pow(tau,-1)
  
  # hyperparameters
  alpha ~ dnorm(142,prec)
  beta ~ dnorm(0,prec)
  prec &lt;- pow(h,-2)
  }"</span><span class='op'>)</span>


<span class='co'># data setup</span>
<span class='fu'><a href='https://rdrr.io/r/base/Random.html'>set.seed</a></span><span class='op'>(</span><span class='fl'>12345</span><span class='op'>)</span>
<span class='va'>x</span><span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>rnorm</a></span><span class='op'>(</span><span class='fl'>100</span>,<span class='fl'>50</span>,<span class='fl'>9</span><span class='op'>)</span>
<span class='va'>error</span><span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>rnorm</a></span><span class='op'>(</span><span class='fl'>100</span>,<span class='fl'>0</span>,<span class='fl'>16</span><span class='op'>)</span>
<span class='va'>y</span><span class='op'>=</span><span class='fl'>150</span><span class='op'>-</span><span class='op'>(</span><span class='fl'>4</span><span class='op'>*</span><span class='va'>x</span><span class='op'>)</span><span class='op'>+</span><span class='va'>error</span>


<span class='va'>dat</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span><span class='st'>"N"</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/length.html'>length</a></span><span class='op'>(</span><span class='va'>y</span><span class='op'>)</span>, h <span class='op'>=</span> <span class='fl'>100</span>, k <span class='op'>=</span> <span class='fl'>5000</span>, y <span class='op'>=</span><span class='va'>y</span>, x <span class='op'>=</span> <span class='va'>x</span><span class='op'>)</span>
<span class='va'>params</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"alpha"</span>,<span class='st'>"beta"</span><span class='op'>)</span>
<span class='va'>inits</span> <span class='op'>=</span> <span class='kw'>function</span><span class='op'>(</span><span class='op'>)</span><span class='op'>{</span><span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>alpha<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>rnorm</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>,beta<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>rnorm</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>,lsigma<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>rnorm</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span><span class='op'>)</span><span class='op'>}</span>


<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, data <span class='op'>=</span>  <span class='va'>dat</span>, inits <span class='op'>=</span> <span class='va'>inits</span>, n.chains <span class='op'>=</span> <span class='fl'>2</span>, quiet <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/stats/update.html'>update</a></span><span class='op'>(</span><span class='va'>model</span>, <span class='fl'>10000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>4000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='va'>samps</span> <span class='op'>=</span> <span class='fu'>purrr</span><span class='fu'>::</span><span class='fu'><a href='https://purrr.tidyverse.org/reference/lift.html'>lift_dl</a></span><span class='op'>(</span><span class='va'>rbind</span><span class='op'>)</span><span class='op'>(</span><span class='va'>samples</span><span class='op'>)</span>

<span class='fu'><a href='https://rdrr.io/r/base/apply.html'>apply</a></span><span class='op'>(</span><span class='va'>samps</span>,<span class='fl'>2</span>,<span class='fu'>HDInterval</span><span class='fu'>::</span><span class='va'><a href='https://rdrr.io/pkg/HDInterval/man/hdi.html'>hdi</a></span>, credMass <span class='op'>=</span> <span class='fl'>0.95</span><span class='op'>)</span><span class='op'>%&gt;%</span>
     <span class='fu'>purrr</span><span class='fu'>::</span><span class='fu'><a href='https://purrr.tidyverse.org/reference/compose.html'>compose</a></span><span class='op'>(</span><span class='op'>~</span><span class='fu'>rownames_to_column</span><span class='op'>(</span><span class='va'>.</span>,<span class='st'>"params"</span><span class='op'>)</span>,<span class='va'>as.data.frame</span>,<span class='va'>t</span><span class='op'>)</span><span class='op'>(</span><span class='op'>)</span><span class='op'>%&gt;%</span>
     <span class='fu'>mutate</span><span class='op'>(</span>mean <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/apply.html'>apply</a></span><span class='op'>(</span><span class='va'>samps</span>,<span class='fl'>2</span>,<span class='va'>mean</span><span class='op'>)</span>, .before <span class='op'>=</span> <span class='st'>"lower"</span><span class='op'>)</span>|&gt;
     <span class='fu'>mutate</span><span class='op'>(</span>params <span class='op'>=</span> <span class='fu'>recode</span><span class='op'>(</span><span class='va'>params</span>, alpha <span class='op'>=</span> <span class='st'>"$\\alpha$"</span>, beta <span class='op'>=</span> <span class='st'>'$\\beta$'</span><span class='op'>)</span><span class='op'>)</span>|&gt;
     <span class='fu'>MyTable</span><span class='op'>(</span><span class='st'>"95% highest density intervals."</span><span class='op'>)</span>
</code></pre></div>

</details><table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:tab1)95% highest density intervals.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> params </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> lower </th>
   <th style="text-align:right;"> upper </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> $\alpha$ </td>
   <td style="text-align:right;"> 142.511306 </td>
   <td style="text-align:right;"> 126.319743 </td>
   <td style="text-align:right;"> 157.367373 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $\beta$ </td>
   <td style="text-align:right;"> -3.842339 </td>
   <td style="text-align:right;"> -4.121486 </td>
   <td style="text-align:right;"> -3.535975 </td>
  </tr>
</tbody>
</table>

</div>


Table \@ref(tab:tab1) shows the 95% highest density intervals of two parameters $\alpha$ and $\beta$.  

Another deserved consideration is convergence diagnostic. To archive this, the number of chains `n.chains` should be greater than 1, we chose `n.chains` = 2 in above example.  

If we fitted the linear model by the frequentist approach, we would have 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>out</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/stats/lm.html'>lm</a></span><span class='op'>(</span><span class='va'>y</span><span class='op'>~</span><span class='va'>x</span><span class='op'>)</span>|&gt;
     <span class='fu'>broom</span><span class='fu'>::</span><span class='fu'><a href='https://generics.r-lib.org/reference/tidy.html'>tidy</a></span><span class='op'>(</span><span class='op'>)</span>
<span class='fu'>tibble</span><span class='op'>(</span>
     params <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"$\\alpha$"</span>, <span class='st'>"$\\beta$"</span><span class='op'>)</span>,
     mean <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='va'>out</span><span class='op'>$</span><span class='va'>estimate</span><span class='op'>)</span>,
     lower <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='va'>out</span><span class='op'>$</span><span class='va'>estimate</span><span class='op'>-</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>qnorm</a></span><span class='op'>(</span><span class='fl'>0.975</span><span class='op'>)</span><span class='op'>*</span><span class='va'>out</span><span class='op'>$</span><span class='va'>std.error</span><span class='op'>)</span>,
     upper <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='va'>out</span><span class='op'>$</span><span class='va'>estimate</span><span class='op'>+</span><span class='fu'><a href='https://rdrr.io/r/stats/Normal.html'>qnorm</a></span><span class='op'>(</span><span class='fl'>0.975</span><span class='op'>)</span><span class='op'>*</span><span class='va'>out</span><span class='op'>$</span><span class='va'>std.error</span><span class='op'>)</span><span class='op'>)</span>|&gt;
     <span class='fu'>MyTable</span><span class='op'>(</span><span class='st'>"95% confidence interval."</span><span class='op'>)</span>
</code></pre></div>

</details><table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:tab2)95% confidence interval.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> params </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> lower </th>
   <th style="text-align:right;"> upper </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> $\alpha$ </td>
   <td style="text-align:right;"> 141.949740 </td>
   <td style="text-align:right;"> 125.070610 </td>
   <td style="text-align:right;"> 158.828871 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> $\beta$ </td>
   <td style="text-align:right;"> -3.831938 </td>
   <td style="text-align:right;"> -4.149498 </td>
   <td style="text-align:right;"> -3.514378 </td>
  </tr>
</tbody>
</table>

</div>










```{.r .distill-force-highlighting-css}
```
