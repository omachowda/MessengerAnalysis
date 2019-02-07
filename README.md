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

Save your unzipped messages folder to an easily accessible directory.





#### TODO ####

Expannd functionality to be able to analyze most common emoji sent.