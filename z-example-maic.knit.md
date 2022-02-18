---
title: "An Example of MAIC"
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




We delineate an example in which we'd like to compare two treatments A and C. However, only individual-patient data of treatment A and B are available, and treatment C is only available through a previous study that compare treatment B and C. All information of treatment C reported in the study comprises mean and standard deviation of outcomes and baseline characteristics, so this type of data is usually called aggregate data. For example, 

<div class="layout-chunk" data-layout="l-body">
<details>
<summary>hide</summary>
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='fu'><a href='https://rdrr.io/r/base/Random.html'>set.seed</a></span><span class='op'>(</span><span class='fl'>1234</span><span class='op'>)</span>
<span class='va'>ipd</span> <span class='op'>=</span> <span class='fu'>tibble</span><span class='op'>(</span>
     trt <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/sample.html'>sample</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"treatment"</span>,<span class='st'>"placebo"</span><span class='op'>)</span>,<span class='fl'>100</span>, replace <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>,
     age <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/stats/Poisson.html'>rpois</a></span><span class='op'>(</span><span class='fl'>100</span>,<span class='fl'>30</span><span class='op'>)</span>,
     gender <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/sample.html'>sample</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"F"</span>,<span class='st'>"M"</span><span class='op'>)</span>,<span class='fl'>100</span>, prob <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>0.3</span>,<span class='fl'>0.7</span><span class='op'>)</span>, replace <span class='op'>=</span> <span class='cn'>T</span><span class='op'>)</span>,
     weight <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/stats/GammaDist.html'>rgamma</a></span><span class='op'>(</span><span class='fl'>100</span>,<span class='fl'>70</span>,<span class='fl'>1.2</span><span class='op'>)</span>,
     region <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/sample.html'>sample</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"urban"</span>,<span class='st'>"rural"</span>,<span class='st'>"suburban"</span><span class='op'>)</span>, <span class='fl'>100</span>, replace <span class='op'>=</span> <span class='cn'>T</span>, prob <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>0.5</span>,<span class='fl'>0.4</span>,<span class='fl'>0.1</span><span class='op'>)</span><span class='op'>)</span>
<span class='op'>)</span>

<span class='va'>ipd</span>|&gt; <span class='fu'>MyTable</span><span class='op'>(</span><span class='st'>"Individual patient data."</span>, format <span class='op'>=</span> <span class='st'>"html"</span><span class='op'>)</span>|&gt;
     <span class='fu'>kableExtra</span><span class='fu'>::</span><span class='fu'><a href='https://rdrr.io/pkg/kableExtra/man/scroll_box.html'>scroll_box</a></span><span class='op'>(</span>height <span class='op'>=</span> <span class='st'>"500px"</span>, width <span class='op'>=</span> <span class='st'>"400px"</span>,
                            extra_css  <span class='op'>=</span> <span class='st'>"border:0.1px; margin-left: auto;margin-right: auto;"</span><span class='op'>)</span>
</code></pre></div>

</details><div style="border: 1px solid #ddd; padding: 0px; border:0.1px; margin-left: auto;margin-right: auto;overflow-y: scroll; height:500px; overflow-x: scroll; width:400px; "><table class="table table-striped" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:tab1)Individual patient data.</caption>
 <thead>
  <tr>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> trt </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> age </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> gender </th>
   <th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;"> weight </th>
   <th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;"> region </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 62.37276 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 64.30257 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 62.59412 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 74.78788 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 66.35611 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 59.93270 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 53.42378 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 70.72815 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 61.22151 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.81023 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 47.02849 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 56.02396 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 52.04123 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 41 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 60.15517 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 72.47055 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 57.34558 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 58.32822 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 60.82424 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 50.54126 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 61.67482 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 46 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 51.13449 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 67.72974 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 60.76068 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.76782 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.79380 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 60.99189 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 65.51572 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 61.10073 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 62.61569 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 50.29106 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 69.76771 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 59.55258 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 63.50016 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 48.90830 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 64.89416 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 55.51830 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 64.09215 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 46.38017 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 53.37846 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 63.87146 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 76.92692 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 60.73254 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 58.16554 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.89751 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 41 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 52.58316 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 64.69451 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 73.02933 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 53.48156 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 51.34947 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 57.87380 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 64.74635 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 75.26688 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 65.82308 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 65.75132 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 59.91626 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 47.58772 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 52.18498 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 55.93831 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 58.81236 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 64.96507 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.19672 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 46.29619 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 72.20572 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.98529 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 66.29455 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 59.62189 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 61.10970 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 61.55268 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 50.11269 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 51.48335 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 55.97795 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 58.08092 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 53.49548 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 48.48593 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 37 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 54.85316 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 57.64042 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 61.03991 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 55.02392 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 69.62402 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 59.67280 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 59.82493 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 58.97129 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 64.86729 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 65.13997 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 55.07439 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 58.08483 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 54.46514 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 66.54571 </td>
   <td style="text-align:left;"> suburban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 64.03493 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 61.34866 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 57.04962 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 66.00696 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 53.49538 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 69.87203 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 47.01138 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 61.83252 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 57.30711 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
  <tr>
   <td style="text-align:left;"> treatment </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 68.87114 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> M </td>
   <td style="text-align:right;"> 53.30723 </td>
   <td style="text-align:left;"> urban </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> F </td>
   <td style="text-align:right;"> 56.43860 </td>
   <td style="text-align:left;"> rural </td>
  </tr>
</tbody>
</table></div>

</div>

```{.r .distill-force-highlighting-css}
```
