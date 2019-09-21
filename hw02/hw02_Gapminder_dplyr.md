---
title: "Assignment 02: Explore Gapminder and use dplyr"
output: 
  html_document:
    keep_md: true
---


## Exercise 1: Basic __dplyr__

### 1.1 
Use __filter()__ to subset the __gapminder__ data to three countries of your choice in the 1970’s.

```r
gapminder %>% 
  filter(country == "Canada" | country == "Chile" | country == "Cambodia",
         year < 1980 & year > 1969)
```

```
## # A tibble: 6 x 6
##   country  continent  year lifeExp      pop gdpPercap
##   <fct>    <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Cambodia Asia       1972    40.3  7450606      422.
## 2 Cambodia Asia       1977    31.2  6978607      525.
## 3 Canada   Americas   1972    72.9 22284500    18971.
## 4 Canada   Americas   1977    74.2 23796400    22091.
## 5 Chile    Americas   1972    63.4  9717524     5494.
## 6 Chile    Americas   1977    67.1 10599793     4757.
```


### 1.2 

Use the pipe operator __%>%__ to select “country” and “gdpPercap” from your filtered dataset in 1.1.

```r
gapminder %>% 
  filter(country == "Canada" | country == "Chile" | country == "Cambodia",
         year < 1980 & year > 1969) %>% 
  select(country, gdpPercap)
```

```
## # A tibble: 6 x 2
##   country  gdpPercap
##   <fct>        <dbl>
## 1 Cambodia      422.
## 2 Cambodia      525.
## 3 Canada      18971.
## 4 Canada      22091.
## 5 Chile        5494.
## 6 Chile        4757.
```

### 1.3 

Filter gapminder to all entries that have experienced a drop in life expectancy. Be sure to include a new variable that’s the increase in life expectancy in your tibble. Hint: you might find the __lag()__ or __diff()__ functions useful.


```r
gapminder %>% 
  mutate(lifeIncrease = lifeExp - lag(lifeExp)) %>% 
  filter(lifeIncrease < 0, year != 1952)#exclude the first entry of each country, where the lifeIncrease is the difference between two countries
```

```
## # A tibble: 102 x 7
##    country  continent  year lifeExp     pop gdpPercap lifeIncrease
##    <fct>    <fct>     <int>   <dbl>   <int>     <dbl>        <dbl>
##  1 Albania  Europe     1992    71.6 3326498     2497.       -0.419
##  2 Angola   Africa     1987    39.9 7874230     2430.       -0.036
##  3 Benin    Africa     2002    54.4 7026113     1373.       -0.371
##  4 Botswana Africa     1992    62.7 1342614     7954.       -0.877
##  5 Botswana Africa     1997    52.6 1536536     8647.      -10.2  
##  6 Botswana Africa     2002    46.6 1630347    11004.       -5.92 
##  7 Bulgaria Europe     1977    70.8 8797022     7612.       -0.09 
##  8 Bulgaria Europe     1992    71.2 8658506     6303.       -0.15 
##  9 Bulgaria Europe     1997    70.3 8066057     5970.       -0.87 
## 10 Burundi  Africa     1992    44.7 5809236      632.       -3.48 
## # … with 92 more rows
```

### 1.4 

Filter gapminder so that it shows the max GDP per capita experienced by each country. Hint: you might find the __max()__ function useful here.

```r
gapminder %>% 
  group_by(country) %>% 
  mutate(maxgpdPercap = max(gdpPercap)) %>% 
  filter(gdpPercap == maxgpdPercap)
```

```
## # A tibble: 142 x 7
## # Groups:   country [142]
##    country     continent  year lifeExp       pop gdpPercap maxgpdPercap
##    <fct>       <fct>     <int>   <dbl>     <int>     <dbl>        <dbl>
##  1 Afghanistan Asia       1982    39.9  12881816      978.         978.
##  2 Albania     Europe     2007    76.4   3600523     5937.        5937.
##  3 Algeria     Africa     2007    72.3  33333216     6223.        6223.
##  4 Angola      Africa     1967    36.0   5247469     5523.        5523.
##  5 Argentina   Americas   2007    75.3  40301927    12779.       12779.
##  6 Australia   Oceania    2007    81.2  20434176    34435.       34435.
##  7 Austria     Europe     2007    79.8   8199783    36126.       36126.
##  8 Bahrain     Asia       2007    75.6    708573    29796.       29796.
##  9 Bangladesh  Asia       2007    64.1 150448339     1391.        1391.
## 10 Belgium     Europe     2007    79.4  10392226    33693.       33693.
## # … with 132 more rows
```

### 1.5

Produce a scatterplot of Canada’s life expectancy vs. GDP per capita using ggplot2, without defining a new variable. That is, after filtering the gapminder data set, pipe it directly into the __ggplot()__ function. Ensure GDP per capita is on a log scale.


```r
gapminder %>% 
  filter(country == "Canada") %>% 
  ggplot() +
  geom_point(aes(lifeExp, log(gdpPercap)))
```

![](hw02_Gapminder_dplyr_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


## Exercise 2: Explore individual variables with __dplyr__

Pick one categorical variable and one quantitative variable to explore. Answer the following questions in whichever way you think is appropriate, using dplyr:

- What are possible values (or range, whichever is appropriate) of each variable?

- What values are typical? What’s the spread? What’s the distribution? Etc., tailored to the variable at hand.

- Feel free to use summary stats, tables, figures.

## Exercise 3: Explore various plot types

Make two plots that have some value to them. That is, plots that someone might actually consider making for an analysis. Just don’t make the same plots we made in class – feel free to use a data set from the datasets R package if you wish.

- A scatterplot of two quantitative variables.

- One other plot besides a scatterplot.

You don’t have to use all the data in every plot! It’s fine to filter down to one country or a small handful of countries.
