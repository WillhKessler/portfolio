---
title: "Parallel Raster Extraction To Points using R and SLURM"
excerpt: "Utilizing a computing cluster to extract raster values to points"
header:
  # image: /assets/images/foo-bar-identity.jpg
  teaser: /assets/images/GDMBR_portfolioclip2.png
sidebar:
  - title: "The Great Divide Mountain Bike Route"
    text: "Fullsize 12 x 36 inches, 800dpi"
  - title: "Tools and Techniques"
    text: "ArcGIS Pro- mapping and layout
    R- elevation profile creation
    route data from personal GPS data collection
    Single-color mapping, pervasive scale technique to indicate scale across the map"
    image: assets/images/White_12x36_portfolio_resize_v2.png
    image_alt: "Reduced resolution full-size GDMBR map"
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

READ ME:
The principal code provided in this repository is a workflow for using the 'terra' R package to extract raster data to points or polygons based on specific time intervals. It was originally written to assist in linking public health cohort data to environmental exposure data in the form of daily or monthly raster data. A user supplies a CSV, SHAPEFILE, or GDB feature class containing point or polygon features with start and end observation dates, and a set of raster layers with the file name ending with an observation date. Using the R parallel processing package, `batchtools` a processing job is created for each feature:variable combination with their corresponding start and end dates. These jobs are then submitted to the cluster utilizing the SLURM job manager. The internally supplied raster functions use the job information regarding features, raster variable and date ranges to identify which rasters fall within a given input feature's observation period and extracts the raster values corresponding to the feature's location. For Polygon features, a weighting raster can be supplied to generate a weighted average within the feature, e.g. a population-weighted average temperature exposure within a census tract. Care should be taken to ensure that the input features and raster data are in the same coordinate reference system as those transformations are not handled here. This workflow is implemented with `batchtools` in R to run in parallel on a compute cluster using the SLURM job manager, or in parallel or serial locally. 

