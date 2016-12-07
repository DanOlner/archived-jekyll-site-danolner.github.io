---
layout: post
title: "zis still workinz"
date: 2016-11-28
comments: true
---



# Geocoding land registry data

I just ran an introduction-to-R one day workshop here at the [Sheffield Methods Institute](https://www.sheffield.ac.uk/smi) on behalf of [AQMeN](http://aqmen.ac.uk/). The aim was to introduce people with no previous experience of R to RStudio, data wrangling with the various [tidyverse libraries](https://blog.rstudio.org/2016/09/15/tidyverse-1-0-0/) and outputting some plots/visualisations with [ggplot](http://docs.ggplot2.org/current/).

I'll be updating the course based on experience of this first one, but if you want a go, the course booklet is written assuming no previous knowledge and should, in theory, work for self-learning. It's all based on open access data so it's available to everyone. If you're interested:

* go [pick a city from here](http:bit.ly/cityzips). Each is a zipped up RStudio project with that city/town's travel-to-work area plus London.

* Make sure you've got [RStudio](https://www.rstudio.com/products/rstudio/download3/) and [R installed](https://www.r-project.org/).

* Look in the **course_materials** folder of your unzipped RStudio project: there's a PDF there (and an HTML doc) with the course in. It'll explain how to open it in RStudio and get going.

The booklet runs through a typical data wrangling scenario using [Land Registry 'price paid' data](https://www.gov.uk/government/collections/price-paid-data): a record of every property sale in England and Wales since 1995. It also does some data linkage, getting geographies from Ordnance Survey [Code-Point Open](https://www.ordnancesurvey.co.uk/opendatadownload/products.html) data that gives precise locations for Great Britain postcodes. (CodePoint Open is some way down the list on that page.)

*****

I'll post again once the material's all updated and I've put it in a more sensible place. For now, I just wanted to run through what happened to the original data to get it into its course-ready state. I did this to focus on the essentials of data wrangling, without getting bogged down in every single processing step - but if anyone wants to get from original data download through to the data used in the course, here's what happened.

* **Note**: a look at the [messy confusion of my preparation code](https://github.com/DanOlner/intro2wranglingNvizInR/blob/master/prepCode/firstRunThrough.R) is salutory - writing this stuff up can present a sanitised version of coding reality that appears way more ordered than it really is.

* **Note note**: the full data prep includes an explanation of how the postcodes were used to geocode geographies into a lookup, so serve as a real-data intro to geocoding in R. This code uses two (also open access) zone shapefiles: [Travel-to-work areas (TTWAs)](http://webarchive.nationalarchives.gov.uk/20160105160709/http://www.ons.gov.uk/ons/guide-method/geography/beginner-s-guide/other/travel-to-work-areas/index.html) and ward shapefiles for England from [the Census Edina boundary data download site](https://census.edina.ac.uk/). (TTWAs are in 'England / Other / 2001-2010, wards are from England / Census / English Census Merged Wards 2011.)

OK, so: I'm assuming now that the Land Registry, CodePoint-Open and shapefile data has been downloaded to an RStudio project into a subfolder called **data**. The Land Registry file is large: currently it's at ~3.5gb and growing every month as more sales are added. Let's look at how it's processed.

*****

First-up: these are the libraries used throughout:


{% highlight r %}
#Tidyverse
library(readr)
library(tidyr)
library(dplyr)
library(ggplot2)
library(lubridate)
#checking object sizes in memory
library(pryr)
#geocoding
library(maptools)
library(rgeos)
{% endhighlight %}

*****

# 1. Reduce the Land Registry file to a more sensible size


{% highlight r %}
#Use readr (allows us to see the progress of the file being loaded
#as well as actually succeeding in loading it.)
#Just under three minutes to load into memory
landRegistry <- read_csv('data/pp-complete.csv',col_names = F)

#"Explanation of column names" is here:
#https://www.gov.uk/guidance/about-the-price-paid-data#explanations-of-column-headers-in-the-ppd
#So we can give the data more sensible column names from that

#Last but one column is PPD. Only "A"s are definitely private sales.
#       A        B 
#21483148   233445 
table(landRegistry$X15)

landRegistry <- landRegistry[landRegistry$X15=='A',]

#Keep just the columns we want 
landRegistry <- landRegistry %>% select(2,3,4,5,11:14)

#Currently 1.37gb
object_size(landRegistry)

#Sensible names for these variables
names(landRegistry) <- c('price','date','postcode','type','locality','town/city','district','county')

write_csv(landRegistry,'data/price-paids_inc_localities.csv')
{% endhighlight %}

Some explanation. The **landRegistry$X15=='A'** filter: this is getting rid of a bunch of sales that appear to mostly be things like bulk council house sales. These often appear in the data in a way that warps any results: for large blocks of sales, the entire block sale price is in each property row. This massively skews things like price averages. (This initially became apparent when plotting those averages.) See the [column explanation web page](https://www.gov.uk/guidance/about-the-price-paid-data#explanations-of-column-headers-in-the-ppd).

We then just select the useful columns (for our purposes), give them sensible names and save. This is a much smaller file. Actually, for the workshop, none of the the various geographies supplied with the Land Registry data were used, so there was no need to actually keep them.

*****

# 2. Postcode data

the **Code-Point Open** data gives eastings and northings for all Great Britain postcodes - and it's open access. This is a big deal, given how many years people were scraping that information from various not-necessarily-legal sources.

When unzipped, it gives separate CSVs for each [postcode area](https://en.wikipedia.org/wiki/Postcodes_in_the_United_Kingdom#Postcode_area). These need combining - which can be done with two lines in R. Assuming it's been unzipped into the **data** folder as **codepo_gb**. This takes a minute or two:


{% highlight r %}
#https://stat.ethz.ch/pipermail/r-help/2010-October/255593.html
filenames <- list.files(path = "data/codepo_gb/Data/CSV",full.names = T)
postcodes <- do.call("rbind", lapply(filenames, read.csv, header = F))
{% endhighlight %}

The download also includes a list of column names that aren't in the CSVs themselves. These can be applied to the newly combined postcode data:


{% highlight r %}
#Again, we have no column names. But the download from Ordnance Survey tells us what's what:
#In codepo_gb/Doc/Code-Point_Open_Column_Headers.csv
#We can actually load this and apply it directly to the postcodes data
postcode_headerNames <- read_csv('data/codepo_gb/Doc/Code-Point_Open_Column_Headers.csv')

names(postcodes) <- postcode_headerNames
{% endhighlight %}

The workshop only used data for England - this was originally because I was going to compare to index of multiple deprivation data. So here you've got the option of dropping postcodes only for Scotland. There's a **Country_code** field for ease of filtering. Some of the fields are NA, but only for Scottish postcodes. Once only England is kept, they're all fully populated:


{% highlight r %}
unique(postcodes$Country_code)
postcodes <- postcodes %>% filter(Country_code == 'E92000001')
{% endhighlight %}

# 3. Geocoding: attaching zones to the postcode data

Rather than geocoding directly in the Land Registry data, I created a postcode lookup: a dataset matching postcodes to the TTWA and ward their centroid is located in. To start with, using the [maptools](https://cran.r-project.org/web/packages/maptools/index.html) package, load the TTWA and wards shapefiles (here I've stuck these in 'data/shapefiles'). See the comments in the code for the rest of the geocoding process (some more explanation after the code):


{% highlight r %}
WARDS <- readShapeSpatial('data/shapefiles/England_wards_2011_gen_clipped/england_wa_2011_gen_clipped.shp')

TTWAS <- readShapeSpatial('data/shapefiles/England_ttwa_2001/england_ttwa_2001.shp')

#spatialify the postcodes using a copy
postcodes_sp <- postcodes
coordinates(postcodes_sp) <- ~Eastings+Northings

#Tell R they're currently British National Grid
#(We know the coordinates used in these three *are* BNG, so this is safe.)
proj4string(WARDS) <- CRS("+init=epsg:27700")
proj4string(TTWAS) <- CRS("+init=epsg:27700")
proj4string(postcodes_sp) <- CRS("+init=epsg:27700")

#Geocode: find the ward and ttwas these points are over
#This will take some time!
pcode_WARDS <- postcodes_sp %over% WARDS
pcode_TTWAS <- postcodes_sp %over% TTWAS

#Same order as original postcodes dataframe so can just apply directly
#(I'll check that in QGIS... Yup, all good.)
postcodes$ward <- pcode_WARDS$NAME
postcodes$ttwa_name <- pcode_TTWAS$NAME

#save
write_csv(postcodes,'data/codepoint_open_combined_EnglandOnly_WARDS_TTWAs.csv')
{% endhighlight %}










