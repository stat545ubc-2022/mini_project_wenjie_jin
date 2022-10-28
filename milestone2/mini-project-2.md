Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.5 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

**The data is tidy** **First of all, Since the sum of duplicates when
applied to the ID variable equals zero. So that we can say each row is
an observation.** **Secondly, although I just showed the first 8
variables, however, for each column in this dataset, they all represent
a individual variable.** **Thirdly, there is only one value stays in
each cell.** **In conclusion, this data is tidy.**

``` r
sum(duplicated(cancer_sample$ID))
```

    ## [1] 0

``` r
head(cancer_sample[2:9])
```

    ## # A tibble: 6 × 8
    ##   diagnosis radius_mean texture_mean perimeter…¹ area_…² smoot…³ compa…⁴ conca…⁵
    ##   <chr>           <dbl>        <dbl>       <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 M                18.0         10.4       123.    1001   0.118   0.278   0.300 
    ## 2 M                20.6         17.8       133.    1326   0.0847  0.0786  0.0869
    ## 3 M                19.7         21.2       130     1203   0.110   0.160   0.197 
    ## 4 M                11.4         20.4        77.6    386.  0.142   0.284   0.241 
    ## 5 M                20.3         14.3       135.    1297   0.100   0.133   0.198 
    ## 6 M                12.4         15.7        82.6    477.  0.128   0.17    0.158 
    ## # … with abbreviated variable names ¹​perimeter_mean, ²​area_mean,
    ## #   ³​smoothness_mean, ⁴​compactness_mean, ⁵​concavity_mean

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

**Since this data is tidy, in the following, I tried to untidy it by
using the pivot_wider, to separate the diagnosis column into two
columns. In this case, this violates the second condition which makes
this dataset untidy.**

``` r
untidy_cancer_sample = cancer_sample %>% pivot_wider(names_from = diagnosis, values_from = radius_mean)
untidy_cancer_sample
```

    ## # A tibble: 569 × 32
    ##          ID texture_mean perim…¹ area_…² smoot…³ compa…⁴ conca…⁵ conca…⁶ symme…⁷
    ##       <dbl>        <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ##  1   842302         10.4   123.    1001   0.118   0.278   0.300   0.147    0.242
    ##  2   842517         17.8   133.    1326   0.0847  0.0786  0.0869  0.0702   0.181
    ##  3 84300903         21.2   130     1203   0.110   0.160   0.197   0.128    0.207
    ##  4 84348301         20.4    77.6    386.  0.142   0.284   0.241   0.105    0.260
    ##  5 84358402         14.3   135.    1297   0.100   0.133   0.198   0.104    0.181
    ##  6   843786         15.7    82.6    477.  0.128   0.17    0.158   0.0809   0.209
    ##  7   844359         20.0   120.    1040   0.0946  0.109   0.113   0.074    0.179
    ##  8 84458202         20.8    90.2    578.  0.119   0.164   0.0937  0.0598   0.220
    ##  9   844981         21.8    87.5    520.  0.127   0.193   0.186   0.0935   0.235
    ## 10 84501001         24.0    84.0    476.  0.119   0.240   0.227   0.0854   0.203
    ## # … with 559 more rows, 23 more variables: fractal_dimension_mean <dbl>,
    ## #   radius_se <dbl>, texture_se <dbl>, perimeter_se <dbl>, area_se <dbl>,
    ## #   smoothness_se <dbl>, compactness_se <dbl>, concavity_se <dbl>,
    ## #   concave_points_se <dbl>, symmetry_se <dbl>, fractal_dimension_se <dbl>,
    ## #   radius_worst <dbl>, texture_worst <dbl>, perimeter_worst <dbl>,
    ## #   area_worst <dbl>, smoothness_worst <dbl>, compactness_worst <dbl>,
    ## #   concavity_worst <dbl>, concave_points_worst <dbl>, symmetry_worst <dbl>, …

**Then the following code will convert the untidy data back to tidy
data.**

