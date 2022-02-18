---
title: "Statistical cost-effectiveness"
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
---

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


# Mathematical background

Let $t \in \mathcal{T}$ be a decision or intervention/treatment, and $o \in \mathcal{O}$ consequence or outcome. We also introduce the random quantity $\boldsymbol{\omega} \in \Omega$, so the outcome can be described by relationship $o = (\boldsymbol{\omega},t)$ which could be interpreted as the result of selecting treatment $t$ with a series of random quantities obtained in the future. 

Since $\boldsymbol{\omega}$ is random component, we have the probability measure $p(\boldsymbol{\omega})$, i.e. $p: \Omega \rightarrow [0,1]$. Also, the value of each outcome is quantified by measure of utility $u: \mathcal{O} \rightarrow \mathbb{R}$. In general, the random component is nothing but distribution, i.e. $\boldsymbol{\omega} = (y,\boldsymbol{\theta})$ where $y \in \mathcal{Y}$ is future result of individual(s), and $\boldsymbol{\theta} \in \mathbb{\Theta}$ is a set of parameters. Having had sub-components of $\boldsymbol{\omega}$, the outcome can be rewritten as 

$$
\begin{aligned}
&o = (y,\boldsymbol{\theta},t), \text{ implying that } \\
&p(o) = u(y,\boldsymbol{\theta},t)
\end{aligned}
$$
where 

$$
p(y,\boldsymbol{\theta}) = p(y|\boldsymbol{\theta})p(\boldsymbol{\theta}).
$$

For example, we shall consider 5 states of happiness denoted by $\{y_i\}_{i=1}^5$, each state has probability $\boldsymbol{\theta}_i^t, t = 0,1$. We temporarily assume that the degree of uncertainty of $\boldsymbol{\theta}^t_i$ equal zero. Thus, The preferred outcome is can be obtained in either of two ways

1. directly, base on the probability of first state of each treatment, i.e. $\boldsymbol{\theta}^t_1$.
2. indirectly, through other states. In fact, state $\{y_{s}\}_{s=2}^5$ is often defined as the utilities equivalent to a scenario in which perfect health is obtained with probability $\pi_s = u(y_s,t)$.

Using the law of total probability we obtain the chance of preferred consequence $y_1$ as follows

