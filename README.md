# Messenger Anlaysis

## About

This project uses personal Facebook Messenger data to produce graphs of messaging trends.

Leverages R and ggplot2 to visualize trends over time by month, weekday, and hour. Also includes most common word analysis, and aggregation of some interesting statistics.

Currently functional for private messaages and group chats.

## Dowload Your Data

Login to [Facebook](https://www.facebook.com "Title") and follow this path:

`Facebook Home > Settings > Your Facebook Information > Download Your Infomation`

1. Deselect all categories
2. Select Messages under Your Information
3. Pick the date range you are interested in
4. Change the format to JSON
5. For Media Quality select low (suggested as this only changes the quality of the images and videos)
6. Create File - Facebook will sometimes take up to a day to make your data available to you
7. Download and unzip the messages folder

## Project Set Up

### Libraries
```r
library(rjson) # to read in the json file
library (ggplot2) # for all the visualizations
library(dplyr) # for dataframe manipulation
library(tidyr) # for array operations
library(tm) # for stopwords library to limit most common words
```

### Reading In Your Data

Save your unzipped messages folder to an easily accessible directory. The JSON file lives here:

`messages > inbox > person_##### > message.json`

Read in the json file using either a manual path:

`result = fromJSON(file =  "/Users/.../message.json")`

or a file selection

`result = fromJSON(file =  file.choose())`

Running this command `as.data.frame(result$messages[1])` will:

1. Validate that the data read was successful
2. Display the structure of the dataframe
3. Show the last message that was sent in the thread

## Data Manipulation

### Outline 

1. Exclude all messages that contain a
    * photo
    * video
    * sticker
    * audio file
    * url
2. Convert timestamps_ms to a useable timestamp
3. Add descriptive time data to our data frame 

### Exclude and Rebuild

Using the O(n) methodology (suggested) we:

1. Pre-allocate our data frame
2. Iterate through the entire dataframe that we just read in exlcuding messages mentioned aboce
3. Drop rows that have null values this


### Convert Timestamp_ms

```r
# PARAMETERS
# ms: a numeric vector of milliseconds (big integers of 13 digits)
# t0: a string of the format "yyyy-mm-dd", specifying the date that
#      corresponds to 0 millisecond
# timezone: a string specifying a timezone that can be recognized by R
# returns: a POSIXct vector representing calendar dates and times  

ms_to_date = function(ms, t0="1970-01-01", timezone){
  sec = ms / 1000
  as.POSIXct(sec, origin=t0, tz=timezone)
}
```

### Useful Time Data

The following code gives time data we can use for our visualizations, includes:

1. Weekday
2. Hour
3. Month/Year combination

```r
mydf$weekdays = factor(weekdays(mydf$datetime),levels = c('Monday', 'Tuesday', 'Wednesday', 'Thursday',
                                                          'Friday', 'Saturday', 'Sunday'))
mydf$months = months((mydf$datetime))
mydf$date = as.Date(mydf$datetime)
mydf$hours = format(mydf$datetime, '%H')
mydf$monthyear = format(mydf$datetime, '%m/%Y')
mydf$monthyeartest = format(mydf$datetime, '%y/%m')
mydf$monthday = format(mydf$datetime, '%m/%d/%Y')
```

## Visualization

All visualizations were created using [ggplot2](https://ggplot2.tidyverse.org/ "Title")

### Pie Chart

Requires some tweaks to work

1. Change sender names
2. Change ` geom_text(aes(y = c(6000,1800)` so that the numbers rotate properly in the pie chart. This is determined by the # of messages sent in the thread.

### Rest of the Charts

Should be pretty straightforward, use ggplot2 documentation for help. Some fun changes you can make:

1. Change colors by modifying `scale_fill_brewer(palette='Set1')`
2. Set the background to white `theme(panel.background = element_rect(fill = 'white', colour = 'white'))`
3. Add and customize plot themes such as minimalplottheme that was used for the pie chart


#### TODO ####

Any suggestions are welcomed

1. Expannd functionality to be able to analyze most common emoji sent.
2. Fix pie chart to be paramterized.