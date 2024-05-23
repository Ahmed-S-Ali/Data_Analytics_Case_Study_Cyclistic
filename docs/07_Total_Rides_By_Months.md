Total Trips By Months
================

## Importing Packages

``` r
# Render the R Markdown file "1_Importing_Packages.Rmd" to import the packages.
rmarkdown::render("1_Importing_Packages.Rmd")
```

## Importing Dataset

``` r
# Importing the cleaned dataset from the previously saved RDS file 3.
bike_trips <- readRDS(here("bike_trips_data.rds"))
```

## Plot

``` r
# Create a bar plot showing the number of trips by month.
ggplot(bike_trips, aes(month)) + geom_bar() + 
  
  # Set Y-axis labels with comma separators and visually appealing breaks.
  scale_y_continuous(labels = comma, breaks = pretty_breaks(n = 7)) + 
  
  # Set plot title label, axis labels.
  labs(title = "Number of Trips by Months",
       x = "Month",
       y = "Total Trips") +
  
  # Apply a dark gray theme to the plot.
  dark_theme_gray() + 
  
  # Set plot title alignment to center, make X and Y axis labels bold.
  theme(plot.title = element_text(hjust = 0.5, face = "bold"), axis.text = element_text(face = "bold")) +
  
  # Add annotation text to the plot at specified coordinates.
  annotate("text", x = 11, y = 700000, label = "- Most Trips Were Taken in Augest.\nWith Total 759,988 Rides.",
           fontface = "bold", size = 4)
```

    ## Inverted geom defaults of fill and color/colour.
    ## To change them back, use invert_geom_defaults().

![](7_Total_Rides_By_Months_files/figure-gfm/Months%20Plot-1.png)<!-- -->

## Summary

``` r
# Start a pipeline with the bike_trips dataset.
bike_trips %>% 
  
  # Group by month.
  group_by(month) %>% 
  
  # Summarize total trips.
  summarise(total_count = n()) %>% 
  
  # Arrange rows by total counts in descending order.
  arrange(desc(total_count)) %>% 
  
  # Format total counts with comma separators.
  mutate(total_count = comma(total_count)) %>% 
  
  # Create a table with a caption, column alignments, and custom column names.
  kable(caption = "Total Trips by Month",
        align = "c",
        col.names = c("Month", "Total Count"))
```

| Month | Total Count |
|:-----:|:-----------:|
|  Aug  |   759,988   |
|  Jul  |   754,845   |
|  Jun  |   707,238   |
|  Sep  |   657,191   |
|  May  |   592,885   |
|  Oct  |   529,741   |
|  Apr  |   416,223   |
|  Nov  |   357,784   |
|  Mar  |   252,242   |
|  Dec  |   221,293   |
|  Feb  |   186,190   |
|  Jan  |   186,015   |

Total Trips by Month