``` r
tidy_cancer_sample = untidy_cancer_sample %>% pivot_longer(cols = c(M, B), names_to  = 'diagnosis', values_to = 'radius_mean', values_drop_na = TRUE) %>% select(ID, diagnosis, radius_mean, everything())
tidy_cancer_sample
```

    ## # A tibble: 569 × 32
    ##          ID diagnosis radius_m…¹ textu…² perim…³ area_…⁴ smoot…⁵ compa…⁶ conca…⁷
    ##       <dbl> <chr>          <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ##  1   842302 M               18.0    10.4   123.    1001   0.118   0.278   0.300 
    ##  2   842517 M               20.6    17.8   133.    1326   0.0847  0.0786  0.0869
    ##  3 84300903 M               19.7    21.2   130     1203   0.110   0.160   0.197 
    ##  4 84348301 M               11.4    20.4    77.6    386.  0.142   0.284   0.241 
    ##  5 84358402 M               20.3    14.3   135.    1297   0.100   0.133   0.198 
    ##  6   843786 M               12.4    15.7    82.6    477.  0.128   0.17    0.158 
    ##  7   844359 M               18.2    20.0   120.    1040   0.0946  0.109   0.113 
    ##  8 84458202 M               13.7    20.8    90.2    578.  0.119   0.164   0.0937
    ##  9   844981 M               13      21.8    87.5    520.  0.127   0.193   0.186 
    ## 10 84501001 M               12.5    24.0    84.0    476.  0.119   0.240   0.227 
    ## # … with 559 more rows, 23 more variables: concave_points_mean <dbl>,
    ## #   symmetry_mean <dbl>, fractal_dimension_mean <dbl>, radius_se <dbl>,
    ## #   texture_se <dbl>, perimeter_se <dbl>, area_se <dbl>, smoothness_se <dbl>,
    ## #   compactness_se <dbl>, concavity_se <dbl>, concave_points_se <dbl>,
    ## #   symmetry_se <dbl>, fractal_dimension_se <dbl>, radius_worst <dbl>,
    ## #   texture_worst <dbl>, perimeter_worst <dbl>, area_worst <dbl>,
    ## #   smoothness_worst <dbl>, compactness_worst <dbl>, concavity_worst <dbl>, …

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

**In milestone 1, I came up with the following four research
questions:**

**Question1: Is there any outliers in those measured variables. To make
this database more meaningful, we should detect outliers and potentially
remove outliers for data processing.**

**Question2: Find out is the number of benign and malicious cancers
balanced since if it is not balanced the database may not reliable. For
instance, if 90 percent of the data are malicious then the conlusion we
made will be biased**

**Question3: How could each variable help us to predict the cancer is
malicious or benign.**

**Question4: Which variables are helpless for predicting the cancer is
malicious or benign.**

<!-------------------------- Start your work below ---------------------------->

**And I would like to choose the following two research questions for
this part **

**1. How could each variable help us to predict the cancer is malicious
or benign.** **2. Question4: Which variables are helpless for predicting
the cancer is malicious or benign.**

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

**For the frist two questions, there are meant to test whether the
database is reliable or not. However, the last two questions can help us
to identify which variables are helpful for predicting the cancer is
benign or malicious and which variables are not. Those two questions are
relatively more meaningful than the first two. **
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

**I will try to use the following functions:** **1. select: to drop some
irrelevant variables** **2. group_by** **3. summarise: to output the
summarized data (find the mean of each variable for both benign and
malicious cancer)** **4. accross** **5. where: to only summarize the
numeric cells**

<!--------------------------- Start your work below --------------------------->

``` r
summarized_cancer_sample = cancer_sample %>%
  select(-c(ID)) %>% 
  group_by(diagnosis) %>% 
  summarize(across(where(is.numeric), mean))
summarized_cancer_sample
```

    ## # A tibble: 2 × 31
    ##   diagnosis radius_mean textur…¹ perim…² area_…³ smoot…⁴ compa…⁵ conca…⁶ conca…⁷
    ##   <chr>           <dbl>    <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 B                12.1     17.9    78.1    463.  0.0925  0.0801  0.0461  0.0257
    ## 2 M                17.5     21.6   115.     978.  0.103   0.145   0.161   0.0880
    ## # … with 22 more variables: symmetry_mean <dbl>, fractal_dimension_mean <dbl>,
    ## #   radius_se <dbl>, texture_se <dbl>, perimeter_se <dbl>, area_se <dbl>,
    ## #   smoothness_se <dbl>, compactness_se <dbl>, concavity_se <dbl>,
    ## #   concave_points_se <dbl>, symmetry_se <dbl>, fractal_dimension_se <dbl>,
    ## #   radius_worst <dbl>, texture_worst <dbl>, perimeter_worst <dbl>,
    ## #   area_worst <dbl>, smoothness_worst <dbl>, compactness_worst <dbl>,
    ## #   concavity_worst <dbl>, concave_points_worst <dbl>, symmetry_worst <dbl>, …

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

