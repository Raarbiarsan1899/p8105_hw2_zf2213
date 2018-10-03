P8105 Homwork 2 Problem 2
================
Zanis Fang
9/26/2018

**Read and clean the Mr. Trash Wheel sheet**

``` r
#load data
trash_wheel <- readxl::read_xlsx("./data/HealthyHarborWaterWheelTotals2017-9-26.xlsx",
                                 sheet = 1,
                                 range = readxl::cell_cols("A:N"))

trash_wheel <- trash_wheel %>%
               janitor::clean_names() %>%
               # omit rows counting totals, note only dumpsters have date information
               filter(!is.na(date)) %>%
               # convert sports balls to integer variable
               mutate(sports_balls = sports_balls %>% round(0) %>% as.integer())
```

**Read and clean precipitation data from 2016 and 2017**

``` r
# define a function to load data and clean data
prcp_load_clean <- function(wh_year) {
 
 # load data
 readxl::read_xlsx("./data/HealthyHarborWaterWheelTotals2017-9-26.xlsx",
                   sheet = paste(wh_year, "Precipitation"),
                   range = "A2:B14"
                   ) %>%
  
  janitor::clean_names() %>%
  # remove rows without data
  filter(!is.na(total)) %>%
  # add year variable
  mutate(year = wh_year)
           
}

# combine two tables
prcp_data <- rbind(prcp_load_clean(2016), prcp_load_clean(2017))

# convert month to a character variable
prcp_data$month <- month.name[prcp_data$month]
```

In Mr. Trash Wheel data, it describes the amount of trash each dumpster colloects. There are 215 individual dumpsters. In each dumpster, it categorizes the kind of trash, like "plastic bags", "cigarette butts", etc. For precipitation data in 2017, there are in total 29.93 inch precipitation. The median number of sports balls in a dumpster in 2016 is 26.
