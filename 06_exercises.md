---
title: 'Weekly Exercises #6'
author: "RACHEL PERCY"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(RColorBrewer)  # for color palettes
library(ggthemes)      # for more themes (including theme_map())
library(patchwork)     # for nicely combining ggplot2 graphs  
library(gt)            # for creating nice tables
library(rvest)         # for scraping data
library(robotstxt)     # for checking if you can scrape data
library("scales")
theme_set(theme_minimal())
```


```r
# Lisa's garden data
data("garden_harvest")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises from tutorial

1. Read in the fake garden harvest data. Find the data [here](https://github.com/llendway/scraping_etc/blob/main/2020_harvest.csv) and click on the `Raw` button to get a direct link to the data. After reading in the data, do one of the quick checks mentioned in the tutorial.


```r
raw_harvest <- read_csv("https://raw.githubusercontent.com/llendway/scraping_etc/main/2020_harvest.csv", 
    col_types = cols(date = col_date(format = "%m/%d/%y"), 
        weight = col_number()), skip = 2)

raw_harvest %>% 
  mutate(across(where(is.character), as.factor)) %>% 
  summary()
```

```
##    ...1             vegetable                    variety   
##  Mode:logical   tomatoes :232   grape                : 37  
##  NA's:685       lettuce  : 68   Romanesco            : 34  
##                 beans    : 38   pickling             : 32  
##                 zucchini : 34   Lettuce Mixture      : 28  
##                 cucumbers: 32   Bonny Best           : 26  
##                 peas     : 27   Farmer's Market Blend: 26  
##                 (Other)  :254   (Other)              :502  
##       date                weight       units    
##  Min.   :2020-06-06   Min.   :   2   grams:685  
##  1st Qu.:2020-07-21   1st Qu.:  87              
##  Median :2020-08-09   Median : 252              
##  Mean   :2020-08-08   Mean   : 504              
##  3rd Qu.:2020-08-26   3rd Qu.: 599              
##  Max.   :2020-10-03   Max.   :7350              
##                       NA's   :4
```

  
2. Read in this [data](https://www.kaggle.com/heeraldedhia/groceries-dataset) from the kaggle website. You will need to download the data first. Save it to your project/repo folder. Do some quick checks of the data to assure it has been read in appropriately.


```r
groceries <- read_csv("Groceries_dataset.csv")

groceries %>% 
  mutate(across(where(is.character), as.factor)) %>% 
  summary()
```

```
##  Member_number          Date               itemDescription 
##  Min.   :1000   21-01-2015:   96   whole milk      : 2502  
##  1st Qu.:2002   21-07-2015:   93   other vegetables: 1898  
##  Median :3005   08-08-2015:   92   rolls/buns      : 1716  
##  Mean   :3004   29-11-2015:   92   soda            : 1514  
##  3rd Qu.:4007   30-04-2015:   91   yogurt          : 1334  
##  Max.   :5000   26-03-2015:   88   root vegetables : 1071  
##                 (Other)   :38213   (Other)         :28730
```


3. Create a table using `gt` with data from your project or from the `garden_harvest` data if your project data aren't ready. Use at least 3 `gt()` functions.


```r
solar_data <- read_csv("mnsolar5.csv")
```


```r
royalton_data <- solar_data %>% 
  select(City, Utility, `Customer Type`, capacity_MW) %>% 
  filter(City == "Royalton")
```


```r
royalton_data %>% 
  gt(
    rowname_col = "row",
    groupname_col = "Utility"
  ) %>% 
  tab_header(
    title = "Solar Capacity for Royalton, MN",
    subtitle = md("Uses the `mnsolar` dataset and the **gt package**")) %>% 
  data_color(columns = c(`Customer Type`),
             colors =  scales::col_factor(palette = "viridis", 
                                           domain = NULL),
             alpha = .7)
```

```{=html}
<div id="vbfnnggubh" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#vbfnnggubh .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#vbfnnggubh .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#vbfnnggubh .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#vbfnnggubh .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#vbfnnggubh .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#vbfnnggubh .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#vbfnnggubh .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#vbfnnggubh .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#vbfnnggubh .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#vbfnnggubh .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#vbfnnggubh .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#vbfnnggubh .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#vbfnnggubh .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#vbfnnggubh .gt_from_md > :first-child {
  margin-top: 0;
}

#vbfnnggubh .gt_from_md > :last-child {
  margin-bottom: 0;
}

#vbfnnggubh .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#vbfnnggubh .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#vbfnnggubh .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#vbfnnggubh .gt_row_group_first td {
  border-top-width: 2px;
}

#vbfnnggubh .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#vbfnnggubh .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#vbfnnggubh .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#vbfnnggubh .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#vbfnnggubh .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#vbfnnggubh .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#vbfnnggubh .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#vbfnnggubh .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#vbfnnggubh .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#vbfnnggubh .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#vbfnnggubh .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#vbfnnggubh .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#vbfnnggubh .gt_left {
  text-align: left;
}

#vbfnnggubh .gt_center {
  text-align: center;
}

#vbfnnggubh .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#vbfnnggubh .gt_font_normal {
  font-weight: normal;
}

#vbfnnggubh .gt_font_bold {
  font-weight: bold;
}

#vbfnnggubh .gt_font_italic {
  font-style: italic;
}

#vbfnnggubh .gt_super {
  font-size: 65%;
}

#vbfnnggubh .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#vbfnnggubh .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#vbfnnggubh .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#vbfnnggubh .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#vbfnnggubh .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="3" class="gt_heading gt_title gt_font_normal" style>Solar Capacity for Royalton, MN</th>
    </tr>
    <tr>
      <th colspan="3" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style>Uses the <code>mnsolar</code> dataset and the <strong>gt package</strong></th>
    </tr>
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">City</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">Customer Type</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">capacity_MW</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr class="gt_group_heading_row">
      <td colspan="3" class="gt_group_heading">Crow Wing Power</td>
    </tr>
    <tr class="gt_row_group_first"><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.03998</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(68,1,84,0.7); color: #FFFFFF;">commercial</td>
<td class="gt_row gt_right">0.03995</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.03850</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.02700</td></tr>
    <tr class="gt_group_heading_row">
      <td colspan="3" class="gt_group_heading">Minnesota Power</td>
    </tr>
    <tr class="gt_row_group_first"><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(68,1,84,0.7); color: #FFFFFF;">commercial</td>
<td class="gt_row gt_right">0.01000</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.01029</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.00492</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.00633</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.00492</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.00392</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.02000</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.01440</td></tr>
    <tr><td class="gt_row gt_left">Royalton</td>
<td class="gt_row gt_left" style="background-color: rgba(253,231,37,0.7); color: #000000;">residential</td>
<td class="gt_row gt_right">0.02400</td></tr>
  </tbody>
  
  
</table>
</div>
```






4. CHALLENGE (not graded): Write code to replicate the table shown below (open the .html file to see it) created from the `garden_harvest` data as best as you can. When you get to coloring the cells, I used the following line of code for the `colors` argument:
  

```r
colors = scales::col_numeric(
      palette = paletteer::paletteer_d(
        palette = "RColorBrewer::YlGn"
      ) %>% as.character()
```


  
5. Use `patchwork` operators and functions to combine at least two graphs using your project data or `garden_harvest` data if your project data aren't read.


```r
graph1 <- garden_harvest %>% 
  group_by(date) %>% 
  summarise(total_daily_harvest = sum(weight * 0.00220462)) %>% 
  mutate(day_lag = lag(total_daily_harvest),
         daily_percent_change = (total_daily_harvest/day_lag)*100) %>% 
  replace_na(list(day_lag = 0, daily_change = 0)) %>% 
  ungroup() %>% 
  ggplot(aes(x = date)) +
  geom_line(aes(y = daily_percent_change),
            color = "darkgreen",
            size = 1.05) +
  labs(x = "", y = "", title = "Daily Percent Change of Daily Harvest Over the Season",
       subtitle = "Week 3 garden graph") +
  scale_y_continuous(labels = comma) +
  theme(panel.grid.major.x = element_blank(),
        panel.grid.minor.x = element_blank())



graph2 <- garden_harvest %>% 
  group_by(date) %>% 
  summarise(total_daily_harvest = sum(weight * 0.00220462)) %>% 
  mutate(day_lag = lag(total_daily_harvest),
         daily_percent_change = ((total_daily_harvest-day_lag)/day_lag)) %>% 
  ungroup() %>% 
  ggplot(aes(x = date)) +
  geom_line(aes(y = daily_percent_change),
            color = "darkgreen",
            size = 1.05) +
  labs(x = "", 
       y = "", 
       title = "Daily Percent Change of Harvest Over the Season",
       subtitle = "Week 4 garden graph",
       caption = "Perfect Garden Graph by Rachel Percy | April 12, 2022") +
  scale_y_continuous(labels = percent) +
  theme(panel.grid.major.x = element_blank(),
        panel.grid.minor.x = element_blank()) +
  coord_cartesian(ylim = c(-10,100))


graph1/graph2 +
  plot_annotation(title = "Look at these cool plots") 
```

![](06_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->



  
## Webscraping exercise (also from tutorial)

Use the data from the [Macalester Registrar's Fall 2017 Class Schedule](https://www.macalester.edu/registrar/schedules/2017fall/class-schedule/#crs10008) to complete all these exercises.

6. Find the correct selectors for the following fields. Make sure that each matches 762 results:

  * Course Number:    .class-schedule-course-number
  * Course Name:      .class-schedule-course-title
  * Day:              .class-schedule-course-title+ .class-schedule-label
  * Time:             .class-schedule-label:nth-child(4)
  * Room:             .class-schedule-label:nth-child(5)
  * Instructor:       .class-schedule-label:nth-child(6)
  * Avail. / Max:     .class-schedule-label:nth-child(7)
  * General Education Requirements (make sure you only match 762; beware of the Mac copyright banner at the bottom of the page!):
                      #content p:nth-child(2)
  * Description:      .collapsed p:nth-child(1)

Then, put all this information into one dataset (tibble or data.frame) Do not include any extraneous information like the words "Instructor: " or "Time: ".


```r
fall2017 <- read_html("https://www.macalester.edu/registrar/schedules/2017fall/class-schedule/#crs10008")
```


```r
course_nums <- 
  fall2017 %>%
  html_elements(".class-schedule-course-number") %>%
  html_text2()
```


```r
course_names <- 
  fall2017 %>%
  html_elements(".class-schedule-course-title") %>%
  html_text2()
```


```r
course_days <- fall2017 %>%
  html_elements("td.class-schedule-label:nth-child(3)") %>% 
  html_text2() %>% 
  str_sub(start = 7)
head(course_days)
```

```
## [1] "W"   "MWF" "MWF" "MWF" "MWF" "MWF"
```


```r
course_times <- 
  fall2017 %>%
  html_nodes(".class-schedule-label:nth-child(4)") %>%
  html_text2() %>% 
  str_sub(start = 7)
head(course_times)
```

```
## [1] "07:00 pm-10:00 pm" "09:40 am-10:40 am" "02:20 pm-03:20 pm"
## [4] "09:40 am-10:40 am" "01:10 pm-02:10 pm" "10:50 am-11:50 am"
```


```r
course_room <-
  fall2017 %>% 
  html_nodes(".class-schedule-label:nth-child(5)") %>% 
  html_text2() %>% 
  str_sub(start = 7)
head(course_room)
```

```
## [1] "ARTCOM 102" "NEILL 111"  "OLRI 205"   "CARN 204"   "MAIN 010"  
## [6] "MAIN 001"
```


```r
course_instructors <-
  fall2017 %>% 
  html_nodes(".class-schedule-label:nth-child(6)") %>% 
  html_text2() %>% 
  str_sub(start = 13)
head(course_instructors)
```

```
## [1] "Gutierrez, Harris"      "Karin Aguilar-San Juan" "Nathan Titman"         
## [4] "Lesley Lavery"          "Crystal Moten"          "Crystal Moten"
```


```r
course_avail <-
  fall2017 %>% 
  html_nodes(".class-schedule-label:nth-child(7)") %>% 
  html_text2() %>%  
  str_sub(start = 14)
```


```r
course_ge <-
  fall2017 %>% 
  html_nodes("#content p:nth-child(2)") %>% 
  html_text2() %>% 
  str_sub(start = 35)
```


```r
course_description <-
  fall2017 %>% 
  html_nodes(".collapsed p:nth-child(1)") %>% 
  html_text2() %>%
  str_sub(start = 3)
```



```r
course_df <- tibble(Number = course_nums,
                    Name = course_names,
                    Days = course_days,
                    Time = course_times,
                    Room = course_room,
                    Instructors = course_instructors,
                    Availability = course_avail,
                    Gen_Ed = course_ge,
                    Description = course_description)
head(course_df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Number"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Name"],"name":[2],"type":["chr"],"align":["left"]},{"label":["Days"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Time"],"name":[4],"type":["chr"],"align":["left"]},{"label":["Room"],"name":[5],"type":["chr"],"align":["left"]},{"label":["Instructors"],"name":[6],"type":["chr"],"align":["left"]},{"label":["Availability"],"name":[7],"type":["chr"],"align":["left"]},{"label":["Gen_Ed"],"name":[8],"type":["chr"],"align":["left"]},{"label":["Description"],"name":[9],"type":["chr"],"align":["left"]}],"data":[{"1":"AMST 101-01","2":"Explorations of Race and Racism","3":"W","4":"07:00 pm-10:00 pm","5":"ARTCOM 102","6":"Gutierrez, Harris","7":"Closed -5 / 25","8":"U.S. Identities and Differences\\n","9":"The main objectives of this introductory course are: to explore the historical construction of racial categories in the United States; to understand the systemic impact of racism on contemporary social processes; to consider popular views about race in the light of emerging scholarship in the field; and to develop an ability to connect personal experiences to larger, collective realities. We will engage several questions as a group: What are the historical and sociological foundations of racial categories? When does focusing on race make someone racist? What is white privilege, and why does it matter? All students will be asked to think and write about their own racial identity. This course, or its equivalent, is required for majors and minors. (4 credits)\\n"},{"1":"AMST 103-01","2":"The Problems of Race in US Social Thought and Policy","3":"MWF","4":"09:40 am-10:40 am","5":"NEILL 111","6":"Karin Aguilar-San Juan","7":"0 / 16","8":"U.S. Identities and Differences\\nWriting WA\\n","9":"In this discussion-based and residential course, we will explore the paradox of a society in which people are increasingly aware of patterns of racism and yet still unable to see or explain how those systems and patterns are connected to everyday life. As awareness increases, why are we not able to develop effective or meaningful responses?\\n\\nOur interdisciplinary and integrative approach will employ multiple methods of inquiry and expression, including: self-reflective essays and maps; a scavenger hunt along University Avenue; library research; and deep, critical analysis of arguments about race/ethnicity/assimilation/multiculturalism.\\n\\nWe will practice engaging in open-ended conversations so that we might discover the questions that truly matter to each of us. To fulfill the WA general education writing requirement, this course will invite you to produce at least 20 pages of college-level writing through various assignments. Each writing assignment will strengthen your use of evidence and argumentation, and will involve drafts, feedback, in person conference, and revision.\\nClass meets MWF, 9:40 am - 10:40 am in Neill Hall 111\\nWriting designation: WA\\nLiving arrangements: Single gender rooms, co-ed floor.\\n"},{"1":"AMST 200-01","2":"Critical Methods for American Studies Research","3":"MWF","4":"02:20 pm-03:20 pm","5":"OLRI 205","6":"Nathan Titman","7":"13 / 20","8":"","9":"This course will introduce students to interdisciplinary research approaches to the study of race, ethnicity, and other categories of difference. Students will learn to conceptualize and design research projects, and will obtain hands-on experience in executing different methods. The course will also consider the critiques of systems of knowledge production and research approaches that have been informed by scholars from fields such as African American history, gender studies, and critical race studies, as well as from the disciplines. The goal is to develop an understanding of the assumptions embedded in many fields of inquiry, and to learn to apply critical approaches to important research questions.\\r"},{"1":"AMST 203-01","2":"Politics and Inequality: American Welfare State","3":"MWF","4":"09:40 am-10:40 am","5":"CARN 204","6":"Lesley Lavery","7":"Closed 0 / 25","8":"U.S. Identities and Differences\\nWriting WP\\n","9":"Americans, at least since the Founding era, have cherished the ideal of political equality. Unlike European nations, the United States did not inherit economic class distinctions from a feudal past. But time and again, American social reformers and mass movements have highlighted inconsistencies between the value of equality and the actual practice of democracy. Through the extension of rights to citizens who were previously excluded or treated as second-class citizens, such as women and African Americans, the polity has become more inclusive. But over the last three decades American citizens have grown increasingly unequal in terms of income and wealth. The central question posed by this course is the implications of such vast economic inequality for American democracy. Do these disparities between citizens curtail, limit, and perhaps threaten the functioning of genuinely representative governance? In this course will 1) Explore what other social scientists, mostly economists and sociologists, know about contemporary inequality, particularly in terms of its causes, manifestation, and socio-economic effects; 2) Consider the concept of inequality in political theory and in American political thought, and; 3) Examine the current relationship between economic inequality and each of three major aspects of the American political system: political voice, representation, and public policy. Cross-listed as Political Science 203. (4 Credits)\\n"},{"1":"AMST 219-01","2":"In Motion: African Americans in the United States","3":"MWF","4":"01:10 pm-02:10 pm","5":"MAIN 010","6":"Crystal Moten","7":"2 / 20","8":"U.S. Identities and Differences\\n","9":"In Motion is an introduction to modern African American History from slavery to contemporary times. In Motion emphasizes the idea that both African Americans and the stories of their lives in the United States are fluid, varied and continually being reinterpreted. Rather than a strict chronological survey, this course is organized thematically. Some of the important themes include movement/mobility/migration; work/labor; resistance to systems of oppression; gender/sexuality/culture/performance; politics/citizenship; and sites of (re)memory. While the course is geographically situated in the United States, we will also consider African American life, culture, thought and resistance in global perspectives. In this course, students will read important historical texts, both primary and secondary, engage in discussion, and write essays that ask them to critically engage the history of African Americans in the US. Cross-listed with History 219. 4 credits.\\r"},{"1":"AMST 229-01","2":"Narrating Black Women's Resistance","3":"MWF","4":"10:50 am-11:50 am","5":"MAIN 001","6":"Crystal Moten","7":"Closed 4 / 14","8":"","9":"This course examines traditions of 20th century African American women’s activism and the ways in which they have changed over time. Too often, the narrative of the “strong black woman” infuses stories of African American women’s resistance which, coupled with a culture of dissemblance, makes the inner workings of their lives difficult to imagine. This course, at its heart, seeks to uncover the motivations, both personal and political, behind African American women’s activism. It also aims to address the ways in which African American women have responded to the pressing social, economic, and political needs of their diverse communities. The course also asks students to consider narrative, voice and audience in historical writing, paying particular attention to the ways in which black women’s history has been written over the course of the twentieth century. Cross-listed with History 229 and Women's and Gender Studies 229. 4 credits.\\r"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>



7. Create a graph that shows the number of sections offered per department. Hint: The department is a substring of the course number - there are `str_XXX()` functions that can help. Yes, COMP and MATH are the same department, but for this exercise you can just show the results by four letter department code, e.g., with COMP and MATH separate.


```r
dept_sections <- course_df %>% 
  mutate(Department = str_sub(Number, start = 1, end = -7)) %>% 
  group_by(Department) %>% 
  summarise(sections = n())

ggplot(dept_sections) +
  geom_col(aes(y = fct_reorder(Department, sections),
               x = sections)) +
  labs(title = "Sections Offered by Each Department at Macalester (Fall 2017)",
       y = "",
       x = "") +
  scale_x_continuous(expand=c(0,0)) 
```

![](06_exercises_files/figure-html/unnamed-chunk-19-1.png)<!-- -->



8. Analyze the typical length of course names by department. To do so, create a new data table based on your courses data table, with the following changes:
  
  * New columns for the length of the title of a course and the length of the description of the course. Hint: `str_length`.  
  * Remove departments that have fewer than 10 sections of courses. To do so, group by department, then remove observations in groups with fewer than 10 sections (Hint: use filter with n()). Then `ungroup()` the data.  
  * Create a visualization of the differences across groups in lengths of course names or course descriptions. Think carefully about the visualization you should be using!


```r
course_df2 <- course_df %>% 
  mutate(name_length = str_length(course_names),
         description_length = str_length(course_description),
         department = str_sub(Number, start = 1, end = -7)) %>% 
  group_by(department) %>% 
  filter(n() >= 10) %>% 
  ungroup()

head(course_df2)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Number"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Name"],"name":[2],"type":["chr"],"align":["left"]},{"label":["Days"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Time"],"name":[4],"type":["chr"],"align":["left"]},{"label":["Room"],"name":[5],"type":["chr"],"align":["left"]},{"label":["Instructors"],"name":[6],"type":["chr"],"align":["left"]},{"label":["Availability"],"name":[7],"type":["chr"],"align":["left"]},{"label":["Gen_Ed"],"name":[8],"type":["chr"],"align":["left"]},{"label":["Description"],"name":[9],"type":["chr"],"align":["left"]},{"label":["name_length"],"name":[10],"type":["int"],"align":["right"]},{"label":["description_length"],"name":[11],"type":["int"],"align":["right"]},{"label":["department"],"name":[12],"type":["chr"],"align":["left"]}],"data":[{"1":"AMST 101-01","2":"Explorations of Race and Racism","3":"W","4":"07:00 pm-10:00 pm","5":"ARTCOM 102","6":"Gutierrez, Harris","7":"Closed -5 / 25","8":"U.S. Identities and Differences\\n","9":"The main objectives of this introductory course are: to explore the historical construction of racial categories in the United States; to understand the systemic impact of racism on contemporary social processes; to consider popular views about race in the light of emerging scholarship in the field; and to develop an ability to connect personal experiences to larger, collective realities. We will engage several questions as a group: What are the historical and sociological foundations of racial categories? When does focusing on race make someone racist? What is white privilege, and why does it matter? All students will be asked to think and write about their own racial identity. This course, or its equivalent, is required for majors and minors. (4 credits)\\n","10":"31","11":"767","12":"AMST"},{"1":"AMST 103-01","2":"The Problems of Race in US Social Thought and Policy","3":"MWF","4":"09:40 am-10:40 am","5":"NEILL 111","6":"Karin Aguilar-San Juan","7":"0 / 16","8":"U.S. Identities and Differences\\nWriting WA\\n","9":"In this discussion-based and residential course, we will explore the paradox of a society in which people are increasingly aware of patterns of racism and yet still unable to see or explain how those systems and patterns are connected to everyday life. As awareness increases, why are we not able to develop effective or meaningful responses?\\n\\nOur interdisciplinary and integrative approach will employ multiple methods of inquiry and expression, including: self-reflective essays and maps; a scavenger hunt along University Avenue; library research; and deep, critical analysis of arguments about race/ethnicity/assimilation/multiculturalism.\\n\\nWe will practice engaging in open-ended conversations so that we might discover the questions that truly matter to each of us. To fulfill the WA general education writing requirement, this course will invite you to produce at least 20 pages of college-level writing through various assignments. Each writing assignment will strengthen your use of evidence and argumentation, and will involve drafts, feedback, in person conference, and revision.\\nClass meets MWF, 9:40 am - 10:40 am in Neill Hall 111\\nWriting designation: WA\\nLiving arrangements: Single gender rooms, co-ed floor.\\n","10":"52","11":"1224","12":"AMST"},{"1":"AMST 200-01","2":"Critical Methods for American Studies Research","3":"MWF","4":"02:20 pm-03:20 pm","5":"OLRI 205","6":"Nathan Titman","7":"13 / 20","8":"","9":"This course will introduce students to interdisciplinary research approaches to the study of race, ethnicity, and other categories of difference. Students will learn to conceptualize and design research projects, and will obtain hands-on experience in executing different methods. The course will also consider the critiques of systems of knowledge production and research approaches that have been informed by scholars from fields such as African American history, gender studies, and critical race studies, as well as from the disciplines. The goal is to develop an understanding of the assumptions embedded in many fields of inquiry, and to learn to apply critical approaches to important research questions.\\r","10":"46","11":"712","12":"AMST"},{"1":"AMST 203-01","2":"Politics and Inequality: American Welfare State","3":"MWF","4":"09:40 am-10:40 am","5":"CARN 204","6":"Lesley Lavery","7":"Closed 0 / 25","8":"U.S. Identities and Differences\\nWriting WP\\n","9":"Americans, at least since the Founding era, have cherished the ideal of political equality. Unlike European nations, the United States did not inherit economic class distinctions from a feudal past. But time and again, American social reformers and mass movements have highlighted inconsistencies between the value of equality and the actual practice of democracy. Through the extension of rights to citizens who were previously excluded or treated as second-class citizens, such as women and African Americans, the polity has become more inclusive. But over the last three decades American citizens have grown increasingly unequal in terms of income and wealth. The central question posed by this course is the implications of such vast economic inequality for American democracy. Do these disparities between citizens curtail, limit, and perhaps threaten the functioning of genuinely representative governance? In this course will 1) Explore what other social scientists, mostly economists and sociologists, know about contemporary inequality, particularly in terms of its causes, manifestation, and socio-economic effects; 2) Consider the concept of inequality in political theory and in American political thought, and; 3) Examine the current relationship between economic inequality and each of three major aspects of the American political system: political voice, representation, and public policy. Cross-listed as Political Science 203. (4 Credits)\\n","10":"47","11":"1457","12":"AMST"},{"1":"AMST 219-01","2":"In Motion: African Americans in the United States","3":"MWF","4":"01:10 pm-02:10 pm","5":"MAIN 010","6":"Crystal Moten","7":"2 / 20","8":"U.S. Identities and Differences\\n","9":"In Motion is an introduction to modern African American History from slavery to contemporary times. In Motion emphasizes the idea that both African Americans and the stories of their lives in the United States are fluid, varied and continually being reinterpreted. Rather than a strict chronological survey, this course is organized thematically. Some of the important themes include movement/mobility/migration; work/labor; resistance to systems of oppression; gender/sexuality/culture/performance; politics/citizenship; and sites of (re)memory. While the course is geographically situated in the United States, we will also consider African American life, culture, thought and resistance in global perspectives. In this course, students will read important historical texts, both primary and secondary, engage in discussion, and write essays that ask them to critically engage the history of African Americans in the US. Cross-listed with History 219. 4 credits.\\r","10":"49","11":"965","12":"AMST"},{"1":"AMST 229-01","2":"Narrating Black Women's Resistance","3":"MWF","4":"10:50 am-11:50 am","5":"MAIN 001","6":"Crystal Moten","7":"Closed 4 / 14","8":"","9":"This course examines traditions of 20th century African American women’s activism and the ways in which they have changed over time. Too often, the narrative of the “strong black woman” infuses stories of African American women’s resistance which, coupled with a culture of dissemblance, makes the inner workings of their lives difficult to imagine. This course, at its heart, seeks to uncover the motivations, both personal and political, behind African American women’s activism. It also aims to address the ways in which African American women have responded to the pressing social, economic, and political needs of their diverse communities. The course also asks students to consider narrative, voice and audience in historical writing, paying particular attention to the ways in which black women’s history has been written over the course of the twentieth century. Cross-listed with History 229 and Women's and Gender Studies 229. 4 credits.\\r","10":"34","11":"948","12":"AMST"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>



```r
course_means <- course_df2 %>% 
  group_by(department) %>%
  summarize(mean_name_length = mean(name_length), 
            mean_desc_length = mean(description_length)) %>% 
  mutate(diff_name_length = (mean_name_length-28))

mean(course_means$mean_name_length)
```

```
## [1] 27.95501
```

```r
#27.95

course_means %>% 
ggplot()+
  geom_col(aes(y = fct_reorder(department, diff_name_length),
               x = diff_name_length,
               stat = "identity")) +
  labs(title = "The Difference in Department Class Names Length \nfrom the Mean Name Length (28)",
       y = "",
       x = "") +
  scale_x_continuous(expand=c(0,0)) 
```

![](06_exercises_files/figure-html/unnamed-chunk-21-1.png)<!-- -->



**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
