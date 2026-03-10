---
title: "Querying Data from Purple Air Monitors"
excerpt: "Using R and the PurpleAir API to find and download indoor and outdoor air quality data"
classes: wide
header:
  image: /assets/images/foo-bar-identity.jpg
  teaser: /assets/images/foo-bar-identity-th.jpg
sidebar:
  - title: "Tools and Techniques"
    text: "R/RStudio- Script set up and execution  \n 
    R packages: dplyr, httr  \n
    API Access- PurpleAir AQ Sensor Network API"
---
asdfasdf
## Pull PurpleAir sensor history for a list of sensors
Accessing an API and pulling data can be both a huge time-suck initially and save immense amounts of time in the long run. The Purple Air Network is a distributed network of air quality monitors for assessing indoor and outdoor air quality. 
This tutorial will show you how to create a list of all the sensors in a given area, and then access historical data averages for a fixed time window, and save the results to ONE combined file

### Get A Purple Air API Key: 
To access the Purple Air API, you need a unique API key. Keys are used to control access to the API, track usage, and charge accounts for frequent requests. 
Thankfully, Purple Air provides 1 million free credits to allow for testing, small data downloads, and non-commercial use. This will be more than sufficient for grabbing daily data for large areas. 

To create our key, we first need to create a Purple Air developers account. This account is free and will allow you to create an API key to access and download data. Use the link below to create an account:
[https://develop.purpleair.com](https://develop.purpleair.com). 

Once your account is created, you'll need to create API keys. Select the 'API Keys' tab at the top of the developer dashboard, and create a key with teh '+ API Key' button. Save the key string, we'll use it later. 

### Determine your Area of Interest: 
If you want to download data from multiple sensors in a geographic area, you'll need to know the latitude and longitude for the Northwest and Southeast corners of your bounding box. At this time the API is limited to rectangular extents, so if you are interested in a specific geography, you'll need to determine the maximum extent of the area, and subset your returned list of sensors later (not covered here). 

An easy way to do this is with Google Maps. Find your NW corner on the map, click on the map, and write down the returned latitude and longitude values. Repeat for the SE corner. 

Now, we'll take this information and begin querying the API. Remember, the free credits are limited, so try not to rerun your analysis more often than necessary. 

### Accessing the API with R
#### Load necessary R packages
First, we will need to set our R working directory and load the necessary packages. 
* `readr`
* `dplyr` for general data structure manipulations
* `lubridate` for formatting date/time objects
* `httr` for API GET data receipt from the API
* `jsonlite` for parsing the json API response
* `purrr` for dataframe manipulations
 
```
setwd("C:/path/to/your/working/directory/here")
library(readr)
library(dplyr)
library(lubridate)
library(httr)
library(jsonlite)
require('purrr')
```

### Create a list of sensors for your geographic area:
Using the extent from the latitude and longitudes you determined earlier, we are going to determine the proper URL formatting. The easiest way to do this is to pre-format the URL using test call at the bottom of the [Get Sensors Data API guide](https://api.purpleair.com/#api-sensors-get-sensors-data). You'll need to supply your API key and the necessary fields.  
```
# PurpleAir READ API key
READ_KEY <- "XXXXXXXXXXXXXX" #


# Get list of Sensors
# Define your URL call to the API, note the modifiers for latitude and longitude:
sensor_call<-"https://api.purpleair.com/v1/sensors?fields=latitude%2C%20longitude%2C%20position_rating%2Clocation_type%2Clast_seen&modified_since=0&nwlat=38.3712232&nwlng=-120.9261171&selat=32.4049387&selng=-116.5087852"

# Query the API. This is where your credits get used. Don't run this line more often than necessary
sensor_list <- GET(sensor_call, add_headers("X-API-Key" = READ_KEY))

# Query results are returned and converted to a list of dataframes from JSON
sensor_txt <- fromJSON(content(sensor_list, "text", encoding = "UTF-8"))

# Grab just the 'data' data frame from the API query, and assign field names
sensor_df <- as.data.frame(sensor_txt$data)
names(sensor_df)<- sensor_txt$fields

# Write the list of sensors to a CSV
write.csv(sensor_df, "./PurpleAir_SouthernCAsensors.csv")
```

### Download daily data for AOI
Now that we have the list of sensors, 
```
# Sensor list CSV
#SENSOR_CSV <- "./PurpleAir_LAsensors.csv"
SENSOR_CSV <- "./PurpleAir_SouthernCAsensors.csv"

# Output directory + file
OUT_DIR <- "./output"
OUT_CSV <- file.path(OUT_DIR, "PurpleAir_AllSCStations_compiled.csv")

# set UTC time window
START_UTC <- "2024-01-01 00:00:00"
END_UTC   <- "2025-05-01 23:59:59"

# PurpleAir request settings
AVERAGE_MIN <- 1440 # in minutes (1440 gives you daily averages)
FIELDS <- c("pm10.0_atm", "pm2.5_alt", "pm2.5_atm", "voc","pm1.0_atm","temperature","humidity") # change these to variables of interest

# Helper functions

to_utc_epoch <- function(dt) {
  as.integer(as.POSIXct(dt, tz = "UTC"))
}

build_history_url <- function(sensor_index, start_epoch, end_epoch, average_min, fields) {
  base <- sprintf("https://api.purpleair.com/v1/sensors/%s/history/csv", sensor_index)
  query <- list(
    start_timestamp = start_epoch,
    end_timestamp   = end_epoch,
    average         = average_min,
    fields          = paste(fields, collapse = ",")
  )
  modify_url(base, query = query)
}

fetch_sensor_df <- function(sensor_index, start_epoch, end_epoch,
                            average_min, fields, read_key) {
  
  url <- build_history_url(sensor_index, start_epoch, end_epoch, average_min, fields)
  resp <- GET(url, add_headers("X-API-Key" = read_key))
  
  if (http_error(resp)) return(NULL)
  
  txt <- content(resp, "text", encoding = "UTF-8")
  df <- read_csv(I(txt), show_col_types = FALSE)
  
  if (!"sensor_index" %in% names(df)) {
    df$sensor_index <- as.numeric(sensor_index)
  }
  
  df
}

# Read sensor list

sensorindex_name <- read_csv(SENSOR_CSV, show_col_types = FALSE) %>%
  mutate(sensor_index = as.numeric(sensor_index))

sensor_list <- sensorindex_name$sensor_index %>%
  unique() %>%
  na.omit()

# Time window

start_dt <- ymd_hms(START_UTC, tz = "UTC")
end_dt   <- ymd_hms(END_UTC,   tz = "UTC")

start_epoch <- to_utc_epoch(start_dt)
end_epoch   <- to_utc_epoch(end_dt)


# Fetch + compile (ONE combined dataset)

data_list <- list()

for (s in sensor_list) {
  df <- tryCatch(
    fetch_sensor_df(
      sensor_index = s,
      start_epoch  = start_epoch,
      end_epoch    = end_epoch,
      average_min  = AVERAGE_MIN,
      fields       = FIELDS,
      read_key     = READ_KEY
    ),
    error = function(e) NULL
  )
  
  if (!is.null(df)) data_list[[as.character(s)]] <- df
}

# Function to replace "null" with NA in character columns of a single tibble
process_tibble <- function(df) {
  df %>%
    # Replace "null" with NA for all character columns
    dplyr::mutate_if(is.character, dplyr::na_if, y = "null") %>%
    # Convert all character columns to numeric; as.numeric handles the NAs created
    dplyr::mutate_if(is.character, as.numeric)
}

# Apply the function to the entire list of tibbles
data_list2 <- map(data_list, process_tibble)

aggregated_data <- bind_rows(data_list2)

# Merge metadata + save

final <- aggregated_data %>%
  mutate(sensor_index = as.numeric(sensor_index)) %>%
  left_join(sensorindex_name, by = "sensor_index")

write_csv(final, OUT_CSV)

```

### Usage
Using this approach, querying 1 month of daily data from the greater LA area (808) sensors cost approximately 314,488 credits.
Using this approach, querying 16 month of daily data from southern California sensors (~2488) cost approximately 1.05 million credits.
