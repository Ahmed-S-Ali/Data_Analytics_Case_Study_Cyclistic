Data Exploration
================

## 1) Importing First File

``` r
# Render the R Markdown file "1_Importing_Packages.Rmd" to import the packages.
rmarkdown::render("1_Importing_Packages.Rmd")
```

## 2) Importing Cleaned Dataset

``` r
# Importing the cleaned dataset from the previously saved RDS file 3.
bike_trips <- readRDS(here("bike_trips_data.rds"))
```

## 3) Creating Start/End time Column

``` r
# Extracting the hour component from the 'started_at' and 'ended_at' timestamps,
# and storing them as new columns in the dataset.
bike_trips <- bike_trips %>%
  mutate(start_time = hour(started_at),
         end_time = hour(ended_at))
```

## 4) Creating Duration Column

``` r
# Calculating the duration of each bike trip in minutes and storing the result as a new column in the dataset,
# And rounding the minutes while setting the duration column to integer data type.
bike_trips$duration <- difftime(bike_trips$ended_at, bike_trips$started_at, units = "mins") %>% 
  round() %>% 
  as.integer()
```

## 5) Filtering Duration

``` r
# Filtering the dataset to include only rows where the duration column is greater than 0.
# Dropping rows with any missing values from the dataset.
bike_trips <- bike_trips %>% filter(bike_trips$duration > 0) %>% 
  drop_na()
```

## 6) Creating Weekdays Column

``` r
# Creating a new column "day" containing the weekdays corresponding to the started_at timestamps column.
bike_trips$day <- factor(weekdays(bike_trips$started_at), 
                         levels = c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"))
```

## 7) Creating Months Column

``` r
# Creating a new column "month" containing the month names corresponding to the started_at timestamps column.
bike_trips$month <- month(bike_trips$started_at, label = TRUE)
```

## 8) Creating Seasons Column

``` r
# Creating a new column "season" based on the month column to represent the seasons of the year.
bike_trips <- bike_trips %>% 
  mutate(season = case_when(
    month %in% c("Dec", "Jan", "Feb") ~ "Winter",
    month %in% c("Mar", "Apr", "May") ~ "Spring",
    month %in% c("Jun", "Jul", "Aug") ~ "Summer",
    month %in% c("Sep", "Oct", "Nov") ~ "Autumn"))
```

## 9) Counting Cleaned Rows

``` r
# Counting the number of rows in the dataset after cleaning.
nrow(bike_trips)
```

    ## [1] 5621635

## 10) Cleaned Dataset

``` r
# Saving the final form of the dataset as an RDS file for later use in data visualizations step.
saveRDS(bike_trips, here("bike_trips_data.rds"))
```
