---
layout: post
title:  "Where oh, where can I park my bike"
date:   2022-03-13
categories: soc
---

I have to share this *win* from idea to interactive map in days.

Just last Thursday, I was going to meet someone for coffee. It wasn't the best of weather days, but I had time, so I considered biking to the coffee shop and then lazily making my way back. 

However, I did a little recon before going. I looked around with Google Street View, and I did not find a place to park my bike. I didn't want to just kickstand it and go inside. That works if you are going to keep an eye on it, but what if you are actually listening to the person talking to you in a conversation? 

That got me thinking - dangerous, I know - that I'm not sure where I can park my bike. Like, I'm not sure where I could plan to "go" and do anything if I wanted to go by bike. _(ignoring the places that I already know I can go and do stuff)_

I raised the question on twitter and I'm not alone. 

So, I thought I could just, like build the resource that I want. I came up with 2 places off the top of my head: Sprouts and the post office in Silverdale. I've parked my bike at both locations - lucky me that those where places where I assumed there would be parking, and there was. 

Someone told me about another. I quickly threw together a github repo, because that is an easy way to publish data quickly and collaborate. 

On Saturday, I kept my eyes peeled for bike racks and took a few pictures. 

By Saturday night, there was a map - but it was only interactive on my own computer. I posted a screenshot on github in the readme. 

Today, I started down the path of using github pages to post an r blog that could keep the map updated. I had to reinstall ruby; it was a whole thing. But, I realized that a faster way would just be to through the map up on RPubs. The whole point was to keep me from spending time buildint another stupid website. 

On the plus side, the interactive map is up on RPubs. On the downside, there is no way to update/refresh RPubs, so I have to delete it and repost a "new document" at RPubs every time I make a change. 

I don't think it will be that big of a deal. And, done is sooooo much better than in my head. 

It's dead simple code. You can see the interactive map on RPubs [kitsap-bike-parking](https://rpubs.com/monkeywithacupcake/kitsap-bike-parking)

{% highlight r %}
library(tidyverse)
dpath <- "https://raw.githubusercontent.com/monkeywithacupcake/kitsap-bike-parking/main/kitsap-bike-parking.csv"
ipath <- "https://raw.githubusercontent.com/monkeywithacupcake/kitsap-bike-parking/main/img/"

bikeparking <- read_csv(dpath)

df <- bikeparking %>%
  mutate(CIMG = if_else(!is.na(IMG), IMG, "no_img.png"),
         POPUP  = paste(sep = "<br/>", 
                        paste0("<b>",NAME,"</b>"),
                        paste("Variant:", VARIANT),
                        paste("Covered?:", COVERED),
                        paste0("<img src = '", ipath, CIMG, "' width = 200>") #img
                        )
         )
library(leaflet)
leaflet(df) %>% addTiles(
  options = tileOptions(minZoom = 9, maxZoom = 17)
) %>%
  addMarkers(lng = ~LNG, lat = ~LAT, 
             #popup = ~POPUP,
             clusterOptions = markerClusterOptions()
             ) %>%
  addCircleMarkers(lng = ~LNG, lat = ~LAT, color = "#301934",
                   #label = ~NAME, 
                   radius = 7,  fillOpacity = 0.7, fillColor = "#D8BFD8",
                   popup = ~POPUP) 

{% end highlight %}