**Since I don’t have such a plot, I will create a new plot as shown
below:** **And I will split the cancer’s radius into 5 different
groups**
<!-------------------------- Start your work below ---------------------------->

``` r
radius_mean_level <- numeric()
radius_mean_level[cancer_sample$radius_mean < 10] <- "small"
radius_mean_level[cancer_sample$radius_mean >= 10 & cancer_sample$radius_mean < 15] <- "small_medium"
radius_mean_level[cancer_sample$radius_mean >= 15 & cancer_sample$radius_mean < 20] <- "medium"
radius_mean_level[cancer_sample$radius_mean >= 20 & cancer_sample$radius_mean < 25] <- "large_medium"
radius_mean_level[cancer_sample$radius_mean >= 25] <- "large"
radius_mean_level <- as.factor(radius_mean_level)

# use ggplot to plot
cancer_sample$radius_mean_level <- radius_mean_level
ggplot(cancer_sample, aes(x=radius_mean_level, ..count..)) + 
    geom_bar(aes(fill=diagnosis)) + labs(x = "radius_mean_level")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 1

**For this task, I will reorder the x axis in ascending order. Since
instead of ordering the radius_mean_level alphabetically, ordering the
radius_mean_level by it’s actual size (in this case, they are small,
small_medium, medium, large_medium, and large) would give us a more
intuitive information about as the radius_mean_level larger than small
medium, the cancer cells are more likely be malicious.**

``` r
ggplot(cancer_sample, aes(x=fct_reorder(radius_mean_level, radius_mean), ..count..)) +
    geom_bar(aes(fill=diagnosis)) + labs(x = "radius_mean_level")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2

**From the task number 1, I noticed that for cancels which have the
radius_mean_level larger and equal to the medium, they are most likely
to be malicious. As a result, we could merge the medium, large_medium,
and large into one category by using the fct_collapse function.**

``` r
ggplot(cancer_sample, aes(x=fct_collapse(radius_mean_level, large = c("medium", "large_medium", "large")), ..count..)) +
    geom_bar(aes(fill=diagnosis)) + labs(x = "radius_mean_level")
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: How could each variable help us to predict the
cancer is malicious or benign.

**Variable of interest**: diagnosis

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
#need to make the y to be either 0 or 1
convered_cancer_sample = cancer_sample %>%
  mutate(diagnosis = ifelse(diagnosis == 'B', 0, 1))

glm_mean = glm(diagnosis ~ ., family = binomial, data = convered_cancer_sample)
```

    ## Warning: glm.fit: algorithm did not converge

    ## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

