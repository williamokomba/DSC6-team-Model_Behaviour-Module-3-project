R Notebook
================

## Defining the Question

segmentation of customers according to their spending habits on
different products.

## Understanding the Context

The wholesale customer segmentation problem is an issue of concern to
most wholesale distributors in the business world. Classification of
customers is fundamental since it enables marketing teams to have
customized strategies meant for different groups of customers.Being able
to understand customer behavior and patterns together with spending
habits is key to strategizing. This influences better selling of
products hence revenue to a business. Our case study is centered in
Lisbon, porto and some parts of Portugal where our client supplies
products.The data set includes the annual spending in monetary units on
diverse product categories.

## Business Objective.

The main objective of this project is; To be able to classify groups of
customers hence customize marketing strategies for each group. To be
able to identify spending habits of customers. Come up with insights
that help with marketing strategies

### Data provided

Dataset Url =
<https://www.kaggle.com/binovi/wholesale-customers-data-set>

### Data grossary

Model Behaviour will be using a dataset sourced from Kaggle. It contains
440 rows and 8 columns;

1.  FRESH: annual spending (m.u.) on fresh products (Continuous);
2.  MILK: annual spending (m.u.) on milk products (Continuous);
3.  GROCERY: annual spending (m.u.)on grocery products (Continuous);
4.  FROZEN: annual spending (m.u.)on frozen products (Continuous)
5.  DETERGENTS\_PAPER: annual spending (m.u.) on detergents and paper
    products (Continuous)
6.  DELICATESSEN: annual spending (m.u.)on and delicatessen products
    (Continuous);
7.  CHANNEL: customersâ€™ Channel - Horeca (Hotel/Restaurant/CafÃ©) or
    Retail channel (Nominal) 1: hotel/restaurant/cafe 2: retail
8.  REGION: customersâ€™ Region â€“ Lisnon, Oporto or Other (Nominal)

-   1: Lisbon / 2: Porto / 3: other region Dimensions are; 440\*8 Region
    Frequency Lisbon 77 Oporto 47 Other Region 316 Total 440 Channel
    Frequency¶ Horeca 298 Retail 142 Total 440

## Defining the metric for success

Being able to cluster customers based on spending habits.

## 1.4 Recording the Experimental

1.Data Loading

2.Data cleaning for missing values and outliers

3.Exploratory Data Analysis

1.  implementation using k-means and hierarchical clustering.

5.Recomendation and conclusion.

## 1.5 Assessing the Relevance of the Data

The dataset is genuine since it was got from kaggle which is trusted
open source.

### Importing and reading dataset

``` r
library(readr)
customers_data_2<- read_csv("C:/Users/william/Downloads/Wholesale customers data.csv")
```

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   Channel = col_double(),
    ##   Region = col_double(),
    ##   Fresh = col_double(),
    ##   Milk = col_double(),
    ##   Grocery = col_double(),
    ##   Frozen = col_double(),
    ##   Detergents_Paper = col_double(),
    ##   Delicassen = col_double()
    ## )

``` r
#
# making dataframe copy
customers_data<-customers_data_2
#
#checking the fist 6 rows of the dataset.
head(customers_data)
```

    ## # A tibble: 6 x 8
    ##   Channel Region Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##     <dbl>  <dbl> <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1       2      3 12669  9656    7561    214             2674       1338
    ## 2       2      3  7057  9810    9568   1762             3293       1776
    ## 3       2      3  6353  8808    7684   2405             3516       7844
    ## 4       1      3 13265  1196    4221   6404              507       1788
    ## 5       2      3 22615  5410    7198   3915             1777       5185
    ## 6       2      3  9413  8259    5126    666             1795       1451

### checking the bottom rows of the dataset

``` r
tail(customers_data)
```

    ## # A tibble: 6 x 8
    ##   Channel Region Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##     <dbl>  <dbl> <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1       1      3 16731  3922    7994    688             2371        838
    ## 2       1      3 29703 12051   16027  13135              182       2204
    ## 3       1      3 39228  1431     764   4510               93       2346
    ## 4       2      3 14531 15488   30243    437            14841       1867
    ## 5       1      3 10290  1981    2232   1038              168       2125
    ## 6       1      3  2787  1698    2510     65              477         52

### getting the dataset summary

``` r
summary(customers_data)
```

    ##     Channel          Region          Fresh             Milk      
    ##  Min.   :1.000   Min.   :1.000   Min.   :     3   Min.   :   55  
    ##  1st Qu.:1.000   1st Qu.:2.000   1st Qu.:  3128   1st Qu.: 1533  
    ##  Median :1.000   Median :3.000   Median :  8504   Median : 3627  
    ##  Mean   :1.323   Mean   :2.543   Mean   : 12000   Mean   : 5796  
    ##  3rd Qu.:2.000   3rd Qu.:3.000   3rd Qu.: 16934   3rd Qu.: 7190  
    ##  Max.   :2.000   Max.   :3.000   Max.   :112151   Max.   :73498  
    ##     Grocery          Frozen        Detergents_Paper    Delicassen     
    ##  Min.   :    3   Min.   :   25.0   Min.   :    3.0   Min.   :    3.0  
    ##  1st Qu.: 2153   1st Qu.:  742.2   1st Qu.:  256.8   1st Qu.:  408.2  
    ##  Median : 4756   Median : 1526.0   Median :  816.5   Median :  965.5  
    ##  Mean   : 7951   Mean   : 3071.9   Mean   : 2881.5   Mean   : 1524.9  
    ##  3rd Qu.:10656   3rd Qu.: 3554.2   3rd Qu.: 3922.0   3rd Qu.: 1820.2  
    ##  Max.   :92780   Max.   :60869.0   Max.   :40827.0   Max.   :47943.0

### checking datatypes

``` r
str(customers_data)
```

    ## tibble [440 x 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Channel         : num [1:440] 2 2 2 1 2 2 2 2 1 2 ...
    ##  $ Region          : num [1:440] 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ Fresh           : num [1:440] 12669 7057 6353 13265 22615 ...
    ##  $ Milk            : num [1:440] 9656 9810 8808 1196 5410 ...
    ##  $ Grocery         : num [1:440] 7561 9568 7684 4221 7198 ...
    ##  $ Frozen          : num [1:440] 214 1762 2405 6404 3915 ...
    ##  $ Detergents_Paper: num [1:440] 2674 3293 3516 507 1777 ...
    ##  $ Delicassen      : num [1:440] 1338 1776 7844 1788 5185 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Channel = col_double(),
    ##   ..   Region = col_double(),
    ##   ..   Fresh = col_double(),
    ##   ..   Milk = col_double(),
    ##   ..   Grocery = col_double(),
    ##   ..   Frozen = col_double(),
    ##   ..   Detergents_Paper = col_double(),
    ##   ..   Delicassen = col_double()
    ##   .. )

