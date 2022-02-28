---
title: matliplot_vs_ggplot
author: aixuexilinling
date: '2021-06-16'
slug: matliplot-vs-ggplot
categories:
  - learn
tags:
  - plot
---

There are many resource online talking about difference library. I find ggplot2 is good and if people is 


```r
datadir =  "C:/Users/zglin/Downloads/"
library('haven')
```

```
## Warning: package 'haven' was built under R version 4.0.5
```

```r
library('dplyr')
```

```
## Warning: package 'dplyr' was built under R version 4.0.5
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
six_city_orig<- read_sas(paste0(datadir, "fev1.sas7bdat"))
names(six_city_orig)<- tolower(names(six_city_orig))
head(six_city_orig)
```

```
## # A tibble: 6 x 6
##      id    ht   age baseht baseage logfev1
##   <dbl> <dbl> <dbl>  <dbl>   <dbl>   <dbl>
## 1     1  1.20  9.34   1.20    9.34   0.215
## 2     1  1.28 10.4    1.20    9.34   0.372
## 3     1  1.33 11.5    1.20    9.34   0.489
## 4     1  1.42 12.5    1.20    9.34   0.751
## 5     1  1.48 13.4    1.20    9.34   0.833
## 6     1  1.5  15.5    1.20    9.34   0.892
```

```r
six_city<- six_city_orig %>% 
 mutate(id= factor(id),
 time= age - baseage,
 age_grp= cut(age, breaks= 0:19,
 labels = 0:18, include.lowest = TRUE),
 age_grp= as.numeric(as.character(age_grp)),
 # time_grp= cut(time, breaks= 0:12,
 # labels = 0:11, include.lowest = TRUE),
 # time_grp= as.numeric(as.character(time_grp))
 time_grp= round(time, 0)
 ) %>% 
 arrange(id, time) %>% 
 group_by(id)
```



```r
six_city<- six_city %>% 
 group_by(age_grp) %>% 
 mutate(logfev1_centred= scale(logfev1, center= TRUE, scale= FALSE)) %>% 
 group_by(id)



six_city<- six_city %>% 
 group_by(age_grp) %>% 
 mutate(logfev1_centred= scale(logfev1, center= TRUE, scale= FALSE)) %>% 
 group_by(id)
```



```r
library('nlme')
```

```
## 
## Attaching package: 'nlme'
```

```
## The following object is masked from 'package:dplyr':
## 
##     collapse
```

```r
library(tidyr)
ols_mdl<- gls(logfev1 ~ age_grp, 
 correlation = corAR1(value= 0, form = ~ 1 |id, fixed= TRUE),
 data= six_city, 
 na.action= na.exclude)
```



```r
tmp_d<- six_city %>% 
 bind_cols(resid= resid(ols_mdl)) %>% 
 arrange(id, time)
```


```r
generate_variogram_d<- function(df, tvar, yvar) {
 require(rlang)
 
 # idvar<- enquo(idvar)
 tvar<- enquo(tvar)
 yvar<- enquo(yvar)
 # time_dif and sqr_dif are symmetric matrices
 time_dif<- outer(df[[quo_name(tvar)]], df[[quo_name(tvar)]], "-")
 sqr_dif <- outer(df[[quo_name(yvar)]], df[[quo_name(yvar)]], 
 function(x, y) .5*(x-y)^2 )
 # so that we do not have divide by 2 later
 idx<- row(time_dif)>col(time_dif)
 out<- data_frame(t_dif= time_dif[idx], sqr_dif= sqr_dif[idx]) %>% 
 arrange(t_dif)
 out
}
```