``` r
summary(glm_mean)
```

    ## 
    ## Call:
    ## glm(formula = diagnosis ~ ., family = binomial, data = convered_cancer_sample)
    ## 
    ## Deviance Residuals: 
    ##        Min          1Q      Median          3Q         Max  
    ## -4.746e-04  -2.000e-08  -2.000e-08   2.000e-08   4.695e-04  
    ## 
    ## Coefficients:
    ##                                 Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)                   -6.625e+03  3.941e+05  -0.017    0.987
    ## ID                             4.956e-07  5.521e-05   0.009    0.993
    ## radius_mean                   -3.977e+02  1.662e+05  -0.002    0.998
    ## texture_mean                   4.672e+01  4.834e+03   0.010    0.992
    ## perimeter_mean                 1.971e+01  1.995e+04   0.001    0.999
    ## area_mean                     -3.976e-01  6.153e+02  -0.001    0.999
    ## smoothness_mean                4.292e+03  1.177e+06   0.004    0.997
    ## compactness_mean              -1.111e+04  1.329e+06  -0.008    0.993
    ## concavity_mean                 5.521e+03  6.509e+05   0.008    0.993
    ## concave_points_mean            7.095e+03  9.503e+05   0.007    0.994
    ## symmetry_mean                 -2.657e+03  2.977e+05  -0.009    0.993
    ## fractal_dimension_mean         1.730e+04  2.365e+06   0.007    0.994
    ## radius_se                     -2.313e+02  1.447e+05  -0.002    0.999
    ## texture_se                     8.444e+01  2.607e+04   0.003    0.997
    ## perimeter_se                  -8.038e+01  1.182e+04  -0.007    0.995
    ## area_se                        1.969e+01  2.205e+03   0.009    0.993
    ## smoothness_se                 -2.718e+04  5.212e+06  -0.005    0.996
    ## compactness_se                 1.641e+04  1.735e+06   0.009    0.992
    ## concavity_se                  -3.890e+03  2.155e+06  -0.002    0.999
    ## concave_points_se              3.661e+04  3.701e+06   0.010    0.992
    ## symmetry_se                   -1.751e+04  1.396e+06  -0.013    0.990
    ## fractal_dimension_se          -1.786e+05  8.947e+06  -0.020    0.984
    ## radius_worst                   3.127e+02  4.722e+04   0.007    0.995
    ## texture_worst                 -6.816e+00  4.664e+03  -0.001    0.999
    ## perimeter_worst                1.159e+01  1.757e+03   0.007    0.995
    ## area_worst                    -1.557e+00  5.289e+02  -0.003    0.998
    ## smoothness_worst              -1.852e+03  7.872e+05  -0.002    0.998
    ## compactness_worst             -2.149e+03  1.888e+05  -0.011    0.991
    ## concavity_worst                3.230e+02  2.047e+05   0.002    0.999
    ## concave_points_worst           2.858e+03  2.204e+05   0.013    0.990
    ## symmetry_worst                 4.049e+03  1.820e+05   0.022    0.982
    ## fractal_dimension_worst        1.823e+04  9.341e+05   0.020    0.984
    ## radius_mean_levellarge_medium  1.873e+03  1.645e+05   0.011    0.991
    ## radius_mean_levelmedium        2.209e+03  2.291e+05   0.010    0.992
    ## radius_mean_levelsmall         7.311e+02  3.444e+05   0.002    0.998
    ## radius_mean_levelsmall_medium  1.907e+03  2.371e+05   0.008    0.994
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 7.5144e+02  on 568  degrees of freedom
    ## Residual deviance: 2.6446e-06  on 533  degrees of freedom
    ## AIC: 72
    ## 
    ## Number of Fisher Scoring iterations: 25

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

**The following will convert the model to tibble by using the tidy
function in the broom library.** **Then I find the p-value of the
radius_mean and store the result into the variable called
p_radius_mean.**

``` r
library(broom)
tidy_model_summary <- tidy(glm_mean)
print(tidy_model_summary)
```

    ## # A tibble: 36 × 5
    ##    term                estimate std.error statistic p.value
    ##    <chr>                  <dbl>     <dbl>     <dbl>   <dbl>
    ##  1 (Intercept)         -6.63e+3   3.94e+5 -0.0168     0.987
    ##  2 ID                   4.96e-7   5.52e-5  0.00898    0.993
    ##  3 radius_mean         -3.98e+2   1.66e+5 -0.00239    0.998
    ##  4 texture_mean         4.67e+1   4.83e+3  0.00966    0.992
    ##  5 perimeter_mean       1.97e+1   2.00e+4  0.000988   0.999
    ##  6 area_mean           -3.98e-1   6.15e+2 -0.000646   0.999
    ##  7 smoothness_mean      4.29e+3   1.18e+6  0.00365    0.997
    ##  8 compactness_mean    -1.11e+4   1.33e+6 -0.00836    0.993
    ##  9 concavity_mean       5.52e+3   6.51e+5  0.00848    0.993
    ## 10 concave_points_mean  7.10e+3   9.50e+5  0.00747    0.994
    ## # … with 26 more rows