### converting the data into a tibble for easy manupulation

``` r
library(caret)
```

    ## Loading required package: lattice

    ## Loading required package: ggplot2

``` r
library(tibble)
#For ease in analysis,we convert the data into a tibble
customers_data<-as_tibble(customers_data) # there is suggestion to use as_tibble instead of as.tibble.
head(customers_data)
```

    ## # A tibble: 6 x 8
    ##   Channel Region Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##     <dbl>  <dbl> <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1       2      3 12669  9656    7561    214             2674       1338
    ## 2       2      3  7057  9810    9568   1762             3293       1776
    ## 3       2      3  6353  8808    7684   2405             3516       7844
    ## 4       1      3 13265  1196    4221   6404              507       1788
    ## 5       2      3 22615  5410    7198   3915             1777       5185
    ## 6       2      3  9413  8259    5126    666             1795       1451

\#\#. Data cleaning

``` r
#checking the missing values
is.null(customers_data)
```

    ## [1] FALSE

``` r
# checking for duplicates 
anyDuplicated(customers_data)
```

    ## [1] 0

### checking for outliers

### installing the packages.

``` r
# install.packages("magrittr") # package installations are only needed the first time you use it
# install.packages("dplyr")    # alternative installation of the %>%
library(magrittr) # needs to be run every time you start R and want to use %>%
library(dplyr)    # alternatively, this also loads %>%
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

## converting some colunms to categorical datatypes.

channel and Region are categorical in nature and not numerical, they
have already been encoded to make them easier to work with. We will
convert them to their appropriate data type.

``` r
##converting colunms to categorical datatypes
#
customers_df <-customers_data
customers_df$Channel  <- as.factor(customers_df$Channel)
customers_df$Region <- as.factor(customers_df$Region)
```

\#\#confirming the datatype changes

``` r
# confirming the data types changes
str(customers_df)
```

    ## tibble [440 x 8] (S3: tbl_df/tbl/data.frame)
    ##  $ Channel         : Factor w/ 2 levels "1","2": 2 2 2 1 2 2 2 2 1 2 ...
    ##  $ Region          : Factor w/ 3 levels "1","2","3": 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ Fresh           : num [1:440] 12669 7057 6353 13265 22615 ...
    ##  $ Milk            : num [1:440] 9656 9810 8808 1196 5410 ...
    ##  $ Grocery         : num [1:440] 7561 9568 7684 4221 7198 ...
    ##  $ Frozen          : num [1:440] 214 1762 2405 6404 3915 ...
    ##  $ Detergents_Paper: num [1:440] 2674 3293 3516 507 1777 ...
    ##  $ Delicassen      : num [1:440] 1338 1776 7844 1788 5185 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Channel = col_double(),
    ##   ..   Region = col_double(),
    ##   ..   Fresh = col_double(),
    ##   ..   Milk = col_double(),
    ##   ..   Grocery = col_double(),
    ##   ..   Frozen = col_double(),
    ##   ..   Detergents_Paper = col_double(),
    ##   ..   Delicassen = col_double()
    ##   .. )

the two variables has been transformed to categorical datatype.

\#\#\#3.2a Identifying the numeric class in the data and evaluating if
there are any outliers

``` r
#
#Checking the data types of the columns
#
data.num<- customers_df %>% select_if(is.numeric)
data.num
```

    ## # A tibble: 440 x 6
    ##    Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##    <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ##  1 12669  9656    7561    214             2674       1338
    ##  2  7057  9810    9568   1762             3293       1776
    ##  3  6353  8808    7684   2405             3516       7844
    ##  4 13265  1196    4221   6404              507       1788
    ##  5 22615  5410    7198   3915             1777       5185
    ##  6  9413  8259    5126    666             1795       1451
    ##  7 12126  3199    6975    480             3140        545
    ##  8  7579  4956    9426   1669             3321       2566
    ##  9  5963  3648    6192    425             1716        750
    ## 10  6006 11093   18881   1159             7425       2098
    ## # ... with 430 more rows

### checking for outliers

``` r
for (i in 1:ncol(data.num)) {
  boxplot(data.num[, i], main=names(data.num[, i]))
}
```

![](wholesales_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-12-2.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-12-3.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-12-4.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-12-5.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-12-6.png)<!-- -->
observation: all variables has outliers. However we will not drop them
as they look genuine.

### renaming the rows of the channel, and region for analysis purpose.

``` r
#Renaming colunms for analysis purpose
#
library(caret)
library(lattice)
library(dplyr)

customers_df$Channel <- with(customers_data,  factor(Channel, levels = c(1,2),labels = c("restaurant", "retail")))

