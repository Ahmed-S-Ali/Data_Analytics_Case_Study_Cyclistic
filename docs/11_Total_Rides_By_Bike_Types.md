Total Trips By Bike Types
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

## Labels

``` r
# Create a vector with bike labels for bike's used type.
bike_label = c("- Casual With 869,706 Total\n   Member With 1,800,090 Total",
               "- Casual  With 1,077,705 Total\n Member With 1,796,310 Total",
               "- Only Casual Uses Docked Bikes\nWith 77,824 Total")

# Create a dataframe 'df_bike_labels' with unique values of 'rideable_type' from dataset and corresponding labels,
# for the annotation text.
df_bike_labels <- data.frame(rideable_type = str_to_title(str_replace_all(unique(bike_trips$rideable_type), "_", " ")), 
                        label = bike_label)
```

## Plot

``` r
# Create a bar plot for Number of Trips by Bike's types with ggplot and adjust bars width to 0.5
ggplot(bike_trips) + geom_bar(aes(member_casual, fill = str_to_title(member_casual)), width = 0.5) +
  
  # Create facets for each bike's type while converting underscores to spaces and title-casing texts.
  facet_wrap(~str_to_title(str_replace_all(rideable_type, "_", " "))) + 
  
  # Set Y-axis labels with comma separators and visually appealing breaks, Set X-axis labels to title case.
  scale_y_continuous(labels = comma, breaks = pretty_breaks(n = 8)) + scale_x_discrete(labels = str_to_title) +
  
  # Set manual fill colors for rider type.
  scale_fill_manual(values = c("#990F02","#228B22")) + 
  
  # Set plot title label, axis labels, and legend box label.
  labs(title = "Number of Trips by Bike Types",
       x = "Riders Type",
       y = "Total Trips",
       fill = "Status") + 
  
  # Apply a dark gray theme to the plot
  dark_theme_gray() + 
  
  # Set plot title alignment to center, make X and Y axis labels bold.
  theme(plot.title= element_text(hjust = 0.5),
        axis.text = element_text(face = "bold")) +
  
  # Add labeled text annotations at specified coordinates.
  geom_text(data = df_bike_labels, aes(x = 1.4, y = 1850000, label = label), fontface = "bold", size = 4)
```

    ## Inverted geom defaults of fill and color/colour.
    ## To change them back, use invert_geom_defaults().

![](11_Total_Rides_By_Bike_Types_files/figure-gfm/Types%20Plot-1.png)<!-- -->

## Summary

``` r
# Start a pipeline with the bike_trips dataset
bike_trips %>%
  
  # Group by rideable_type and member_casual (converted to title case).
  group_by(rideable_type, str_to_title(member_casual)) %>%
  
  # Convert 'rideable_type' to title case and replace underscores with spaces
  mutate(rideable_type = str_to_title(str_replace_all(rideable_type, "_", " "))) %>% 
  
  # Summarize total trips, drop intermediate grouping levels to avoid the message.
  summarise(total_count = n(), .groups = "drop") %>% 
  
  # Arrange rows by rideable_type in ascending order and total count in descending order.
  arrange(rideable_type, desc(total_count)) %>% 
  
  # Format total counts with comma separators.
  mutate(total_count = comma(total_count)) %>% 
  
  # Create a table with a caption, column alignments, and custom column names
  kable(caption = "Total Trips by Riders Type Per Weekday",
        align = "c",
        col.names = c("Bike Type", "Rider Type", "Total Count"))
```

|   Bike Type   | Rider Type | Total Count |
|:-------------:|:----------:|:-----------:|
| Classic Bike  |   Member   |  1,800,090  |
| Classic Bike  |   Casual   |   869,706   |
|  Docked Bike  |   Casual   |   77,824    |
| Electric Bike |   Member   |  1,796,310  |
| Electric Bike |   Casual   |  1,077,705  |

Total Trips by Riders Type Per Weekday
