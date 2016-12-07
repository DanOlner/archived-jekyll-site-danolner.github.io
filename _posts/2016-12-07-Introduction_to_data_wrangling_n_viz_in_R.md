---
layout: post
title: "Introduction to data wrangling n viz in R"
date: 2016-12-07
comments: true
---

# Introduction to R

I just ran an introduction-to-R one day workshop here at the [Sheffield Methods Institute](https://www.sheffield.ac.uk/smi) on behalf of [AQMeN](http://aqmen.ac.uk/). The aim was to introduce people with no previous experience of R to RStudio, data wrangling with the various [tidyverse libraries](https://blog.rstudio.org/2016/09/15/tidyverse-1-0-0/) and outputting some plots/visualisations with [ggplot](http://docs.ggplot2.org/current/).

I'll be updating the course based on experience of this first one, but if you want a go, the course booklet is written assuming no previous knowledge and should, in theory, work for self-learning. It's all based on open access data so it's available to everyone. If you're interested:

* go [pick a city from here](http://bit.ly/cityzips). Each is a zipped up RStudio project with that city/town's travel-to-work area plus London.

* Make sure you've got [RStudio](https://www.rstudio.com/products/rstudio/download3/) and [R installed](https://www.r-project.org/).

* Look in the **course_materials** folder of your unzipped RStudio project: there's a PDF there (and an HTML doc) with the course in. (Or the HTML one is [online here](https://danolner.github.io/intro2wranglingNvizInR/).) It'll explain how to open it in RStudio and get going.

The booklet runs through a typical data wrangling scenario using [Land Registry 'price paid' data](https://www.gov.uk/government/collections/price-paid-data): a record of every property sale in England and Wales since 1995. It also does some data linkage, getting geographies from Ordnance Survey [Code-Point Open](https://www.ordnancesurvey.co.uk/opendatadownload/products.html) data that gives precise locations for Great Britain postcodes. (CodePoint Open is some way down the list on that page.)

Any comments/suggestions, do let me know (d dot older at sheffield dot ac dot uk).

* **Note**: the course uses data I've already got into a rather more useable form, to keep the focus on the essentials. If you want to know how to get from the original downloaded data, take a look at the [preparation code](https://github.com/DanOlner/intro2wranglingNvizInR/blob/master/prepCode/firstRunThrough.R). Though it ain't pretty... which in itself is salutory: writing this stuff up can present a sanitised version of coding reality that appears way more ordered than it really is.

* **Note note**: the full data prep code includes an explanation of how the postcodes were used to geocode geographies into a lookup, so serve as a real-data intro to geocoding in R. This code uses two (also open access) zone shapefiles: [Travel-to-work areas (TTWAs)](http://webarchive.nationalarchives.gov.uk/20160105160709/http://www.ons.gov.uk/ons/guide-method/geography/beginner-s-guide/other/travel-to-work-areas/index.html) and ward shapefiles for England from [the Census Edina boundary data download site](https://census.edina.ac.uk/). (TTWAs are in 'England / Other / 2001-2010, wards are from England / Census / English Census Merged Wards 2011.)

<img src="http://danolner.github.io/figs/intro2R_writeUp/unnamed-chunk-2-1.png" title="center" alt="center" style="display: block; margin: auto;" width = "650" />

