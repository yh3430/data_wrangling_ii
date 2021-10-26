Reading Data From the Web
================
Yu He
10/26/2021

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.4     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.1     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## NSDUH data

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = 
  read_html(url)

drug_use_df =
  drug_use_html %>% 
  html_table() %>% 
  first() %>% 
  slice(-1)
```

## Star wars

Get some star wars data.

``` r
sw_url = "https://www.imdb.com/list/ls070150896/"

sw_html = 
  read_html(sw_url)

sw_titles =
  sw_html %>% 
  html_elements(".lister-item-header a") %>% 
  html_text()

sw_revenue = 
  sw_html %>% 
  html_elements(".text-small:nth-child(7) span:nth-child(5)") %>% 
  html_text()


sw_df =
  tibble(
    title = sw_titles,
    revenue = sw_revenue
  )
```

## Napoleon dynamite

Dynamite reviews

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = 
  read_html(dynamite_url)

dynamite_review_titles = 
  dynamite_html %>% 
  html_elements(".a-text-bold span") %>% 
  html_text()

dynamite_stars = 
  dynamite_html %>% 
  html_elements("#cm_cr-review_list .review-rating") %>% 
  html_text()

dynamite_df =
  tibble(
    titles = dynamite_review_titles,
    stars = dynamite_stars
  )
```
