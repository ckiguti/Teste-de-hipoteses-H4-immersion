ANCOVA test for `pos.score`\~`pre.score`+`scenario`\*`immersion`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “pos.score”](#ancova-plots-for-the-dependent-variable-pos.score)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for pos.score
    [../data/table-for-pos.score.csv](../data/table-for-pos.score.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | immersion | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr | symmetry | skewness | kurtosis |
|:-------------|:----------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|:---------|---------:|---------:|
| gamified     | lower     | pos.score |   8 | 9.125 |      9 |   8 |  10 | 0.835 | 0.295 | 0.698 | 1.25 | few data |    0.000 |    0.000 |
| gamified     | upper     | pos.score |   8 | 8.750 |      9 |   7 |  10 | 1.035 | 0.366 | 0.865 | 1.25 | YES      |   -0.254 |   -1.377 |
| non.gamified | lower     | pos.score |   7 | 7.143 |      7 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 | few data |    0.000 |    0.000 |
| non.gamified | upper     | pos.score |   6 | 6.667 |      7 |   5 |   8 | 1.033 | 0.422 | 1.084 | 0.75 | YES      |   -0.370 |   -1.372 |
| NA           | NA        | pos.score |  29 | 8.034 |      8 |   5 |  10 | 1.375 | 0.255 | 0.523 | 2.00 | YES      |   -0.218 |   -0.852 |

![](ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(

)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|           | var       |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----------|:----------|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| pos.score | pos.score |  29 |    0.282 |   -1.013 | YES      |     0.958 | Shapiro-Wilk | 0.288 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | immersion | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:----------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|:----------|:-------------|----------:|------:|:---------|
| gamified     | lower     | pos.score |   8 | 9.125 |      9 |   8 |  10 | 0.835 | 0.295 | 0.698 | 1.25 | NO        | NA           |        NA | 1.000 | NA       |
| gamified     | upper     | pos.score |   8 | 8.750 |      9 |   7 |  10 | 1.035 | 0.366 | 0.865 | 1.25 | YES       | Shapiro-Wilk |     0.917 | 0.408 | ns       |
| non.gamified | lower     | pos.score |   7 | 7.143 |      7 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 | NO        | NA           |        NA | 1.000 | NA       |
| non.gamified | upper     | pos.score |   6 | 6.667 |      7 |   5 |   8 | 1.033 | 0.422 | 1.084 | 0.75 | YES       | Shapiro-Wilk |     0.915 | 0.473 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["pos.score"]], x=covar, y="pos.score", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|             | var       | method         | formula                         |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------------|:----------|:---------------|:--------------------------------|----:|--------:|--------:|----------:|------:|:---------|
| pos.score.1 | pos.score | Levene’s test  | `.res`\~`scenario`\*`immersion` |  29 |       3 |      25 |     0.730 | 0.544 | ns       |
| pos.score.2 | pos.score | Anova’s slopes | `.res`\~`scenario`\*`immersion` |  29 |       3 |      21 |     0.436 | 0.729 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|             | scenario     | immersion | variable  |   n |  mean | median | min | max |    sd |    se |    ci |  iqr |
|:------------|:-------------|:----------|:----------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|
| pos.score.1 | gamified     | lower     | pos.score |   8 | 9.125 |      9 |   8 |  10 | 0.835 | 0.295 | 0.698 | 1.25 |
| pos.score.2 | gamified     | upper     | pos.score |   8 | 8.750 |      9 |   7 |  10 | 1.035 | 0.366 | 0.865 | 1.25 |
| pos.score.3 | non.gamified | lower     | pos.score |   7 | 7.143 |      7 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 |
| pos.score.4 | non.gamified | upper     | pos.score |   6 | 6.667 |      7 |   5 |   8 | 1.033 | 0.422 | 1.084 | 0.75 |

![](ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var       | Effect             | DFn | DFd |    SSn |   SSd |      F |     p |   ges | p.signif |
|:----------|:-------------------|----:|----:|-------:|------:|-------:|------:|------:|:---------|
| pos.score | pre.score          |   1 |  24 | 12.226 | 10.34 | 28.377 | 0.000 | 0.542 | \*\*\*\* |
| pos.score | scenario           |   1 |  24 | 23.941 | 10.34 | 55.570 | 0.000 | 0.698 | \*\*\*\* |
| pos.score | immersion          |   1 |  24 |  0.066 | 10.34 |  0.153 | 0.699 | 0.006 | ns       |
| pos.score | scenario:immersion |   1 |  24 |  0.032 | 10.34 |  0.075 | 0.787 | 0.003 | ns       |

### Pairwise comparison

| var       | scenario     | immersion | group1   | group2       | estimate | conf.low | conf.high |    se | statistic |     p | p.adj | p.adj.signif |
|:----------|:-------------|:----------|:---------|:-------------|---------:|---------:|----------:|------:|----------:|------:|------:|:-------------|
| pos.score | NA           | lower     | gamified | non.gamified |    1.910 |    1.208 |     2.612 | 0.340 |     5.618 | 0.000 | 0.000 | \*\*\*\*     |
| pos.score | NA           | upper     | gamified | non.gamified |    1.775 |    1.034 |     2.517 | 0.359 |     4.943 | 0.000 | 0.000 | \*\*\*\*     |
| pos.score | gamified     | NA        | lower    | upper        |   -0.045 |   -0.742 |     0.652 | 0.338 |    -0.133 | 0.895 | 0.895 | ns           |
| pos.score | non.gamified | NA        | lower    | upper        |   -0.180 |   -0.975 |     0.615 | 0.385 |    -0.467 | 0.645 | 0.645 | ns           |

### Descriptive Statistic of Estimated Marginal Means

| var       | scenario     | immersion | pre.score | emmean | se.emms |  df | conf.low | conf.high | method       |   n |  mean | median | min | max |    sd | se.ds |    ci |  iqr | n.pre.score | mean.pre.score | median.pre.score | min.pre.score | max.pre.score | sd.pre.score | se.pre.score | ci.pre.score | iqr.pre.score | sd.emms |
|:----------|:-------------|:----------|----------:|-------:|--------:|----:|---------:|----------:|:-------------|----:|------:|-------:|----:|----:|------:|------:|------:|-----:|------------:|---------------:|-----------------:|--------------:|--------------:|-------------:|-------------:|-------------:|--------------:|--------:|
| pos.score | gamified     | lower     |     6.828 |  8.841 |   0.238 |  24 |    8.350 |     9.333 | Emmeans test |   8 | 9.125 |      9 |   8 |  10 | 0.835 | 0.295 | 0.698 | 1.25 |           8 |          7.250 |              7.5 |             6 |             8 |        0.886 |        0.313 |        0.741 |          1.25 |   0.673 |
| pos.score | gamified     | upper     |     6.828 |  8.886 |   0.233 |  24 |    8.404 |     9.368 | Emmeans test |   8 | 8.750 |      9 |   7 |  10 | 1.035 | 0.366 | 0.865 | 1.25 |           8 |          6.625 |              7.0 |             5 |             8 |        0.916 |        0.324 |        0.766 |          1.00 |   0.660 |
| pos.score | non.gamified | lower     |     6.828 |  6.931 |   0.251 |  24 |    6.412 |     7.450 | Emmeans test |   7 | 7.143 |      7 |   6 |   8 | 0.900 | 0.340 | 0.832 | 1.50 |           7 |          7.143 |              7.0 |             6 |             8 |        0.900 |        0.340 |        0.832 |          1.50 |   0.665 |
| pos.score | non.gamified | upper     |     6.828 |  7.111 |   0.281 |  24 |    6.532 |     7.690 | Emmeans test |   6 | 6.667 |      7 |   5 |   8 | 1.033 | 0.422 | 1.084 | 0.75 |           6 |          6.167 |              6.5 |             4 |             8 |        1.472 |        0.601 |        1.545 |          1.75 |   0.687 |

### Ancova plots for the dependent variable “pos.score”

``` r
plots <- twoWayAncovaPlots(sdat[["pos.score"]], "pos.score", between
, aov[["pos.score"]], pwc[["pos.score"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `pos.score` \~ `scenario`

``` r
plots[["scenario"]]
```

![](ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

#### Plot for: `pos.score` \~ `immersion`

``` r
plots[["immersion"]]
```

![](ancova_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “pre.score”, ANCOVA tests
with independent between-subjects variables “scenario” (gamified,
non.gamified) and “immersion” (lower, upper) were performed to determine
statistically significant difference on the dependent varibles
“pos.score”. For the dependent variable “pos.score”, there was
statistically significant effects in the factor “pre.score” with
F(1,24)=28.377, p&lt;0.001 and ges=0.542 (effect size) and in the factor
“scenario” with F(1,24)=55.57, p&lt;0.001 and ges=0.698 (effect size).

Pairwise comparisons using the Estimated Marginal Means (EMMs) were
computed to find statistically significant diferences among the groups
defined by the independent variables, and with the p-values ajusted by
the method “bonferroni”. For the dependent variable “pos.score”, the
mean in the scenario=“gamified” (adj M=8.841 and SD=0.835) was
significantly different than the mean in the scenario=“non.gamified”
(adj M=6.931 and SD=0.9) with p-adj&lt;0.001; the mean in the
scenario=“gamified” (adj M=8.886 and SD=1.035) was significantly
different than the mean in the scenario=“non.gamified” (adj M=7.111 and
SD=1.033) with p-adj&lt;0.001.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.