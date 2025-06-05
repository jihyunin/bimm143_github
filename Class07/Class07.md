# Class07
Jihyun In

- [Clustering](#clustering)
  - [Hierarchial clustering](#hierarchial-clustering)
- [Principal Component Analysis](#principal-component-analysis)
  - [Data import](#data-import)
  - [PCA to the rescue](#pca-to-the-rescue)

Today we will explore unsupervised machine learning methods starting
iwht clustering and idmensionality reduction.

## Clustering

``` r
hist(rnorm(10000, mean=3))
```

![](Class07_files/figure-commonmark/unnamed-chunk-1-1.png)

Teturn 30 numbers centred on -3.

``` r
tmp <- c(rnorm(30, mean=-3),
rnorm(30, mean=3))

x<- cbind(x=tmp, y= rev(tmp))

x
```

                  x         y
     [1,] -2.281366  3.497582
     [2,] -3.297619  1.855907
     [3,] -2.148562  3.239762
     [4,] -2.015018  3.121405
     [5,] -4.091002  1.896211
     [6,] -2.599866  4.512784
     [7,] -3.764080  1.438860
     [8,] -4.384199  3.036023
     [9,] -3.105268  4.460266
    [10,] -2.002561  3.987186
    [11,] -3.416894  5.148642
    [12,] -3.216032  3.653630
    [13,] -2.289555  4.368922
    [14,] -5.439005  1.992645
    [15,] -3.072224  2.478361
    [16,] -2.805334  3.622838
    [17,] -5.821277  1.897879
    [18,] -2.816857  1.755360
    [19,] -3.621822  2.601005
    [20,] -2.728142  4.492329
    [21,] -4.105558  3.825086
    [22,] -4.513616  3.728788
    [23,] -2.776341  2.939444
    [24,] -2.512433  4.079511
    [25,] -3.290049  1.837970
    [26,] -1.241093  3.303281
    [27,] -1.529847  2.405407
    [28,] -4.456850  3.369081
    [29,] -2.272429  4.162354
    [30,] -2.565920  2.875275
    [31,]  2.875275 -2.565920
    [32,]  4.162354 -2.272429
    [33,]  3.369081 -4.456850
    [34,]  2.405407 -1.529847
    [35,]  3.303281 -1.241093
    [36,]  1.837970 -3.290049
    [37,]  4.079511 -2.512433
    [38,]  2.939444 -2.776341
    [39,]  3.728788 -4.513616
    [40,]  3.825086 -4.105558
    [41,]  4.492329 -2.728142
    [42,]  2.601005 -3.621822
    [43,]  1.755360 -2.816857
    [44,]  1.897879 -5.821277
    [45,]  3.622838 -2.805334
    [46,]  2.478361 -3.072224
    [47,]  1.992645 -5.439005
    [48,]  4.368922 -2.289555
    [49,]  3.653630 -3.216032
    [50,]  5.148642 -3.416894
    [51,]  3.987186 -2.002561
    [52,]  4.460266 -3.105268
    [53,]  3.036023 -4.384199
    [54,]  1.438860 -3.764080
    [55,]  4.512784 -2.599866
    [56,]  1.896211 -4.091002
    [57,]  3.121405 -2.015018
    [58,]  3.239762 -2.148562
    [59,]  1.855907 -3.297619
    [60,]  3.497582 -2.281366

Make a plot o f`x`

``` r
plot(x)
```

![](Class07_files/figure-commonmark/unnamed-chunk-3-1.png)

\###K-means The main function in “base R” for K-means clustering is
called `kmeans()`

``` r
km <- kmeans(x, centers=2)
km
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1  3.186126 -3.139361
    2 -3.139361  3.186126

    Clustering vector:
     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

    Within cluster sum of squares by cluster:
    [1] 62.78889 62.78889
     (between_SS / total_SS =  90.5 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

The `kmeans`() function returns a “list” with 9 ccomponents. You can see
the named components of any list with the `attributes()` function.

``` r
attributes(km)
```

    $names
    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

    $class
    [1] "kmeans"

> Q. How many points are in each cluster?

``` r
km$size
```

    [1] 30 30

> Q. Cluster assignment/membership vector

``` r
km$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

> Q. Cluster centers?

``` r
km$centers
```

              x         y
    1  3.186126 -3.139361
    2 -3.139361  3.186126

> Q. Make a plot of our `kmeans()` reslts showing cluster assignment and
> using different colors for each cluster/group of points and cluster
> centers in blue.

``` r
plot(x, col=km$cluster + 1) #didn't like black as a point color
points(km$centers, col="blue", pch=15, cex=2)
```

![](Class07_files/figure-commonmark/unnamed-chunk-9-1.png)

> Q. Run `kmeans()` again on `x` and this cluster into 4 groups/clusters
> and plot the same result figure as above.

``` r
km4 <- kmeans(x, centers=4)
km4
```

    K-means clustering with 4 clusters of sizes 14, 10, 30, 6

    Cluster means:
              x         y
    1  3.975035 -2.473897
    2  2.208380 -3.082576
    3 -3.139361  3.186126
    4  2.974917 -4.786751

    Clustering vector:
     [1] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 2 1 4 2 1 2 1 2
    [39] 4 4 1 2 2 4 1 2 4 1 1 1 1 1 4 2 1 2 1 1 2 1

    Within cluster sum of squares by cluster:
    [1]  8.747230  7.105186 62.788892  5.880263
     (between_SS / total_SS =  93.6 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

``` r
plot(x, col=km4$cluster + 3) #didn't like black as a point color
points(km4$centers, col="blue", pch=4, cex=2) #changed pch shape for preference
```

![](Class07_files/figure-commonmark/unnamed-chunk-11-1.png)

> **Key-point:** K-means clustering is super popular but cna be misused.
> One big limitation is that it can impose a clustering pattern on your
> data even if clear natrual grouping doesn’t exist - i.e. it does what
> you tell it to do in terms of `centers`.

### Hierarchial clustering

The main function in “base” R for hierarchical clustering is called
`hclust()`

You can’t just pass our dataset as is into `hclust()`, you must give
“distance matrix” as input. We can get this from the `dist()` function
in R.

``` r
d <- dist(x)
hc <- hclust(d)
hc
```


    Call:
    hclust(d = d)

    Cluster method   : complete 
    Distance         : euclidean 
    Number of objects: 60 

the results of `hclust()` don’t have a useful `print()` method but do
have a special `plot()` method.

``` r
plot(hc)
abline(h=8, col="red")
```

![](Class07_files/figure-commonmark/unnamed-chunk-13-1.png)

To get our main cluster assignemnt (membership vector), we need to “cut”
the tree at the “big goalposts”

``` r
grps <- cutree(hc, h=8)
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

``` r
table(grps)
```

    grps
     1  2 
    30 30 

``` r
plot(x, col=grps +1) #Again, I like colors
```

![](Class07_files/figure-commonmark/unnamed-chunk-16-1.png)

Hierarchial Clustering is distinct in that the dendrogram(tree figure)
can reveal the potential grouping in your data (unlike K-means).

## Principal Component Analysis

PCA is a common and highly useful dimensionality reduction technique
used in many fields - particularly bioinformatics’

Here we will analyze some data from the UK on food consumption.

### Data import

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url)
head(x)
```

                   X England Wales Scotland N.Ireland
    1         Cheese     105   103      103        66
    2  Carcass_meat      245   227      242       267
    3    Other_meat      685   803      750       586
    4           Fish     147   160      122        93
    5 Fats_and_oils      193   235      184       209
    6         Sugars     156   175      147       139

``` r
x <- read.csv(url, row.names=1)
head(x)
```

                   England Wales Scotland N.Ireland
    Cheese             105   103      103        66
    Carcass_meat       245   227      242       267
    Other_meat         685   803      750       586
    Fish               147   160      122        93
    Fats_and_oils      193   235      184       209
    Sugars             156   175      147       139

``` r
barplot(as.matrix(x), beside=F, col=rainbow(nrow(x)))
```

![](Class07_files/figure-commonmark/unnamed-chunk-19-1.png)

One conventional plot that can be useful is called a “pairs” plot.

``` r
pairs(x, col=rainbow(nrow(x)), pch=16)
```

![](Class07_files/figure-commonmark/unnamed-chunk-20-1.png)

### PCA to the rescue

The main function in base for PCA is called `prcomp()`.

``` r
t(x)
```

              Cheese Carcass_meat  Other_meat  Fish Fats_and_oils  Sugars
    England      105           245         685  147            193    156
    Wales        103           227         803  160            235    175
    Scotland     103           242         750  122            184    147
    N.Ireland     66           267         586   93            209    139
              Fresh_potatoes  Fresh_Veg  Other_Veg  Processed_potatoes 
    England               720        253        488                 198
    Wales                 874        265        570                 203
    Scotland              566        171        418                 220
    N.Ireland            1033        143        355                 187
              Processed_Veg  Fresh_fruit  Cereals  Beverages Soft_drinks 
    England              360         1102     1472        57         1374
    Wales                365         1137     1582        73         1256
    Scotland             337          957     1462        53         1572
    N.Ireland            334          674     1494        47         1506
              Alcoholic_drinks  Confectionery 
    England                 375             54
    Wales                   475             64
    Scotland                458             62
    N.Ireland               135             41

``` r
pca <-prcomp(t(x))
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3       PC4
    Standard deviation     324.1502 212.7478 73.87622 3.176e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.000e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.000e+00

The `precomp()` function returns a list object of our results with

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

The two main results in here are `pca$x` and `pca$rotation`. The first
of these (`pca$x`) contains the scores o fthe dta on the new PC axis -
we use these to make our “PCA plot”.

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945 -4.894696e-14
    Wales     -240.52915 -224.646925 -56.475555  5.700024e-13
    Scotland   -91.86934  286.081786 -44.415495 -7.460785e-13
    N.Ireland  477.39164  -58.901862  -4.877895  2.321303e-13

``` r
library(ggplot2)
library(ggrepel)
#Make  aplot of pca$x with PC1 vs. PC2

ggplot(pca$x)  + 
  aes(PC1, PC2, label=rownames(pca$x)) + 
  geom_point() +
  geom_text_repel() +
  theme_bw()
```

![](Class07_files/figure-commonmark/unnamed-chunk-24-1.png)

the above plot plots PC2 vs. PC1, showing the scores along each axis.
This shows that along PC1, N.Ireland is an outlier, and along PC2,
Scotland is far from wales.

The second major result is contained in the `pca$rotation` object or
component. Let’s plot this to see what PCA is picking up…

``` r
ggplot(pca$rotation) + 
  aes(PC1, rownames(pca$rotation)) + 
  geom_col()
```

![](Class07_files/figure-commonmark/unnamed-chunk-25-1.png)

The above bar plot shows how much each category of food contributes to
PC1. Fresh potatoes and soft drinks explain most of the positive
variance (what Ireland, the positive outlier, consumes more of), and
fresh fruit and alcoholic drinks explain a large part of what Ireland
consumes less of.
