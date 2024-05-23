Data Preparation
================

## 1) Importing First File

``` r
# Render the R Markdown file "1_Importing_Packages.Rmd" to import the packages.
rmarkdown::render("01_Importing_Packages.Rmd")
```

## 2) Reading The Dataset

``` r
# Importing the dataset from the previously saved RDS file 2.
bike_trips <- readRDS(here("bike_trips_data.rds"))
```

## 3) Counting Rows Before Cleaning

``` r
# Importing the combined dataset from the previously saved RDS file.
nrow(bike_trips)
```

    ## [1] 5719877

## 4) Checking NA

``` r
# Generating a summary table of NA values for each column in the dataset.
# Using kable for better visualizing.
kable(colSums(is.na(bike_trips)))
```

|                    |      x |
|:-------------------|-------:|
| ride_id            |      0 |
| rideable_type      |      0 |
| started_at         |      0 |
| ended_at           |      0 |
| start_station_name | 875716 |
| start_station_id   | 875848 |
| end_station_name   | 929202 |
| end_station_id     | 929343 |
| start_lat          |      0 |
| start_lng          |      0 |
| end_lat            |   6990 |
| end_lng            |   6990 |
| member_casual      |      0 |

## 5) Removing Unnecessary Columns

``` r
# Selecting specific columns from the dataset and storing the result into "bike_trips".
bike_trips <- bike_trips %>% select(c(rideable_type, started_at, ended_at, member_casual))
```

## 6) Removing Empty Rows

``` r
# Removing rows and columns with missing values from the dataset.
bike_trips <- remove_empty(bike_trips, which = c("rows", "cols"))

# Removing duplicate rows from the dataset.
bike_trips <- distinct(bike_trips)
```

## 7) Head

``` r
# Displaying the first 5 rows of the cleaned dataset in a table format using kable.
kable(head(bike_trips))
```

| rideable_type | started_at          | ended_at            | member_casual |
|:--------------|:--------------------|:--------------------|:--------------|
| electric_bike | 2023-01-21 20:05:42 | 2023-01-21 20:16:33 | member        |
| classic_bike  | 2023-01-10 15:37:36 | 2023-01-10 15:46:05 | member        |
| electric_bike | 2023-01-02 07:51:57 | 2023-01-02 08:05:11 | casual        |
| classic_bike  | 2023-01-22 10:52:58 | 2023-01-22 11:01:44 | member        |
| classic_bike  | 2023-01-12 13:58:01 | 2023-01-12 14:13:20 | member        |
| electric_bike | 2023-01-31 07:18:03 | 2023-01-31 07:21:16 | member        |

## 8) Tail

``` r
# Displaying the last 5 rows of the cleaned dataset in a table format using kable.
kable(tail(bike_trips))
```

| rideable_type | started_at          | ended_at            | member_casual |
|:--------------|:--------------------|:--------------------|:--------------|
| electric_bike | 2023-12-04 23:34:11 | 2023-12-04 23:39:16 | member        |
| electric_bike | 2023-12-07 13:15:24 | 2023-12-07 13:17:37 | casual        |
| classic_bike  | 2023-12-08 18:42:21 | 2023-12-08 18:45:56 | casual        |
| classic_bike  | 2023-12-05 14:09:11 | 2023-12-05 14:13:01 | member        |
| electric_bike | 2023-12-02 21:36:07 | 2023-12-02 21:53:45 | casual        |
| classic_bike  | 2023-12-11 13:07:46 | 2023-12-11 13:11:24 | member        |

## 9) Structure

``` r
# Displaying the structure of the 'bike_trips' dataset.
str(bike_trips)
```

    ## tibble [5,719,676 × 4] (S3: tbl_df/tbl/data.frame)
    ##  $ rideable_type: chr [1:5719676] "electric_bike" "classic_bike" "electric_bike" "classic_bike" ...
    ##  $ started_at   : POSIXct[1:5719676], format: "2023-01-21 20:05:42" "2023-01-10 15:37:36" ...
    ##  $ ended_at     : POSIXct[1:5719676], format: "2023-01-21 20:16:33" "2023-01-10 15:46:05" ...
    ##  $ member_casual: chr [1:5719676] "member" "member" "casual" "member" ...

## 10) Summary

``` r
# Creating a summary table of the 'bike_trips' dataset in a table format using kable.
kable(summary(bike_trips))
```

|     | rideable_type    | started_at                     | ended_at                       | member_casual    |
|:----|:-----------------|:-------------------------------|:-------------------------------|:-----------------|
|     | Length:5719676   | Min. :2023-01-01 00:01:58.00   | Min. :2023-01-01 00:02:41.00   | Length:5719676   |
|     | Class :character | 1st Qu.:2023-05-21 12:50:25.00 | 1st Qu.:2023-05-21 13:13:48.00 | Class :character |
|     | Mode :character  | Median :2023-07-20 18:02:36.00 | Median :2023-07-20 18:19:37.00 | Mode :character  |
|     | NA               | Mean :2023-07-16 10:27:18.52   | Mean :2023-07-16 10:45:28.66   | NA               |
|     | NA               | 3rd Qu.:2023-09-16 20:08:46.75 | 3rd Qu.:2023-09-16 20:28:06.25 | NA               |
|     | NA               | Max. :2023-12-31 23:59:38.00   | Max. :2024-01-01 23:50:51.00   | NA               |

## 11) Filter

``` r
# Filtering the 'bike_trips' dataset to include only rows where the 'ended_at' year is less than 2024.
bike_trips <- bike_trips %>% filter(year(ended_at) < 2024)
```

## 12) Saving Dataset

``` r
# Saving the cleaned dataset as an RDS file to ensure that the cleaned dataset can be easily reloaded,
# and used in future analyses without needing to repeat the cleaning steps.
saveRDS(bike_trips, here("bike_trips_data.rds"))
```