customers_df$Region <- with(customers_data,  factor(Region, levels = c(1,2,3), labels = c("Lisbon", "Porto", "Other region")))
#
#confirming the changes
head(customers_df)
```

    ## # A tibble: 6 x 8
    ##   Channel    Region       Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##   <fct>      <fct>        <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1 retail     Other region 12669  9656    7561    214             2674       1338
    ## 2 retail     Other region  7057  9810    9568   1762             3293       1776
    ## 3 retail     Other region  6353  8808    7684   2405             3516       7844
    ## 4 restaurant Other region 13265  1196    4221   6404              507       1788
    ## 5 retail     Other region 22615  5410    7198   3915             1777       5185
    ## 6 retail     Other region  9413  8259    5126    666             1795       1451

observation: we converted encoded variable to categorical variable.

\#\#\#checking dataset summary after data cleaning

``` r
# getting the main summary
summary(customers_df)
```

    ##        Channel             Region        Fresh             Milk      
    ##  restaurant:298   Lisbon      : 77   Min.   :     3   Min.   :   55  
    ##  retail    :142   Porto       : 47   1st Qu.:  3128   1st Qu.: 1533  
    ##                   Other region:316   Median :  8504   Median : 3627  
    ##                                      Mean   : 12000   Mean   : 5796  
    ##                                      3rd Qu.: 16934   3rd Qu.: 7190  
    ##                                      Max.   :112151   Max.   :73498  
    ##     Grocery          Frozen        Detergents_Paper    Delicassen     
    ##  Min.   :    3   Min.   :   25.0   Min.   :    3.0   Min.   :    3.0  
    ##  1st Qu.: 2153   1st Qu.:  742.2   1st Qu.:  256.8   1st Qu.:  408.2  
    ##  Median : 4756   Median : 1526.0   Median :  816.5   Median :  965.5  
    ##  Mean   : 7951   Mean   : 3071.9   Mean   : 2881.5   Mean   : 1524.9  
    ##  3rd Qu.:10656   3rd Qu.: 3554.2   3rd Qu.: 3922.0   3rd Qu.: 1820.2  
    ##  Max.   :92780   Max.   :60869.0   Max.   :40827.0   Max.   :47943.0

the above shows the mean, median, max, and quantile for numerical
variables and frequences for categorical variables.

\#\#getting the dataset description

``` r
library(psych)
```

    ## 
    ## Attaching package: 'psych'

    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     %+%, alpha

``` r
describe(customers_df)
```

    ##                  vars   n     mean       sd median trimmed     mad min    max
    ## Channel*            1 440     1.32     0.47    1.0    1.28    0.00   1      2
    ## Region*             2 440     2.54     0.77    3.0    2.68    0.00   1      3
    ## Fresh               3 440 12000.30 12647.33 8504.0 9864.61 8776.25   3 112151
    ## Milk                4 440  5796.27  7380.38 3627.0 4375.52 3647.20  55  73498
    ## Grocery             5 440  7951.28  9503.16 4755.5 6158.43 4586.42   3  92780
    ## Frozen              6 440  3071.93  4854.67 1526.0 2144.07 1607.88  25  60869
    ## Detergents_Paper    7 440  2881.49  4767.85  816.5 1849.73 1060.80   3  40827
    ## Delicassen          8 440  1524.87  2820.11  965.5 1113.24  945.16   3  47943
    ##                   range  skew kurtosis     se
    ## Channel*              1  0.76    -1.43   0.02
    ## Region*               2 -1.27    -0.13   0.04
    ## Fresh            112148  2.54    11.33 602.94
    ## Milk              73443  4.03    24.25 351.85
    ## Grocery           92777  3.56    20.56 453.05
    ## Frozen            60844  5.87    53.80 231.44
    ## Detergents_Paper  40824  3.61    18.68 227.30
    ## Delicassen        47940 11.08   167.97 134.44

observation: it gives us the a glimpse of the variance,mean,variance,max
and so on.

## Univariate Analysis

### univariate Analysis for Numerical data

``` r
#preview of the numerical columns
#
library(dplyr)    # alternatively, this also loads %>%
data.num<- customers_df %>% select_if(is.numeric)
head(data.num)
```

    ## # A tibble: 6 x 6
    ##   Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##   <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1 12669  9656    7561    214             2674       1338
    ## 2  7057  9810    9568   1762             3293       1776
    ## 3  6353  8808    7684   2405             3516       7844
    ## 4 13265  1196    4221   6404              507       1788
    ## 5 22615  5410    7198   3915             1777       5185
    ## 6  9413  8259    5126    666             1795       1451

### previewing the numerical variables using histograms to check skewness.

``` r
#3.3ai installing ggplot2 package for visualization.
#install.packages('ggplot2')
#
#installing ggplot2 library
#
library(ggplot2)
#
#note: the above  have been installed in the console section.
```

## univariate analysis

### 1. Numerical variables distribution

``` r
#visualizing the Fresh  colunm to check for skewness
#
qplot(data = data.num, x = Fresh)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

``` r
#visualizing the milk colunm to check for skewness
#
qplot(data = data.num, x = Milk,)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

``` r
#visualizing the grocery  colunm to check for skewness
#
qplot(data = data.num, x = Grocery)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

``` r
#visualizing the Frozen  colunm to check for skewness
#
qplot(data = data.num, x = Frozen)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

``` r
#visualizing the Detergents_Paper colunm to check for skewness
#
qplot(data = data.num, x = Detergents_Paper)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
#visualizing the Delicassen  column to check for skewness
#
qplot(data = data.num, x = Delicassen)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

Observation: All our numerical variables are positively skewed.This
means the variable mean is greater than the mode.

### we can check skewness for numerical values at once as below

``` r
## we can check skewness for numerical values at once as below
library(caret)
library(ggplot2)
library(dplyr)    # alternatively, this also loads %>%
library(tidyr)
```

    ## 
    ## Attaching package: 'tidyr'

    ## The following object is masked from 'package:magrittr':
    ## 
    ##     extract

