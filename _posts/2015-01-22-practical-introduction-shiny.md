---
layout: post
title: Practical introduction to Shiny - workshop write-up
categories:
 - R
tags:
 - open source
 - GIS
 - maps
---

I recently delivered a workshop on a practical introduction to
[shiny](http://shiny.rstudio.com/), an R package that enables development,
testing and deployment of interactive web applications.
Delivered at the University of Sydney's Institute for Transport and Logistics
Studies ([ITLS](http://sydney.edu.au/business/itls)), it was designed for people
who are a) fairly new to R (which can seem intimidating) and b) completely new to
shiny.

This article provides resources for people wanting to apply shiny to
real-world applications and some context which explains the motivations
behind running the workshop. The pdf tutorial, example code
to create and modify your own apps and a place to contribute to this
free teaching resource is available at the following GitHub repository:
[github.com/Robinlovelace/learning-shiny](https://github.com/Robinlovelace/learning-shiny)

<!--more-->

## My first shiny app

My first shiny app was [rentSplit](https://robinlovelace.shinyapps.io/rentSplit/),
which overcomes the problem faced when people move in together but can't decide
on who should pay what because some rooms are better than others. (I would
like to add 'income' to this app to allow for minted people to pay more!):

<iframe src="https://robinlovelace.shinyapps.io/rentSplit/" style="border: none; width: 600px; height: 800px"></iframe>

The relatively painless experience of creating this app made me wonder if shiny
could be used in transport policy applications and help us achieve the transition
away from [fossil fuels](http://www.postcarbon.org/) that 
[scientists say is needed to avoid dangerous climate change](http://www.nature.com/nature/journal/v517/n7533/full/nature14016.html). 

## Context

To put shiny in the context of transport planning I began with a talk on the 
motivations behind online visualisation tools: the increased
focus on '[impact](http://www.esrc.ac.uk/funding-and-guidance/impact-toolkit/)'
in academic funding decisions, the need for public engagement and the
democratisation of decision-making processes, as illustrated by the
interactive [web map](http://www.cyclecityconnect.co.uk/participate.php) created by transport consultancy Steer Davies Gleave
([SDG](http://www.steerdaviesgleave.com/)) to enable geolocated online discussion
of a new £20 million [cycle path](http://www.cyclecityconnect.co.uk/)
that will connect the English cities of [Leeds](http://leeds.gisruk.org/)
and [Bradford](http://en.wikipedia.org/wiki/Bradford):

![cityconnect](http://robinlovelace.net/figure/cconnect.png)

This is a fantastic example of the new levels of engagement unleashed by
online digital technologies such as [Leaflet](http://leafletjs.com/)
and [shiny](http://www.r-bloggers.com/search/shiny): for the first time ever,
people can voice their opinions and pinpoint them to a specific place on the map
in a real-time discussion. The potential for improving transparency and
accountability in transport decision making is huge, as I described
in a short [article on web-mapping](http://robinlovelace.net/software/2014/03/05/webmap-test.html) and as outlined in
an academic paper on the subject
([Schulz and Newig, 2014](http://onlinelibrary.wiley.com/doi/10.1002/eet.1655/full)).

As stated in the [talk](http://rpubs.com/RobinLovelace/54614),
the [audio](https://soundcloud.com/robin-lovelace-1/r-and-shiny-for-transport-applications)
of which is available online (see below),
this is a fast-moving area that is likely to grow rapidly in the coming years.
So it's worth knowing what's out there and what is possible at present.

<iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/187078099&amp;color=ff5500&amp;auto_play=false&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false"></iframe>

The [practical](https://github.com/Robinlovelace/learning-shiny/raw/master/learning-shiny.pdf) avoids [reinventing the wheel](http://en.wikipedia.org/wiki/Reinventing_the_wheel)
by building on and creating a narrative for [RStudio's excellent tutorial and documentation of shiny](http://shiny.rstudio.com/tutorial/). For example, the
[himod](https://robinlovelace.shinyapps.io/himod/) shiny app is simply a modified
version of the '[01_hello](http://glimmer.rstudio.com/shiny/01_hello/)'
demonstration app used to illustrate how new
[widgets](http://shiny.rstudio.com/tutorial/lesson3/) are created and how to
use their id's as objects (via `input$id`) in server.R:

<iframe src="https://robinlovelace.shinyapps.io/himod/" style="border: none; width: 600px; height: 650px"></iframe>

So please take a look at the resources generated for this practical. I'd
be interested to hear peoples' thoughts on online interactive tools for
transport planning and the
[digital democracy](http://theconversation.com/digital-democracy-lets-you-write-your-own-laws-21483) agenda. More specifically, what's the simplest way to get
an interactive map on shiny, as illustrated by the impressive
[map of the USA in the SuperZip shiny Gallery example](http://shiny.rstudio.com/gallery/superzip-example.html)?

## Resources

- An evolving [tutorial](https://github.com/Robinlovelace/learning-shiny/raw/master/learning-shiny.pdf) (which is may contain some errors)
- A [GitHub repo](https://github.com/Robinlovelace/learning-shiny) where you can access the source code for this tutorial and contribute to it for the community if you wish.
- The [slides from my talk](http://rpubs.com/RobinLovelace/54614), complete with an
[audio recording](https://soundcloud.com/robin-lovelace-1/r-and-shiny-for-transport-applications) of the introduction to it.
