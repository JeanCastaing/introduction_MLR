Grammar of Graphic
================
Jean Castaing
6 octobre 2018

Understanding the grammar of graphics
=====================================

1) Data
=======

Datasets mtcars : Motor Trend Car Road Tests

### Description

The data was extracted from the 1974 Motor Trend US magazine, and comprises fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973-74 models).

### Format

A data frame with 32 observations on 11 (numeric) variables.

\[, 1\] mpg Miles/(US) gallon \[, 2\] cyl Number of cylinders \[, 3\] disp Displacement (cu.in.) \[, 4\] hp Gross horsepower \[, 5\] drat Rear axle ratio \[, 6\] wt Weight (1000 lbs) \[, 7\] qsec 1/4 mile time \[, 8\] vs Engine (0 = V-shaped, 1 = straight) \[, 9\] am Transmission (0 = automatic, 1 = manual) \[,10\] gear Number of forward gears \[,11\] carb Number of carburetors

Note that it is recommended to use wide format (but not of everytime !)

``` r
library(ggplot2) # Load ggplot2 package
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
head(mtcars)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

2) Aesthetics
=============

Aesthetics is mapping

x-axis: wt: weight y-axis:qsec = 1/4 mile time col: cyl = number of cylinders

You can use aesthetics in the geom layer but only do that when you want to combine different datasets

``` r
ggplot(mtcars,aes(x=wt,y=qsec,col=hp))
```

3) Geometries
=============

geom\_point()

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-3-1.png) Note that you can combine several geometries:

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec))+
  geom_point(alpha=0.4)+
  geom_smooth(se=FALSE,col="black")+
  geom_smooth(aes(col=as.factor(cyl)),se=FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-4-1.png) Look like to over fit too much + I would like to have indicators for each points

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point(alpha=0.4)+
  geom_smooth(method = "lm", se = FALSE) +
  geom_smooth(aes(group = 1), method = "lm", se = FALSE, linetype = 2)
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-5-1.png) You can change the display of your geomtry in the geom\_layer

``` r
ggplot(mtcars, aes(x=wt,y=qsec, col=as.factor(cyl)))+geom_point() 
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-6-1.png)

``` r
ggplot(mtcars, aes(x=wt,y=qsec, col=as.factor(cyl)))+geom_point(shape=1, size=2)
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-7-1.png) The mapping did not change !

| Aesthetic |              Description              |
|:---------:|:-------------------------------------:|
|     x     |            x-axis position            |
|     y     |            y-axis position            |
|   colour  |        colour of outlines shape       |
|    fill   |              fill colour              |
|    size   | Diameter of points, thickness of line |
|   alpha   | alpha from 0 (invisble) to 1 (opaque) |
|   labels  |         Text on a plot or axes        |
|  linetype |            Line dash pa!ern           |
|   shape   |                 Shape                 |

### Modifying the aesthetics

``` r
cyl_am <- ggplot(mtcars, aes(x = factor(cyl), fill = factor(am)))
cyl_am+geom_bar()
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-8-1.png)

``` r
cyl_am+geom_bar(position="fill")
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
cyl_am+geom_bar(position="dodge")
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-10-1.png) The axes need to be refine (I don't like the color and the name):

``` r
val = c("darkblue", "skyblue")
lab = c("Manual", "Automatic")
 
cyl_am+geom_bar(position="dodge")+
  scale_x_discrete("Cylinders") + 
  scale_y_continuous("Number") +
  scale_fill_manual("Transmission", 
                    values = val,
                    labels = lab) 
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-11-1.png)

4) Facets
=========

`facet_grid(rows ~ columns)`: create several views Really useful when we have several variables to plot, especially ordinal.

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  facet_grid(.~as.factor(carb))
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  facet_grid(as.factor(am)~as.factor(carb))
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-13-1.png) If you want to drop some levels (no observations): `space="free_y"`