``` r
customers_df%>%
  gather(attributes, value, 3:8) %>%
  ggplot(aes(x = value)) +
  geom_histogram(fill = 'lightblue2', color = 'black') +
  facet_wrap(~attributes, scales = 'free_x') +
  labs(x="Values", y="Frequency") +
  theme_bw()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](wholesales_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->
observation: shows skewness to the right(positive skewness meaning the
tail on the right side is bigger than the left side).

### 2. univariate Analysis for categorical Data

``` r
## Identifying the categorical class in the data 
#
df_cat<- customers_df %>% select_if(is.factor)
df_cat
```

    ## # A tibble: 440 x 2
    ##    Channel    Region      
    ##    <fct>      <fct>       
    ##  1 retail     Other region
    ##  2 retail     Other region
    ##  3 retail     Other region
    ##  4 restaurant Other region
    ##  5 retail     Other region
    ##  6 retail     Other region
    ##  7 retail     Other region
    ##  8 retail     Other region
    ##  9 restaurant Other region
    ## 10 retail     Other region
    ## # ... with 430 more rows

## creating tables for bar plots.

``` r
# create tables of all categorical variables to be able to create bar plots with them
Channel_table <- table(customers_df$Channel)
Region_table <- table(customers_df$Region)
```

### adjusting plot size bf plotting.

``` r
# function for adjusting plot size
set_plot_dimensions <- function(width_choice, height_choice) {
    options(repr.plot.width = width_choice, repr.plot.height = height_choice)
}
```

``` r
# barplot for channel variable
set_plot_dimensions(5, 4)
Channel_table
```

    ## 
    ## restaurant     retail 
    ##        298        142

``` r
barplot(Channel_table, ylab = "count", xlab="Channel", col="dark red")
```

![](wholesales_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->
observation: restaurant has more sales compared to retail variable.

``` r
# barplot of the region representation.
set_plot_dimensions(5, 4)
Region_table
```

    ## 
    ##       Lisbon        Porto Other region 
    ##           77           47          316

``` r
barplot(Region_table, ylab = "count", xlab="Region", col="sky blue")
```

![](wholesales_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->
observation:other regions has more sales, followed by Lisbon city, and
finally Porto.

## Bivariate Analysis

``` r
# previewing numerical data once again.
#
head(data.num)
```

    ## # A tibble: 6 x 6
    ##   Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##   <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1 12669  9656    7561    214             2674       1338
    ## 2  7057  9810    9568   1762             3293       1776
    ## 3  6353  8808    7684   2405             3516       7844
    ## 4 13265  1196    4221   6404              507       1788
    ## 5 22615  5410    7198   3915             1777       5185
    ## 6  9413  8259    5126    666             1795       1451

### plotting scatter plots for numerical data

``` r
for (i in 1:(ncol(data.num)/2)) {
  for (j in 4:ncol(data.num)) {
       plot(as.numeric(unlist(data.num[, j]))
, as.numeric(unlist(data.num[, i]))
, xlab = names(as.vector(data.num[, i]))
, ylab = names(as.vector(data.num[, j])))
  
  }

}
```

![](wholesales_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-2.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-3.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-4.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-5.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-6.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-7.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-8.png)<!-- -->![](wholesales_files/figure-gfm/unnamed-chunk-31-9.png)<!-- -->

``` r
#scatterplot 
```

observation:

1.  Frozen vs Fresh. As the expenditures for frozen products go up those
    of fresh products equally go up though not correspondingly. For both
    products most customers spent averagely small amounts of money and
    those who spent more were few.
2.  Detergents and Fresh Products. The amount spent on these products
    don’t have a direct relationship. As the expenditures of detergents
    goes up, that of Fresh products don’t necessarily go up also and
    vice versa.
3.  Delicatessen vs Fresh Products. For Delicatessen and fresh products,
    the amounts spent on delicatessen by customers progressively
    increases without much significant increase in that of fresh
    products. There is a chance customers prefer more delicatessen from
    this wholesale compared to milk products therefore the marketing
    team should focus on ensuring that supply delicatessen products is
    constant and look into bettering quality of milk products maybe that
    could encourage customers to purchase their milk products as much as
    they do delicassens.
4.  Frozen products vs Milk. For both of these products, most customers
    spend less on them them but purchases of both don’t have a relation.
5.  Detergent paper and milk products. Both have a positive correlation.
    As the amount spent on either increases so does the amount spent on
    the other.
6.  Delicassen and Milk. For both these two most customers spend about
    the same amount of money.
7.  Frozen and Grocery. Most customers for both products spend the least
    amount spendable then individually increase without any correlation.
8.  Delicatessen and Grocery. When these two are compared, the amounts
    spend on groceries remain low as that spent on groceries increase.
    Marketing focus should be put on groceries in this case. The
    marketing team should probably come up with advertisements for
    healthy eating habits to push the grocery sales or even have them in
    the same position at the store with fresh products this could prompt
    buyers who prefer fresh products to buy them.

``` r
cor(data.num)
```

    ##                        Fresh      Milk     Grocery      Frozen Detergents_Paper
    ## Fresh             1.00000000 0.1005098 -0.01185387  0.34588146       -0.1019529
    ## Milk              0.10050977 1.0000000  0.72833512  0.12399376        0.6618157
    ## Grocery          -0.01185387 0.7283351  1.00000000 -0.04019274        0.9246407
    ## Frozen            0.34588146 0.1239938 -0.04019274  1.00000000       -0.1315249
    ## Detergents_Paper -0.10195294 0.6618157  0.92464069 -0.13152491        1.0000000
    ## Delicassen        0.24468997 0.4063683  0.20549651  0.39094747        0.0692913
    ##                  Delicassen
    ## Fresh             0.2446900
    ## Milk              0.4063683
    ## Grocery           0.2054965
    ## Frozen            0.3909475
    ## Detergents_Paper  0.0692913
    ## Delicassen        1.0000000

### plotting correlation of the numerical variables.

``` r
#install.packages("corrplot") 
library(corrplot)
```

    ## corrplot 0.84 loaded

``` r
#
## Let’s build a correlation matrix to understand the relation between each attributes
corrplot(cor(data.num), type = 'upper', method = 'number', tl.cex = 0.9, bg="black")
```

![](wholesales_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->
observation: There is a strong linear correlation between a couple of
variables.

1.Grocery and Detergent\_papers have a high positive correlation of
0.92.

1.  Milk and Grocery have a high positive correlation of 0.73.

2.  Detergent and Milk have a high positive correlated of 0.66,

3.  Delicassen and Milk have a high positive correlation of 0.41.

4.  Frozen and Detergent\_paper have a negative correlation of -0.13.

## clustering

we will use unsupervised learning

## Feature Selection

### 1.filter method

``` r
# Calculating the correlation matrix
# ---
#
feature_num<-data.num
cor_Matrix <- cor(feature_num)
cor_Matrix
```

    ##                        Fresh      Milk     Grocery      Frozen Detergents_Paper
    ## Fresh             1.00000000 0.1005098 -0.01185387  0.34588146       -0.1019529
    ## Milk              0.10050977 1.0000000  0.72833512  0.12399376        0.6618157
    ## Grocery          -0.01185387 0.7283351  1.00000000 -0.04019274        0.9246407
    ## Frozen            0.34588146 0.1239938 -0.04019274  1.00000000       -0.1315249
    ## Detergents_Paper -0.10195294 0.6618157  0.92464069 -0.13152491        1.0000000
    ## Delicassen        0.24468997 0.4063683  0.20549651  0.39094747        0.0692913
    ##                  Delicassen
    ## Fresh             0.2446900
    ## Milk              0.4063683
    ## Grocery           0.2054965
    ## Frozen            0.3909475
    ## Detergents_Paper  0.0692913
    ## Delicassen        1.0000000

``` r
# Find attributes that are highly correlated
# ---
#
high_cor <- findCorrelation(cor_Matrix, cutoff=0.75)
high_cor
```

    ## [1] 3

``` r
#
names(data.num[,high_cor])
```

    ## [1] "Grocery"

observation; grocery variable is highly correlated.

### Dropping variable with high correlation.

``` r
# We can remove the variables with a higher correlation 
# and comparing the results graphically as shown below
# 
# Removing Redundant Features 
# ---
# 
new_num<-feature_num[-high_cor]
new_num
```

    ## # A tibble: 440 x 5
    ##    Fresh  Milk Frozen Detergents_Paper Delicassen
    ##    <dbl> <dbl>  <dbl>            <dbl>      <dbl>
    ##  1 12669  9656    214             2674       1338
    ##  2  7057  9810   1762             3293       1776
    ##  3  6353  8808   2405             3516       7844
    ##  4 13265  1196   6404              507       1788
    ##  5 22615  5410   3915             1777       5185
    ##  6  9413  8259    666             1795       1451
    ##  7 12126  3199    480             3140        545
    ##  8  7579  4956   1669             3321       2566
    ##  9  5963  3648    425             1716        750
    ## 10  6006 11093   1159             7425       2098
    ## # ... with 430 more rows

variable Grocery has been dropped

\#\#\#. performing the comparison

``` r
# Performing our graphical comparison
# ---
# 
par(mfrow = c(1, 2))
corrplot(cor_Matrix, order = "hclust")
corrplot(cor(new_num), order = "hclust")
```

![](wholesales_files/figure-gfm/unnamed-chunk-37-1.png)<!-- --> removing
one variable reduce Multicollinearity as it remove redundancy

\#\#1. Wrapper Methods for feature selection we use the clustvarsel
function will implement variable section methodology for model-based
clustering to find the optimal subset of variables in a dataset.

``` r
#preview of dataset
#

