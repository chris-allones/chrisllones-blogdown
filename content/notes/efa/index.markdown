---
title: "Exploratory Factor Analysis (EFA)"
authors:
- admin
date: 2022-02-01T21:13:14-05:00
output:
  html_document:
    highlight: textmate
    toc: true
    number_sections: true
categories: ["R"]
subtitle: Guide for doing an exploratory factor analysis (EFA) in R.
summary: This is a guide for doing an exploratory factor analysis using R. The guide is intended as preparatory in doing confirmatory factor analysis.
tags: ["EFA", "Exploratory", "Factor analysis"]
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

<style type="text/css">
.scroll-200 {
  max-height: 200px;
  overflow-y: auto;
  background-color: inherit;
}
</style>



# Table of contents

[1. Objectives of factor analysis](#objectives)

[2. Designing an EFA](#design)

[3. Assumption in Exploratory factor analysis](#assumption)

[4. Deriving factors](#deriving)

[5. Assessing overall fit](#assessment)

[6. Interpreting factors](#interpretation)

***

<br>

# 1. Objectives of factor analysis <a name="objectives"></a>

This EFA guide aims primarily on preparing researcher in doing confirmatory factor analysis (CFA) and the structural equation modelling (SEM). To better understand the relation of EFA with other factor analysis, consider the figure below.

<img src="materials/overview_factor.jpg" width="50%" style="display: block; margin: auto;" />




# 2. Designing an EFA <a name="design"></a>


# 3. Assumption in EFA <a name="assumption"></a>


# 4. Deriving factors <a name="deriving"></a>


# 5. Assessing overall fit <a name="assessment"></a>


# 6. Interpreting factors <a name="interpretation"></a>



**R-package and importing data**


```r
#loading packages using `pacman`
pacman::p_load(tidyverse, psych, lavaan,
               haven, kableExtra, likert,
               EFAtools)
```


```r
#loading data
data <- readRDS("social.rds")
```

# Descriptives

## Common descriptive statistics

Initially, we can understand the general characteristics of our data through basic descriptive of our measurement items. It is a good practice to start with a descriptive analysis to explore our data and inspect any possible errors. Any unusual or extreme values may indicate possible errors that need to be inspected. Table 1 shows the descriptive of the data we are going to use to demonstrate the analysis on structural equation modeling.

Refer to the accompanying notes for the full details of each items.


```r
data %>% describe() %>% 
     kable(caption = "Descriptives of the measurement items.",
                  digits = 3) %>%
     kable_classic(full_width = F, html_font = "Arial")
```

<table class=" lightable-classic" style="font-family: Arial; width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Table 1: Descriptives of the measurement items.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> vars </th>
   <th style="text-align:right;"> n </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> sd </th>
   <th style="text-align:right;"> median </th>
   <th style="text-align:right;"> trimmed </th>
   <th style="text-align:right;"> mad </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
   <th style="text-align:right;"> range </th>
   <th style="text-align:right;"> skew </th>
   <th style="text-align:right;"> kurtosis </th>
   <th style="text-align:right;"> se </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> bridging1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.217 </td>
   <td style="text-align:right;"> 1.482 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.373 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.950 </td>
   <td style="text-align:right;"> 1.007 </td>
   <td style="text-align:right;"> 0.085 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bridging2 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.276 </td>
   <td style="text-align:right;"> 1.415 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.422 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.905 </td>
   <td style="text-align:right;"> 0.976 </td>
   <td style="text-align:right;"> 0.081 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bridging3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.263 </td>
   <td style="text-align:right;"> 1.427 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.414 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.882 </td>
   <td style="text-align:right;"> 0.856 </td>
   <td style="text-align:right;"> 0.082 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bonding1 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.033 </td>
   <td style="text-align:right;"> 1.524 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.172 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.747 </td>
   <td style="text-align:right;"> 0.302 </td>
   <td style="text-align:right;"> 0.087 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bonding2 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.095 </td>
   <td style="text-align:right;"> 1.557 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.258 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.817 </td>
   <td style="text-align:right;"> 0.347 </td>
   <td style="text-align:right;"> 0.089 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bonding3 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.128 </td>
   <td style="text-align:right;"> 1.500 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.279 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.791 </td>
   <td style="text-align:right;"> 0.431 </td>
   <td style="text-align:right;"> 0.086 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bonding4 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.082 </td>
   <td style="text-align:right;"> 1.484 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.225 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.751 </td>
   <td style="text-align:right;"> 0.397 </td>
   <td style="text-align:right;"> 0.085 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ca1 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 5.003 </td>
   <td style="text-align:right;"> 1.475 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.131 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.719 </td>
   <td style="text-align:right;"> 0.419 </td>
   <td style="text-align:right;"> 0.085 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ca2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.974 </td>
   <td style="text-align:right;"> 1.528 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.107 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.647 </td>
   <td style="text-align:right;"> 0.122 </td>
   <td style="text-align:right;"> 0.088 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ca3 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.891 </td>
   <td style="text-align:right;"> 1.491 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.000 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.595 </td>
   <td style="text-align:right;"> 0.243 </td>
   <td style="text-align:right;"> 0.086 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ca4 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.980 </td>
   <td style="text-align:right;"> 1.487 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 5.107 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.657 </td>
   <td style="text-align:right;"> 0.330 </td>
   <td style="text-align:right;"> 0.085 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> om1 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.408 </td>
   <td style="text-align:right;"> 1.602 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.439 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.170 </td>
   <td style="text-align:right;"> -0.587 </td>
   <td style="text-align:right;"> 0.092 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> om2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.388 </td>
   <td style="text-align:right;"> 1.748 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 4.443 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.233 </td>
   <td style="text-align:right;"> -0.822 </td>
   <td style="text-align:right;"> 0.100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> om3 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.559 </td>
   <td style="text-align:right;"> 1.680 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 4.639 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.286 </td>
   <td style="text-align:right;"> -0.626 </td>
   <td style="text-align:right;"> 0.096 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> om4 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 4.714 </td>
   <td style="text-align:right;"> 1.584 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 4.795 </td>
   <td style="text-align:right;"> 1.483 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> -0.345 </td>
   <td style="text-align:right;"> -0.470 </td>
   <td style="text-align:right;"> 0.091 </td>
  </tr>
</tbody>
</table>

## Graphical inspection of the measurement items

Mostly, farmers show high social capital in a bonding and bridging distinction wherein majority responded positively in social capital question items. Similar results is observed for collective action activities while roughly 50 percent for items in OM constructs.


```r
data %>% as.data.frame() %>% 
     likert(nlevels = 7) %>% 
     plot()
```

<div class="figure" style="text-align: center">
<img src="{{< blogdown/postref >}}index_files/figure-html/grapical-1.png" alt="Farmer's responses on each question item." width="70%" />
<p class="caption">Figure 1: Farmer's responses on each question item.</p>
</div>

# Exploratory Factor Analysis

The critical assumptions underlying exploratory factor analysis are more conceptual than statistical. The concerns center as much on the character and composition of variable included in the analysis as on their statistical qualities. More on conceptual issues and statistical issues can be found in the accompanying notes.

## Exploring possible factors

Exploring possible factors refer to identifying the underlying structure of relationship. The decision must be made concerning;

1.  The method of extracting the factors (CFA vs PCA), and;
2.  The number of factors selected to represent the underlying structure in the data.

In this guide, we already have a prior idea of the expected number of factors to be extracted before undertaking the factor analysis. However, we will use a scree test and parallel analysis to check if the number of underlying factors from the tests conforms with our expectation.

Common to the tests demonstrated here follows the famous eigenvalues greater than 1. The rationale for this is that any individual factor should account for the variance of at least a single variable if it is to be retained for interpretation.

Note that an eigenvalue is simply the sum of the squared loading (variance) of variables on that factor.

Rule is, do not retain any factors which account for less variance than a single variable.

### Test 1: Scree test

The scree test is used to identify the optimum number of factors that can be extracted before the amount of unique variance begins to dominate the common variance structure. Refer to the accompanying notes for more details on unique and common variance.

All factors contain at least some unique variance, while the proportion of unique varince is substantially higher in later factors.

The plot is derived by plotting the latent roots against the number of factors in their order of extraction.

The inflection point "elbow" - point at which the curve first begins to straighten out- is considered to represent factors containing more unique rather common variances and thus less suitable for retention.


```r
scree(data)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/scree-test-1.png" width="70%" style="display: block; margin: auto;" />

### Test 2: Parallel analysis

The procedure generates a large number of simulated dataset with random values for the same number of variables and sample sizes. Then each of these simulated dataset is then factor analyzed, either with PCA and CFA and the eigenvalues are averaged for each factor across all the dataset. The results is the average eigenvalues for the first, second and so on across the set of simulated dataset. These values are then compared to the eigenvalues extracted for the original data. All factors with eigenvalues above those average eigenvalues are retained.

The parallel analysis is not yet incorporated into the major statistical packages like SPSS.


```r
fa.parallel(data, fm = "ml")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/parallel-analysis-1.png" width="70%" style="display: block; margin: auto;" />

```
Parallel analysis suggests that the number of factors =  4  and the number of components =  3 
```

### Test 3: Kaiser-Guttman Criterion

The criterion is also known as "eigenvalues greater than 1 rule". Hence, the test suggests how many factors to retain based on eigenvalues greater than 1.

In the code, we predetermined the number of possible factors for exploratory factor analysis. The results recommends 3 factors to retained. The bonding and bridging construct reflect under social capital, the two factors may have been grouped in one resulting only to three recommended factors.


```r
KGC(x = data,
    eigen_type = c("PCA", "EFA", "SMC"),
    n_factors = 4)
```

```
i 'x' was not a correlation matrix. Correlations are found from entered raw data.
```

```

Eigenvalues were found using PCA, EFA, and SMC.

-- Number of factors suggested by Kaiser-Guttmann criterion --------------------

( ) With PCA-determined eigenvalues:  3
( ) With SMC-determined eigenvalues:  3
( ) With EFA-determined eigenvalues:  3
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="70%" style="display: block; margin: auto;" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="70%" style="display: block; margin: auto;" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-3.png" width="70%" style="display: block; margin: auto;" />

## Assessing factorability

### Test 1: Kaiser-Meyen-Olkin (KMO)

An index ranges from 0 to 1, approaching 1 means each variable is perfectly predicted without error by other variables.

It is a statistic indicating the proportion of variance in your variables that might be caused by underlying factor.

Some guidelines:

-   `\(\ge 0.90\)` - marvelous
-   `\(\ge 0.80\)` - meritorious
-   `\(\ge 0.70\)` - middling
-   `\(\ge 0.60\)` - mediocre
-   `\(\ge 0.50\)` - miserable
-   `\(< 0.50\)` - unacceptable

The MSA values may increase from the following:

1.  sample size increases
2.  the average correlation increases
3.  the number of variable increases
4.  the number of factor decreases

If MSA falls below 0.50, then variable specific MSA values can identify variable for deletion to achieve an overall value of greater than 0.50 overall MSA value. May follow the following steps.

1.  Identify the variables with lowest MSA subject for deletion then recalculate factor analysis.
2.  Repeat the process until an MSA 0.50 and above is achieved.

Purpose of elimination with MSA under 0.50 is that these variables' correlation with other variables are poorly representing the extracted factors.

Often low MSA end up as a single variable on a factor result from their lack of association with any of the other variables. It means they are not correlated high enough with other variables in the analysis to be suitable for EFA.

In the sample data, both the overall and individual MSA ranging from meritorious to marvelous.


```r
KMO(data)
```

```
i 'x' was not a correlation matrix. Correlations are found from entered raw data.
```

```

-- Kaiser-Meyer-Olkin criterion (KMO) ------------------------------------------

v The overall KMO value for your data is marvellous.
  These data are probably suitable for factor analysis.

  Overall: 0.902

  For each variable:
bridging1 bridging2 bridging3  bonding1  bonding2  bonding3  bonding4       ca1 
    0.884     0.876     0.911     0.924     0.919     0.879     0.931     0.924 
      ca2       ca3       ca4       om1       om2       om3       om4 
    0.902     0.902     0.901     0.925     0.925     0.856     0.890 
```

### Test 2: Bartlett Test of Sphericity

The test examines the entire correlation matrix. Test the hypothesis that correlation matrix is an identity matrix. It is a statistical test for the presence of correlation among the variables. Providing statistical significance indicating the correlation matrix has significant correlation among at least some of the variables.

Increasing sample size causes the Bartlett test to become more sensitive in detecting correlation among the variables.

We use the `BARTLETT()` function from `EFAtools` to conduct the test. A significant results signifies data are appropriate for factor analysis.


```r
BARTLETT(x = data,
         N = nrow(data))
```

```
i 'x' was not a correlation matrix. Correlations are found from entered raw data.
```

```
Warning in BARTLETT(x = data, N = nrow(data)): ! 'N' was set and data entered. Taking N from data.
```

```

v The Bartlett's test of sphericity was significant at an alpha level of .05.
  These data are probably suitable for factor analysis.

  <U+0001D712>²(105) = 3342.25, p < .001
```

## Extracting factor components

The process of estimating factors and loadings involve the selection fo the principal component analysis versus the common factor. Principal component analysis (PCA) is used when the objective is to summarize the most of the original information (variance). It considers the total variance and derivatives factors that contain small proportions of unique variance and, in some instances, error variance. Whereas, common factor analysis is used primarily to identify underlying factors or dimensions that reflect what the variables share in common. It considers only the common or shared variance, assuming that both the unique and error variance are not of interest in defining the structure of the variables.

The most important tool in interpreting the factor is 'factor rotation'. The ultimate effect of rotating the factor matrix is to redistribute the variance from earlier factors to later ones to achieve a simpler, theoretically more meaningful factor pattern. Factor rotation achieve a simpler and theoretically more meaningful factor solutions. In most cases rotation of the factors improves the interpretation by reducing some of the ambiguities that often accompany initial unrotated factor solutions. Categorically, rotation falls under orthogonal and oblique factor rotation.

## Orthogonal Factor Rotation

Orthogonal rotation are more frequently applied because the analytical procedure for performing oblique rotations are not as well developed and are still subject to some controversy.

### Varimax

The varimax is the mostly widely used orthogonal rotation method. The criterion centers on simplifying the columns of the factor matrix. It maximizes the sum of variance of required loadings of the factor matrix. Results are some high loadings (i.e., close to -1 or +1) along with some loadings near 0 in each column of the matrix. Closer to -1 or +1 indicating a clear positive or negative associations between variables and factors. Close to 0 indicate a clear lack of association. Varimax rotation often give a clear separation of factors.

Using `pscyh` package and the previous tests on the factorability of the data, four factors were specified with varimax rotation. We added an argument `cor` to specify the type of method in extracting the correlation coefficient or factor loadings. Since the data are in a response scale, pearson correlation may be less suitable as it requires measurement to be continuous. Here we used a polychoric correlation to compute for the factor loading. The use of polychoric correlation permit the use of any combination of categorical and continuous indicator.


```r
efa_varimax <- psych::fa(r = data,
                         nfactors = 4, 
                         rotate = "varimax",
                         cor = "poly")

# printing the results, excluding loadings less than 0.6
print(efa_varimax$loadings, cutoff = 0.6)
```

```

Loadings:
          MR2   MR1   MR3   MR4  
bridging1                   0.788
bridging2                   0.837
bridging3                   0.742
bonding1        0.716            
bonding2        0.774            
bonding3        0.854            
bonding4        0.706            
ca1                   0.742      
ca2                   0.758      
ca3                   0.763      
ca4                   0.749      
om1       0.759                  
om2       0.773                  
om3       0.869                  
om4       0.790                  

                 MR2   MR1   MR3   MR4
SS loadings    3.062 2.976 2.811 2.449
Proportion Var 0.204 0.198 0.187 0.163
Cumulative Var 0.204 0.403 0.590 0.753
```

### Quartimax

Centers on simplifying rows of a factor matrix. Focuses on rotating the initial factors so that a variable loads high on one factor and as low as possible on all other factors. Many variables can load high or near high on the same factors because the technique centers on the simplifying the rows. Quartimax often does not proved successful in producing simpler structures.

The quartimax identified bridging and bonding items under one construct. Although the results is higly likely since both bonding and bridging refers to social capital concept.


```r
efa_quartimax <- fa(r = data,
                    nfactors = 4,
                    rotate = "quartimax",
                    cor = "poly")
```

```
Loading required namespace: GPArotation
```

```r
print(efa_quartimax$loadings, cutoff = 0.6)
```

```

Loadings:
          MR1    MR2    MR3    MR4   
bridging1  0.805                     
bridging2  0.840                     
bridging3  0.784                     
bonding1   0.837                     
bonding2   0.833                     
bonding3   0.826                     
bonding4   0.756                     
ca1                      0.682       
ca2                      0.700       
ca3                      0.700       
ca4                      0.698       
om1               0.760              
om2               0.774              
om3               0.874              
om4               0.796              

                 MR1   MR2   MR3   MR4
SS loadings    5.384 3.112 2.105 0.698
Proportion Var 0.359 0.207 0.140 0.047
Cumulative Var 0.359 0.566 0.707 0.753
```

### Equamax

A compromise between varimax and quartimax. Rather concentrating on simplification of the rows or on simplification of the column, it tries to accomplish some of each. The equamax has bot gained widespread acceptance and is used infrequently.


```r
efa_equamax <- fa(r = data,
                    nfactors = 4,
                    rotate = "equamax",
                    cor = "poly")

print(efa_equamax$loadings, cutoff = 0.6)
```

```

Loadings:
          MR2   MR1   MR4   MR3  
bridging1             0.814      
bridging2             0.862      
bridging3             0.767      
bonding1        0.695            
bonding2        0.756            
bonding3        0.842            
bonding4        0.690            
ca1                         0.736
ca2                         0.752
ca3                         0.758
ca4                         0.745
om1       0.756                  
om2       0.771                  
om3       0.867                  
om4       0.787                  

                 MR2   MR1   MR4   MR3
SS loadings    3.018 2.788 2.767 2.726
Proportion Var 0.201 0.186 0.184 0.182
Cumulative Var 0.201 0.387 0.572 0.753
```

## Oblique rotation

More flexible as this rotation need not to be orthogonal. Allow correlated factors instead of maintaining independence between the rotated factors. More realistic because theoretically important underlying dimension are not assumed to be uncorrelated with each other. Since in reality more or less variables has some degree of association. The promax and oblimin are the widely used rotation under oblique rotation. When statistical softwares were not used, promax provide easier manual computation than oblimin, however the used of computer eliminate this advantage of promax over oblimin. Today, oblimin are used more than promax although both provide quite the same output.

In comparison with the sample data, oblimin provide better extraction based on the factor loadings.

### Promax


```r
efa_promax <- fa(r = data,
                    nfactors = 4,
                    rotate = "promax",
                    cor = "poly")

print(efa_promax$loadings, cutoff = 0.6)
```

```

Loadings:
          MR2    MR1    MR3    MR4   
bridging1                       0.870
bridging2                       0.946
bridging3                       0.810
bonding1          0.714              
bonding2          0.840              
bonding3          1.008              
bonding4          0.769              
ca1                      0.807       
ca2                      0.843       
ca3                      0.808       
ca4                      0.835       
om1        0.819                     
om2        0.844                     
om3        0.933                     
om4        0.819                     

                 MR2   MR1   MR3   MR4
SS loadings    2.956 2.842 2.723 2.384
Proportion Var 0.197 0.189 0.182 0.159
Cumulative Var 0.197 0.387 0.568 0.727
```

### Oblimin


```r
efa_oblimin <- fa(r = data,
                    nfactors = 4,
                    rotate = "oblimin",
                    cor = "poly")

print(efa_oblimin$loadings, cutoff = 0.6)
```

```

Loadings:
          MR2    MR3    MR1    MR4   
bridging1                       0.865
bridging2                       0.939
bridging3                       0.805
bonding1                 0.697       
bonding2                 0.820       
bonding3                 0.985       
bonding4                 0.751       
ca1               0.806              
ca2               0.843              
ca3               0.806              
ca4               0.833              
om1        0.816                     
om2        0.841                     
om3        0.929                     
om4        0.816                     

                 MR2   MR3   MR1   MR4
SS loadings    2.934 2.714 2.709 2.355
Proportion Var 0.196 0.181 0.181 0.157
Cumulative Var 0.196 0.377 0.557 0.714
```

