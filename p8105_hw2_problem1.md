P8105 Homework 2 Problem 1
================
Zanis Fang
9/26/2018

### Data loading and cleaning

``` r
# load data
nyc_transit <- read_csv(file = "./data/NYC_Transit_Subway_Entrance_And_Exit_Data.csv")

# data cleaning
nyc_transit <- nyc_transit %>% 
               janitor::clean_names() %>% 
               #select needed rows
               select(line:ada, -exit_only, -staffing, -staff_hours)  %>%
               #recode entry to logic
               mutate(entry = recode(entry, "YES" = TRUE, "NO" = FALSE))
```

Each row represents an entrance. The dataframe contains line names, station names, station location, connected train routes, type of entrance, entry or exit, has vending or not, ADA compliant or not. Use "janitor::clean\_names" to clean the column names, select indicated columns, and change entry to TRUE or FALSE. There are 1868 rows, 19 columns. The data is not tidy. The route 1-11 are actually data rather than variable. Besides, data type should be TRUE or FALSE instead of YES or NO.

**Q1. How many distinct stations are there? Note that stations are identified both by name and by line (e.g. 125th St A/B/C/D; 125st 1; 125st 4/5); the distinct function may be useful here.**

``` r
#unique station with ADA compliant information
station_ada <- distinct(nyc_transit, line, station_name, ada)
```

There are 465 distinct stations.

**Q2. How many stations are ADA compliant?**

There are 84 stations which are ADA compliant.

**Q3. What proportion of station entrances / exits without vending allow entrance?**

``` r
entry_no_vending <- nyc_transit %>% filter(vending == "NO", entry == TRUE)
no_vending <- nyc_transit %>% filter(vending == "NO")
```

The proportion of station entrances/exits without vending is 0.3770492.

### Reform table

``` r
nyc_transit <- gather(nyc_transit, key = "route_number", value = "route_name", route1:route11)

# distinct station serve A train with ADA information
station_train_a <- nyc_transit %>%
                   distinct(line, station_name, route_name, ada) %>% 
                   filter(route_name == "A")
```

There are 60 stations serve the A train, among which 17 are ADA compliant.