head(new_num)
```

    ## # A tibble: 6 x 5
    ##   Fresh  Milk Frozen Detergents_Paper Delicassen
    ##   <dbl> <dbl>  <dbl>            <dbl>      <dbl>
    ## 1 12669  9656    214             2674       1338
    ## 2  7057  9810   1762             3293       1776
    ## 3  6353  8808   2405             3516       7844
    ## 4 13265  1196   6404              507       1788
    ## 5 22615  5410   3915             1777       5185
    ## 6  9413  8259    666             1795       1451

### scaling the datset.

``` r
# scaling our dataset by use of scale function.
#
library(dplyr)
wrapper_norm <- as.data.frame(scale(new_num))
#
#previewing the scaled dataset.
head(wrapper_norm)
```

    ##         Fresh        Milk     Frozen Detergents_Paper  Delicassen
    ## 1  0.05287300  0.52297247 -0.5886970      -0.04351919 -0.06626363
    ## 2 -0.39085706  0.54383861 -0.2698290       0.08630859  0.08904969
    ## 3 -0.44652098  0.40807319 -0.1373793       0.13308016  2.24074190
    ## 4  0.09999758 -0.62331041  0.6863630      -0.49802132  0.09330484
    ## 5  0.83928412 -0.05233688  0.1736612      -0.23165413  1.29786952
    ## 6 -0.20457266  0.33368675 -0.4955909      -0.22787885 -0.02619421

observation: our data is now scaled and is now on the same scale.

### installing the packages to be used

``` r
# Installing  package
# install.packages("mclust")
# Install.packages("clustvarsel")
#
#loading libraries 
library(mclust)                       
```

    ## Package 'mclust' version 5.4.7
    ## Type 'citation("mclust")' for citing this R package in publications.

    ## 
    ## Attaching package: 'mclust'

    ## The following object is masked from 'package:psych':
    ## 
    ##     sim

``` r
library(clustvarsel)
```

    ## Package 'clustvarsel' version 2.3.4

    ## Type 'citation("clustvarsel")' for citing this R package in publications.

### selecting the best features

``` r
# Sequential forward greedy search (default)
# ---
#
out = clustvarsel(wrapper_norm, G = 1:5)
out
```

    ## ------------------------------------------------------ 
    ## Variable selection for Gaussian model-based clustering
    ## Stepwise (forward/backward) greedy search
    ## ------------------------------------------------------ 
    ## 
    ##  Variable proposed Type of step   BICclust Model G   BICdiff Decision
    ##   Detergents_Paper          Add  -313.9955     V 5 945.84285 Accepted
    ##         Delicassen          Add  -868.2711   VVI 5 705.56265 Accepted
    ##             Frozen          Add -1445.6586   VVI 5 608.31377 Accepted
    ##             Frozen       Remove  -868.2711   VVI 5 608.31377 Rejected
    ##               Milk          Add -1958.6442   VVI 5 389.11513 Accepted
    ##               Milk       Remove -1445.6586   VVI 5 389.11513 Rejected
    ##              Fresh          Add -3112.8447   VVI 5  54.62052 Accepted
    ##              Fresh       Remove -1958.6442   VVI 5  54.62052 Rejected
    ##              Fresh       Remove -1958.6442   VVI 5  54.62052 Rejected
    ## 
    ## Selected subset: Detergents_Paper, Delicassen, Frozen, Milk, Fresh

observation: surprisingly all the 5 features in our dataset were
accepted.

### finding the best features

``` r
# Having identified the variables that we use, we proceed to build the clustering model:
# ---
#
# Building the model
Subset1 = wrapper_norm[out$subset]
mod = Mclust(Subset1, G = 1:5)
summary(mod)
```

    ## ---------------------------------------------------- 
    ## Gaussian finite mixture model fitted by EM algorithm 
    ## ---------------------------------------------------- 
    ## 
    ## Mclust VVI (diagonal, varying volume and shape) model with 5 components: 
    ## 
    ##  log-likelihood   n df       BIC       ICL
    ##       -1392.079 440 54 -3112.845 -3201.723
    ## 
    ## Clustering table:
    ##   1   2   3   4   5 
    ## 165  90  94  60  31

\#\#\#2. Embedded Methods future selection

We will use the ewkm function from the wskm package.

This is a weighted subspace clustering algorithm that is well suited to
very high dimensional data.

### installing the package to use

``` r
# We install and load our wskm package
#install.packages("wskm")
#
#loading library
library(wskm)
```

    ## Loading required package: latticeExtra

    ## 
    ## Attaching package: 'latticeExtra'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     layer

    ## Loading required package: fpc

### model building

``` r
embed_data <-wrapper_norm
set.seed(6)
model <- ewkm(embed_data[1:5], 2, lambda=2, maxiter=1000)
model
```

    ## K-means clustering with 2 clusters of sizes 204, 236
    ## 
    ## Cluster means:
    ##        Fresh       Milk     Frozen Detergents_Paper  Delicassen
    ## 1 -0.2744347  0.3146299 -0.3696722        0.5510653 -0.03458459
    ## 2  0.2372232 -0.2719682  0.3195471       -0.4763446  0.02989516
    ## 
    ## Clustering vector:
    ##   [1] 1 1 1 2 2 1 1 1 1 1 1 2 1 1 1 2 1 2 1 1 1 2 2 2 1 1 2 2 1 1 1 2 2 2 2 1 2
    ##  [38] 1 1 2 2 1 1 1 1 1 1 1 1 1 2 2 1 1 2 1 1 1 2 1 1 1 1 1 2 1 1 1 2 2 2 2 2 2
    ##  [75] 1 2 2 1 2 1 2 1 1 2 1 1 1 2 2 2 2 2 1 2 1 2 1 1 1 2 1 1 1 2 1 1 1 1 1 2 2
    ## [112] 1 2 2 2 2 1 2 2 2 2 2 2 1 2 2 2 1 2 2 2 2 2 2 1 1 2 2 2 2 2 2 2 2 2 1 1 1
    ## [149] 2 1 1 2 2 2 2 1 1 2 1 1 1 1 2 1 1 1 1 1 1 2 1 1 1 1 2 1 2 1 1 2 2 2 1 2 2
    ## [186] 2 2 2 1 1 2 1 2 1 2 2 2 1 2 2 1 1 2 2 1 1 2 1 1 1 2 1 1 1 1 1 1 2 1 2 1 1
    ## [223] 2 2 1 2 1 2 1 2 2 1 2 2 2 1 2 2 1 2 2 2 2 1 1 1 2 2 2 2 2 1 2 2 2 2 2 2 2
    ## [260] 2 2 2 2 2 1 2 1 2 1 2 2 1 2 2 2 1 2 2 2 1 1 1 2 2 2 2 1 2 2 1 2 2 2 1 2 1
    ## [297] 2 1 1 2 1 1 1 1 1 1 1 1 2 1 2 2 1 2 1 1 2 2 2 1 2 2 2 1 2 2 2 2 2 2 2 1 2
    ## [334] 1 2 1 2 2 2 2 1 1 1 1 2 1 1 1 1 1 2 1 1 1 2 2 2 1 2 2 2 1 2 2 2 1 1 1 2 1
    ## [371] 2 2 2 1 1 2 2 1 2 2 2 2 2 2 2 2 2 2 1 2 2 1 2 2 1 2 1 2 2 2 2 2 2 2 2 2 2
    ## [408] 1 1 1 1 1 1 2 2 1 1 1 1 1 1 1 2 1 1 2 1 2 2 2 2 2 2 2 1 2 2 1 1 2
    ## 
    ## Within cluster sum of squares by cluster:
    ## [1]  755.1876 1205.5871
    ##  (between_SS / total_SS =  10.7 %)
    ## 
    ## Available components:
    ## 
    ##  [1] "cluster"          "centers"          "totss"            "withinss"        
    ##  [5] "tot.withinss"     "betweenss"        "size"             "iterations"      
    ##  [9] "total.iterations" "restarts"         "weights"

``` r
##Loading and installing our cluster package to be used
#
#install.packages("cluster")
#
# loading the library
library("cluster")
```

``` r
# Cluster Plot against 1st 2 principal components
# ---
#
clusplot(embed_data[1:5], model$cluster, color=TRUE, shade=TRUE,
         labels=6, lines=1,main='Cluster Analysis for wholesale dataset')
