Total Trips By Trip Duration
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
# Create a vector with duration labels for members and casual riders.
duration_label = c(
  "- Members typically take shorter trips than Casual riders\nBut Members total number of bike rides is higher",
  "- Although Casual riders tend to take longer trips\nMembers make more overall bike trips")

# Create a data frame 'df_labels' with unique values of 'member_casual' from 'bike_trips' and corresponding labels
# for the annotation text
df_labels <- data.frame(member_casual = unique(bike_trips$member_casual), 
                        label = duration_label)
```

## Plot

``` r
# Create a ggplot object, filtering 'bike_trips' dataset to include only trips with duration <= 180 minutes.
ggplot(filter(bike_trips, duration <= 180)) + 
  
  # Creating a bar plot, Showing the number of trips by trips duration in minutes, Create facets for each rider.
  geom_bar(aes(duration, fill = str_to_title(member_casual))) + facet_wrap(~str_to_title(member_casual)) +
  
  # Set Y-axis labels with comma separators and visually appealing breaks.
  scale_y_continuous(labels = comma, breaks = pretty_breaks(n = 7)) + 
  
  # Set manual fill colors for rider type.
  scale_fill_manual(values = c("#990F02","#228B22")) +
  
  # Set plot title label, axis labels, and legend box label.
  labs(title = "Number of Trips by Duration in Minutes",
       x = "Duration in Minutes",
       y = "Total Trips",
       fill = "Status") + 
  
  # Apply a dark gray theme to the plot.
  dark_theme_gray() + 
  
  # Set plot title alignment to center, make X and Y axis labels bold.
  theme(plot.title= element_text(hjust = 0.5),
        axis.text = element_text(face = "bold")) +
  
  # Add labeled text annotations at specified coordinates.
  geom_text(data = df_labels, aes(x = 100, y = 275000, label = label), fontface = "bold", size = 4)
```

    ## Inverted geom defaults of fill and color/colour.
    ## To change them back, use invert_geom_defaults().

![](10_Total_Rides_By_Duration_files/figure-gfm/Duration%20Plot-1.png)<!-- -->

## Summary

``` r
# Start a pipeline with the bike_trips dataset.
bike_trips %>%
  
  # Filter the rows to include only trips with a duration of 60 minutes or less.
  filter(duration <= 60) %>% 
  
  # Group by member_casual (converted to title case) and duration
  group_by(str_to_title(member_casual), duration) %>%
  
  # Summarize total trips, drop intermediate grouping levels to avoid the message.
  summarise(total_count = n(), .groups = "drop") %>% 
  
  # Arrange rows by total_count in descending order.
  arrange(desc(total_count)) %>% 
  
  # Format total counts with comma separators.
  mutate(total_count = comma(total_count)) %>% 
  
  # Create a table with a caption, column alignments, and custom column names.
  kable(caption = "Total Trips by Riders Type Per Weekday",
        align = "c",
        col.names = c("Rider Type", "Duration in Minutes", "Total Count"))
