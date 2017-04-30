---
layout: post
title: Coxcomb plots and 'spiecharts' in R
categories:
- R
tags:
- visualisation
- R
- open source
- computing
---


I was contacted recently by a housing organisation who wanted 
an attractive visualisation of their finances, arranged in a circular 
form. Because there were two 4 continuous variables to include, all
of which were proportions of each other, the client suggested a plot
similar to a pie chart, but with each segment extending out a different
radius from the segment. I realised later that what I had been asked to 
make was a modified [coxcomb](http://en.wikipedia.org/wiki/Coxcomb_diagram#Polar_area_diagram) 
plot, invented by 
[Florence Nightingale](http://en.wikipedia.org/wiki/Florence_Nightingale) 
to represent statistics on cause of death during the Crimean War.
In fact, I had been asked to make a "[spie chart](http://www.cs.huji.ac.il/~feit/papers/Spie03TR.pdf)."
This post demonstrates, for the first time to my knowledge, how it can be done 
using ggplot2. A reproducible example of this, including sample data input, can be 
found on the project's github repository: https://github.com/Robinlovelace/lilacPlot . Please fork and attribute as appropriate!

![plot of chunk unnamed-chunk-5](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-53.png) 

<!--more-->

# Reading and looking at the data

This is the original dataset I was given:


```r
f <- read.csv("F2.csv")
f[1:10, 1:12]
```


Without worrying too much about the details, the basics of the dataset are
as follows: 

 - One observation per row, these will later be bars on the box plot
 - Two components of data - captital and revenue
 - Different orders of magnitude: some data is in absolute monetary terms, some in percentages
 
Base on the above points, a prerequisite was to create preliminary plots and manipulate the 
data so it would better fit in a coxcomb plot.

The first stage, however, is to demonstrate how the addition of 
`coord_polar` to a barchart can conver it into a pie chart:


```r
(p <- ggplot(f, aes(x = H, y = Allocation)) + geom_bar(color = "black", stat = "identity", 
    width = 1))
```

![plot of chunk unnamed-chunk-2](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-21.png) 

```r
p + coord_polar()
```

![plot of chunk unnamed-chunk-2](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-22.png) 


The above example works well, but notice that all the bars are of equal widths. 
What we want is to be proportional to a value (variable "Value") of each observation.
To do this we use the age-old function `cumsum`, as described in an
answer to a [stackexchange question](http://stackoverflow.com/questions/20688376/how-to-make-variable-bar-widths-in-ggplot2-not-overlap-or-gap).


```r
w <- f$Value
pos <- 0.5 * (cumsum(w) + cumsum(c(0, w[-length(w)])))

(p <- ggplot(f, aes(x = pos)) + geom_bar(aes(y = Allocation), width = w, color = "black", 
    stat = "identity"))
```

![plot of chunk unnamed-chunk-3](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-31.png) 

```r
p + coord_polar(theta = "x") + scale_x_continuous(labels = f$H, breaks = pos)
```

![plot of chunk unnamed-chunk-3](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-32.png) 


Finally a spie chart has been created. After that revelation, it was essentially about adding the 'bells and 
whistles', including a 10% line to represent how much more or less than their share each observation was
paying.

# Adding the 10 %


```r
f$Deposit/f$Value
```

```
##  [1] 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1 0.1
## [18] 0.1 0.1 0.1
```

```r

# add 10% in there
p <- ggplot(f)
p + geom_bar(aes(x = pos, y = Allocation), width = w, color = "black", stat = "identity") + 
    geom_bar(aes(x = pos, y = 0.1), width = w, color = "black", stat = "identity", 
        fill = "green") + coord_polar()
```

![plot of chunk unnamed-chunk-4](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-41.png) 

```r

# make proportional to area
f$Allo <- sqrt(f$Allocation)

p <- ggplot(f)
p + geom_bar(aes(x = pos, y = Allo, width = w), color = "black", stat = "identity") + 
    geom_bar(aes(x = pos, y = sqrt(0.1), width = w), color = "black", stat = "identity", 
        fill = "green") + coord_polar()
```

![plot of chunk unnamed-chunk-4](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-42.png) 

```r

# add capital
capital <- (f$Captial + f$Deposit)/(f$Value) * f$Allocation
capital <- sqrt(capital)

p + geom_bar(aes(x = pos, y = Allo, width = w), color = "black", stat = "identity") + 
    geom_bar(aes(x = pos, y = capital, width = w), color = "black", stat = "identity", 
        fill = "red") + geom_bar(aes(x = pos, y = sqrt(0.1), width = w), color = "black", 
    stat = "identity", fill = "green") + coord_polar() + scale_x_continuous(labels = f$H, 
    breaks = pos)
```

![plot of chunk unnamed-chunk-4](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-43.png) 

```r

# add ablines
p + geom_bar(aes(x = pos, y = Allo, width = w), color = "grey40", stat = "identity", 
    fill = "lightgrey") + geom_bar(aes(x = pos, y = capital, width = w), color = "grey40", 
    stat = "identity", fill = "red") + geom_bar(aes(x = pos, y = sqrt(0.1), 
    width = w), color = "grey40", stat = "identity", fill = "green") + geom_abline(intercept = 1, 
    slope = 0, linetype = 2) + geom_abline(intercept = sqrt(1.1), slope = 0, 
    linetype = 3) + geom_abline(intercept = sqrt(0.9), slope = 0, linetype = 3)
```

![plot of chunk unnamed-chunk-4](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-44.png) 

```r

# calculate vertical ablines of divisions
v1 <- 0.51 * f$Value[1]
v2 <- cumsum(f$Value)[17] + f$Value[18] * 0.31
v3 <- cumsum(f$Value)[17] + f$Value[18] * 0.64

p + geom_bar(aes(x = pos, y = Allo, width = w), color = "grey40", stat = "identity", 
    fill = "lightgrey") + geom_vline(x = v1, linetype = 5) + geom_vline(x = v2, 
    linetype = 5) + geom_vline(x = v3, linetype = 5) + coord_polar()
```

![plot of chunk unnamed-chunk-4](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-45.png) 

```r

# putting it all together
p <- ggplot(f)
p + geom_bar(aes(x = pos, y = Allo, width = w), color = "grey40", stat = "identity", 
    fill = "lightgrey") + geom_bar(aes(x = pos, y = capital, width = w), color = "grey40", 
    stat = "identity", fill = "red") + geom_bar(aes(x = pos, y = sqrt(0.1), 
    width = w), color = "grey40", stat = "identity", fill = "green") + geom_abline(intercept = 1, 
    slope = 0, linetype = 2) + geom_abline(intercept = sqrt(1.1), slope = 0, 
    linetype = 3) + geom_abline(intercept = sqrt(0.9), slope = 0, linetype = 3) + 
    geom_vline(x = v1, linetype = 5) + geom_vline(x = v2, linetype = 5) + geom_vline(x = v3, 
    linetype = 5) + coord_polar() + scale_x_continuous(labels = f$H, breaks = pos) + 
    theme_classic()
```

![plot of chunk unnamed-chunk-4](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-46.png) 


The above looks great, but ideally, for an 'infographic' feel, it would 
have no annoying axes clogging up the visuals. This was done by creating an 
entirely new ggpot theme.

# Create theme with no axes


```r
theme_infog <- theme_classic() + theme(axis.line = element_blank(), axis.title = element_blank(), 
    axis.ticks = element_blank(), axis.text.y = element_blank())
last_plot() + theme_infog
```

![plot of chunk unnamed-chunk-5](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-5.png) 


# Creating a ring

To add the revenue element to the graph is not a task to be taken likely. 
This was how I tackled the problem, by creating a tall, variable-width 
bar chart first, and later adding the original spie chart after:


```r
f$Cap.r <- f$Cap/mean(f$Cap) * 0.1 + 1.2
f$Cont.r <- f$Contribution/mean(f$Cap) * 0.1 + 1.2
f$Rep.r <- f$Cont.r + f$Repayments/mean(f$Cap) * 0.1
f$H <- c("a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", 
    "o", "p", "q", "r", "s", "t")

p <- ggplot(f)
p + geom_bar(aes(x = pos, y = Allo, width = w), color = "grey40", stat = "identity", 
    fill = "lightgrey")
```

![plot of chunk unnamed-chunk-6](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-61.png) 

```r
# we need the axes to be bigger for starters - try 1.3 to 1.5

p + geom_bar(aes(x = pos, y = Cap.r, width = w), color = "grey40", stat = "identity", 
    fill = "white") + geom_bar(aes(x = pos, y = Rep.r, width = w), color = "grey40", 
    stat = "identity", fill = "grey80") + geom_bar(aes(x = pos, y = Cont.r, 
    width = w), color = "grey40", stat = "identity", fill = "grey30") + geom_bar(aes(x = pos, 
    y = 1.196, width = w), color = "white", stat = "identity", fill = "white")
```

![plot of chunk unnamed-chunk-6](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-62.png) 

```r

last_plot() + geom_bar(aes(x = pos, y = Allo, width = w), color = "grey40", 
    stat = "identity", fill = "grey80") + geom_bar(aes(x = pos, y = capital, 
    width = w), color = "grey40", stat = "identity", fill = "grey30") + geom_bar(aes(x = pos, 
    y = sqrt(0.1), width = w), color = "grey40", stat = "identity", fill = "black") + 
    geom_abline(intercept = 1, slope = 0, linetype = 5) + geom_abline(intercept = sqrt(1.1), 
    slope = 0, linetype = 3) + geom_abline(intercept = sqrt(0.9), slope = 0, 
    linetype = 3) + coord_polar() + scale_x_continuous(labels = f$H, breaks = pos) + 
    theme_infog
```

![plot of chunk unnamed-chunk-6](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-63.png) 


# Just inner

After all that it was decided it looked nicer with only the inner ring anyway.
Here is the finished product:


```r
p <- ggplot(f)
p + geom_bar(aes(x = pos, y = Allo, width = w), color = "grey40", stat = "identity", 
    fill = "grey80") + geom_bar(aes(x = pos, y = capital, width = w), color = "grey40", 
    stat = "identity", fill = "grey30") + geom_bar(aes(x = pos, y = sqrt(0.1), 
    width = w), color = "grey40", stat = "identity", fill = "black") + geom_abline(intercept = 1, 
    slope = 0, linetype = 5) + geom_abline(intercept = sqrt(1.1), slope = 0, 
    linetype = 3) + geom_abline(intercept = sqrt(0.9), slope = 0, linetype = 3) + 
    coord_polar() + scale_x_continuous(labels = f$H, breaks = pos) + theme_infog
```

![plot of chunk unnamed-chunk-7](https://raw.github.com/Robinlovelace/robinlovelace.github.io/master/figure/unnamed-chunk-7.png) 

```r
ggsave("just-inner.png", width = 7, height = 7, dpi = 800)
```