```

![](wholesales_files/figure-gfm/unnamed-chunk-46-1.png)<!-- -->
observation, with 2 clusters, the two components explains 72.46% of the
point variability.

The red cluster shows that some customers spending habits increases
significantly over different products. Blue cluster shows these
customers spending habits on one product increases where as their
spending habits decreases on other products.

### calculating weight for each cluster

Weights are calculated for each variable and cluster.

They are a measure of the relative importance of each variable with
regards to the membership of the observations to that cluster.

The weights are incorporated into the distance function, typically
reducing the distance for more important variables.

``` r
# checking the Weights 
# 
round(model$weights*100,2)
```

    ##   Fresh Milk Frozen Detergents_Paper Delicassen
    ## 1     0    0  99.99             0.00          0
    ## 2     0    0   0.00            99.99          0

observation:

In cluster 1, Frozen has a high weight of 1

In cluster 2, Detergent\_paper has a high weigh of 1

### 4. Feature ranking

We use the FSelector Package. This is a package containing functions for
selecting attributes from a given dataset.

### installing packages and loading the library required

``` r
#Feature Ranking
#
#install.packages("FSelector")
#
library(FSelector)
#
```

### checking correlation.

``` r
library(caret)
library(corrplot)
#
corrplot(cor(embed_data), type = 'upper', method = 'number', tl.cex = 0.9)
```

![](wholesales_files/figure-gfm/unnamed-chunk-49-1.png)<!-- --> \#\#\#
ranking the features

``` r
# From the FSelector package, we use the correlation coefficient as a unit of valuation. 
# This would be one of the several algorithms contained 
# in the FSelector package that can be used rank the variables.
# ---
# 
Scores <- linear.correlation(embed_data)
Scores
```

    ##                  attr_importance
    ## Milk                   0.1005098
    ## Frozen                 0.3458815
    ## Detergents_Paper       0.1019529
    ## Delicassen             0.2446900

explanation: From the output above, we observe a list containing rows of
variables on the left and score on the right.

we will now make decision

### our decision will be based on cuttoff k=4

``` r
# In order to make a decision, we define a cutoff.
# we want to use the top 5 representative variables, 
# through the use of the cutoff.k function included in the FSelector package. 
# Alternatively, we could define our cutoff visually 
# but in cases where there are few variables than in high dimensional datasets.
# 
# cutoff.k: The algorithms select a subset from a ranked attributes.