If you would like to have adaptative scale: `scale="free_y"`

5) Statistics
=============

We add a linear regression.

stat\_functions can be closed to geom. There is no differences between `geom_smooth(method = "lm", se = F, col = "red")` and `stat_smooth(method = "lm", se = F, col = "red")`

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  facet_grid(.~as.factor(carb))+
  stat_smooth(method = "lm", se = F, col = "red")
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-14-1.png)

Several statistics:

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  stat_smooth(method = "loess", se = F)+
  stat_smooth(method = "lm", se = F,linetype="dotdash")+
  stat_smooth(method = "lm", se = F,linetype="dotdash",aes(group=1,col="All lm"))+
  stat_smooth(method = "loess",
              se = F,
              aes(group=1,col="All"),
              span=0.7)+
  stat_quantile(quantiles = 0.5,aes(group=1,col="All median lm"))
```

    ## Loading required package: SparseM

    ## 
    ## Attaching package: 'SparseM'

    ## The following object is masked from 'package:base':
    ## 
    ##     backsolve

    ## Smoothing formula not specified. Using: y ~ x

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-15-1.png) \# 6) Coordinates

Note that we find the scale\_function that enables us to access to the aesthetics.

-   `scale_y_continuous`
-   `scale_x_discrete`
-   `scale_color_discrete`
-   `scale_fill_continuous`
-   `scale_shape...`
-   `scale_linetype...`

Useful to zoom on the plot. \* `xlim()` \* `scale_x_continuous` \* `coordcartesian(xlim=)`

Aspect ratio, by default 1:1. Beware when you change it, it can be very deceiptful ! \* `coord_fixed()` \* `coord_equal()`

``` r
ggplot(mtcars,aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  facet_grid(.~as.factor(carb))+
  stat_smooth(method = "lm",
              se = F,
              col = "red")+
  scale_y_continuous("1/4 time time in sec",
                     limits=c(15,25))+
  coord_equal()
```

    ## Warning: Removed 2 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 2 rows containing missing values (geom_point).

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-16-1.png)

Use polar coordinate to create pie chart

``` r
ggplot(mtcars, aes(x = 1, fill = as.factor(cyl)))+
  geom_bar()
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-17-1.png)

``` r
ggplot(mtcars, aes(x = 1, fill = as.factor(cyl)))+
  geom_bar()+
  coord_polar(theta = "y")
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-18-1.png)

7) Themes
=========

Themes control all the non-ink data: all the visual elements that are not part of the data.

4 types: \* text: `element_text() * line:`element\_line()`* rectangle:`element\_rect()`* delete:`element\_blank()\`

![Elements of theme](image/Elements%20of%20theme.PNG)

``` r
ggplot(mtcars%>%filter(carb<6 & qsec>15),aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  facet_grid(.~as.factor(carb))+
  stat_smooth(method = "lm",
              se = F,
              col = "red")+
  scale_y_continuous("1/4 time time in sec",
                     limits=c(15,25))+
  theme(plot.background =element_rect(fill = "white",color="black",size=1),
        panel.grid.major = element_blank(),
        axis.line=element_line(color="red"),
        axis.ticks=element_line(color="red"),
        legend.position = "bottom")
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-19-1.png)

Use a pre-defined theme

``` r
ggplot(mtcars%>%filter(carb<6 & qsec>15),aes(x=wt,
                  y=qsec,
                  col=as.factor(cyl)))+
  geom_point()+
  facet_grid(.~as.factor(carb))+
  stat_smooth(method = "lm",
              se = F,
              col = "red")+
  scale_y_continuous("1/4 time time in sec",
                     limits=c(15,25))+
  theme_minimal()
```

![](Grammar_of_Graphic_files/figure-markdown_github/unnamed-chunk-20-1.png)

Conclusion
==========

Building a graph is manipulating the grammar of graphic ! s

![Layers of Grammar](image/Grammar%20of%20graphics.PNG)