$$
\begin{aligned}
\mathbb{P}(y_1|\boldsymbol{\theta}^t) &= \boldsymbol{\theta}_1^t + \sum_{s=2}^5\pi_s\mathbb{P}(y_s|\boldsymbol{\theta}^t)
\\&= \boldsymbol{\theta}_1^t+ \sum_{s=1}^5\pi_s\boldsymbol{\theta}_s^t
\\&=\sum_{s=1}^5u(y_s,t)\boldsymbol{\theta}_s^t
\\&= \mathbb{E}[u(Y,t)]
\end{aligned}
(\#eq:1)
$$

In practice, $\boldsymbol{\theta}^t$ always involves uncertainty, so we will need to consider $p(\boldsymbol{\theta}^t|\mathcal{D})$, where $\mathcal{D}$ is the background information. The implication of Eq.\@ref(eq:1) is that the probability of obtaining the preferred options amounts to the expectation of utility function, more commonly referred to as the **expected utility** denoting $\mathcal{U}^t$. Thus, the optimal decision criterion is to maximize the expected utility, i.e.

:::{.makebox}
$$
t^* = \arg\max_{\mathcal{T}}\mathcal{U}^t
$$
:::

Also, we can generalize Eq.\@ref(eq:1) as follows

$$
\mathcal{U}^t = \int_{\Theta}\int_{\mathcal{Y}} u(y,t)p(y|\boldsymbol{\theta}^t)p(\boldsymbol{\theta}^t|\mathcal{D})d\color{red}{y}d\boldsymbol{\color{blue}{\theta}}^t
(\#eq:2)
$$


# The setup of decision-making in health economics

## Statistical framework

Let $t \in \mathcal{T}:= \{0,1\}$ be a set of interventions and $y_i$ outcome of $i^{th}$ subject which is constituted by effectiveness and cost, i.e. $y = (e,c)$. Also, $\bf{\theta} = (\theta_0,\theta_1)^{\top}$ is the vector of parameters of interest. First, we aim to find the conditional distribution of $\bf\theta$ given data, i.e. posterior distribution of $\bf\theta$. Let $\mathcal{D}_t = \{y_i\}_{i=1}^{n_t}$ and $\mathcal{D} = \bigcup_{t\in\mathcal{T}}\mathcal{D}_t$, so the posterior has the form $p(\bf{\theta}|\mathcal{D})$. In the second step, therefore, we are interested in predictive model which can be obtained by simulation.

For example, suppose that $\pi_1$ and $\pi_0$ are probabilities that a patient is treated by active treatment and placebo, respectively, and $\pi_1 = \pi_0\rho$ where $\rho$ is the measure of difference, i.e absolute or relative difference. Conditionally on $\pi_t$, $\gamma$ is the probability of needing ambulatory care and $1-\gamma$ is the probability a patient requires hospital admission. 

We take the sample size of $N =1000$ and the number of patients experiencing side effect is $SE_t \sim \mathcal{Bin}(\pi_t,N)$; the number of patients requiring ambulatory care is $A_t \sim \mathcal{Bin}(\gamma,SE_t)$, and hence the number of patients with side effects requiring hospital admission is $H_t = SE_t-A_t$. Also, $c_t^{drug}$ is a fixed cost value and $c^{hosp}$ and $c^{amb}$ are costs of ambulatory care and hospital admission, such two costs are independent. We now modify the parameters of interest as $\bf{\theta} = (\bf{\theta}_0,\bf{\theta}_1)$ where $\bf{\theta}_0 = (\pi_0,\gamma,c^{amb},c^{hosp})$ and $\bf{\theta}_1 = (\pi_1,\gamma,c^{amb},c^{hosp})$. Therefore, condition on $\bf\theta$ the observable outcome is $Y = (SE_t,A_t,H_t)$. 

## Decision process

Suppose an intervention $t$ is applied and we obtain the outcome $y$. Thus, we can quantify by combining a clinical effectiveness $e$ of the outcome $y$ and the costs $c$ associated with intervention $t$, for each $(y,t)$ we associate $(e,c)$. We now let 

$$
\Delta_e := \mathbb{E}(e|\theta_1) - \mathbb{E}(e|\theta_0), \text{ and}\quad \Delta_c:=\mathbb{E}(c|\theta_1) - \mathbb{E}(c|\theta_0)
$$

are the *increment in the mean effectiveness* and the *increment in the mean cost*. Obviously, these two functions are the function of $\bf\theta$. Recall that we aim to find $\mathcal{U}^*:= \max\mathcal{U}_t$, i.e. we choose $t=1$ if 

$$
EIB:= \mathcal{U}_1 - \mathcal{U}_0 > 0
(\#eq:3)
$$

thus,

$$
\mathcal{U}^* = \max\{EIB,0\}+\mathcal{U}_0.
$$

## Choosing a utility function: The net benefit

To obtain Eq.\@ref(eq:2) we will need to a pre-defined function of utility $u(y,t)$ through $e$ and $c$, the *net benefit*, for example,

$$
u(y,t) = ke-c,
$$

where $k$ is a willingness-to-pay parameter used to put the cost and benefit on the same scale, and it is the budget the decision-maker is willing to pay to upgrade the benefits by one unit. Let us review the relationship among aforementioned measures: 

:::{.makebox}
- The expected incremental benefit: $EIB:=\mathcal{U}_t-\mathcal{U}_0$;
- The expected utility $\mathcal{U}_t = \mathbb{E}[u(y,t)]$;
- $u(y,t) = ke-c$
- $\Delta_e:= \mathbb{E}(e|\theta_1)-\mathbb{E}(e|\theta_0)$, and $\Delta_c:= \mathbb{E}(c|\theta_1) - \mathbb{E}(c|\theta_0)$
:::

Thus, 

$$
EIB = \mathbb{E}(k\Delta_e-\Delta_c) = k\mathbb{E}(\Delta_e) - \mathbb{E}(\Delta_c),
(\#eq:4)
$$

and because $\Delta_e$ and $\Delta_c$ are expectations of $e|\bf \theta$ and $c|\bf\theta$, by total of expectation, $EIB$ is an expectation on distribution of $\bf\theta$. Let us divide both sides of Eq.\@ref(eq:4) and arrange terms we obtain 

$$
k = \frac{EIB+\mathbb{E}(\Delta_c)}{\mathbb{E}(\Delta_e)} = \frac{EIB}{\mathbb{E}(\Delta_e)}+ICER
$$

thus,

$$
ICER = k-\frac{EIB}{\mathbb{E}(\Delta_e)}
$$

if $EIB >0$ (by Eq.\@ref(eq:3) this is the condition we expect to see), we have

$$
ICER < k,
$$

so interventions having the $ICER$ is less than the willing-to-pay threshold are deemed cost-effective. 

Also, we will need to define an appropriate measures of the cost and effectiveness. The total cost relative to each treatment can be added up the cost of each patient, i.e

$$
c_t := c_t^{drug}(N-SE_t)+ (c_t^{drug}+c^{amb})A_t+(c_t^{drug}+c^{hosp})H_t.
(\#eq:5minus2)
$$

As for the measure of effectiveness, we count the number of patients who do not experience side effects

$$
e_t:= N-SE_t.
(\#eq:5minus1)
$$

# The value of information (VoI)

## Motivation

We shall consider the following table 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>tab1</span><span class='op'>&lt;-</span>
<span class='fu'>tibble</span><span class='op'>(</span>decision <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"d1"</span>,<span class='st'>"d2"</span>,<span class='st'>"d3"</span>,<span class='st'>"probability"</span><span class='op'>)</span>,
       `state 1` <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>12</span>,<span class='fl'>15</span>,<span class='fl'>5</span>,<span class='fl'>0.5</span><span class='op'>)</span>,
       `state 2` <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>9</span>,<span class='fl'>11</span>,<span class='fl'>18</span>,<span class='fl'>0.2</span><span class='op'>)</span>, 
       `state 3` <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>13</span>,<span class='fl'>8</span>,<span class='fl'>10</span>,<span class='fl'>0.3</span><span class='op'>)</span>
       <span class='op'>)</span>
<span class='fu'>MyTable</span><span class='op'>(</span><span class='va'>tab1</span>, <span class='st'>"current information."</span><span class='op'>)</span>
</code></pre></div>

</details><table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:tab1)current information.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> decision </th>
   <th style="text-align:right;"> state 1 </th>
   <th style="text-align:right;"> state 2 </th>
   <th style="text-align:right;"> state 3 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> d1 </td>
   <td style="text-align:right;"> 12.0 </td>
   <td style="text-align:right;"> 9.0 </td>
   <td style="text-align:right;"> 13.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> d2 </td>
   <td style="text-align:right;"> 15.0 </td>
   <td style="text-align:right;"> 11.0 </td>
   <td style="text-align:right;"> 8.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> d3 </td>
   <td style="text-align:right;"> 5.0 </td>
   <td style="text-align:right;"> 18.0 </td>
   <td style="text-align:right;"> 10.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> probability </td>
   <td style="text-align:right;"> 0.5 </td>
   <td style="text-align:right;"> 0.2 </td>
   <td style="text-align:right;"> 0.3 </td>
  </tr>
</tbody>
</table>

</div>


and two measures *expected value with perfect information (EVwPI)* and *expected value without perfect information (EVwoPI)*, which are defined as follows

$$
EVwPI = \mathbb{E}[\max(S)];
(\#eq:5)
$$
$$
EVwoPI = \max\mathbb{E}(S).
(\#eq:6)
$$

Hence, we obtain $\mathbb{E}(S)$ for $d_1$, $d_2$ and $d_3$ are 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>exp_s</span><span class='op'>&lt;-</span>
<span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>sapply</a></span><span class='op'>(</span><span class='va'>tab1</span><span class='op'>[</span><span class='op'>-</span><span class='fl'>4</span>,<span class='op'>-</span><span class='fl'>1</span><span class='op'>]</span>, <span class='kw'>function</span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span> <span class='fu'><a href='https://rdrr.io/r/base/matrix.html'>as.matrix</a></span><span class='op'>(</span><span class='va'>tab1</span><span class='op'>[</span><span class='fl'>4</span>,<span class='op'>-</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>)</span><span class='op'><a href='https://rdrr.io/r/base/matmult.html'>%*%</a></span><span class='va'>x</span><span class='op'>)</span>
<span class='va'>exp_s</span>
</code></pre></div>

</details>

```
state 1 state 2 state 3 
   10.5    12.1    11.1 
```

</div>


so $\max\mathbb{E}(S)=$ 12.1. Then we calculate $\mathbb{E}[\max(S)]$

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>exp_max</span><span class='op'>&lt;-</span>
<span class='fu'><a href='https://rdrr.io/r/base/matrix.html'>as.matrix</a></span><span class='op'>(</span><span class='va'>tab1</span><span class='op'>[</span><span class='fl'>4</span>,<span class='op'>-</span><span class='fl'>1</span><span class='op'>]</span><span class='op'>)</span><span class='op'><a href='https://rdrr.io/r/base/matmult.html'>%*%</a></span><span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>sapply</a></span><span class='op'>(</span><span class='va'>tab1</span><span class='op'>[</span><span class='op'>-</span><span class='fl'>4</span>,<span class='op'>-</span><span class='fl'>1</span><span class='op'>]</span>, <span class='va'>max</span><span class='op'>)</span><span class='op'>%&gt;%</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='op'>)</span>
<span class='va'>exp_max</span>
</code></pre></div>

</details>

```
[1] 15
```

</div>


Finally, the so-called the *expected value of perfect information (EVPI)* is the difference between EVwPI and EVwoPI, i.e. $EVwoPI - EVwPI =$ 2.9.

The rationale behind EVPI is very much similar to these calculations.

## Probabilistic sensitivity analysis (PSA)

We now suppose the prior distribution $p(\bf{\theta}|\mathcal{D})$ is close to one-point distribution at the true value. Hence, the expected utility is 

$$
U(\theta_t) := \mathbb{E}[u(Y,t)|\mathcal{D}] = \int u(y,t)p(y|\theta_t)d\color{red}{y}.
$$

and $U^*(\bf{\theta}):= \max_t U(\theta_t)$. Thus, we would choose $t=1$ as the *incremental benefit* $IB(\bf{\theta}) > 0$ where

$$
IB(\bf{\theta}):= U(\theta_1) - U(\theta_0),
$$

and as previous expansion, we are able to obtain $U^*(\bf{\theta}) = \max\{IB(\bf{\theta}),0\}+U(\theta_0)$. In practice, the certainty of $\bf \theta$ is not able to happen, so the value of information is often used rather than PSA.

## The value of information (VoI)

VoI will be obtained by two utilities term, namely $U^*(\bf{\theta})$ and $\mathcal{U}^*$ relative to the *expectation value with perfect information (EVwPI)* and the *expectation value without perfect information (EVwoPI)*. Indeed, 

$$
VI(\bf{\theta}) := U^*(\bf{\theta}) - \mathcal{U}^*,
$$

which is a function of a random component $\bf{\theta}$, and hence we are interested in the expectation of $VI$ instead of itself, i.e the *expected value of perfect information (EVPI)*

$$
EVPI := \mathbb{E}[VI(\bf{\theta})] = \mathbb{E}_{\bf{\theta}|\mathcal{}D}[U^*(\bf{\theta})] -\mathbb{E}_{\bf{\theta}|\mathcal{D}}(\mathcal{U}^*) = \mathcal{V}^*-\mathcal{U}^*,
$$

we can consider $\mathcal{V}^*$ as EVwPI or "expectation of maximum", and $\mathcal{U}^*$ EVwoPI or "maximum of expectation".

EVPI can also be described through the *opportunity loss (OL)*. Let $\tau =\arg\max_t(\mathcal{U}_t)$, we have 

$$
OL(\bf{\theta}) := U^*(\bf{\theta}) - U(\theta_{\tau})
$$

and then 

$$
\mathbb{E}[OL(\bf{\theta})] = \mathbb{E}_{\bf{\theta}|\mathcal{D}}[U^*(\bf{\theta})] - \mathbb{E}_{\theta_{\tau}|\mathcal{D}}[U(\theta_{\tau})] = \mathcal{V}^*-\mathcal{U}^*
$$

note that $\mathbb{E}_{\theta_{\tau}|\mathcal{D}}[U(\theta_{\tau})]$ is equivalent to $\max_{\tau}\mathbb{E}_{\bf{\theta}|\mathcal{D}}[U(\bf{\theta})]$

## The value of partial information

# Examples

For the sake of illustration, we shall consider the following example. Let $t \in \{0,1\}$ be treatment $t$, $\pi_t$ the probability of an exposure under treatment $t$. The effect size of interest is usually ratio or difference between probabilities $\pi_0$ and $\pi_1$, so we assume $\pi_1 = \pi_0\rho$ where $\rho = \pi_1/\pi_0$ is the relative difference between two treatments. Conditioning on $\pi_t$, $\gamma$ indicates the probability of needing ambulatory care. Therefore, the probability that an exposure needs hospital admission is $(1-\gamma)$. Furthermore, let $N= 1000$, so the total number of patients having exposure is $SE_t \sim \mathcal{B}in(N,\pi_t)$ and $A_t \sim \mathcal{B}in(\gamma,SE_t)$ is the number of exposures requiring ambulatory care. As a result, $H_t=SE_t-A_t$ is the total number of exposures requiring hospital admission. 

As for the costs, all patients are subjected to the cost $c^{drug}_t$. Additionally, those exposed patients are required paying extra cost, namely $c^{amb}$ for ambulatory care and $c^{hosp}$ for hospital admission. Hence, we consider parameter $\bf{\theta} = (\bf{\theta}_0,\bf{\theta}_1)$ where $\bf{\theta}_t = (\pi_t,\gamma,c^{amb},c^{hosp})$ with $t=0,1$. 

We now summary the model as follows:

Suppose we have observed `N.studies` studies, each study reported the number of exposures `se` out of $n$ patients and `amb` patients exposed requires only ambulatory care. This information would be used to update $\pi_0$ and $\gamma$. Thus, the prior information in this case can be expressed as $Y = (se_0,amb_0)$. $t=0$ is the current treatment being used.

1. Based on the prior information we can can set $se_0 \sim \mathcal{B}in(\pi_0,n)$ and $amb_0 \sim \mathcal{B}in(\gamma,se_0)$;
2. Also, 
   i. $\pi_1 := \pi_0\rho$, and 
   ii. $\pi_0 \sim \mathcal{B}eta(a_{\pi_0},b_{\pi_0})$, $\rho \sim \mathcal{N}(m_{\rho},\tau_{\rho})$ and $\gamma \sim \mathcal{B}eta(a_{\gamma},b_{\gamma})$. 
   iii. For the costs, we set $c^{amb} \sim \log\mathcal{N}(m_{amb},\tau_{amb})$ and $c^{hosp} \sim \log\mathcal{N}(m_{hosp},\tau_{hosp})$.
3. For prediction, we estimate the number of exposures under treatment $t =1$, i.e $SE_1 \sim \mathcal{B}in(\pi_1,N)$, and the number of patients exposed requiring ambulatory $A_1 \sim \mathcal{B}in(\gamma,SE_1)$.
4. Besides, we set $c_0^{drug} = 110$ and $c_1^{drug} = 520$.

Obviously, step (1) is likelihood that we can observed from data; step (2) is prior information obtained from previous studies; step (4) is predictive distribution of interest; and step (4) is pre-defined values. 

In step 1, we shall generate fake data as follows

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/base/Random.html'>set.seed</a></span><span class='op'>(</span><span class='fl'>1234</span><span class='op'>)</span>
<span class='co'>## Generates data for the number of side effects under t=0</span>
<span class='va'>N.studies</span> <span class='op'>&lt;-</span> <span class='fl'>5</span> <span class='co'># the number of accessible studies </span>
<span class='va'>sample.size</span> <span class='op'>&lt;-</span> <span class='fl'>32</span> <span class='co'># potential sample size in each study </span>
<span class='va'>prop.pi</span> <span class='op'>&lt;-</span> <span class='fl'>.24</span> <span class='co'># TRUE VALUE of the proportion of exposure</span>
<span class='va'>n</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/Poisson.html'>rpois</a></span><span class='op'>(</span><span class='va'>N.studies</span>,<span class='va'>sample.size</span><span class='op'>)</span> <span class='co'># we simulate sample size for N.studies studies. </span>
<span class='va'>se</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/Binomial.html'>rbinom</a></span><span class='op'>(</span><span class='va'>N.studies</span>,<span class='va'>n</span>,<span class='va'>prop.pi</span><span class='op'>)</span> <span class='co'># simulate the number of exposure for each study </span>

<span class='co'>## Generates the data for the number of patients with exposure requiring ambulatory care</span>
<span class='va'>prop.gamma</span> <span class='op'>&lt;-</span> <span class='fl'>.619</span> <span class='co'># TRUE VALUE of the proportion of patients exposed requiring ambulatory</span>
<span class='va'>amb</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/Binomial.html'>rbinom</a></span><span class='op'>(</span><span class='va'>N.studies</span>,<span class='va'>se</span>,<span class='va'>prop.gamma</span><span class='op'>)</span> <span class='co'># simulate the number of ambulatory </span>

<span class='va'>dat</span><span class='op'>&lt;-</span> <span class='fu'>tibble</span><span class='op'>(</span>
          Study <span class='op'>=</span> <span class='fl'>1</span><span class='op'>:</span><span class='fl'>5</span>,
          n <span class='op'>=</span> <span class='va'>n</span>,
          exposure <span class='op'>=</span> <span class='va'>se</span>,
          ambulatory <span class='op'>=</span> <span class='va'>amb</span><span class='op'>)</span>

<span class='fu'>MyTable</span><span class='op'>(</span><span class='va'>dat</span>, <span class='st'>"Observed data"</span><span class='op'>)</span>
</code></pre></div>

</details><table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:tab2)Observed data</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Study </th>
   <th style="text-align:right;"> n </th>
   <th style="text-align:right;"> exposure </th>
   <th style="text-align:right;"> ambulatory </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
</tbody>
</table>

</div>


In step 2, we shall set hyper-parameters for prior distributions. There being 5 prior distributions, hyper-parameters should be chosen such that such the available prior information at hand can be delineated adequately through hyper-parameters. Thus,

- *Distribution of $\pi_0$*: describes the proportion of exposure and follows $\mathcal{B}eta$ distribution. Thus, such hyper-parameters are used to warrant uncertainties of parameters. for illustration, we select mean $m_{\pi_0} = 0.5$ and standard deviation $s_{\pi_0} = \sqrt{0.125}$ for the $\mathcal{B}eta$ distribution. 
- *Distribution of $\gamma$*:  delineates the probability that patients exposed requires ambulatory .Again, we set $\gamma$ to follow $\mathcal{B}eta$ distribution with $m_{\gamma} = 0.5$ and $s_{\gamma} = \sqrt{0.125}$
- *Distribution of $\rho$*: delineates relative difference between two groups, and set $\rho \sim \mathcal{N}(0.8,0.2^2)$.

We shall consider prior distributions in step (2.iii), i.e. distribution costs

- Both costs for ambulatory  and hospital follow log-normal with mean and standard deviation $(120,20)$ and $(5500,980)$, respectively.
- Also, we need to define functions that convert prior metrics to distribution's parameters. More specifically, we obtain parameters for Beta and log-normal distribution. Thus

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>betaPar</span> <span class='op'>&lt;-</span> <span class='kw'>function</span><span class='op'>(</span><span class='va'>m</span>,<span class='va'>s</span><span class='op'>)</span><span class='op'>{</span>
<span class='va'>a</span> <span class='op'>&lt;-</span> <span class='va'>m</span><span class='op'>*</span><span class='op'>(</span><span class='op'>(</span><span class='va'>m</span><span class='op'>*</span><span class='op'>(</span><span class='fl'>1</span><span class='op'>-</span><span class='va'>m</span><span class='op'>)</span><span class='op'>/</span><span class='va'>s</span><span class='op'>^</span><span class='fl'>2</span><span class='op'>)</span><span class='op'>-</span><span class='fl'>1</span><span class='op'>)</span>
<span class='va'>b</span> <span class='op'>&lt;-</span> <span class='op'>(</span><span class='fl'>1</span><span class='op'>-</span><span class='va'>m</span><span class='op'>)</span><span class='op'>*</span><span class='op'>(</span><span class='op'>(</span><span class='va'>m</span><span class='op'>*</span><span class='op'>(</span><span class='fl'>1</span><span class='op'>-</span><span class='va'>m</span><span class='op'>)</span><span class='op'>/</span><span class='va'>s</span><span class='op'>^</span><span class='fl'>2</span><span class='op'>)</span><span class='op'>-</span><span class='fl'>1</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>a<span class='op'>=</span><span class='va'>a</span>,b<span class='op'>=</span><span class='va'>b</span><span class='op'>)</span>
<span class='op'>}</span>

<span class='va'>lognPar</span> <span class='op'>&lt;-</span> <span class='kw'>function</span><span class='op'>(</span><span class='va'>m</span>,<span class='va'>s</span><span class='op'>)</span> <span class='op'>{</span>
<span class='va'>s2</span> <span class='op'>&lt;-</span> <span class='va'>s</span><span class='op'>^</span><span class='fl'>2</span>
<span class='va'>mulog</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/Log.html'>log</a></span><span class='op'>(</span><span class='va'>m</span><span class='op'>)</span><span class='op'>-</span><span class='fl'>0.5</span><span class='op'>*</span><span class='fu'><a href='https://rdrr.io/r/base/Log.html'>log</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>+</span><span class='va'>s2</span><span class='op'>/</span><span class='va'>m</span><span class='op'>^</span><span class='fl'>2</span><span class='op'>)</span>
<span class='va'>s2log</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/Log.html'>log</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>+</span><span class='op'>(</span><span class='va'>s2</span><span class='op'>/</span><span class='va'>m</span><span class='op'>^</span><span class='fl'>2</span><span class='op'>)</span><span class='op'>)</span>
<span class='va'>sigmalog</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/MathFun.html'>sqrt</a></span><span class='op'>(</span><span class='va'>s2log</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span>mulog<span class='op'>=</span><span class='va'>mulog</span>,sigmalog<span class='op'>=</span><span class='va'>sigmalog</span><span class='op'>)</span>
<span class='op'>}</span>
</code></pre></div>

</details>

</div>


Then, we utilize above functions to obtain hyper-parameters as follows abut

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='co'>## Computes the hyper-parameters for the informative prior distributions </span>
<span class='va'>m.pi</span> <span class='op'>&lt;-</span> <span class='fl'>.5</span> 
<span class='va'>s.pi</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/MathFun.html'>sqrt</a></span><span class='op'>(</span><span class='fl'>.125</span><span class='op'>)</span>
<span class='va'>a.pi</span> <span class='op'>&lt;-</span> <span class='fu'>betaPar</span><span class='op'>(</span><span class='va'>m.pi</span>,<span class='va'>s.pi</span><span class='op'>)</span><span class='op'>$</span><span class='va'>a</span>
<span class='va'>b.pi</span> <span class='op'>&lt;-</span> <span class='fu'>betaPar</span><span class='op'>(</span><span class='va'>m.pi</span>,<span class='va'>s.pi</span><span class='op'>)</span><span class='op'>$</span><span class='va'>b</span>

<span class='va'>m.gamma</span> <span class='op'>&lt;-</span> <span class='fl'>.5</span>
<span class='va'>s.gamma</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/MathFun.html'>sqrt</a></span><span class='op'>(</span><span class='fl'>.125</span><span class='op'>)</span>
<span class='va'>a.gamma</span> <span class='op'>&lt;-</span> <span class='fu'>betaPar</span><span class='op'>(</span><span class='va'>m.gamma</span>,<span class='va'>s.gamma</span><span class='op'>)</span><span class='op'>$</span><span class='va'>a</span>
<span class='va'>b.gamma</span> <span class='op'>&lt;-</span> <span class='fu'>betaPar</span><span class='op'>(</span><span class='va'>m.gamma</span>,<span class='va'>s.gamma</span><span class='op'>)</span><span class='op'>$</span><span class='va'>b</span>

<span class='va'>m.rho</span> <span class='op'>&lt;-</span> <span class='fl'>0.8</span>
<span class='va'>s.rho</span> <span class='op'>&lt;-</span> <span class='fl'>0.2</span>
<span class='va'>tau.rho</span> <span class='op'>&lt;-</span> <span class='fl'>1</span><span class='op'>/</span><span class='va'>s.rho</span><span class='op'>^</span><span class='fl'>2</span>

<span class='va'>mu.amb</span> <span class='op'>&lt;-</span> <span class='fl'>120</span>
<span class='va'>sd.amb</span> <span class='op'>&lt;-</span> <span class='fl'>20</span>
<span class='va'>m.amb</span> <span class='op'>&lt;-</span> <span class='fu'>lognPar</span><span class='op'>(</span><span class='va'>mu.amb</span>,<span class='va'>sd.amb</span><span class='op'>)</span><span class='op'>$</span><span class='va'>mulog</span>
<span class='va'>s.amb</span> <span class='op'>&lt;-</span> <span class='fu'>lognPar</span><span class='op'>(</span><span class='va'>mu.amb</span>,<span class='va'>sd.amb</span><span class='op'>)</span><span class='op'>$</span><span class='va'>sigmalog</span>
<span class='va'>tau.amb</span> <span class='op'>&lt;-</span> <span class='fl'>1</span><span class='op'>/</span><span class='va'>s.amb</span><span class='op'>^</span><span class='fl'>2</span>

<span class='va'>mu.hosp</span> <span class='op'>&lt;-</span> <span class='fl'>5500</span>
<span class='va'>sd.hosp</span> <span class='op'>&lt;-</span> <span class='fl'>980</span>
<span class='va'>m.hosp</span> <span class='op'>&lt;-</span> <span class='fu'>lognPar</span><span class='op'>(</span><span class='va'>mu.hosp</span>,<span class='va'>sd.hosp</span><span class='op'>)</span><span class='op'>$</span><span class='va'>mulog</span>
<span class='va'>s.hosp</span> <span class='op'>&lt;-</span> <span class='fu'>lognPar</span><span class='op'>(</span><span class='va'>mu.hosp</span>,<span class='va'>sd.hosp</span><span class='op'>)</span><span class='op'>$</span><span class='va'>sigmalog</span>
<span class='va'>tau.hosp</span> <span class='op'>&lt;-</span> <span class='fl'>1</span><span class='op'>/</span><span class='va'>s.hosp</span><span class='op'>^</span><span class='fl'>2</span>
</code></pre></div>

</details>

</div>


Finally, in step (3) and (4), we need to set population size that we want to predict and the cost of drug in each group.

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>N</span> <span class='op'>=</span> <span class='fl'>1000</span>
<span class='va'>c.drug</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>110</span>,<span class='fl'>520</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>


We now define the model and run Bayesian 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>model_string</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/textconnections.html'>textConnection</a></span><span class='op'>(</span>
  <span class='st'>"model{
  
  #likelihood
  for(i in 1:N.studies){
  se[i] ~ dbin(pi[1],n[i])
  amb[i] ~ dbin(gamma,se[i])
  }
  
  # prior
  pi[1] ~ dbeta(a.pi,b.pi)
  pi[2]&lt;- pi[1]*rho
  
  rho ~ dnorm(m.rho, tau.rho)
  gamma ~ dbeta(a.gamma,b.gamma)
  c.amb ~ dlnorm(m.amb,tau.amb)
  c.hosp ~ dlnorm(m.hosp,tau.hosp)
  
  # Predictive distributions on the clinical outcomes
  for(i in 1:2){
  SE[i] ~ dbin(pi[i],N)
  A[i] ~ dbin(gamma,SE[i])
  H[i] &lt;- SE[i] - A[i]
  }
  }"</span><span class='op'>)</span>


<span class='co'># data setup</span>
<span class='va'>dat</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span><span class='st'>"a.pi"</span> <span class='op'>=</span> <span class='va'>a.pi</span>,<span class='st'>"b.pi"</span> <span class='op'>=</span> <span class='va'>b.pi</span>,<span class='st'>"a.gamma"</span> <span class='op'>=</span> <span class='va'>a.gamma</span>,<span class='st'>"b.gamma"</span> <span class='op'>=</span> <span class='va'>b.gamma</span>,
           <span class='st'>"m.amb"</span> <span class='op'>=</span> <span class='va'>m.amb</span>,<span class='st'>"tau.amb"</span> <span class='op'>=</span> <span class='va'>tau.amb</span>,
           <span class='st'>"m.hosp"</span> <span class='op'>=</span> <span class='va'>m.hosp</span>,<span class='st'>"tau.hosp"</span> <span class='op'>=</span> <span class='va'>tau.hosp</span>, <span class='st'>"m.rho"</span> <span class='op'>=</span> <span class='va'>m.rho</span>,
           <span class='st'>"tau.rho"</span> <span class='op'>=</span> <span class='va'>tau.rho</span>,<span class='st'>"se"</span> <span class='op'>=</span> <span class='va'>se</span>,<span class='st'>"amb"</span> <span class='op'>=</span> <span class='va'>amb</span>,
           <span class='st'>"N.studies"</span> <span class='op'>=</span> <span class='va'>N.studies</span>, <span class='st'>"n"</span> <span class='op'>=</span> <span class='va'>n</span>, N <span class='op'>=</span> <span class='va'>N</span><span class='op'>)</span>