``` r
## step2 
p_radius_mean <- tidy_model_summary %>%
                        filter(term == "radius_mean") %>%
                        select(p.value) 
print(p_radius_mean)
```

    ## # A tibble: 1 × 1
    ##   p.value
    ##     <dbl>
    ## 1   0.998

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
### Take a summary table 
write_csv(as.data.frame(summary(cancer_sample)), here::here("output", "3.1.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(glm_mean, file = here::here("output", "glm_mean"))

glm_mean_loaded = readRDS(here::here("output/glm_mean"))

summary(glm_mean_loaded)
```

    ## 
    ## Call:
    ## glm(formula = diagnosis ~ ., family = binomial, data = convered_cancer_sample)
    ## 
    ## Deviance Residuals: 
    ##        Min          1Q      Median          3Q         Max  
    ## -4.746e-04  -2.000e-08  -2.000e-08   2.000e-08   4.695e-04  
    ## 
    ## Coefficients:
    ##                                 Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)                   -6.625e+03  3.941e+05  -0.017    0.987
    ## ID                             4.956e-07  5.521e-05   0.009    0.993
    ## radius_mean                   -3.977e+02  1.662e+05  -0.002    0.998
    ## texture_mean                   4.672e+01  4.834e+03   0.010    0.992
    ## perimeter_mean                 1.971e+01  1.995e+04   0.001    0.999
    ## area_mean                     -3.976e-01  6.153e+02  -0.001    0.999
    ## smoothness_mean                4.292e+03  1.177e+06   0.004    0.997
    ## compactness_mean              -1.111e+04  1.329e+06  -0.008    0.993
    ## concavity_mean                 5.521e+03  6.509e+05   0.008    0.993
    ## concave_points_mean            7.095e+03  9.503e+05   0.007    0.994
    ## symmetry_mean                 -2.657e+03  2.977e+05  -0.009    0.993
    ## fractal_dimension_mean         1.730e+04  2.365e+06   0.007    0.994
    ## radius_se                     -2.313e+02  1.447e+05  -0.002    0.999
    ## texture_se                     8.444e+01  2.607e+04   0.003    0.997
    ## perimeter_se                  -8.038e+01  1.182e+04  -0.007    0.995
    ## area_se                        1.969e+01  2.205e+03   0.009    0.993
    ## smoothness_se                 -2.718e+04  5.212e+06  -0.005    0.996
    ## compactness_se                 1.641e+04  1.735e+06   0.009    0.992
    ## concavity_se                  -3.890e+03  2.155e+06  -0.002    0.999
    ## concave_points_se              3.661e+04  3.701e+06   0.010    0.992
    ## symmetry_se                   -1.751e+04  1.396e+06  -0.013    0.990
    ## fractal_dimension_se          -1.786e+05  8.947e+06  -0.020    0.984
    ## radius_worst                   3.127e+02  4.722e+04   0.007    0.995
    ## texture_worst                 -6.816e+00  4.664e+03  -0.001    0.999
    ## perimeter_worst                1.159e+01  1.757e+03   0.007    0.995
    ## area_worst                    -1.557e+00  5.289e+02  -0.003    0.998
    ## smoothness_worst              -1.852e+03  7.872e+05  -0.002    0.998
    ## compactness_worst             -2.149e+03  1.888e+05  -0.011    0.991
    ## concavity_worst                3.230e+02  2.047e+05   0.002    0.999
    ## concave_points_worst           2.858e+03  2.204e+05   0.013    0.990
    ## symmetry_worst                 4.049e+03  1.820e+05   0.022    0.982
    ## fractal_dimension_worst        1.823e+04  9.341e+05   0.020    0.984
    ## radius_mean_levellarge_medium  1.873e+03  1.645e+05   0.011    0.991
    ## radius_mean_levelmedium        2.209e+03  2.291e+05   0.010    0.992
    ## radius_mean_levelsmall         7.311e+02  3.444e+05   0.002    0.998
    ## radius_mean_levelsmall_medium  1.907e+03  2.371e+05   0.008    0.994
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 7.5144e+02  on 568  degrees of freedom
    ## Residual deviance: 2.6446e-06  on 533  degrees of freedom
    ## AIC: 72
    ## 
    ## Number of Fisher Scoring iterations: 25

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
