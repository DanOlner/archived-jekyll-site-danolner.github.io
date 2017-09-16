---
layout: post
title: "English House prices - how have they changed compared to median wages"
date: 2017-09-16
comments: true
---






This follows on from the write-up of the [one day training course](http://danolner.github.io/2017/09/Learn_GGPLOT_and_R_using_English_house_price_and_wage_data) 
for visualising data in R and ggplot. All the data and links are over there. Here, I just want to say...

I was pretty startled by how house prices have changed when compared to median wages. House prices are raw - without adjusting, their value over time is not really comparable. Adjusting them just for straight inflation can be misleading as house prices aren't actually included in that. A better measure (I think) is 'cost of housing as a multiple of wages'. This also allows for a more geographically interesting picture. [NOMIS](https://www.nomisweb.co.uk/) median wage data is available yearly for each local authority and goes back to 1997, so you can build a house price index for each local authority up to 2016. (E.g. 'this house is worth four times the yearly median wage here.')

The graph in the [previous post](http://danolner.github.io/2017/09/Learn_GGPLOT_and_R_using_English_house_price_and_wage_data) shows this index for the top and bottom five local authorities (from a rank of 2016 wage multiples). What it **doesn't** include is the following...

**Note: I've pre-selected a sub-set of local authorities for the course - London would dominate the top values if all local authorities were included.**

![center](http://danolner.github.io/figs/housePriceWageMultiple/unnamed-chunk-2-1.png)

I'd excluded Kensington and Chelsea - it's so far above the other values, it makes them rather unreadable in this plot. (We did look at Kensington/Chelsea in another section, so it was covered.) 

But it's pretty amazing, huh? The average house there has gone from just under twenty times the median wage to not far off **seventy times** now. Good God. Part of the reason is shown if you just plot the wage multiple against the median wage itself:

![center](http://danolner.github.io/figs/housePriceWageMultiple/unnamed-chunk-3-1.png)

Kensingon and Chelsea is out there on the right by itself: actually not that high a median wage but by far the largest wage multiple for housing. So what this is likely showing: as well as having some of the priciest housing, it also has many lower-waged people. It's a highly mixed area. In some imaginary future (I'm sure this would *never* happen / isn't happening...) where much of the area's social housing tenants had moved elsewhere, the index would actually start to drop.

The NOMIS data actually contains every wage percentile - it's very gappy for 90th percentile as they don't include numbers that are not statistically reliable and there aren't enough people in that category. At any rate - looking at other percentiles might provide a deeper look at what's going on.

The lowest-value areas (easier to see in the minus-Kensington graph in [the other post](http://danolner.github.io/2017/09/Learn_GGPLOT_and_R_using_English_house_price_and_wage_data)) have certainly seen the gap increase - something like five times to ten times the median wage over the data period. But there's also a dynamic that I've seen in data at local scales too: after the crash, the poorest areas have seen house values **decline** (even relative to wages, it appears) where the richer areas have continued on up. For some of the richest, the crash looks like little more than a dink in that.

There are some other slightly puzzling areas with very high median wages and low house values. I'm a little suspicious of the data for those, but more digging also required...

All data / methods and tutorials on using it available in the [day course post](http://danolner.github.io/2017/09/Learn_GGPLOT_and_R_using_English_house_price_and_wage_data)!
