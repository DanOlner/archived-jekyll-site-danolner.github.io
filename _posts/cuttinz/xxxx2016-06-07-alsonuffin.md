---
layout: post
title: "initial thingyo"
date: 2016-06-06
comments: true
---

# Not sure what to pick on for 

Github's [update to Jekyll 3](https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0) has made Windows-jekylling easy again, after a little gnarly period - and now I've got a backlog of stuff to yabbit about. I'm eight months into a three year post at the [Sheffield Methods Institute](https://www.sheffield.ac.uk/smi)/[Urban Big Data Centre](http://ubdc.ac.uk/) and it's given me a chance to get stuck into some properly challenging coding projects. They've all been wonderfully geographical, and I've had the chance to use stuff for the first time - in particular, Python / in more particular, Python in QGIS. Overall I'm spending most of my time in R but still managing to give some quality time to good old Java (in fact, I don't know how one of the projects would have been possible with Java. I'll get to that in due course.)

Haven't got to the visualisation part yet 

Exciting for me, boring to read. So anyhoo - what I'd like to do now is outline some of the things I've been doing that I hope to write about in more depth in the coming weeks. There are various little findings and ideas along the way I want to get down (for my own benefit, since I keep on having to go back and look at my own code) as well as give more of an overview. And then pick on one of these to wibble about here, so this post has some actual point to it.

Two main projects have been taking up my time (github repo links):

* A [line of sight analysis](https://github.com/DanOlner/viewshed): which houses in Scotland can see wind turbines? Do landscape features block their view - or do other buildings block it? We're just in the process of using the result to analyse the impact of turbines on houses prices. This has been very pleasing: a slightly sprawling tangle of Python/QGIS and R for data processing and preparation, in order to create manageable batches of data for a bespoke Java line-of-sight program (a much faster option than producing full viewsheds).
* [Stitching census geographies together over time](https://github.com/DanOlner/dataStitching): between the 1971 and 2011 censuses, create time series data for consistent geographies. This one's got a particularly deep technical history - we'll see how my approach bears up. (A colleague has also just pointed out to me that there's a Liverpool-based secondary-data project that's been doing something very similar.) 

As both of those projects use house price data (and it's used elsewhere) I've also been working with [Registers of Scotland](https://www.ros.gov.uk/) housing data, creating a single record over 23 years from a couple of different sources. This is the one that sounds easiest - it's probably been the hardest part of the job. Getting under the skin of complex and often messy data like this and getting it into a useable state... yeah, it's been a thing.




