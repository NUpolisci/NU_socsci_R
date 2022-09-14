# Data Science Exercise 

## "I have a job for you..." 

It's late Tuesday night, the evening after your first day of graduate school, when you receive an email from a professor that you know, but like you *don't really* know. The subject is vague: "I have a job for you...".

Ok... 

So, you open it and read the following. 

*Welcome to Northwestern! I am in need of an RA to do some data tasks. I figured that you should be learning this stuff about now,  so it might be a good opportunity to put you to work! The pay is $24/hour. Here's what I'd be asking you to do:*

1. Download some [data](https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-09-13/bigfoot.csv) from [this tidytuesday post](https://github.com/rfordatascience/tidytuesday/tree/master/data/2022/2022-09-13). 

2. Go through the data and get a feel of what the variables are about. 

3. Sort the data by date. See if you can get a by-year count of observations...

4. Can you also get a count of observations in each state? Do any counties have more than one observation? Which counties have the most over time? 

5. What's the mean number of sightings or observations per year? 

6. I need you to save the following data for me: the mean visibility, moon phase, and windspeed? 

7. You'll see some testimonies included in the "observed" column. What are the 4 most recent testimonies? 

8. This might be a little advanced, but maybe you can make some graphs: 

  - Bar graph of observations per season (none of the base R stuff... my associates only want to see ggplot2 graphics...)
  
  - Line graph of observations per year since the beginning of the dataset
  
... the graphics would be even BETTER if we can have them ready for print. I'm thinking New York Times or  Wall Street Journal style? 
  
*I don't use Github, but I need to make this information accessible **FAST**. If you can push this to your GitHub repository soon, my other associates will take it from there.*

You're even more puzzled than you were to begin with, but you spent too much money on your new snow boots and winter coat in preparation for Chicago winter that you *really* can't turn down the opportunity for some more money. 

What's the worst that can happen? So, you get started... 

## Step One: Get the Data 

So, I know I need to get the data. And I remember Jennifer taught me how to load it directly from GitHub. But I kind of want to have a local copy on my computer? So I can put everything on Github later. 

I'll just deal with this later...let's load in the data. Let's name the dataframe something intuitive that makes sense for now. I'll also make sure to load in the tidyverse because it seems like I'll take a look at it. I can also use `glimpse()` to view the data names and types. 


```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
## ✔ readr   2.1.2      ✔ forcats 0.5.1 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
prof_data<-read.csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-09-13/bigfoot.csv")

glimpse(prof_data)
```

```
## Rows: 5,021
## Columns: 28
## $ observed           <chr> "I was canoeing on the Sipsey river in Alabama. It …
## $ location_details   <chr> "", "East side of Prince William Sound", "Great swa…
## $ county             <chr> "Winston County", "Valdez-Chitina-Whittier County",…
## $ state              <chr> "Alabama", "Alaska", "Rhode Island", "Pennsylvania"…
## $ season             <chr> "Summer", "Fall", "Fall", "Summer", "Spring", "Fall…
## $ title              <chr> "", "", "Report 6496: Bicycling student has night e…
## $ latitude           <dbl> NA, NA, 41.45000, NA, NA, 35.30110, 39.38745, 41.29…
## $ longitude          <dbl> NA, NA, -71.50000, NA, NA, -99.17020, -81.67339, -7…
## $ date               <chr> "", "", "1974-09-20", "", "", "1973-09-28", "1971-0…
## $ number             <dbl> 30680, 1261, 6496, 8000, 703, 9765, 4983, 31940, 56…
## $ classification     <chr> "Class B", "Class A", "Class A", "Class B", "Class …
## $ geohash            <chr> "", "", "drm5ucxrc0", "", "", "9y32z667yc", "dpjbj6…
## $ temperature_high   <dbl> NA, NA, 78.17, NA, NA, 71.86, NA, 92.24, NA, NA, 74…
## $ temperature_mid    <dbl> NA, NA, 73.425, NA, NA, 61.425, NA, 80.810, NA, NA,…
## $ temperature_low    <dbl> NA, NA, 68.68, NA, NA, 50.99, NA, 69.38, NA, NA, 53…
## $ dew_point          <dbl> NA, NA, 65.72, NA, NA, 51.03, NA, 67.34, 32.55, NA,…
## $ humidity           <dbl> NA, NA, 0.86, NA, NA, 0.79, NA, 0.68, 0.45, NA, 0.7…
## $ cloud_cover        <dbl> NA, NA, 0.86, NA, NA, 0.11, NA, 0.05, 0.00, NA, 0.6…
## $ moon_phase         <dbl> NA, NA, 0.16, NA, NA, 0.07, NA, 0.76, 0.02, NA, 0.1…
## $ precip_intensity   <dbl> NA, NA, 0.0000, NA, NA, NA, NA, 0.0000, 0.0000, NA,…
## $ precip_probability <dbl> NA, NA, 0.00, NA, NA, NA, NA, 0.00, 0.00, NA, 0.70,…
## $ precip_type        <chr> "", "", "", "", "", "rain", "", "", "", "", "rain",…
## $ pressure           <dbl> NA, NA, 1020.61, NA, NA, 1017.26, NA, 1016.80, 1012…
## $ summary            <chr> "", "", "Foggy until afternoon.", "", "", "Partly c…
## $ uv_index           <int> NA, NA, 4, NA, NA, 7, NA, 8, 8, NA, 6, 10, 6, 7, NA…
## $ visibility         <dbl> NA, NA, 2.750, NA, NA, 10.000, NA, 6.922, 8.880, NA…
## $ wind_bearing       <int> NA, NA, 198, NA, NA, 259, NA, 219, 285, NA, 262, 19…
## $ wind_speed         <dbl> NA, NA, 6.92, NA, NA, 8.41, NA, 1.01, 4.01, NA, 0.4…
```

... So, that told me nothing. I guess I probably should have taken a look at the information about the data first? (**YES, ALWAYS!**). 

## [CHECK OUT THE METADATA](https://github.com/rfordatascience/tidytuesday/tree/master/data/2022/2022-09-13)

This is a dataset... about Bigfoot? Ok, well at least it comes from a reputable-sounding source, Bigfoot Field Researchers Organization. And there's this [article](https://timothyrenner.github.io/datascience/2017/06/30/finding-bigfoot.html)? Great! 

So, I really don't have much more information than this. The data are about Bigfoot sightings around the US and North America. There are variables about the location of the sighting, the report provided, environmental and climate data. Ok, well I guess as long as this is ethical research, it's still research! 

Now that I know what this is, I want to save this data to my computer so I can save the changes I might make to it and don't have to continually download it from the tidytuesday post. 


```r
# i want to rename the data now that I know what it's about... something that makes recall of it easy, like "bigfoot" 

bigfoot<- prof_data

# need to find out what the working directory is set to 

getwd()
```

```
## [1] "/Users/sarahmoore/Library/CloudStorage/OneDrive-NorthwesternUniversity/Teaching/R_textbook_v1/nu_socscir"
```

```r
#setwd("/Users/sarahmoore/Desktop")
```

## Sort the data by date. See if you can get a by-year count of observations...

Next the professor asked if I would sort the data by date and count how many reports were made by year. 

I think there is some verb in `dplyr` that I can use to do this... maybe `arrange()`? I guess I can do this on the date variable, but how can I get the year out of there? 






















