
<!-- README.md is generated from README.Rmd. Please edit that file -->

[![DOI](http://joss.theoj.org/papers/10.21105/joss.00584/status.svg)](https://doi.org/10.21105/joss.00584)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1173313.svg)](https://doi.org/10.5281/zenodo.1173313)
[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/vtreat)](https://cran.r-project.org/package=vtreat)

`vtreat` is a `data.frame` processor/conditioner (available [for
`R`](https://github.com/WinVector/vtreat), and [for
`Python`](https://github.com/WinVector/pyvtreat)) that prepares
real-world data for supervised machine learning or predictive modeling
in a statistically sound manner.

`vtreat` takes an input `data.frame` that has a specified column called
“the outcome variable” (or “y”) that is the quantity to be predicted
(and must not have missing values). Other input columns are possible
explanatory variables (typically numeric or categorical/string-valued,
these columns may have missing values) that the user later wants to use
to predict “y”. In practice such an input `data.frame` may not be
immediately suitable for machine learning procedures that often expect
only numeric explanatory variables, and may not tolerate missing values.

To solve this, `vtreat` builds a transformed `data.frame` where all
explanatory variable columns have been transformed into a number of
numeric explanatory variable columns, without missing values. The
`vtreat` implementation produces derived numeric columns that capture
most of the information relating the explanatory columns to the
specified “y” or dependent/outcome column through a number of numeric
transforms (indicator variables, impact codes, prevalence codes, and
more). This transformed `data.frame` is suitable for a wide range of
supervised learning methods from linear regression, through gradient
boosted machines.

The idea is: you can take a `data.frame` of messy real world data and
easily, faithfully, reliably, and repeatably prepare it for machine
learning using documented methods using `vtreat`. Incorporating `vtreat`
into your machine learning workflow lets you quickly work with very
diverse structured data.

In all cases (classification, regression, unsupervised, and multinomial
classification) the intent is that `vtreat` transforms are essentially
one liners.

The preparation commands are organized as follows:

  - **Regression**: [`R` regression
    example](https://github.com/WinVector/vtreat/blob/master/Examples/Regression/Regression.md),
    [`Python` regression
    example](https://github.com/WinVector/pyvtreat/blob/master/Examples/Regression/Regression.md).
  - **Classification**: [`R` classification
    example](https://github.com/WinVector/vtreat/blob/master/Examples/Classification/Classification.md),
    [`Python` classification
    example](https://github.com/WinVector/pyvtreat/blob/master/Examples/Classification/Classification.md).
  - **Unsupervised tasks**: [`R` unsupervised
    example](https://github.com/WinVector/vtreat/blob/master/Examples/Unsupervised/Unsupervised.md),
    [`Python` unsupervised
    example](https://github.com/WinVector/pyvtreat/blob/master/Examples/Unsupervised/Unsupervised.md).
  - **Multinomial classification**: [`R` multinomial classification
    example](https://github.com/WinVector/vtreat/blob/master/Examples/Multinomial/MultinomialExample.md),
    [`Python` multinomial classification
    example](https://github.com/WinVector/pyvtreat/blob/master/Examples/Multinomial/MultinomialExample.md).

In all cases: variable preperation is intended to be a “one liner.”

These current revisions of the examples are designed to be small, yet
complete. So as a set they have some overlap, but the user can rely
mostly on a single example for a single task type.

For more detail please see here: [arXiv:1611.09477
stat.AP](https://arxiv.org/abs/1611.09477) (the documentation describes
the `R` version, however all of the examples can be found worked in
`Python`
[here](https://github.com/WinVector/pyvtreat/tree/master/Examples/vtreat_paper1)).

`vtreat` is available as an [`R`
package](https://github.com/WinVector/vtreat), and also as a
[`Python`/`Pandas` package](https://github.com/WinVector/vtreat).

![](https://github.com/WinVector/vtreat/raw/master/tools/vtreat.png)

(logo: Julie Mount, source: “The Harvest” by Boris Kustodiev 1914)

Even with modern machine learning techniques (random forests, support
vector machines, neural nets, gradient boosted trees, and so on) or
standard statistical methods (regression, generalized regression,
generalized additive models) there are *common* data issues that can
cause modeling to fail. vtreat deals with a number of these in a
principled and automated fashion.

In particular vtreat emphasizes a concept called “y-aware
pre-processing” and implements:

  - Treatment of missing values through safe replacement plus indicator
    column (a simple but very powerful method when combined with
    downstream machine learning algorithms).
  - Treatment of novel levels (new values of categorical variable seen
    during test or application, but not seen during training) through
    sub-models (or impact/effects coding of pooled rare events).
  - Explicit coding of categorical variable levels as new indicator
    variables (with optional suppression of non-significant indicators).
  - Treatment of categorical variables with very large numbers of levels
    through sub-models (again [impact/effects
    coding](http://www.win-vector.com/blog/2012/07/modeling-trick-impact-coding-of-categorical-variables-with-many-levels/)).
  - (optional) User specified significance pruning on levels coded into
    effects/impact sub-models.
  - Correct treatment of nested models or sub-models through data split
    (see
    [here](https://winvector.github.io/vtreat/articles/vtreatOverfit.html))
    or through the generation of “cross validated” data frames (see
    [here](https://winvector.github.io/vtreat/articles/vtreatCrossFrames.html));
    these are issues similar to what is required to build statistically
    efficient stacked models or super-learners).
  - Safe processing of “wide data” (data with very many variables, often
    driving common machine learning algorithms to over-fit) through [out
    of sample per-variable significance estimates and user controllable
    pruning](https://winvector.github.io/vtreat/articles/vtreatSignificance.html)
    (something we have lectured on previously
    [here](https://github.com/WinVector/WinVector.github.io/tree/master/DS)
    and
    [here](http://www.win-vector.com/blog/2014/02/bad-bayes-an-example-of-why-you-need-hold-out-testing/)).
  - Collaring/Winsorizing of unexpected out of range numeric inputs.
  - (optional) Conversion of all variables into effects (or “y-scale”)
    units (through the optional `scale` argument to `vtreat::prepare()`,
    using some of the ideas discussed
    [here](http://www.win-vector.com/blog/2014/06/skimming-statistics-papers-for-the-ideas-instead-of-the-complete-procedures/)).
    This allows correct/sensible application of principal component
    analysis pre-processing in a machine learning context.
  - Joining in additional training distribution data (which can be
    useful in analysis, called “catP” and “catD”).

The idea is: even with a sophisticated machine learning algorithm there
are *many* ways messy real world data can defeat the modeling process,
and vtreat helps with at least ten of them. We emphasize: these problems
are already in your data, you simply build better and more reliable
models if you attempt to mitigate them. Automated processing is no
substitute for actually looking at the data, but vtreat supplies
efficient, reliable, documented, and tested implementations of many of
the commonly needed transforms.

To help explain the methods we have prepared some documentation:

  - The [vtreat package
    overall](https://winvector.github.io/vtreat/index.html).
  - [Preparing data for analysis using R
    white-paper](http://winvector.github.io/DataPrep/EN-CNTNT-Whitepaper-Data-Prep-Using-R.pdf)
  - The [types of new
    variables](https://winvector.github.io/vtreat/articles/vtreatVariableTypes.html)
    introduced by vtreat processing (including how to limit down to
    domain appropriate variable types).
  - Statistically sound treatment of the nested modeling issue
    introduced by any sort of pre-processing (such as vtreat itself):
    [nested over-fit
    issues](https://winvector.github.io/vtreat/articles/vtreatOverfit.html)
    and a general [cross-frame
    solution](https://winvector.github.io/vtreat/articles/vtreatCrossFrames.html).
  - [Principled ways to pick significance based pruning
    levels](https://winvector.github.io/vtreat/articles/vtreatSignificance.html).

Data treatments are “y-aware” (use distribution relations between
independent variables and the dependent variable). For binary
classification use `designTreatmentsC()` and for numeric regression use
`designTreatmentsN()`.

After the design step, `prepare()` should be used as you would use
model.matrix. `prepare()` treated variables are all numeric and never
take the value NA or +-Inf (so are very safe to use in modeling).

In application we suggest splitting your data into three sets: one for
building vtreat encodings, one for training models using these
encodings, and one for test and model evaluation.

The purpose of `vtreat` library is to reliably prepare data for
supervised machine learning. We try to leave as much as possible to the
machine learning algorithms themselves, but cover most of the truly
necessary typically ignored precautions. The library is designed to
produce a `data.frame` that is entirely numeric and takes common
precautions to guard against the following real world data issues:

  - Categorical variables with very many levels.
    
    We re-encode such variables as a family of indicator or dummy
    variables for common levels plus an additional [impact
    code](http://www.win-vector.com/blog/2012/07/modeling-trick-impact-coding-of-categorical-variables-with-many-levels/)
    (also called “effects coded”). This allows principled use (including
    smoothing) of huge categorical variables (like zip-codes) when
    building models. This is critical for some libraries (such as
    `randomForest`, which has hard limits on the number of allowed
    levels).

  - Rare categorical levels.
    
    Levels that do not occur often during training tend not to have
    reliable effect estimates and contribute to over-fit. vtreat helps
    with 2 precautions in this case. First the `rareLevel` argument
    suppresses levels with this count our below from modeling, except
    possibly through a grouped contribution. Also with enough data
    vtreat attempts to estimate out of sample performance of derived
    variables. Finally we suggest users reserve a portion of data for
    vtreat design, separate from any data used in additional training,
    calibration, or testing.

  - Novel categorical levels.
    
    A common problem in deploying a classifier to production is: new
    levels (levels not seen during training) encountered during model
    application. We deal with this by encoding categorical variables in
    a possibly redundant manner: reserving a dummy variable for all
    levels (not the more common all but a reference level scheme). This
    is in fact the correct representation for regularized modeling
    techniques and lets us code novel levels as all dummies
    simultaneously zero (which is a reasonable thing to try). This
    encoding while limited is cheaper than the fully Bayesian solution
    of computing a weighted sum over previously seen levels during model
    application.

  - Missing/invalid values NA, NaN, +-Inf.
    
    Variables with these issues are re-coded as two columns. The first
    column is clean copy of the variable (with missing/invalid values
    replaced with either zero or the grand mean, depending on the user
    chose of the `scale` parameter). The second column is a dummy or
    indicator that marks if the replacement has been performed. This is
    simpler than imputation of missing values, and allows the downstream
    model to attempt to use missingness as a useful signal (which it
    often is in industrial data).

  - Extreme values.
    
    Variables can be restricted to stay in ranges seen during training.
    This can defend against some run-away classifier issues during model
    application.

  - Constant and near-constant variables.
    
    Variables that “don’t vary” or “nearly don’t vary” are suppressed.

  - Need for estimated single-variable model effect sizes and
    significances.
    
    It is a dirty secret that even popular machine learning techniques
    need some variable pruning (when exposed to very wide data frames,
    see
    [here](http://www.win-vector.com/blog/2014/02/bad-bayes-an-example-of-why-you-need-hold-out-testing/)
    and [here](https://www.youtube.com/watch?v=X_Rn3EOEjGE)). We make
    the necessary effect size estimates and significances easily
    available and supply initial variable pruning.

The above are all awful things that often lurk in real world data.
Automating these steps ensures they are easy enough that you actually
perform them and leaves the analyst time to look for additional data
issues. For example this allowed us to essentially automate a number of
the steps taught in chapters 4 and 6 of [*Practical Data Science with R*
(Zumel, Mount; Manning 2014)](http://practicaldatascience.com/) into a
[very short
worksheet](http://winvector.github.io/KDD2009/KDD2009RF.html) (though we
think for understanding it is *essential* to work all the steps by hand
as we did in the book). The 2nd edition of *Practical Data Science with
R* covers using `vtreat` in `R` in chapter 8 “Advanced Data
Preparation.”

The idea is: `data.frame`s prepared with the `vtreat` library are
somewhat safe to train on as some precaution has been taken against all
of the above issues. Also of interest are the `vtreat` variable
significances (help in initial variable pruning, a necessity when there
are a large number of columns) and `vtreat::prepare(scale=TRUE)` which
re-encodes all variables into effect units making them suitable for
y-aware dimension reduction (variable clustering, or principal component
analysis) and for geometry sensitive machine learning techniques
(k-means, knn, linear SVM, and more). You may want to do more than the
`vtreat` library does (such as Bayesian imputation, variable clustering,
and more) but you certainly do not want to do less.

There have been a number of recent substantial improvements to the
library, including:

  - Out of sample scoring.
  - Ability to use `parallel`.
  - More general calculation of effect sizes and significances.

Some of our related articles (which should make clear some of our
motivations, and design decisions):

  - [Modeling trick: impact coding of categorical variables with many
    levels](http://www.win-vector.com/blog/2012/07/modeling-trick-impact-coding-of-categorical-variables-with-many-levels/)
  - [A bit more on impact
    coding](http://www.win-vector.com/blog/2012/08/a-bit-more-on-impact-coding/)
  - [vtreat: designing a package for variable
    treatment](http://www.win-vector.com/blog/2014/08/vtreat-designing-a-package-for-variable-treatment/)
  - [A comment on preparing data for
    classifiers](http://www.win-vector.com/blog/2014/12/a-comment-on-preparing-data-for-classifiers/)
  - [Nina Zumel presenting on
    vtreat](http://www.slideshare.net/ChesterChen/vtreat)
  - [What is new in the vtreat
    library?](http://www.win-vector.com/blog/2015/05/what-is-new-in-the-vtreat-library/)
  - [How do you know if your data has
    signal?](http://www.win-vector.com/blog/2015/08/how-do-you-know-if-your-data-has-signal/)

Examples of current best practice using `vtreat` (variable coding,
train, test split) can be found
[here](https://winvector.github.io/vtreat/articles/vtreatOverfit.html)
and [here](http://winvector.github.io/KDD2009/KDD2009RF.html).

Trivial example:

``` r
library("vtreat")
packageVersion("vtreat")
 #  [1] '1.4.7'
citation('vtreat')
 #  
 #  To cite package 'vtreat' in publications use:
 #  
 #    John Mount and Nina Zumel (2019). vtreat: A Statistically Sound
 #    'data.frame' Processor/Conditioner.
 #    https://github.com/WinVector/vtreat/,
 #    https://winvector.github.io/vtreat/.
 #  
 #  A BibTeX entry for LaTeX users is
 #  
 #    @Manual{,
 #      title = {vtreat: A Statistically Sound 'data.frame' Processor/Conditioner},
 #      author = {John Mount and Nina Zumel},
 #      year = {2019},
 #      note = {https://github.com/WinVector/vtreat/, https://winvector.github.io/vtreat/},
 #    }

# categorical example
dTrainC <- data.frame(x=c('a', 'a', 'a', 'b', 'b', NA, NA),
   z=c(1, 2, 3, 4, NA, 6, NA),
   y=c(FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, TRUE))
dTestC <- data.frame(x=c('a', 'b', 'c', NA), z=c(10, 20, 30, NA))

# help("designTreatmentsC")

treatmentsC <- designTreatmentsC(dTrainC, colnames(dTrainC), 'y', TRUE,
                                 verbose=FALSE)
print(treatmentsC$scoreFrame[, c('origName', 'varName', 'code', 'rsq', 'sig', 'extraModelDegrees')])
 #    origName   varName  code         rsq        sig extraModelDegrees
 #  1        x    x_catP  catP 0.111456141 0.30194137                 2
 #  2        x    x_catB  catB 0.115273608 0.29380616                 2
 #  3        z         z clean 0.237601767 0.13176020                 0
 #  4        z   z_isBAD isBAD 0.296065432 0.09248399                 0
 #  5        x  x_lev_NA   lev 0.296065432 0.09248399                 0
 #  6        x x_lev_x_a   lev 0.130005705 0.26490379                 0
 #  7        x x_lev_x_b   lev 0.006067337 0.80967242                 0

# help("prepare")

dTrainCTreated <- prepare(treatmentsC, dTrainC, pruneSig=1.0, scale=TRUE)
varsC <- setdiff(colnames(dTrainCTreated), 'y')
# all input variables should be mean 0
sapply(dTrainCTreated[, varsC, drop=FALSE], mean)
 #         x_catP        x_catB             z       z_isBAD      x_lev_NA 
 #   2.537498e-16 -1.268826e-16  6.336166e-17  2.536414e-16 -2.537653e-16 
 #      x_lev_x_a     x_lev_x_b 
 #  -6.345680e-17  1.189718e-17
# all non NA slopes should be 1
sapply(varsC, function(c) { lm(paste('y', c, sep='~'),
   data=dTrainCTreated)$coefficients[[2]]})
 #      x_catP     x_catB          z    z_isBAD   x_lev_NA  x_lev_x_a 
 #  0.23254609 0.05841932 0.16062145 0.03162633 0.03162633 0.23254609 
 #   x_lev_x_b 
 #  0.24663035
dTestCTreated <- prepare(treatmentsC, dTestC, pruneSig=c(), scale=TRUE)
print(dTestCTreated)
 #        x_catP    x_catB         z   z_isBAD  x_lev_NA  x_lev_x_a  x_lev_x_b
 #  1 -1.0238626 -3.248380  7.437329 -5.420438 -5.420438 -1.0238626  0.1158472
 #  2  0.7678969 -2.550396 18.374578 -5.420438 -5.420438  0.7678969 -0.2896179
 #  3  3.4555361 -2.260694 29.311827 -5.420438 -5.420438  0.7678969  0.1158472
 #  4  0.7678969  7.422967  0.000000 13.551095 13.551095  0.7678969  0.1158472
```

``` r
# numeric example
dTrainN <- data.frame(x=c('a', 'a', 'a', 'a', 'b', 'b', NA, NA),
   z=c(1, 2, 3, 4, 5, NA, 7, NA), y=c(0, 0, 0, 1, 0, 1, 1, 1))
dTestN <- data.frame(x=c('a', 'b', 'c', NA), z=c(10, 20, 30, NA))
# help("designTreatmentsN")
treatmentsN = designTreatmentsN(dTrainN, colnames(dTrainN), 'y',
                                verbose=FALSE)
print(treatmentsN$scoreFrame[, c('origName', 'varName', 'code', 'rsq', 'sig', 'extraModelDegrees')])
 #    origName   varName  code          rsq       sig extraModelDegrees
 #  1        x    x_catP  catP 2.500000e-01 0.2070312                 2
 #  2        x    x_catN  catN 3.282051e-01 0.1377186                 2
 #  3        x    x_catD  catD 3.743113e-01 0.1069707                 2
 #  4        z         z clean 2.880952e-01 0.1701892                 0
 #  5        z   z_isBAD isBAD 3.333333e-01 0.1339746                 0
 #  6        x  x_lev_NA   lev 3.333333e-01 0.1339746                 0
 #  7        x x_lev_x_a   lev 2.500000e-01 0.2070312                 0
 #  8        x x_lev_x_b   lev 1.110223e-16 1.0000000                 0
dTrainNTreated <- prepare(treatmentsN, dTrainN, pruneSig=1.0, scale=TRUE)
varsN <- setdiff(colnames(dTrainNTreated), 'y')
# all input variables should be mean 0
sapply(dTrainNTreated[, varsN, drop=FALSE], mean) 
 #         x_catP        x_catN        x_catD             z       z_isBAD 
 #   2.775558e-17  0.000000e+00 -2.775558e-17  4.857226e-17  6.938894e-18 
 #       x_lev_NA     x_lev_x_a     x_lev_x_b 
 #   6.938894e-18  0.000000e+00  7.703720e-34
# all non NA slopes should be 1
sapply(varsN, function(c) { lm(paste('y', c, sep='~'),
   data=dTrainNTreated)$coefficients[[2]]}) 
 #     x_catP    x_catN    x_catD         z   z_isBAD  x_lev_NA x_lev_x_a 
 #          1         1         1         1         1         1         1 
 #  x_lev_x_b 
 #          1
dTestNTreated <- prepare(treatmentsN, dTestN, pruneSig=c(), scale=TRUE)
print(dTestNTreated)
 #    x_catP x_catN      x_catD         z    z_isBAD   x_lev_NA x_lev_x_a
 #  1 -0.250  -0.25 -0.06743804 0.9952381 -0.1666667 -0.1666667     -0.25
 #  2  0.250   0.00 -0.25818161 2.5666667 -0.1666667 -0.1666667      0.25
 #  3  0.625   0.00 -0.25818161 4.1380952 -0.1666667 -0.1666667      0.25
 #  4  0.250   0.50  0.39305768 0.0000000  0.5000000  0.5000000      0.25
 #        x_lev_x_b
 #  1 -2.266233e-17
 #  2  6.798700e-17
 #  3 -2.266233e-17
 #  4 -2.266233e-17

# for large data sets you can consider designing the treatments on 
# a subset like: d[sample(1:dim(d)[[1]], 1000), ]

# One can also use treatment plans as pipe targets.
dTrainN %.>% 
  treatmentsN %.>% 
  knitr::kable(.)
```

| x\_catP | x\_catN |   x\_catD |        z | z\_isBAD | x\_lev\_NA | x\_lev\_x\_a | x\_lev\_x\_b | y |
| ------: | ------: | --------: | -------: | -------: | ---------: | -----------: | -----------: | -: |
|    0.50 |  \-0.25 | 0.5000000 | 1.000000 |        0 |          0 |            1 |            0 | 0 |
|    0.50 |  \-0.25 | 0.5000000 | 2.000000 |        0 |          0 |            1 |            0 | 0 |
|    0.50 |  \-0.25 | 0.5000000 | 3.000000 |        0 |          0 |            1 |            0 | 0 |
|    0.50 |  \-0.25 | 0.5000000 | 4.000000 |        0 |          0 |            1 |            0 | 1 |
|    0.25 |    0.00 | 0.7071068 | 5.000000 |        0 |          0 |            0 |            1 | 0 |
|    0.25 |    0.00 | 0.7071068 | 3.666667 |        1 |          0 |            0 |            1 | 1 |
|    0.25 |    0.50 | 0.0000000 | 7.000000 |        0 |          1 |            0 |            0 | 1 |
|    0.25 |    0.50 | 0.0000000 | 3.666667 |        1 |          1 |            0 |            0 | 1 |

Related work:

  - Cohen J, Cohen P (1983). Applied Multiple Regression/Correlation
    Analysis For The Behavioral Sciences. 2 edition. Lawrence Erlbaum
    Associates, Inc. ISBN 0-89859-268-2.
  - [“A preprocessing scheme for high-cardinality categorical attributes
    in classification and prediction
    problems”](http://dl.acm.org/citation.cfm?id=507538) Daniele
    Micci-Barreca; ACM SIGKDD Explorations, Volume 3 Issue 1, July 2001
    Pages 27-32.
  - [“Modeling Trick: Impact Coding of Categorical Variables with Many
    Levels”](http://www.win-vector.com/blog/2012/07/modeling-trick-impact-coding-of-categorical-variables-with-many-levels/)
    Nina Zumel; Win-Vector blog, 2012.
  - “Big Learning Made Easy – with Counts\!”, Misha Bilenko, Cortana
    Intelligence and Machine Learning Blog, 2015.

## Installation

To install, from inside `R` please run:

``` r
install.packages("vtreat")
```

## Note

Note: `vtreat` is meant only for “tame names”, that is: variables and
column names that are also valid *simple* (without quotes) `R` variables
names.