<span class='co'># initial stochastic variables</span>
<span class='va'>inits</span> <span class='op'>&lt;-</span> <span class='kw'>function</span><span class='op'>(</span><span class='op'>)</span><span class='op'>{</span>              
  <span class='va'>SE</span><span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Binomial.html'>rbinom</a></span><span class='op'>(</span><span class='fl'>2</span>,<span class='va'>N</span>,<span class='fl'>.2</span><span class='op'>)</span>
  <span class='va'>rho</span><span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Uniform.html'>runif</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>

  <span class='fu'><a href='https://rdrr.io/r/base/list.html'>list</a></span><span class='op'>(</span><span class='st'>"pi"</span><span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/stats/Uniform.html'>runif</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>,<span class='cn'>NA</span><span class='op'>)</span>,gamma<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Uniform.html'>runif</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>,c.amb<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Lognormal.html'>rlnorm</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>,
  c.hosp<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Lognormal.html'>rlnorm</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>)</span>,rho <span class='op'>=</span> <span class='va'>rho</span>,SE<span class='op'>=</span><span class='va'>SE</span>,A<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/stats/Binomial.html'>rbinom</a></span><span class='op'>(</span><span class='fl'>2</span>,<span class='va'>SE</span>,<span class='fl'>.2</span><span class='op'>)</span>
  <span class='op'>)</span>
<span class='op'>}</span>

