---
title: "Visualizing Text and Distributions"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---

# Data Visualization Project 03


In this exercise you will explore methods to visualize text data and practice how to recreate charts that show the distributions of a continuous variable. 


## Part 1: Density Plots

Using the dataset obtained from FSU's [Florida Climate Center](https://climatecenter.fsu.edu/climate-data-access-tools/downloadable-data), for a station at Tampa International Airport (TPA) from 2016 to 2017, attempt to recreate the charts shown below


```r
library(viridis)
library(tidyverse)
weather_tpa <- read_csv("https://github.com/reisanar/datasets/raw/master/tpa_weather_16_17.csv")
# random sample 
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 x 6
##    year month   day precipitation max_temp min_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>
## 1  2016     2    26          0          62       49
## 2  2016    11    15          0          75       59
## 3  2016    12    23          0          82       60
## 4  2016     6     5          0.77       89       76
```

See https://www.reisanar.com/slides/relationships-models#10 for a reminder on how to use this dataset with the `lubridate` package for dates and times.


(a) Recreate the plot below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_facet.png" width="80%" style="display: block; margin: auto;" />

Hint: the option `binwidth = 3` was used with the `geom_histogram()` function.


```r
weather_tpa %>%
  ggplot(aes(x=max_temp, fill=month)) +
  geom_histogram(binwidth = 3) +
  labs(x="Maximum temperatures", y="Number of days") +
  theme_bw() +
  theme(legend.position = "none")+
  facet_wrap(~ fct_reorder(month.name[month], month)) +
  scale_fill_viridis(option = "viridis")
```

![](wang_project_03_files/figure-html/unnamed-chunk-3-1.png)<!-- -->


(b) Recreate the plot below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_density.png" width="80%" style="display: block; margin: auto;" />

Hint: check the `kernel` parameter of the `geom_density()` function, and use `bw = 0.5`.



```r
weather_tpa %>% 
  ggplot(aes(x=max_temp)) +
  geom_density(bw=0.5, size=1, fill="gray46", position = "identity", kernel="epanechnikov") +
  geom_hline(yintercept=0, colour="white", size=1)+
  labs(x="Maximum temperature", y="density") +
  theme_bw() +
  theme(panel.border = element_blank()) 
```

![](wang_project_03_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


(c) Recreate the chart below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_density_facet.png" width="80%" style="display: block; margin: auto;" />

Hint: default options for `geom_density()` were used. 


```r
weather_tpa %>% 
  ggplot(aes(x=max_temp, fill=month, alpha=0.2)) +
  geom_density(size=1) +
  labs(x="Maximum temperature", y="", title="Density plots for each month in 2016") +
  facet_wrap(~ fct_reorder(month.name[month], month)) +
  scale_fill_viridis()+
  theme_bw() +
  theme(legend.position = "none")
```

![](wang_project_03_files/figure-html/unnamed-chunk-7-1.png)<!-- -->



(d) Recreate the chart below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_ridges.png" width="80%" style="display: block; margin: auto;" />

Hint: default options for `geom_density()` were used. 

```r
library(ggridges)
weather_tpa %>% 
  ggplot(aes(x=max_temp, y=fct_reorder(month.name[month], month), fill = month)) +
  geom_density_ridges(rel_min_height = 0, scale=2, size=1, quantile_lines=T, quantiles=2) +
  scale_fill_viridis(option="viridis") +
  labs(x="Maximum temperature", y="") +
  theme_bw() +
  theme(legend.position = "none", panel.border = element_blank()) 
```

```
## Picking joint bandwidth of 1.49
```

![](wang_project_03_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


(e) Recreate the plot below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_ridges.png" width="80%" style="display: block; margin: auto;" />

```
## Picking joint bandwidth of 1.49
```

<img src="wang_project_03_files/figure-html/unnamed-chunk-10-2.png" width="80%" style="display: block; margin: auto;" />

Hint: use the`ggridges` package, and the `geom_density_ridges()` function paying close attention to the `quantile_lines` and `quantiles` parameters.




(f) Recreate the chart below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_ridges_plasma.png" width="80%" style="display: block; margin: auto;" />

Hint: this uses the `plasma` option (color scale) for the _viridis_ palette.

```r
weather_tpa %>% 
  ggplot(aes(x=max_temp, y=fct_reorder(month.name[month], month), fill = stat(x))) +
  geom_density_ridges_gradient(rel_min_height = 0, scale=2, size=1, quantile_lines=T, quantiles=2) +
  scale_fill_viridis_c(option = "C") +
  labs(x="Maximum temperature", y="", fill="") +
  theme_bw() +
  theme(panel.border = element_blank()) 
```

```
## Picking joint bandwidth of 1.49
```

![](wang_project_03_files/figure-html/unnamed-chunk-12-1.png)<!-- -->



## Part 2: Visualizing Text Data

Review the set of slides (and additional resources linked in it) for visualizing text data: https://www.reisanar.com/slides/text-viz#1

Choose any dataset with text data, and create at least one visualization with it. For example, you can create a frequency count of most used bigrams, a sentiment analysis of the text data, a network visualization of terms commonly used together, and/or a visualization of a topic modeling approach to the problem of identifying words/documents associated to different topics in the text data you decide to use. 

Make sure to include a copy of the dataset in the `data/` folder, and reference your sources if different from the ones listed below:

- [Billboard Top 100 Lyrics](https://github.com/reisanar/datasets/blob/master/BB_top100_2015.csv)

- [RateMyProfessors comments](https://github.com/reisanar/datasets/blob/master/rmp_wit_comments.csv)

- [FL Poly News 2020](https://github.com/reisanar/datasets/blob/master/poly_news_FL20.csv)

- [FL Poly News 2019](https://github.com/reisanar/datasets/blob/master/poly_news_FL19.csv)

(to get the "raw" data from any of the links listed above, simply click on the `raw` button of the GitHub page and copy the URL to be able to read it in your computer using the `read_csv()` function)






