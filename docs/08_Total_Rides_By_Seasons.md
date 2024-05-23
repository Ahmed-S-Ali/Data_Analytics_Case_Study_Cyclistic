Total Trips By Seasons
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
# Create a bar plot showing the number of trips by seasons and rider type with bars positioned to dodge.
ggplot(bike_trips, aes(season, fill = str_to_title(member_casual))) + geom_bar(position = "dodge") + 
  
  # Set manual fill colors for rider type.
  scale_fill_manual(values = c("#990F02","#228B22")) +
  
  # Set Y-axis labels with comma separators and visually appealing breaks.
  scale_y_continuous(labels = comma, breaks = pretty_breaks(n = 6)) +
  
  # Set plot title label, axis labels, and legend box label.
  labs(title = "Number of Trips by Seasons",
       x = "Seasons",
       y = "Total Trips",
       fill = "Status") + 
  
  # Apply a dark gray theme to the plot
  dark_theme_gray() + 
  
  # Set plot title alignment to center, make X and Y axis labels bold.
  theme(plot.title = element_text(hjust = 0.5),
        axis.text = element_text(face = "bold")) +
  
  # Add annotation text to the plot at specified coordinates
  annotate("text", x = 1.5, y = 1200000,
  label = "                 - Summer is The Favourite Season for Both Member and Casual\n
           Total Casual Trips During Summer: 928,526\n            Total Member Trips During Summer: 1,293,545",
           fontface = "bold", size = 4)
```

    ## Inverted geom defaults of fill and color/colour.
    ## To change them back, use invert_geom_defaults().

![](8_Total_Rides_By_Seasons_files/figure-gfm/Seasons%20Plot-1.png)<!-- -->

## Summary

``` r
# Start a pipeline with the bike_trips dataset.
bike_trips %>% 
  
  # Group by season and member type, with member type converted to title case.
  group_by(season, str_to_title(member_casual)) %>% 
  
  # Summarize total trips, drop intermediate grouping levels to avoid the message.
  summarise(total_count = n(), .groups = "drop") %>% 
  
  # Arrange rows by total counts in descending order.
  arrange(desc(total_count)) %>% 
  
  # Format total counts with comma separators.
  mutate(total_count = comma(total_count)) %>% 
  
  # Create a table with a caption, column alignments, and custom column names.
  kable(caption = "Total Trips by Seasons",
        align = "c",
        col.names = c("Season", "Rider Type", "Total Count"))
```

| Season | Rider Type | Total Count |
|:------:|:----------:|:-----------:|
| Summer |   Member   |  1,293,545  |
| Autumn |   Member   |  1,015,246  |
| Summer |   Casual   |   928,526   |
| Spring |   Member   |   826,521   |
| Autumn |   Casual   |   529,470   |
| Winter |   Member   |   461,088   |
| Spring |   Casual   |   434,829   |
| Winter |   Casual   |   132,410   |

Total Trips by Seasons
