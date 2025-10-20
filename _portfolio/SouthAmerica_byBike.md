---
title: "South America by Bike"
excerpt: "A commemoration of a 2017 cycling trip from Puerto Escondido, Colombia to Peru. Watercolors"
header:
  image: /assets/images/foo-bar-identity.jpg
  teaser: assets/images/GDMBR_portfolioclip2.png
sidebar:
  - title: "South America by Bike"
    text: "Fullsize 12 x 36 inches, 800dpi"
  - title: "Tools and Techniques"
    text: "ArcGIS Pro- mapping and layout
    R- elevation profile creation
    route data from personal GPS data collection
    Single-color mapping, pervasive scale technique to indicate scale across the map"
    image: assets/images/White_12x36_portfolio_resize_v2.png
    image_alt: "Reduced resolution full-size SA by bike map"
gallery:
  - url: /assets/images/GDMBR_portfolioclip2.png
    image_path: assets/images/GDMBR_portfolioclip2.png
    alt: "Zoomed in view of Montana region of GDMBR"
  - url: /assets/images/White_12x36_portfolio_resize_v2.png
    image_path: assets/images/White_12x36_portfolio_resize_v2.png
    alt: "Reduced resolution full-size GDMBR map"
  - url: /assets/images/unsplash-gallery-image-3.jpg
    image_path: assets/images/unsplash-gallery-image-3-th.jpg
    alt: "placeholder image 3"
---

The Great Divide Mountain Bike Route (GDMBR) is arguably the most famous long-distance off-road cycling route in the world. First pioneered by the non-profit Adventure Cycling Association in 1998, the route remains almost entirely unpaved along its roughly 2700 mile length from Banff, Canada to Antelope Wells, New Mexico. The route climbs nearly 200,000 feet of elevation as it criss-crosses the Continental Divide, and traverses the national treasures that are US and Canadian public lands. 

This cartography project was an exploration of single-color, large-format mapping stemming from the author's personal cycling trip across the United States on the GDMBR in 2022. The monochromatic color scheme and limited features draws the eye to the magnitude of the adventure being presented. One of the primary highlights of the GDMBR beyond it's magnitude is the celebration of the US and Canadian public lands which are also highlighted here.  



{% include gallery caption="Examples of the GDMBR map" %}

This map drew inspiration from <a href="https://alexhotchin.com/">Alex Hotchin's</a> beautiful hand drawn and watercolored maps. While Alex's style defies reproduction, I thought a watercolor style map would perfectly complement the raw imperfection that was this trip.  
For the Route data, I used ArcPro's 'GPX to Feature' tool and the editing toolbox to create and stitch together line features from daily gps tracks (gpx files) collected using a Garmin cycling computer during the trip. 
Elevation data came from ESRI's world atlas global elevation dataset, and public lands data was sourced from the Protected Area Database Inventory (PADI) and other sources.
To personalize the map and commemorate my ride, I chose to indicate the campsites I used along way as labeled waypoints. 

