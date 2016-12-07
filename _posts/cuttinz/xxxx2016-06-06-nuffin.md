---
layout: post
title: "nuffin"
date: 2016-06-06
comments: true
category: tech
---

# Testing Jekyll 3

And here is some R code. This'll work, right?


~~~~~~
asdbob <- 563
~~~~~~

```r
agents$zone <- ifelse(agents$moving == 1,#If I decided to move...
                        ifelse(agents$prob==0,#Move based on my preference of zone (or no preference)
                               even[floor(runif(n = nrow(agents[agents$prob==0,]), min=1, max=length(even)+1))],
                               bias[floor(runif(n = nrow(agents[agents$prob==1,]), min=1, max=length(bias)+1))]),
                        agents$zone
  )
```

{% highlight r %}
#Store of time series data (to match how table gets converted to dataframe for rbinding)
#Time is iteration
store <- data.frame(zone = as.integer(), 
                    agent_type = as.integer(),
                    number_of_agents = as.integer(),
                    time = as.integer())
{% endhighlight %}