```

| Rider Type | Duration in Minutes | Total Count |
|:----------:|:-------------------:|:-----------:|
|   Member   |          4          |   290,482   |
|   Member   |          5          |   285,698   |
|   Member   |          6          |   278,782   |
|   Member   |          7          |   246,259   |
|   Member   |          3          |   232,215   |
|   Member   |          8          |   229,734   |
|   Member   |          9          |   198,213   |
|   Member   |         10          |   183,086   |
|   Member   |         11          |   155,107   |
|   Member   |         12          |   140,778   |
|   Member   |          2          |   137,895   |
|   Member   |         13          |   120,565   |
|   Casual   |          6          |   117,001   |
|   Casual   |          7          |   111,215   |
|   Member   |         14          |   109,834   |
|   Casual   |          8          |   109,814   |
|   Casual   |          5          |   109,263   |
|   Casual   |          9          |   99,747    |
|   Casual   |          4          |   99,640    |
|   Casual   |         10          |   94,619    |
|   Member   |         15          |   93,179    |
|   Member   |         16          |   85,766    |
|   Casual   |         11          |   83,892    |
|   Casual   |         12          |   78,895    |
|   Member   |         17          |   72,344    |
|   Casual   |          3          |   69,383    |
|   Casual   |         13          |   69,309    |
|   Member   |         18          |   66,936    |
|   Casual   |         14          |   64,605    |
|   Member   |          1          |   63,214    |
|   Member   |         19          |   57,201    |
|   Casual   |         15          |   56,561    |
|   Casual   |         16          |   53,362    |
|   Member   |         20          |   53,028    |
|   Casual   |         17          |   46,608    |
|   Member   |         21          |   45,265    |
|   Casual   |         18          |   43,944    |
|   Member   |         22          |   42,372    |
|   Casual   |         19          |   39,346    |
|   Casual   |          2          |   38,702    |
|   Casual   |         20          |   36,864    |
|   Member   |         23          |   36,684    |
|   Member   |         24          |   34,276    |
|   Casual   |          1          |   33,817    |
|   Casual   |         21          |   32,886    |
|   Casual   |         22          |   31,473    |
|   Member   |         25          |   29,709    |
|   Casual   |         23          |   28,026    |
|   Member   |         26          |   27,484    |
|   Casual   |         24          |   27,044    |
|   Member   |         27          |   24,194    |
|   Casual   |         25          |   23,719    |
|   Casual   |         26          |   22,686    |
|   Member   |         28          |   22,126    |
|   Casual   |         27          |   20,166    |
|   Member   |         29          |   19,683    |
|   Casual   |         28          |   19,487    |
|   Member   |         30          |   18,576    |
|   Casual   |         29          |   17,479    |
|   Casual   |         30          |   16,691    |
|   Member   |         31          |   16,437    |
|   Member   |         32          |   15,244    |
|   Casual   |         31          |   14,821    |
|   Casual   |         32          |   14,279    |
|   Member   |         33          |   13,566    |
|   Casual   |         33          |   13,092    |
|   Member   |         34          |   12,736    |
|   Casual   |         34          |   12,299    |
|   Member   |         35          |   11,194    |
|   Casual   |         35          |   11,097    |
|   Casual   |         36          |   10,773    |
|   Member   |         36          |   10,408    |
|   Casual   |         37          |    9,999    |
|   Casual   |         38          |    9,563    |
|   Member   |         37          |    9,340    |
|   Member   |         38          |    9,101    |
|   Casual   |         39          |    8,691    |
|   Casual   |         40          |    8,351    |
|   Member   |         39          |    8,067    |
|   Casual   |         41          |    7,851    |
|   Member   |         40          |    7,586    |
|   Casual   |         42          |    7,470    |
|   Casual   |         43          |    6,904    |
|   Casual   |         44          |    6,785    |
|   Member   |         41          |    6,672    |
|   Member   |         42          |    6,287    |
|   Casual   |         45          |    6,163    |
|   Casual   |         46          |    6,142    |
|   Casual   |         48          |    5,558    |
|   Casual   |         47          |    5,527    |
|   Member   |         43          |    5,369    |
|   Casual   |         49          |    5,232    |
|   Casual   |         50          |    5,102    |
|   Member   |         44          |    5,053    |
|   Casual   |         51          |    4,725    |
|   Casual   |         52          |    4,656    |
|   Casual   |         54          |    4,384    |
|   Member   |         45          |    4,326    |
|   Casual   |         53          |    4,300    |
|   Casual   |         56          |    4,028    |
|   Casual   |         55          |    4,019    |
|   Member   |         46          |    3,718    |
|   Casual   |         58          |    3,638    |
|   Casual   |         57          |    3,560    |
|   Casual   |         60          |    3,460    |
|   Casual   |         59          |    3,399    |
|   Member   |         47          |    3,062    |
|   Member   |         48          |    2,889    |
|   Member   |         49          |    2,530    |
|   Member   |         50          |    2,305    |
|   Member   |         51          |    1,941    |
|   Member   |         52          |    1,869    |
|   Member   |         53          |    1,744    |
|   Member   |         54          |    1,563    |
|   Member   |         55          |    1,452    |
|   Member   |         56          |    1,315    |
|   Member   |         57          |    1,232    |
|   Member   |         58          |    1,151    |
|   Member   |         59          |     949     |
|   Member   |         60          |     930     |

Total Trips by Riders Type Per Weekday