Subset <- cutoff.k(Scores, 4)
as.data.frame(Subset)
```

    ##             Subset
    ## 1           Frozen
    ## 2       Delicassen
    ## 3 Detergents_Paper
    ## 4             Milk

observation: the top 4 features are frozen,delicassen,Detergents\_paper,
milk and Grocery.

### cutoff based on percentage

``` r
# We  set cutoff as a percentage which would indicate 
# that we would want to work with the percentage of the best variables.
# ---
#
Subset2 <-cutoff.k.percent(Scores, 0.6)
Subset2
```

    ## [1] "Frozen"     "Delicassen"

observation: at 60% we get 3 variables as the best to work with.

\#\#K-mean clustering

### confirmating all the datatypes are numerical

# 

``` r
#previewing the dataset to be 
embed_data<-
head(customers_df)
```

## k-means method

### getting numerical variables for clustering

``` r
head(new_num)
```

    ## # A tibble: 6 x 5
    ##   Fresh  Milk Frozen Detergents_Paper Delicassen
    ##   <dbl> <dbl>  <dbl>            <dbl>      <dbl>
    ## 1 12669  9656    214             2674       1338
    ## 2  7057  9810   1762             3293       1776
    ## 3  6353  8808   2405             3516       7844
    ## 4 13265  1196   6404              507       1788
    ## 5 22615  5410   3915             1777       5185
    ## 6  9413  8259    666             1795       1451

observation: variables have the right datatypes.

\#\#\#1. k-means clustering

``` r
# using already scaled data. 
#
summary(new_num)
```

    ##      Fresh             Milk           Frozen        Detergents_Paper 
    ##  Min.   :     3   Min.   :   55   Min.   :   25.0   Min.   :    3.0  
    ##  1st Qu.:  3128   1st Qu.: 1533   1st Qu.:  742.2   1st Qu.:  256.8  
    ##  Median :  8504   Median : 3627   Median : 1526.0   Median :  816.5  
    ##  Mean   : 12000   Mean   : 5796   Mean   : 3071.9   Mean   : 2881.5  
    ##  3rd Qu.: 16934   3rd Qu.: 7190   3rd Qu.: 3554.2   3rd Qu.: 3922.0  
    ##  Max.   :112151   Max.   :73498   Max.   :60869.0   Max.   :40827.0  
    ##    Delicassen     
    ##  Min.   :    3.0  
    ##  1st Qu.:  408.2  
    ##  Median :  965.5  
    ##  Mean   : 1524.9  
    ##  3rd Qu.: 1820.2  
    ##  Max.   :47943.0

our dataset is in different dimension.we will scale it before we do
clustering

``` r
# normalizing our dataset by use of scale function.
#
library(dplyr)
df_Normalize <- as.data.frame(scale(new_num))
#
#previewing the scaled dataset.
head(df_Normalize)
```

    ##         Fresh        Milk     Frozen Detergents_Paper  Delicassen
    ## 1  0.05287300  0.52297247 -0.5886970      -0.04351919 -0.06626363
    ## 2 -0.39085706  0.54383861 -0.2698290       0.08630859  0.08904969
    ## 3 -0.44652098  0.40807319 -0.1373793       0.13308016  2.24074190
    ## 4  0.09999758 -0.62331041  0.6863630      -0.49802132  0.09330484
    ## 5  0.83928412 -0.05233688  0.1736612      -0.23165413  1.29786952
    ## 6 -0.20457266  0.33368675 -0.4955909      -0.22787885 -0.02619421

observation: our data has been scaled and is now on the same scale.

### Getting the optimal clusters.

\#\#\#installing the packages to be used

``` r
library(factoextra)
```

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

``` r
library(cluster)
library(gridExtra) # for grid.arrange
```

    ## 
    ## Attaching package: 'gridExtra'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine

``` r
library(dplyr)
```

\#\#\#1.a Elbow method

### Getting the optimal cluster for k-means elbow method

``` r
# Determining Optimal clusters (k) Using Elbow method
fviz_nbclust(x = df_Normalize,FUNcluster = kmeans, method = 'wss' )
```

![](wholesales_files/figure-gfm/unnamed-chunk-58-1.png)<!-- -->
observation: elbow methods gives 2 as the optimal cluster, we will check
other methods.

\#\#\#1.b k-means silhouette method optimal cluster

``` r
# Determining Optimal clusters (k) Using Average Silhouette Method

fviz_nbclust(x = df_Normalize,FUNcluster = kmeans, method = 'silhouette' )
```

![](wholesales_files/figure-gfm/unnamed-chunk-59-1.png)<!-- -->
observation; silhouette method chooses 2 as optimal cluster.

\#\#\#1.c K-means Gap-Statistic method

``` r
#loading libraries required
library(factoextra)
library(cluster)

# compute gap statistic
set.seed(1234)
gap_stat <- clusGap(x = df_Normalize, FUN = kmeans, K.max = 15, nstart = 25,iter.max=100)

# Print the result
print(gap_stat, method = "firstmax")
```

    ## Clustering Gap statistic ["clusGap"] from call:
    ## clusGap(x = df_Normalize, FUNcluster = kmeans, K.max = 15, nstart = 25,     iter.max = 100)
    ## B=100 simulated reference sets, k = 1..15; spaceH0="scaledPCA"
    ##  --> Number of clusters (method 'firstmax'): 1
    ##           logW   E.logW      gap      SE.sim
    ##  [1,] 5.548652 7.129865 1.581213 0.009219731
    ##  [2,] 5.449888 6.995793 1.545905 0.009991787
    ##  [3,] 5.384440 6.922024 1.537584 0.009804606
    ##  [4,] 5.261183 6.859947 1.598764 0.009421083
    ##  [5,] 5.145344 6.808299 1.662955 0.009489560
    ##  [6,] 5.096112 6.763149 1.667037 0.009795126
    ##  [7,] 5.037667 6.722761 1.685094 0.009570377
    ##  [8,] 4.973473 6.687051 1.713578 0.009486315
    ##  [9,] 4.937628 6.656072 1.718444 0.010033475
    ## [10,] 4.892399 6.628184 1.735785 0.010190786
    ## [11,] 4.839106 6.601789 1.762683 0.010451775
    ## [12,] 4.815152 6.577865 1.762713 0.010595444
    ## [13,] 4.785596 6.556162 1.770566 0.010839477
    ## [14,] 4.748170 6.535631 1.787461 0.010797960
    ## [15,] 4.708260 6.516785 1.808525 0.010884256

\#visualizing to get the optimal cluster

``` r
# plot the result to determine the optimal number of clusters.
fviz_gap_stat(gap_stat)
```

![](wholesales_files/figure-gfm/unnamed-chunk-61-1.png)<!-- --> gap
statistics method gives as 1 as the optimal cluster.

``` r
# Compute k-means clustering with k = 2, optimal cluster
set.seed(123)
kmean_df <- kmeans(new_num, centers = 2, nstart = 25)
print(kmean_df)
```

    ## K-means clustering with 2 clusters of sizes 376, 64
    ## 
    ## Cluster means:
    ##       Fresh    Milk   Frozen Detergents_Paper Delicassen
    ## 1  7962.609 5279.63 2477.753         3005.846   1228.902
    ## 2 35721.719 8831.50 6562.734         2150.922   3263.688
    ## 
    ## Clustering vector:
    ##   [1] 1 1 1 1 2 1 1 1 1 1 1 1 2 1 2 1 1 1 1 1 1 1 2 2 2 1 1 1 1 2 1 1 1 2 1 1 2
    ##  [38] 1 1 2 2 1 1 1 1 1 1 2 1 1 1 1 2 1 2 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1
    ##  [75] 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1
    ## [112] 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 1 1
    ## [149] 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 2 1 2 1
    ## [186] 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## [223] 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 2 2 2 1 1 1 1 1 1 1 1 1 1 1 2 1 2 1 1 2
    ## [260] 2 1 1 2 1 1 1 1 1 1 1 1 1 1 2 1 1 2 1 1 1 1 1 2 2 2 2 1 1 1 2 1 1 1 1 1 1
    ## [297] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 1 1 1 1
    ## [334] 1 1 2 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## [371] 2 1 1 1 1 1 1 2 1 1 2 1 2 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 2 2 2 1 1 2
    ## [408] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 2 1 1 1 1 1 1 1 2 2 1 1 1
    ## 
    ## Within cluster sum of squares by cluster:
    ## [1] 39030411454 34907223610
    ##  (between_SS / total_SS =  37.3 %)
    ## 
    ## Available components:
    ## 
    ## [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    ## [6] "betweenss"    "size"         "iter"         "ifault"

We can visualize the results using the below code.

### visualizing the kmeans clusters

``` r
library(factoextra)
library(cluster)

