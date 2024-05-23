Importing Dataset
================

## 1) Importing First File

``` r
# Render the R Markdown file "1_Importing_Packages.Rmd" to import the packages.
rmarkdown::render("01_Importing_Packages.Rmd")
```

## 2) Importing CSV Files

``` r
# Setting the path where the CSV files are stored (Change "[file_path]" with actual folder path).
dataset_path <- "[file_path]\\"

# Generating a vector containing paths to individual CSV files.
files_path <- paste0(dataset_path, c(
                "202301-divvy-tripdata.csv",
                "202302-divvy-tripdata.csv",
                "202303-divvy-tripdata.csv",
                "202304-divvy-tripdata.csv",
                "202305-divvy-tripdata.csv",
                "202306-divvy-tripdata.csv",
                "202307-divvy-tripdata.csv",
                "202308-divvy-tripdata.csv",
                "202309-divvy-tripdata.csv",
                "202310-divvy-tripdata.csv",
                "202311-divvy-tripdata.csv",
                "202312-divvy-tripdata.csv"))
```

## 3) Dataset Frame

``` r
# Reading multiple CSV files into a list and combining them into a single dataframe.
bike_trips <- files_path %>%
  lapply(read_csv) %>% 
  bind_rows()

# Removing the CSV files folder path and the vector containing file paths to free up memory.
remove(files_path, dataset_path)
```

## 4) Saving Dataset

``` r
# Saving the dataset as an RDS file to ensure that the dataset can be easily reloaded,
# and used in future analyses without needing to repeat the steps.
saveRDS(bike_trips, here("bike_trips_data.rds"))
```