<span class='co'># fitting model and burn-in 10000 iterations</span>
<span class='va'>model</span> <span class='op'>&lt;-</span> <span class='fu'>jags.model</span><span class='op'>(</span><span class='va'>model_string</span>, data <span class='op'>=</span>  <span class='va'>dat</span>, inits <span class='op'>=</span> <span class='fu'>inits</span><span class='op'>(</span><span class='op'>)</span>, n.chains <span class='op'>=</span> <span class='fl'>2</span><span class='op'>)</span>
</code></pre></div>

</details>

```
Compiling model graph
   Resolving undeclared variables
   Allocating nodes
Graph information:
   Observed stochastic nodes: 10
   Unobserved stochastic nodes: 9
   Total graph size: 39

Initializing model
```

<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/stats/update.html'>update</a></span><span class='op'>(</span><span class='va'>model</span>, <span class='fl'>10000</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>

<span class='co'># parameters of interest</span>
<span class='va'>params</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"pi"</span>,<span class='st'>"gamma"</span>,<span class='st'>"c.amb"</span>,<span class='st'>"c.hosp"</span>,<span class='st'>"rho"</span>,<span class='st'>"SE"</span>,<span class='st'>"A"</span>,<span class='st'>"H"</span><span class='op'>)</span>
<span class='co'># get samples</span>
<span class='va'>samples</span> <span class='op'>&lt;-</span> <span class='fu'>coda.samples</span><span class='op'>(</span><span class='va'>model</span>,
                        variable.names<span class='op'>=</span><span class='va'>params</span>,
                        n.iter<span class='op'>=</span><span class='fl'>500</span>, progress.bar<span class='op'>=</span><span class='st'>"none"</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>



Cost-effectiveness analysis

cost and effectiveness is calculated using Eq.\@ref(eq:5minus1) and Eq.\@ref(eq:5minus2).  Here we shall address the cost effectiveness by two manners, using function `bcea` of the package `BCEA` and coding manually. Then, we compare both results to ensure the later result aligns with the former. 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>amb</span> <span class='op'>=</span> <span class='fu'>lift_dl</span><span class='op'>(</span><span class='va'>rbind</span><span class='op'>)</span><span class='op'>(</span><span class='va'>samples</span><span class='op'>)</span>
<span class='va'>ef</span> <span class='op'>=</span> <span class='va'>N</span> <span class='op'>-</span> <span class='va'>amb</span><span class='op'>[</span>,<span class='fu'><a href='https://rdrr.io/pkg/BCEA/man/Smoking.html'>c</a></span><span class='op'>(</span><span class='st'>"A[1]"</span>,<span class='st'>"A[2]"</span><span class='op'>)</span><span class='op'>]</span>
<span class='va'>cost</span> <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>sapply</a></span><span class='op'>(</span><span class='fl'>1</span><span class='op'>:</span><span class='fl'>2</span>, \<span class='op'>(</span><span class='va'>i</span><span class='op'>)</span><span class='op'>{</span>
     <span class='va'>c.drug</span><span class='op'>[</span><span class='va'>i</span><span class='op'>]</span><span class='op'>*</span><span class='va'>ef</span><span class='op'>[</span>,<span class='va'>i</span><span class='op'>]</span><span class='op'>+</span><span class='op'>(</span><span class='va'>c.drug</span><span class='op'>[</span><span class='va'>i</span><span class='op'>]</span><span class='op'>+</span><span class='va'>amb</span><span class='op'>[</span>,<span class='st'>'c.amb'</span><span class='op'>]</span><span class='op'>)</span><span class='op'>*</span><span class='va'>amb</span><span class='op'>[</span>,<span class='va'>i</span><span class='op'>]</span><span class='op'>+</span><span class='op'>(</span><span class='va'>c.drug</span><span class='op'>[</span><span class='va'>i</span><span class='op'>]</span><span class='op'>+</span><span class='va'>amb</span><span class='op'>[</span>,<span class='st'>'c.hosp'</span><span class='op'>]</span><span class='op'>)</span><span class='op'>*</span><span class='va'>amb</span><span class='op'>[</span>,<span class='va'>i</span><span class='op'>+</span><span class='fl'>2</span><span class='op'>]</span>
<span class='op'>}</span><span class='op'>)</span>

<span class='kw'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='op'>(</span><span class='va'><a href='http://www.statistica.it/gianluca/software/bcea/'>BCEA</a></span><span class='op'>)</span>
<span class='va'>treats</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/pkg/BCEA/man/Smoking.html'>c</a></span><span class='op'>(</span><span class='st'>"Old Chemotherapy"</span>,<span class='st'>"New Chemotherapy"</span><span class='op'>)</span>
<span class='va'>m</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/pkg/BCEA/man/bcea.html'>bcea</a></span><span class='op'>(</span>e<span class='op'>=</span><span class='va'>ef</span>,c<span class='op'>=</span><span class='va'>cost</span>,ref<span class='op'>=</span><span class='fl'>2</span>,interventions<span class='op'>=</span><span class='va'>treats</span>,Kmax<span class='op'>=</span><span class='fl'>50000</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/base/summary.html'>summary</a></span><span class='op'>(</span><span class='va'>m</span><span class='op'>)</span>
</code></pre></div>

</details>

</div>































<!-- ------------------------------------------------------------------- -->
(cont.)








```{.r .distill-force-highlighting-css}
```