fviz_cluster(kmean_df, data = customers_data)
```

![](wholesales_files/figure-gfm/unnamed-chunk-63-1.png)<!-- --> These
are the 2 optimal clusters.these shows clients spend less and can be
attributed to particular product buying,while others spend more which
time for example restaurants.

``` r
head(kmean_df)
```

    ## $cluster
    ##   [1] 1 1 1 1 2 1 1 1 1 1 1 1 2 1 2 1 1 1 1 1 1 1 2 2 2 1 1 1 1 2 1 1 1 2 1 1 2
    ##  [38] 1 1 2 2 1 1 1 1 1 1 2 1 1 1 1 2 1 2 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1
    ##  [75] 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1
    ## [112] 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 1 1
    ## [149] 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 2 1 2 1
    ## [186] 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## [223] 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 2 2 2 1 1 1 1 1 1 1 1 1 1 1 2 1 2 1 1 2
    ## [260] 2 1 1 2 1 1 1 1 1 1 1 1 1 1 2 1 1 2 1 1 1 1 1 2 2 2 2 1 1 1 2 1 1 1 1 1 1
    ## [297] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 1 1 1 1
    ## [334] 1 1 2 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## [371] 2 1 1 1 1 1 1 2 1 1 2 1 2 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 2 2 2 1 1 2
    ## [408] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 1 1 1 1 2 1 1 1 1 1 1 1 2 2 1 1 1
    ## 
    ## $centers
    ##       Fresh    Milk   Frozen Detergents_Paper Delicassen
    ## 1  7962.609 5279.63 2477.753         3005.846   1228.902
    ## 2 35721.719 8831.50 6562.734         2150.922   3263.688
    ## 
    ## $totss
    ## [1] 117949721617
    ## 
    ## $withinss
    ## [1] 39030411454 34907223610
    ## 
    ## $tot.withinss
    ## [1] 73937635063
    ## 
    ## $betweenss
    ## [1] 44012086554

observation: the above shows the means of the two clusters.

``` r
head(customers_df)
```

    ## # A tibble: 6 x 8
    ##   Channel    Region       Fresh  Milk Grocery Frozen Detergents_Paper Delicassen
    ##   <fct>      <fct>        <dbl> <dbl>   <dbl>  <dbl>            <dbl>      <dbl>
    ## 1 retail     Other region 12669  9656    7561    214             2674       1338
    ## 2 retail     Other region  7057  9810    9568   1762             3293       1776
    ## 3 retail     Other region  6353  8808    7684   2405             3516       7844
    ## 4 restaurant Other region 13265  1196    4221   6404              507       1788
    ## 5 retail     Other region 22615  5410    7198   3915             1777       5185
    ## 6 retail     Other region  9413  8259    5126    666             1795       1451

``` r
# Verifying the results of clustering
# ---
# 
par(mfrow = c(2,2), mar = c(5,4,2,2))

# Plotting to see how Sepal.Length and Sepal.Width data points have been distributed in clusters
plot(customers_df[c(5:6)], col = kmean_df$cluster)
plot(customers_df[c(4,5)], col = kmean_df$cluster)
plot(customers_df[c(3,4)], col = kmean_df$cluster)
# Plotting to see how Sepal.Length and Sepal.Width data points have been distributed 
# originally as per "class" attribute in dataset
# ---
```

![](wholesales_files/figure-gfm/unnamed-chunk-66-1.png)<!-- --> There
are two clusters of customers according to spending habits; Those that
spend more on fresh products and not groceries and vice versa. Those
that spend more on fresh products and not milk and vice versa. Those
whose spending habits are more on groceries and milk but that of milk is
slightly higher than that of groceries.

``` r
# showing how the clusters respond to the region variable
table(kmean_df$cluster, customers_df$Region)
```

    ##    
    ##     Lisbon Porto Other region
    ##   1     66    43          267
    ##   2     11     4           49

observation: based on the above findings, lisbon has a higher
representation compared to cluster Porto. thus the wholesale supplier
should concentrate on Lisbon.

``` r
# showing how the clusters respond to the region variable
table(kmean_df$cluster, customers_df$Channel)
```

    ##    
    ##     restaurant retail
    ##   1        246    130
    ##   2         52     12

observation: restaurants has representation on cluster 1, note that
cluster one is the high spending group.

### We can extract the clusters and add to our initial data to do some descriptive statistics at the cluster level

``` r
library(tidyr)
#customers_df %>% 
 # mutate(Cluster = kmean_df$cluster) %>%
 # group_by(Cluster) %>%
 # summarize_all('median')
```

\#\#checking the cluster size.

``` r
# Cluster size
kmean_df$size
```

    ## [1] 376  64

observation: we can see customers in cluster one has a high spending
habit.

## Recommendation

1.  Delicatessen and Milk There is a chance customers prefer more
    delicatessen from this wholesale compared to milk products therefore
    the marketing team should focus on ensuring that supply delicatessen
    products is constant and look into bettering quality of milk
    products maybe that could encourage customers to purchase their milk
    products as much as they do delicassens.

2.  Delicatessen and Grocery. When these two are compared, the amounts
    spend on groceries remain low as that spent on groceries increase.
    Marketing focus should be put on groceries in this case. The
    marketing team should probably come up with advertisements for
    healthy eating habits to push the grocery sales or even have them in
    the same position at the store with fresh products this could prompt
    buyers who prefer fresh products to buy them.

3.Since Lisbon is a high representation on high spending customers, the
wholesalers should focus on distributing products preferred by
restaurant to maximize revenue.
