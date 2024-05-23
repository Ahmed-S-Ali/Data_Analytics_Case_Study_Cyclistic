Total Trips By Weekdays
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
# Create a bar plot showing the number of trips by weekdays and rider type with bars positioned to dodge.
ggplot(bike_trips, aes(day, fill = str_to_title(member_casual))) + geom_bar(position = "dodge") +
  
  # Set manual fill colors for rider type.
  scale_fill_manual(values = c("#990F02","#228B22")) +
  
  # Set Y-axis labels with comma separators and visually appealing breaks.
  scale_y_continuous(labels = comma, breaks = pretty_breaks(n = 7)) + 
  
  # Set plot title label, axis labels, and legend box label.
  labs(title = "Number of Trip Rides by Weekdays",
       x = "Weekdays",
       y = "Total Trips",
       fill = "Status") + 
  
  # Apply a dark gray theme to the plot.
  dark_theme_gray() + 
  
  # Set plot title alignment to center, make X and Y axis labels bold.
  theme(plot.title= element_text(hjust = 0.5),
        axis.text = element_text(face = "bold")) +
  
  # Add annotation text to the plot at specified coordinates.
  annotate("text", x = 6, y = 550000, label = "- All Weekdays Are Crowded By Members",
           fontface = "bold", size = 4)
```

    ## Inverted geom defaults of fill and color/colour.
    ## To change them back, use invert_geom_defaults().

![](9_Total_Rides_By_Weekdays_files/figure-gfm/Weekdays%20Plot-1.png)<!-- -->

## Summary

``` r
# Start a pipeline with the bike_trips dataset.
bike_trips %>%
  
  # Group by day and member_casual, with 'member_casual' converted to title case.
  group_by(day, str_to_title(member_casual)) %>%
  
  # Summarize total trips, drop intermediate grouping levels to avoid the message.
  summarise(total_count = n(), .groups = "drop") %>% 
  
  # Arrange tows by day in ascending order and total_count in descending
  arrange(day, desc(total_count)) %>% 
  
  # Format total counts with comma separators.
  mutate(total_count = comma(total_count)) %>% 
  
  # Create a table with a caption, column alignments, and custom column names.
  kable(caption = "Total Trips by Riders Type Per Weekday",
        align = "c",
        col.names = c("Weekday", "Rider Type", "Total Count"))
```

|  Weekday  | Rider Type | Total Count |
|:---------:|:----------:|:-----------:|
|  Monday   |   Member   |   486,049   |
|  Monday   |   Casual   |   231,007   |
|  Tuesday  |   Member   |   566,884   |
|  Tuesday  |   Casual   |   242,210   |
| Wednesday |   Member   |   576,528   |
| Wednesday |   Casual   |   245,021   |
| Thursday  |   Member   |   579,042   |
| Thursday  |   Casual   |   266,149   |
|  Friday   |   Member   |   521,916   |
|  Friday   |   Casual   |   306,735   |
| Saturday  |   Member   |   464,234   |
| Saturday  |   Casual   |   404,005   |
|  Sunday   |   Member   |   401,747   |
|  Sunday   |   Casual   |   330,108   |

Total Trips by Riders Type Per Weekday
