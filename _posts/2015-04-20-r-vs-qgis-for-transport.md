---
layout: post
title: R vs QGIS for sustainable transport planning
categories:
 - R
tags:
 - open source
 - GIS
 - maps
---

The 23rd iteration of the GIS Research UK conference
([#GISRUK](https://github.com/Robinlovelace/stplanr))
conference was the largest ever. 250 researchers,
industry representatives and academics attended from the
vibrant geospatial research communities in the
UK, Europe and beyond. GISRUK has become a centrepoint for discussion
of new methods, software and applications in the field. I
was on the [organising committee](http://leeds.gisruk.org/contact.html),
reviewed some excellent papers for the event
(a full list of these is
[available for download here](http://leeds.gisruk.org/programme.html)) and 
attended some truly ground-breaking talks.
This experience has shown that the geospatial community
in the UK is strong,
especially with regards to growth in open access
data and open source software in the field.

<a href="https://www.flickr.com/photos/97888609@N02/17025120518" title="img_2814 by Robin Lovelace, on Flickr"><img src="https://farm6.staticflickr.com/5450/17025120518_ebfba47bff_s.jpg" width="75" height="75" alt="img_2814"></a>

<!--more-->

This article is about one part of GISRUK and insights gleaned from it
about R, QGIS and other tools for sustainable transport planning.
[GIS for Transport Applications](https://github.com/Robinlovelace/GIS4TA)
([#GIS4TA for short](https://twitter.com/hashtag/gis4ta?src=hash)) was a practical day-long workshop
that preceded the main event.
I organised the workshop and (with help from
[Eusebio Odiari](http://www.geog.leeds.ac.uk/people/e.odiari),
[The Transport Geography Research Group](https://tgrg.wordpress.com/)
and the
[Royal Geographical Society](http://www.rgs.org/HomePage.htm))
it seems to have been a great success. More than 30
people attended, including a decent portion
from transport consultancies such as
[Integrated Transport Planning Ltd](http://www.itpworld.net/index.html)
[TRP Consulting](http://www.trpconsult.com/) and the
[European Railway Association (ERA)](http://www.era.europa.eu/Pages/Home.aspx).
Specifically, it is about the use of R
and QGIS tools for transport planning and the potential for their
adoption in academic, public and private-sector transport planning.
The focus of the workshop was deliberately on open source software
and sustainable transport because these are growth
areas in the field that are essential for
democratic
and healthy
transport systems compatible with the
science of climate change (<a href="http://www.opentraffic.net/en/"><span class="citation">Tamminga, 2012</span></a>).
A recent report, for example, suggests we need to almost completely
transition away from fossil fuels by 2050
([McGlade et al., 2015](http://dx.doi.org/10.1038/nature14016)).
New datasets and methods for analysing and modelling them can
help get us there in the recalcitrant transport sector
([Gossling, 2014](http://dx.doi.org/10.1016/j.jtrangeo.2014.07.010)).

# R for transport applications

The workshop kicked-off with a short
talk on 'R and QGIS for transport applications',
which laid out some of the motivations for running the
workshop outlined above. Other than a few
'early adopters', the transport modelling community
is generally conservative, based largely on mature
proprietary products such as
[SATURN](http://www.saturnsoftware.co.uk/7.html)
and 
[Vissim](http://vision-traffic.ptvgroup.com/en-us/products/ptv-vissim/).

The slides from this talk are available here:

<script async class="speakerdeck-embed" data-id="91fce9cf5c36405b8969c6b6954c4fe6" data-ratio="1.41436464088398" src="//speakerdeck.com/assets/embed.js"></script>

**Tutorial: [Introduction to R and QGIS for transport applications](https://github.com/Robinlovelace/GIS4TA/releases/download/0.1/intro-r-qgis-4ta.pdf)**

# Routing with R

The second talk was by Nick Bearman, who provided an overview of
routing in R, as well as an excellent
[practical tutorial](https://github.com/nickbearman/transport-workshop/blob/master/transport-workshop.pdf).

The practical demonstrated 2 ways of routing in R:

1. Using **ggmap**. The following code was used to navigate to the event!


```
from <- 'Leeds station, New Station Street, Leeds LS1 5DL, United Kingdom'
to <- 'LS2 9JT'
route_df <- route(from, to, structure = 'route', mode = 'walking')
```

```
qmap('Merrion Centre', zoom = 15) +
  geom_path(
    aes(x = lon, y = lat),  colour = 'red', size = 1.5,
    data = route_df, lineend = 'round')
```


![](https://github.com/Robinlovelace/robinlovelace.github.io/raw/master/_posts/writeup_files/figure-html/unnamed-chunk-2-1.png) 

2. The [package I created, **stplanr**](https://github.com/Robinlovelace/stplanr),
to get routes optimised for cyclists (see [transport-workshop.Rmd](https://github.com/nickbearman/transport-workshop/blob/master/transport-workshop.Rmd) for a working version):


```
rquiet <- gLines2CyclePath(l = rlines, plan = "quietest")
plot(rquiet[1,]) # route from Leeds station to Leeds University (North - South)
plot(rquiet[2,]) # route from Leeds to Manchester!
```

![](https://github.com/Robinlovelace/robinlovelace.github.io/raw/master/_posts/writeup_files/figure-html/unnamed-chunk-4-1.png) 

**Tutorial: [Route analysis using R](https://github.com/nickbearman/transport-workshop/blob/master/transport-workshop.pdf)**

# Large gps datasets with PostGIS

The most technical session involved using R to query huge datasets storing
GPS data containing 100,000+ rows. Amazingly, Richard and Adrian Ellison
set up a remotely accessible database instance **from their laptop** which
participants queried via
[**RPostgreSQL**](http://cran.r-project.org/web/packages/RPostgreSQL/index.html).
Their session information can be seen here:

[**github.com/richardellison/GIS4TA_GPS**](https://github.com/richardellison/GIS4TA_GPS)

# A hackathon

Finally there was a miniature hackathon organised by Godwin Yeboah.
Participants made progress in better understanding the travel
patterns of cyclists using real data. The hackathon notes can be found here:

[github.com/spatialscientist/GIS4TA2015](https://github.com/spatialscientist/GIS4TA2015)

# Summary

GIS is a field of knowledge that has a huge amount to offer transport 
planners and researchers, especially regarding new and open source
software tools that can effectively generate, process and analyse 
transport-related data. R is well-suited to fill this research gap and
has a wide range of tools to help. Packages such as **ggmap**
(Kahle and Wickham 2013),
**RPostgreSQL**
and the new [**stplanr**](https://github.com/Robinlovelace/stplanr)
have great potential to help plan the transport systems of the future.
QGIS is also increasingly attractive for transport applications, with
it inbuilt support for
[PGRouting](http://planet.qgis.org/planet/tag/pgrouting/), 
[flow analysis](http://plugins.qgis.org/plugins/FlowMapper/)
and a friendly user interface that many will be used to.

Photos taken during the hackathon are testament to its role as a forum
for not only learning but also debate about the future of GIS in transport.
These can be seen here:

[flickr.com/photos/97888609@N02/sets/72157652012715766](https://www.flickr.com/photos/97888609@N02/sets/72157652012715766)

Hearing feedback from users new to R using it to solve transport problems
provided an insight into how it compares to traditional tools. The removal
of 'glass ceilings' imposed by restrictive licenses or the need to buy
'add-on' features was one comment, but that applies equally to QGIS and
other [FOSS4G](http://foss4g.org/)
offerings. The steep learning curve of R seems to still
be an issue compared with QGIS, although this is becoming less of an issue
with the evolution of RStudio as an GUI for R. In conclusion, both R and
QGIS are coming of age as tools in the transport planner's 'war cabinet'.
The latest evidence unequivocally shows the impact of transport decisions
on obesity, environmental degradation and quality of life. So it is time,
surely, to harness this new open source software to 'save the world'!

# Acknowledgements

Thanks to the Consumer Data Research Centre and the Royal Geographical
society for subsidising the event. Thanks to all the participants and
especially the demonstrators Godwin, Nick, Adrian and Richard for making it
happen.

# References

Gössling, Stefan, and Scott Cohen. 2014. “Why sustainable transport policies will fail: EU climate policy in the light of transport taboos.” Journal of Transport Geography 39 (July). Elsevier Ltd: 197–207. doi:10.1016/j.jtrangeo.2014.07.010.

Kahle, D, and Hadley Wickham. 2013. “ggmap: Spatial Visualization with ggplot2.” The R Journal 5: 144–61. [citeseerx.ist.psu.edu](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.375.8693&rep=rep1&type=pdf).

McGlade, Christophe, and Paul Ekins. 2015. “The geographical distribution of fossil fuels unused when limiting global warming to 2 °C.” Nature 517 (7533). Nature Publishing Group: 187–90. doi:10.1038/nature14016.

Tamminga, Guus, Marc Miska, Edgar Santos, Hans van Lint, Arturo Nakasone, Helmut Prendinger, and Serge Hoogendoorn. 2012. “Design of Open Source Framework for Traffic and Travel Simulation.” Transportation Research Record: Journal of the Transportation Research Board 2291 (-1): 44–52. doi:10.3141/2291-06.
