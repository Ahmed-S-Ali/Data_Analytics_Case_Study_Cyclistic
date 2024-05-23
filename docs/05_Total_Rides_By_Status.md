Total Trips By Status
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
# Create a bar plot for Number of Trips by Status of bike trips data with ggplot and adjust bars width to 0.2
ggplot(bike_trips, aes(member_casual, fill = str_to_title(member_casual))) + geom_bar(width = 0.2) +
  
  # Set X-axis labels to title case and Y-axis labels with comma separators and visually appealing breaks.
  scale_x_discrete(labels = str_to_title) + scale_y_continuous(labels = comma, breaks = pretty_breaks(n = 7)) +
  
  # Set manual fill colors for bars, casual with Red and member with Green.
  scale_fill_manual(values = c("#990F02","#228B22")) +
  
  # Set plot title label, axis labels, and legend box label.
  labs(title = "Number of Trips by Status",
       x = "Status",
       y = "Total Trips",
       fill = "Status")  +
  
  # Apply a dark gray theme to the plot from ggdark package.
  dark_theme_gray() + 
  
  # Set plot title alignment to center, make X and Y axis labels bold.
  theme(plot.title = element_text(hjust = 0.5), axis.text = element_text(face = "bold")) +
  
  # Add annotation text to the plot at specified coordinates.
  annotate("text", x = 2.3, y = 1000000, label = "Casual:   2,025,235\nMember: 3,596,400",
           fontface = "bold", size = 4)
```

![](./assets/Status.png)<!-- -->

## Summary

``` r
# Start a pipeline with the bike_trips dataset.
bike_trips %>% 
  
  # Group by member type, with member type converted to title case.
  group_by(str_to_title(member_casual)) %>% 
  
  # Summarize the total count of trips for each member status.
  summarise(total_count = n()) %>% 
  
  # Arrange the summary table by total count in descending order.
  arrange(desc(total_count)) %>% 
  
  # Format the total count with comma separators.
  mutate(total_count = comma(total_count)) %>%

  # Create a table with a caption, column alignments, and custom column names.
  kable(caption = "Total Bike Trips by Riders Type",
  align = "c",
  col.names = c("Rider Type", "Total Count"))
```

| Rider Type | Total Count |
|:----------:|:-----------:|
|   Member   |  3,596,400  |
|   Casual   |  2,025,235  |

Total Bike Trips by Riders Type
