Activity 5
================
Name

## Data and packages

Again, we will load all of the `{tidyverse}` for this Activity.

``` r
library(tidyverse)
```

We continue our exploration of college majors and earnings from the data
behind the FiveThirtyEight story [The Economic Guide To Picking A
College
Major](https://fivethirtyeight.com/features/the-economic-guide-to-picking-a-college-major/).
Remember that there are many considerations that go into picking a
major. Earning potential and employment prospects are two (important) of
these considerations, but they do not tell the entire story.

We read in the same data from Activity 4 below, but notice that this
code is now surrounded in parentheses.

``` r
(df <- read_csv("data/recent-grads.csv"))
```

    ## # A tibble: 173 x 21
    ##     rank major_code major           major_category total sample_size   men women
    ##    <dbl>      <dbl> <chr>           <chr>          <dbl>       <dbl> <dbl> <dbl>
    ##  1     1       2419 Petroleum Engi… Engineering     2339          36  2057   282
    ##  2     2       2416 Mining And Min… Engineering      756           7   679    77
    ##  3     3       2415 Metallurgical … Engineering      856           3   725   131
    ##  4     4       2417 Naval Architec… Engineering     1258          16  1123   135
    ##  5     5       2405 Chemical Engin… Engineering    32260         289 21239 11021
    ##  6     6       2418 Nuclear Engine… Engineering     2573          17  2200   373
    ##  7     7       6202 Actuarial Scie… Business        3777          51  2110  1667
    ##  8     8       5001 Astronomy And … Physical Scie…  1792          10   832   960
    ##  9     9       2414 Mechanical Eng… Engineering    91227        1029 80320 10907
    ## 10    10       2408 Electrical Eng… Engineering    81527         631 65511 16016
    ## # … with 163 more rows, and 13 more variables: sharewomen <dbl>,
    ## #   employed <dbl>, employed_fulltime <dbl>, employed_parttime <dbl>,
    ## #   employed_fulltime_yearround <dbl>, unemployed <dbl>,
    ## #   unemployment_rate <dbl>, p25th <dbl>, median <dbl>, p75th <dbl>,
    ## #   college_jobs <dbl>, non_college_jobs <dbl>, low_wage_jobs <dbl>

``` r
## why not use readr::
```

Compare this code output to the `load_data` chunk in your knitted
Activity 4 `.md` report. What does enclosing an assignment code (i.e.,
`object_name <- r_code`) in parentheses do?

**Response**: small printout vs no printout

### Data Codebook

Descriptions of the variables are again provided below. Again note that
the ACS only asks [one
question](https://www.census.gov/acs/www/about/why-we-ask-each-question/sex/)
about a person’s sexual identity.

| Header                         | Description                                                                 |
|:-------------------------------|:----------------------------------------------------------------------------|
| `rank`                         | Rank by median earnings                                                     |
| `major_code`                   | Major code, FO1DP in ACS PUMS                                               |
| `major`                        | Major description                                                           |
| `major_category`               | Category of major from Carnevale et al                                      |
| `total`                        | Total number of people with major                                           |
| `sample_size`                  | Sample size (unweighted) of full-time, year-round ONLY (used for earnings)  |
| `men`                          | Male graduates                                                              |
| `women`                        | Female graduates                                                            |
| `sharewomen`                   | Women as share of total                                                     |
| `employed`                     | Number employed (ESR == 1 or 2)                                             |
| `employed_full_time`           | Employed 35 hours or more                                                   |
| `employed_part_time`           | Employed less than 35 hours                                                 |
| `employed_full_time_yearround` | Employed at least 50 weeks (WKW == 1) and at least 35 hours (WKHP &gt;= 35) |
| `unemployed`                   | Number unemployed (ESR == 3)                                                |
| `unemployment_rate`            | Unemployed / (Unemployed + Employed)                                        |
| `median`                       | Median earnings of full-time, year-round workers                            |
| `p25th`                        | 25th percentile of earnings                                                 |
| `p75th`                        | 75th percentile of earnings                                                 |
| `college_jobs`                 | Number with job requiring a college degree                                  |
| `non_college_jobs`             | Number with job not requiring a college degree                              |
| `low_wage_jobs`                | Number in low-wage service jobs                                             |

The questions we will answer in this activity are:

-   How do the distributions of median income compare across major
    categories?
-   Do women tend to choose majors with lower or higher earnings?

## Analysis

### Median Earnings Description

### Median … Median Earnings

For the rest of this semester, I will no longer provide you with R code
chunks. Have no fear! There are a number of ways to create a code chunk:

-   Tired: Copy-and-paste a previous code chunk, delete the code, then
    add your new code
-   Wired: Click on the ![new chunk icon](README-img/new-chunk-icon.png)
    and select ![r chunk icon](README-img/r-chunk-icon.png) (notice all
    the different types of code chunks that you can use within an
    RMarkdown file!)
-   Inspired: Ctrl/Command + Alt/Option + I

Below, create a code chunk and name it `median_earnings`. Make sure
there is an empty line above and below the code chunk.

``` r
median_earnings = df %>%
  select(major,median) %>%
  filter(median<=36000)
```

In your newly created R code chunk, verify that the median income for
all majors was $36,000. Using the `college_recent_grads` dataset and
functions from `{dplyr}`, verify the *median* summary statistic for the
variable median earnings of full-time, year-round workers (`median`).
Name this numerical summary `median_all_majors`.

``` r
median_earnings_allmajors = df %>%
  
  summarise('med' = median(median))
```

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Additional Summaries of Median Earnings

Often we would like more information than the median to help us to
better understand the distribution of a variable. Using the
`college_recent_grads` dataset and functions from `{dplyr}`, obtain the
sample size (i.e., *n*umber of observations), *mean*, *s*tandard
*d*eviation, *min*imum, *median*, and *max*imum summaries for the
variable `median` earnings of full-time, year-round workers. Be careful
when you name your output summaries as we are dealing with things that
could use the same name (i.e., “median”). When I and obtaining numerical
summaries for variables, I like to include the variable name in my
summary name (e.g., `mean_med_earnings = mean(median)`). Create a code
chunk and name it `summary_earnings`.

``` r
summary_earnings = df %>%
  select(employed_fulltime_yearround, median)%>%
  summarise(total = sum(median), max = max(median), mean = mean(median),std=sd(median),mini=min(median),med=median(median))
```

Provide a discussion on what you believe the distribution of median
earnings will look like. You should discuss the center, spread, and
potential shape only using these values - I do NOT want to see any data
visualizations here.

**Response**:

### Median Earnings by Major Category

Now we will see how the different major categories compare to the
overall distribution of median earnings. Using the
`college_recent_grads` dataset and functions from `{dplyr}`, obtain
similar summaries of the variable `median` earnings of full-time,
year-round workers as your `summary_earnings` code chunk, *by* for each
`major_category`. *Arrange* this summary table by the median earning.
Create a code chunk and name it `major_earnings`.

``` r
major_earnings_summary = df %>%
  select(major_category,employed_fulltime_yearround, median)%>%
  group_by(major_category)%>%
  arrange(desc(median))%>%
  summarise(total = sum(median), max = max(median), mean = mean(median),std=sd(median),mini=min(median),med=median(median))
```

Provide a discussion on how each major compares to the overall
distribution. You should discuss the center, spread, and potential shape
only using these summary values - I do NOT want to see any data
visualizations here.

**Response**:

Before we continue, add the following to the end of your pipeline (you
will need to pipe first) in your `major_earnings` code chunk:

``` r
major_earnings_summary = df %>%
  select(major_category,employed_fulltime_yearround, median)%>%
  group_by(major_category)%>%
  arrange(desc(median))%>%
  summarise(total = sum(median), max = max(median), mean = mean(median),std=sd(median),mini=min(median),med=median(median))%>%
  knitr::kable()
```

Knit your document with and without this last piped code. What changes
about the output? When would this `knitr::kable` code be useful?

**Response**:

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Visualize Median Earnings by Major Category

Let us see how well your descriptions in the [Median Earnings by Major
Category](#median-earnings-by-major-category) section compare to the
actual distributions. Plot the distribution of the variable `median`
earnings of full-time, year-round workers for each `major_category`
using the *boxplot* and *jitter* geometries. Create a code chunk and
name it `major_boxplot`.

``` r
major_earnings_summaryss = df %>%
  select(major_category,employed_fulltime_yearround, median)%>%
  group_by(major_category)%>%
  #arrange(desc(median))%>%
  ggplot() + geom_boxplot(aes(y=median, x=major_category))
```

``` r
major_earnings_summaryss = df %>%
  select(major_category,employed_fulltime_yearround, median)%>%
  group_by(major_category)%>%
  #arrange(desc(median))%>%
  ggplot() + geom_jitter(aes(y=median, x=major_category))+ geom_jitter(aes(y=median, x=major_category))


major_earnings_summarysss = df %>%
  select(major_category,employed_fulltime_yearround, median)%>%
  group_by(major_category)%>%
  #arrange(desc(median))%>%
  ggplot() + geom_boxplot(aes(y=median, x=major_category))


major_earnings_summaryssss = df %>%
  select(major_category,employed_fulltime_yearround, median)%>%
  group_by(major_category)%>%
  #arrange(desc(median))%>%
  ggplot() + geom_jitter(aes(y=median, x=major_category))
```

``` r
major_earnings_summaryss
```

![](activity05-data-summarization_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
major_earnings_summarysss
```

![](activity05-data-summarization_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

``` r
major_earnings_summaryssss
```

![](activity05-data-summarization_files/figure-gfm/unnamed-chunk-8-3.png)<!-- -->

Provide a discussion on how your descriptions in the Median Earnings by
Major Category section compares.

**Response**:

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to start working on your
project. The remainder of this activity will help to expand these
commands.

### Multiple Rankings

#### Ranking by `major_category`

The current rankings provided in the data are by `major`. Here we will
develop a series of rankings to see how the `major_category` levels
perform. Create a code chunk and name it `category_rankings`. In this
code chunk,

1.  Group `college_recent_grads` by `major_category`
2.  Summarize the variable `total` as the *sum* across all majors (to
    get the total number of majors within a `major_category`) and the
    following variables by their *median* value: `sharewomen`,
    `unemployment_rate`, and `median` earnings. Provide a meaningful
    name to each summarized value.
3.  Assign/create a *rank* for each summarized value (rank for `total`,
    rank for `sharewomen`, etc.) and provide a meaningful name to each
    ranked column value.
4.  Arrange the results so that `major_category` appear in alphabetical
    order (“A” at the top).

``` r
rankings = df %>%
  select(major_category,total,sharewomen,unemployment_rate,median)%>%
  group_by(major_category)%>%
  summarise(tot=sum(total), a=median(sharewomen),b=median(median),c=median(unemployment_rate))%>%
  mutate(ranktot = min_rank(desc(tot)),
         ranka = min_rank(desc(a)),
         rankb = min_rank(desc(b)),
         rankc = min_rank(desc(c)))%>%
  arrange(desc(major_category))
```

``` r
rankings
```

    ## # A tibble: 16 x 9
    ##    major_category              tot      a     b      c ranktot ranka rankb rankc
    ##    <chr>                     <dbl>  <dbl> <dbl>  <dbl>   <int> <int> <int> <int>
    ##  1 Social Science           529966  0.543 38000 0.0972       5     9     5     1
    ##  2 Psychology & Social Wo…  481007  0.799 30000 0.0651       6     1    16    10
    ##  3 Physical Sciences        185479  0.520 39500 0.0511      13    10     4    15
    ##  4 Law & Public Policy      179107  0.476 36000 0.0825      14    11     7     4
    ##  5 Interdisciplinary         12296  0.771 35000 0.0709      15     3     8     7
    ##  6 Industrial Arts & Cons…  229792  0.232 35000 0.0557      12    14     8    13
    ##  7 Humanities & Liberal A…  713468  0.690 32000 0.0817       2     5    14     5
    ##  8 Health                   463230  0.783 35000 0.0643       7     2     8    11
    ##  9 Engineering              537583  0.227 57000 0.0598       4    15     1    12
    ## 10 Education                559129  0.769 32750 0.0488       3     4    13    16
    ## 11 Computers & Mathematics  299008  0.269 45000 0.0908      11    13     2     2
    ## 12 Communications & Journ…  392601  0.672 35000 0.0722       9     6     8     6
    ## 13 Business                1302376  0.441 40000 0.0697       1    12     3     8
    ## 14 Biology & Life Science   453862  0.583 36300 0.0680       8     8     6     9
    ## 15 Arts                     357130  0.667 30750 0.0895      10     7    15     3
    ## 16 Agriculture & Natural …      NA NA     35000 0.0553      NA    NA     8    14

Provide a discussion on how the `major_category` rankings compare.

**Response**:

![](README-img/noun_pause.png) **(Final) Planned Pause Point**: If you
have any questions, contact your instructor. Otherwise feel free to
continue on.

Knit, then stage everything listed in your **Git** pane, commit (with a
meaningful commit message), and push to your GitHub repo. Go to GitHub
and verify that your `activity04-data-pieplines.Rmd` file appears as you
intended it to.

You can now go back to the `README` file.

## Attribution

This activity is inspired by a lab from [Dr. Mine
Çetinkaya-Rundel](http://www2.stat.duke.edu/~mc301/)’s STA 199 course.
