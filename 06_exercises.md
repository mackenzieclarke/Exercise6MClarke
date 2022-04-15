---
title: 'Weekly Exercises #6'
author: "Mackenzie Clarke"
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
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(gifski)        # for creating the gif (don't need to load this library every time,but need it installed)
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
library(patchwork)     # for nicely combining ggplot2 graphs  
library(gt)            # for creating nice tables
library(rvest)         # for scraping data
library(robotstxt)     # for checking if you can scrape data
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
datagarden <- read_csv("https://raw.githubusercontent.com/llendway/scraping_etc/main/2020_harvest.csv", 
    col_types = cols(...1 = col_skip(), date = col_character(), 
        weight = col_number()), 
    na = "MISSING", 
    skip = 2)
```

  
2. Read in this [data](https://www.kaggle.com/heeraldedhia/groceries-dataset) from the kaggle website. You will need to download the data first. Save it to your project/repo folder. Do some quick checks of the data to assure it has been read in appropriately.


```r
Groceries_dataset <- read_csv("Groceries_dataset.csv", 
    col_types = cols(Date = col_character()))
```


3. Create a table using `gt` with data from your project or from the `garden_harvest` data if your project data aren't ready. Use at least 3 `gt()` functions.


```r
datagarden %>% 
  gt() %>% 
  tab_header("Garden Harvest Data") %>% 
  tab_source_note("Data Courtesy of Lisa Lendway") %>%
  tab_style(cell_text(weight = "bold"),
            locations = cells_body(
            columns = 2,
            rows = 1:5))
```

```{=html}
<div id="ilnexcbvvj" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#ilnexcbvvj .gt_table {
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

#ilnexcbvvj .gt_heading {
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

#ilnexcbvvj .gt_title {
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

#ilnexcbvvj .gt_subtitle {
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

#ilnexcbvvj .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ilnexcbvvj .gt_col_headings {
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

#ilnexcbvvj .gt_col_heading {
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

#ilnexcbvvj .gt_column_spanner_outer {
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

#ilnexcbvvj .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#ilnexcbvvj .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#ilnexcbvvj .gt_column_spanner {
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

#ilnexcbvvj .gt_group_heading {
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

#ilnexcbvvj .gt_empty_group_heading {
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

#ilnexcbvvj .gt_from_md > :first-child {
  margin-top: 0;
}

#ilnexcbvvj .gt_from_md > :last-child {
  margin-bottom: 0;
}

#ilnexcbvvj .gt_row {
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

#ilnexcbvvj .gt_stub {
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

#ilnexcbvvj .gt_stub_row_group {
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

#ilnexcbvvj .gt_row_group_first td {
  border-top-width: 2px;
}

#ilnexcbvvj .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ilnexcbvvj .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#ilnexcbvvj .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#ilnexcbvvj .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ilnexcbvvj .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ilnexcbvvj .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#ilnexcbvvj .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#ilnexcbvvj .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ilnexcbvvj .gt_footnotes {
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

#ilnexcbvvj .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ilnexcbvvj .gt_sourcenotes {
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

#ilnexcbvvj .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ilnexcbvvj .gt_left {
  text-align: left;
}

#ilnexcbvvj .gt_center {
  text-align: center;
}

#ilnexcbvvj .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#ilnexcbvvj .gt_font_normal {
  font-weight: normal;
}

#ilnexcbvvj .gt_font_bold {
  font-weight: bold;
}

#ilnexcbvvj .gt_font_italic {
  font-style: italic;
}

#ilnexcbvvj .gt_super {
  font-size: 65%;
}

#ilnexcbvvj .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#ilnexcbvvj .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#ilnexcbvvj .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#ilnexcbvvj .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#ilnexcbvvj .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="5" class="gt_heading gt_title gt_font_normal gt_bottom_border" style>Garden Harvest Data</th>
    </tr>
    
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">vegetable</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">variety</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">date</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">weight</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">units</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left" style="font-weight: bold;">reseed</td>
<td class="gt_row gt_left">6/6/20</td>
<td class="gt_row gt_right">20</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left" style="font-weight: bold;">Garden Party Mix</td>
<td class="gt_row gt_left">6/6/20</td>
<td class="gt_row gt_right">36</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left" style="font-weight: bold;">reseed</td>
<td class="gt_row gt_left">6/8/20</td>
<td class="gt_row gt_right">15</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left" style="font-weight: bold;">reseed</td>
<td class="gt_row gt_left">6/9/20</td>
<td class="gt_row gt_right">10</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left" style="font-weight: bold;">Garden Party Mix</td>
<td class="gt_row gt_left">6/11/20</td>
<td class="gt_row gt_right">67</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/11/20</td>
<td class="gt_row gt_right">12</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/11/20</td>
<td class="gt_row gt_right">9</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">leaves</td>
<td class="gt_row gt_left">6/11/20</td>
<td class="gt_row gt_right">8</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">6/13/20</td>
<td class="gt_row gt_right">53</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">NA</td>
<td class="gt_row gt_left">6/13/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/13/20</td>
<td class="gt_row gt_right">14</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">6/13/20</td>
<td class="gt_row gt_right">10</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/17/20</td>
<td class="gt_row gt_right">NA</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/17/20</td>
<td class="gt_row gt_right">58</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">6/17/20</td>
<td class="gt_row gt_right">8</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/17/20</td>
<td class="gt_row gt_right">121</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">chives</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">6/17/20</td>
<td class="gt_row gt_right">8</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">6/18/20</td>
<td class="gt_row gt_right">40</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/18/20</td>
<td class="gt_row gt_right">47</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">NA</td>
<td class="gt_row gt_left">6/18/20</td>
<td class="gt_row gt_right">59</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">leaves</td>
<td class="gt_row gt_left">6/18/20</td>
<td class="gt_row gt_right">25</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/19/20</td>
<td class="gt_row gt_right">58</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/19/20</td>
<td class="gt_row gt_right">39</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">leaves</td>
<td class="gt_row gt_left">6/19/20</td>
<td class="gt_row gt_right">11</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/19/20</td>
<td class="gt_row gt_right">NA</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">22</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">25</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">18</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">16</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">71</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">148</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">asparagus</td>
<td class="gt_row gt_left">asparagus</td>
<td class="gt_row gt_left">6/20/20</td>
<td class="gt_row gt_right">20</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">37</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">71</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">95</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">51</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">13</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">leaves</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">57</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">6/21/20</td>
<td class="gt_row gt_right">60</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/22/20</td>
<td class="gt_row gt_right">37</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/22/20</td>
<td class="gt_row gt_right">52</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/22/20</td>
<td class="gt_row gt_right">40</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">6/22/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">6/22/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/22/20</td>
<td class="gt_row gt_right">18</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">6/23/20</td>
<td class="gt_row gt_right">40</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/23/20</td>
<td class="gt_row gt_right">165</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/23/20</td>
<td class="gt_row gt_right">41</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cilantro</td>
<td class="gt_row gt_left">cilantro</td>
<td class="gt_row gt_left">6/23/20</td>
<td class="gt_row gt_right">2</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">6/23/20</td>
<td class="gt_row gt_right">5</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/24/20</td>
<td class="gt_row gt_right">34</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/24/20</td>
<td class="gt_row gt_right">122</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/25/20</td>
<td class="gt_row gt_right">22</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/25/20</td>
<td class="gt_row gt_right">30</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">6/26/20</td>
<td class="gt_row gt_right">17</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/26/20</td>
<td class="gt_row gt_right">425</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/27/20</td>
<td class="gt_row gt_right">52</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">6/27/20</td>
<td class="gt_row gt_right">89</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">NA</td>
<td class="gt_row gt_left">6/27/20</td>
<td class="gt_row gt_right">60</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">6/27/20</td>
<td class="gt_row gt_right">333</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/28/20</td>
<td class="gt_row gt_right">793</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/28/20</td>
<td class="gt_row gt_right">99</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/28/20</td>
<td class="gt_row gt_right">111</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/29/20</td>
<td class="gt_row gt_right">58</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">mustard greens</td>
<td class="gt_row gt_left">6/29/20</td>
<td class="gt_row gt_right">23</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">6/29/20</td>
<td class="gt_row gt_right">625</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">6/29/20</td>
<td class="gt_row gt_right">561</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">6/29/20</td>
<td class="gt_row gt_right">30</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">6/29/20</td>
<td class="gt_row gt_right">82</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">6/30/20</td>
<td class="gt_row gt_right">32</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">6/30/20</td>
<td class="gt_row gt_right">80</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/1/20</td>
<td class="gt_row gt_right">60</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">7/2/20</td>
<td class="gt_row gt_right">144</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">7/2/20</td>
<td class="gt_row gt_right">16</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">7/2/20</td>
<td class="gt_row gt_right">798</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/2/20</td>
<td class="gt_row gt_right">743</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/3/20</td>
<td class="gt_row gt_right">217</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">7/3/20</td>
<td class="gt_row gt_right">216</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">7/3/20</td>
<td class="gt_row gt_right">88</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/3/20</td>
<td class="gt_row gt_right">9</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/4/20</td>
<td class="gt_row gt_right">285</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">7/4/20</td>
<td class="gt_row gt_right">457</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/4/20</td>
<td class="gt_row gt_right">147</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/6/20</td>
<td class="gt_row gt_right">17</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/6/20</td>
<td class="gt_row gt_right">175</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/6/20</td>
<td class="gt_row gt_right">NA</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">7/6/20</td>
<td class="gt_row gt_right">NA</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">7/6/20</td>
<td class="gt_row gt_right">433</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/6/20</td>
<td class="gt_row gt_right">48</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/7/20</td>
<td class="gt_row gt_right">67</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Gourmet Golden</td>
<td class="gt_row gt_left">7/7/20</td>
<td class="gt_row gt_right">62</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">7/7/20</td>
<td class="gt_row gt_right">10</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">7/7/20</td>
<td class="gt_row gt_right">43</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/7/20</td>
<td class="gt_row gt_right">11</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/7/20</td>
<td class="gt_row gt_right">13</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">75</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">252</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">178</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">39</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">181</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Gourmet Golden</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">83</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">96</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">7/8/20</td>
<td class="gt_row gt_right">75</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/9/20</td>
<td class="gt_row gt_right">61</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/9/20</td>
<td class="gt_row gt_right">131</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/9/20</td>
<td class="gt_row gt_right">140</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">7/9/20</td>
<td class="gt_row gt_right">69</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/9/20</td>
<td class="gt_row gt_right">78</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/10/20</td>
<td class="gt_row gt_right">61</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/10/20</td>
<td class="gt_row gt_right">150</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">60</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">77</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">79</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">105</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">701</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/11/20</td>
<td class="gt_row gt_right">24</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/12/20</td>
<td class="gt_row gt_right">130</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">7/12/20</td>
<td class="gt_row gt_right">89</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/12/20</td>
<td class="gt_row gt_right">492</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/12/20</td>
<td class="gt_row gt_right">83</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">47</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">145</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">50</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">85</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">53</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">137</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">40</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/13/20</td>
<td class="gt_row gt_right">443</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">7/14/20</td>
<td class="gt_row gt_right">128</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/14/20</td>
<td class="gt_row gt_right">152</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">7/14/20</td>
<td class="gt_row gt_right">207</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/14/20</td>
<td class="gt_row gt_right">526</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/14/20</td>
<td class="gt_row gt_right">152</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/15/20</td>
<td class="gt_row gt_right">393</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/15/20</td>
<td class="gt_row gt_right">743</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/15/20</td>
<td class="gt_row gt_right">1057</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">7/15/20</td>
<td class="gt_row gt_right">39</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">7/16/20</td>
<td class="gt_row gt_right">29</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Farmer's Market Blend</td>
<td class="gt_row gt_left">7/16/20</td>
<td class="gt_row gt_right">61</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Delicious Duo</td>
<td class="gt_row gt_left">7/16/20</td>
<td class="gt_row gt_right">50</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/17/20</td>
<td class="gt_row gt_right">88</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cilantro</td>
<td class="gt_row gt_left">cilantro</td>
<td class="gt_row gt_left">7/17/20</td>
<td class="gt_row gt_right">33</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/17/20</td>
<td class="gt_row gt_right">16</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">7/17/20</td>
<td class="gt_row gt_right">20</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/17/20</td>
<td class="gt_row gt_right">347</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/18/20</td>
<td class="gt_row gt_right">77</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">7/18/20</td>
<td class="gt_row gt_right">172</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">7/18/20</td>
<td class="gt_row gt_right">61</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/18/20</td>
<td class="gt_row gt_right">81</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/18/20</td>
<td class="gt_row gt_right">294</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/18/20</td>
<td class="gt_row gt_right">660</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">7/19/20</td>
<td class="gt_row gt_right">113</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/19/20</td>
<td class="gt_row gt_right">531</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/19/20</td>
<td class="gt_row gt_right">344</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/19/20</td>
<td class="gt_row gt_right">37</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Magnolia Blossom</td>
<td class="gt_row gt_left">7/19/20</td>
<td class="gt_row gt_right">140</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">134</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">179</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peas</td>
<td class="gt_row gt_left">Super Sugar Snap</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">336</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Gourmet Golden</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">107</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">128</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">hot peppers</td>
<td class="gt_row gt_left">thai</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">12</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">519</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">hot peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">559</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">197</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">123</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">178</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">7/20/20</td>
<td class="gt_row gt_right">102</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">110</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">86</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">137</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">339</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">21</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">21</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/21/20</td>
<td class="gt_row gt_right">7</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/22/20</td>
<td class="gt_row gt_right">76</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/22/20</td>
<td class="gt_row gt_right">351</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/22/20</td>
<td class="gt_row gt_right">655</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/22/20</td>
<td class="gt_row gt_right">23</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/23/20</td>
<td class="gt_row gt_right">129</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">King Midas</td>
<td class="gt_row gt_left">7/23/20</td>
<td class="gt_row gt_right">56</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">7/23/20</td>
<td class="gt_row gt_right">466</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">7/23/20</td>
<td class="gt_row gt_right">91</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/23/20</td>
<td class="gt_row gt_right">130</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">525</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">31</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">140</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">247</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">220</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">1321</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">100</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">32</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">93</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">16</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">3</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">68</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">King Midas</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">178</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Dragon</td>
<td class="gt_row gt_left">7/24/20</td>
<td class="gt_row gt_right">80</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">7/25/20</td>
<td class="gt_row gt_right">463</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/25/20</td>
<td class="gt_row gt_right">106</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">7/25/20</td>
<td class="gt_row gt_right">121</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/25/20</td>
<td class="gt_row gt_right">901</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/26/20</td>
<td class="gt_row gt_right">81</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">7/26/20</td>
<td class="gt_row gt_right">148</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">1542</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">728</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">785</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">113</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">29</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">801</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">99</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">49</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Gourmet Golden</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">149</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">radish</td>
<td class="gt_row gt_left">Garden Party Mix</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">39</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">King Midas</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">174</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">129</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">broccoli</td>
<td class="gt_row gt_left">Yod Fah</td>
<td class="gt_row gt_left">7/27/20</td>
<td class="gt_row gt_right">372</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">King Midas</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">160</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">611</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">203</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">312</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">315</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">131</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">91</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/28/20</td>
<td class="gt_row gt_right">76</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">153</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">442</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">240</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">209</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">73</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">40</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">457</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">514</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">305</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">7/29/20</td>
<td class="gt_row gt_right">280</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">91</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">101</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">94</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">116</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">King Midas</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">107</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/30/20</td>
<td class="gt_row gt_right">626</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">307</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">197</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">633</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">290</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">100</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">1215</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">592</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">strawberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">23</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">31</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">107</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">7/31/20</td>
<td class="gt_row gt_right">174</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">435</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">320</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">619</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">97</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">436</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">168</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">164</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">1130</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">137</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">74</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cilantro</td>
<td class="gt_row gt_left">cilantro</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">17</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Delicious Duo</td>
<td class="gt_row gt_left">8/1/20</td>
<td class="gt_row gt_right">182</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">1175</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">509</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">857</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">336</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">156</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">211</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/2/20</td>
<td class="gt_row gt_right">102</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/3/20</td>
<td class="gt_row gt_right">308</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/3/20</td>
<td class="gt_row gt_right">252</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/3/20</td>
<td class="gt_row gt_right">1155</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/3/20</td>
<td class="gt_row gt_right">572</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/3/20</td>
<td class="gt_row gt_right">65</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">8/3/20</td>
<td class="gt_row gt_right">383</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">387</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">231</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">73</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">339</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">118</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">270</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">162</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">56</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">192</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">195</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">green</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">81</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">87</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">hot peppers</td>
<td class="gt_row gt_left">thai</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">24</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">hot peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">40</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">44</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/4/20</td>
<td class="gt_row gt_right">427</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">563</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">290</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">781</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">223</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">382</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">217</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">67</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">41</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/5/20</td>
<td class="gt_row gt_right">234</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">393</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">307</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">175</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">303</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">127</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">98</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">164</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Dragon</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">442</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">purple</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">317</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">yellow</td>
<td class="gt_row gt_left">8/6/20</td>
<td class="gt_row gt_right">439</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">359</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">356</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">233</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">364</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">1045</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">562</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">292</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">1219</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">1327</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">255</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/7/20</td>
<td class="gt_row gt_right">19</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">162</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">81</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">564</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">184</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Chinese Red Noodle</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">108</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">122</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">1697</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">545</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">445</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">8/8/20</td>
<td class="gt_row gt_right">305</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">179</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">591</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">1102</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">308</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">54</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">64</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">443</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">118</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">8/9/20</td>
<td class="gt_row gt_right">302</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">13</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">yellow</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">272</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">purple</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">168</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">216</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">241</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">309</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">8/10/20</td>
<td class="gt_row gt_right">221</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">731</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">302</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">307</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">160</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">755</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">1029</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Chinese Red Noodle</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">78</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">245</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">218</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">802</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">354</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">359</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">506</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">92</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">109</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Dorinny Sweet</td>
<td class="gt_row gt_left">8/11/20</td>
<td class="gt_row gt_right">330</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/12/20</td>
<td class="gt_row gt_right">73</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">1774</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">468</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">122</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">421</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">332</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">727</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">642</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">413</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Chinese Red Noodle</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">65</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">599</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">12</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">198</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Gourmet Golden</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">308</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">517</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Sweet Merlin</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">2209</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beets</td>
<td class="gt_row gt_left">Gourmet Golden</td>
<td class="gt_row gt_left">8/13/20</td>
<td class="gt_row gt_right">2476</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Dorinny Sweet</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">1564</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">80</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">711</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">238</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">525</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">181</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">266</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">490</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">126</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/14/20</td>
<td class="gt_row gt_right">371</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Golden Bantam</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">383</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">351</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">859</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">25</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">137</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">71</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/15/20</td>
<td class="gt_row gt_right">56</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">477</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">328</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">45</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">543</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">599</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">560</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">291</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">238</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">397</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">660</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/16/20</td>
<td class="gt_row gt_right">693</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">364</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">305</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">588</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">764</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">436</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">306</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">350</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Chinese Red Noodle</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">105</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">30</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">67</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Golden Bantam</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">344</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">8/17/20</td>
<td class="gt_row gt_right">173</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">27</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">126</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">112</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">1151</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">225</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">2888</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">608</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">136</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">148</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">317</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">105</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">271</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">spinach</td>
<td class="gt_row gt_left">Catalina</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">39</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">87</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">233</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">8/18/20</td>
<td class="gt_row gt_right">527</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">purple</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">323</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">yellow</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">278</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">hot peppers</td>
<td class="gt_row gt_left">thai</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">31</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">872</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">579</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">615</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">997</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">335</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">264</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">451</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/19/20</td>
<td class="gt_row gt_right">306</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">99</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">70</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">333</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">483</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">632</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">360</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">230</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">344</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">1010</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">328</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">287</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Tatsoi</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">322</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">493</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">green</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">252</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">70</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">834</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">8/20/20</td>
<td class="gt_row gt_right">113</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">1122</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">34</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">509</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">1601</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">842</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">1538</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">428</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">243</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">330</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">997</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">265</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">562</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Dragon</td>
<td class="gt_row gt_left">8/21/20</td>
<td class="gt_row gt_right">457</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">1542</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">801</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">436</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">747</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">1573</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">704</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">446</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">269</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Dorinny Sweet</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">661</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">2436</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/23/20</td>
<td class="gt_row gt_right">111</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/24/20</td>
<td class="gt_row gt_right">134</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">green</td>
<td class="gt_row gt_left">8/24/20</td>
<td class="gt_row gt_right">115</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/24/20</td>
<td class="gt_row gt_right">75</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">8/24/20</td>
<td class="gt_row gt_right">117</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">578</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">871</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">115</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">629</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">186</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">320</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">488</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">506</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">920</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">cucumbers</td>
<td class="gt_row gt_left">pickling</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">179</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">1400</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">993</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/25/20</td>
<td class="gt_row gt_right">1026</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">1886</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">666</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">1042</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">593</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">216</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">309</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">497</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">261</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">819</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Dorinny Sweet</td>
<td class="gt_row gt_left">8/26/20</td>
<td class="gt_row gt_right">1607</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/27/20</td>
<td class="gt_row gt_right">14</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">8/28/20</td>
<td class="gt_row gt_right">29</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">8/28/20</td>
<td class="gt_row gt_right">3244</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">8/28/20</td>
<td class="gt_row gt_right">85</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">basil</td>
<td class="gt_row gt_left">Isle of Naxos</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">24</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Long Keeping Rainbow</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">289</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">380</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">737</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">1033</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">1097</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">483</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">627</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">352</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">purple</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">262</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">yellow</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">716</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">888</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">566</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">greens</td>
<td class="gt_row gt_left">8/29/20</td>
<td class="gt_row gt_right">169</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">861</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">460</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">2934</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">599</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">155</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">822</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">589</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">393</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">752</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">8/30/20</td>
<td class="gt_row gt_right">833</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">2831</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">1953</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">160</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">4758</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">2342</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Blue (saved)</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">3227</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Blue (saved)</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">5150</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">Cinderella's Carraige</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">7350</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">805</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">178</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Cherokee Purple</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">201</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">1537</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">773</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">9/1/20</td>
<td class="gt_row gt_right">1202</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Dorinny Sweet</td>
<td class="gt_row gt_left">9/2/20</td>
<td class="gt_row gt_right">798</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">green</td>
<td class="gt_row gt_left">9/2/20</td>
<td class="gt_row gt_right">370</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">9/2/20</td>
<td class="gt_row gt_right">43</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">9/2/20</td>
<td class="gt_row gt_right">60</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/3/20</td>
<td class="gt_row gt_right">1131</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/3/20</td>
<td class="gt_row gt_right">610</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">9/3/20</td>
<td class="gt_row gt_right">1265</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">9/3/20</td>
<td class="gt_row gt_right">102</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">2160</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">2899</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">442</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">1234</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">1178</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">255</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">430</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">onions</td>
<td class="gt_row gt_left">Delicious Duo</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">33</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">256</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">jalapeño</td>
<td class="gt_row gt_left">giant</td>
<td class="gt_row gt_left">9/4/20</td>
<td class="gt_row gt_right">58</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">corn</td>
<td class="gt_row gt_left">Dorinny Sweet</td>
<td class="gt_row gt_left">9/5/20</td>
<td class="gt_row gt_right">214</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">edamame</td>
<td class="gt_row gt_left">9/5/20</td>
<td class="gt_row gt_right">1644</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/6/20</td>
<td class="gt_row gt_right">2377</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">9/6/20</td>
<td class="gt_row gt_right">710</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">9/6/20</td>
<td class="gt_row gt_right">1317</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">9/6/20</td>
<td class="gt_row gt_right">1649</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/6/20</td>
<td class="gt_row gt_right">615</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">9/7/20</td>
<td class="gt_row gt_right">3284</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">zucchini</td>
<td class="gt_row gt_left">Romanesco</td>
<td class="gt_row gt_left">9/8/20</td>
<td class="gt_row gt_right">1300</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">yellow</td>
<td class="gt_row gt_left">9/9/20</td>
<td class="gt_row gt_right">843</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">broccoli</td>
<td class="gt_row gt_left">Main Crop Bravado</td>
<td class="gt_row gt_left">9/9/20</td>
<td class="gt_row gt_right">102</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">9/9/20</td>
<td class="gt_row gt_right">228</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">692</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">674</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">1392</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">316</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Jet Star</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">754</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">413</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/10/20</td>
<td class="gt_row gt_right">509</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">9/12/20</td>
<td class="gt_row gt_right">108</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/15/20</td>
<td class="gt_row gt_right">258</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/15/20</td>
<td class="gt_row gt_right">725</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">potatoes</td>
<td class="gt_row gt_left">Russet</td>
<td class="gt_row gt_left">9/16/20</td>
<td class="gt_row gt_right">629</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">broccoli</td>
<td class="gt_row gt_left">Main Crop Bravado</td>
<td class="gt_row gt_left">9/16/20</td>
<td class="gt_row gt_right">219</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">9/16/20</td>
<td class="gt_row gt_right">8</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">King Midas</td>
<td class="gt_row gt_left">9/17/20</td>
<td class="gt_row gt_right">160</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">9/17/20</td>
<td class="gt_row gt_right">168</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kohlrabi</td>
<td class="gt_row gt_left">Crispy Colors Duo</td>
<td class="gt_row gt_left">9/17/20</td>
<td class="gt_row gt_right">191</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/17/20</td>
<td class="gt_row gt_right">212</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Brandywine</td>
<td class="gt_row gt_left">9/18/20</td>
<td class="gt_row gt_right">714</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">9/18/20</td>
<td class="gt_row gt_right">228</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">9/18/20</td>
<td class="gt_row gt_right">670</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">9/18/20</td>
<td class="gt_row gt_right">1052</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">9/18/20</td>
<td class="gt_row gt_right">1631</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">raspberries</td>
<td class="gt_row gt_left">perrenial</td>
<td class="gt_row gt_left">9/18/20</td>
<td class="gt_row gt_right">137</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2934</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">304</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1058</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">307</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">397</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">537</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">314</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">494</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">484</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">454</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">480</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">252</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">294</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">delicata</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">437</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Waltham Butternut</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1834</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Waltham Butternut</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1655</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Waltham Butternut</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1927</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Waltham Butternut</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1558</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Waltham Butternut</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1183</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Red Kuri</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1178</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Red Kuri</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">706</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Red Kuri</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1686</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Red Kuri</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1785</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Blue (saved)</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1923</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Blue (saved)</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2120</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Blue (saved)</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2325</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">squash</td>
<td class="gt_row gt_left">Blue (saved)</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1172</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">Cinderella's Carraige</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1311</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">Cinderella's Carraige</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">6250</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1154</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1208</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2882</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2689</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">3441</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">saved</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">7050</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1109</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1028</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1131</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1302</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1570</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1359</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1608</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2277</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">1743</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">pumpkins</td>
<td class="gt_row gt_left">New England Sugar</td>
<td class="gt_row gt_left">9/19/20</td>
<td class="gt_row gt_right">2931</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">9/20/20</td>
<td class="gt_row gt_right">163</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">9/21/20</td>
<td class="gt_row gt_right">714</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/21/20</td>
<td class="gt_row gt_right">95</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Bonny Best</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">477</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Amish Paste</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">2738</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Black Krim</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">236</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Old German</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">1823</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">grape</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">819</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Mortgage Lifter</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">2006</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Big Beef</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">659</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">Better Boy</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">1239</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">tomatoes</td>
<td class="gt_row gt_left">volunteers</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">1978</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">kale</td>
<td class="gt_row gt_left">Heirloom Lacinto</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">28</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">24</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">broccoli</td>
<td class="gt_row gt_left">Main Crop Bravado</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">75</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">peppers</td>
<td class="gt_row gt_left">variety</td>
<td class="gt_row gt_left">9/25/20</td>
<td class="gt_row gt_right">84</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">apple</td>
<td class="gt_row gt_left">unknown</td>
<td class="gt_row gt_left">9/26/20</td>
<td class="gt_row gt_right">156</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">9/26/20</td>
<td class="gt_row gt_right">95</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Bush Bush Slender</td>
<td class="gt_row gt_left">9/27/20</td>
<td class="gt_row gt_right">94</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">beans</td>
<td class="gt_row gt_left">Classic Slenderette</td>
<td class="gt_row gt_left">9/28/20</td>
<td class="gt_row gt_right">81</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">lettuce</td>
<td class="gt_row gt_left">Lettuce Mixture</td>
<td class="gt_row gt_left">9/29/20</td>
<td class="gt_row gt_right">139</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">broccoli</td>
<td class="gt_row gt_left">Main Crop Bravado</td>
<td class="gt_row gt_left">9/30/20</td>
<td class="gt_row gt_right">134</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Dragon</td>
<td class="gt_row gt_left">10/1/20</td>
<td class="gt_row gt_right">883</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">carrots</td>
<td class="gt_row gt_left">Bolero</td>
<td class="gt_row gt_left">10/2/20</td>
<td class="gt_row gt_right">449</td>
<td class="gt_row gt_left">grams</td></tr>
    <tr><td class="gt_row gt_left">Swiss chard</td>
<td class="gt_row gt_left">Neon Glow</td>
<td class="gt_row gt_left">10/3/20</td>
<td class="gt_row gt_right">232</td>
<td class="gt_row gt_left">grams</td></tr>
  </tbody>
  <tfoot class="gt_sourcenotes">
    <tr>
      <td class="gt_sourcenote" colspan="5">Data Courtesy of Lisa Lendway</td>
    </tr>
  </tfoot>
  
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
  filter(vegetable %in% "tomatoes") %>% 
  group_by(variety) %>% 
  mutate(weight_lb = weight * 0.00220462) %>%
  ggplot(aes(x = weight_lb, 
             y = fct_reorder(variety, date, min, .desc = TRUE), 
             color = variety, 
             fill = variety))+
  geom_col() +
  labs(x = "Weight (lbs)", 
       y = "", 
       title = "Total Tomato Harvest for Each Variety") +
   theme(legend.position = "none") 
```


```r
graph2 <- garden_harvest %>% 
  filter(vegetable %in% c("kale", "spinach", "lettuce", "tomatoes")) %>% 
  group_by(vegetable, date) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb)) %>% 
  ggplot(aes(x = date, 
             y = cum_harvest_lb, 
             color = vegetable)) +
  geom_line() +
  scale_color_manual(values = c("kale" = "darkslategray",
                                "spinach" = "darkgreen",
                                "lettuce" = "chartreuse", 
                                "tomatoes" = "red")) +
  labs(x = "", 
       y = "",
       title = "Cumulative harvest (lb)") +
  theme(legend.position = "none")
```


```r
(graph1/graph2) + 
  plot_annotation(title = "Total Tomato Harvest Compared With Cumulative Harvest of Multiple Vegetables") 
```

![](06_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

  
## Webscraping exercise (also from tutorial)

Use the data from the [Macalester Registrar's Fall 2017 Class Schedule](https://www.macalester.edu/registrar/schedules/2017fall/class-schedule/#crs10008) to complete all these exercises.

6. Find the correct selectors for the following fields. Make sure that each matches 762 results:

  * Course Number
  * Course Name
  * Day
  * Time
  * Room
  * Instructor
  * Avail. / Max
  * General Education Requirements (make sure you only match 762; beware of the Mac copyright banner at the bottom of the page!)
  * Description

Then, put all this information into one dataset (tibble or data.frame) Do not include any extraneous information like "Instructor: ".


```r
paths_allowed(paths = "https://www.macalester.edu/registrar/schedules/2017fall/class-schedule/#crs10008")
```

```
## [1] TRUE
```

```r
fall_2017 <- read_html("https://www.macalester.edu/registrar/schedules/2017fall/class-schedule/#crs10008")
```


```r
course_numbers <- fall_2017 %>% 
  html_elements(".class-schedule-course-number") %>% 
  html_text2()
head(course_numbers)
```

```
## [1] "AMST 101-01" "AMST 103-01" "AMST 200-01" "AMST 203-01" "AMST 219-01"
## [6] "AMST 229-01"
```


```r
course_names <- fall_2017 %>% 
  html_elements(".class-schedule-course-title") %>%
  html_text2()
head(course_names)
```

```
## [1] "Explorations of Race and Racism"                     
## [2] "The Problems of Race in US Social Thought and Policy"
## [3] "Critical Methods for American Studies Research"      
## [4] "Politics and Inequality: American Welfare State"     
## [5] "In Motion: African Americans in the United States"   
## [6] "Narrating Black Women's Resistance"
```


```r
course_df <- tibble(number=course_numbers, name=course_names)
head(course_df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["number"],"name":[1],"type":["chr"],"align":["left"]},{"label":["name"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"AMST 101-01","2":"Explorations of Race and Racism"},{"1":"AMST 103-01","2":"The Problems of Race in US Social Thought and Policy"},{"1":"AMST 200-01","2":"Critical Methods for American Studies Research"},{"1":"AMST 203-01","2":"Politics and Inequality: American Welfare State"},{"1":"AMST 219-01","2":"In Motion: African Americans in the United States"},{"1":"AMST 229-01","2":"Narrating Black Women's Resistance"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
course_day <- fall_2017 %>%
  html_elements(".class-schedule-course-title+ .class-schedule-label") %>%
  html_text2()
head(course_day)
```

```
## [1] "Days: W"   "Days: MWF" "Days: MWF" "Days: MWF" "Days: MWF" "Days: MWF"
```


```r
course_day <- fall_2017 %>%
  html_elements("td.class-schedule-label:nth-child(3)") %>% 
  html_text2() %>% 
  str_sub(start = 7)

head(course_day)
```

```
## [1] "W"   "MWF" "MWF" "MWF" "MWF" "MWF"
```


```r
course_time <- fall_2017 %>% 
  html_elements(".class-schedule-label:nth-child(4)") %>% 
  html_text2() %>% 
  str_sub(start = 7)
head(course_time)
```

```
## [1] "07:00 pm-10:00 pm" "09:40 am-10:40 am" "02:20 pm-03:20 pm"
## [4] "09:40 am-10:40 am" "01:10 pm-02:10 pm" "10:50 am-11:50 am"
```


```r
course_room <- fall_2017 %>% 
  html_elements(".class-schedule-label:nth-child(5)") %>% 
  html_text2() %>% 
  str_sub(start = 7)
head(course_room)
```

```
## [1] "ARTCOM 102" "NEILL 111"  "OLRI 205"   "CARN 204"   "MAIN 010"  
## [6] "MAIN 001"
```


```r
course_instructor <- fall_2017 %>% 
  html_elements(".class-schedule-label:nth-child(6)") %>% 
  html_text2() %>% 
  str_sub(start = 13)
head(course_instructor)
```

```
## [1] "Gutierrez, Harris"      "Karin Aguilar-San Juan" "Nathan Titman"         
## [4] "Lesley Lavery"          "Crystal Moten"          "Crystal Moten"
```


```r
course_avail <- fall_2017 %>% 
  html_elements(".class-schedule-label:nth-child(7)") %>%
  html_text2() %>% 
  str_sub(start = 14)
head(course_avail)
```

```
## [1] "Closed -5 / 25" "0 / 16"         "13 / 20"        "Closed 0 / 25" 
## [5] "2 / 20"         "Closed 4 / 14"
```


```r
course_gen <- fall_2017 %>% 
  html_elements("#content p:nth-child(2)") %>% 
  html_text2() %>% 
  str_sub(start = 35) %>% 
  str_replace_all("\n", " ")
head(course_gen)
```

```
## [1] "U.S. Identities and Differences "           
## [2] "U.S. Identities and Differences Writing WA "
## [3] ""                                           
## [4] "U.S. Identities and Differences Writing WP "
## [5] "U.S. Identities and Differences "           
## [6] ""
```


```r
course_desc <- fall_2017 %>% 
  html_elements(".collapsed p:nth-child(1)") %>% 
  html_text2() %>% 
  str_sub(start = 3) %>% 
  str_replace_all("\n", "") %>% 
  str_replace_all("\r", "")
  
head(course_desc)
```

```
## [1] "The main objectives of this introductory course are: to explore the historical construction of racial categories in the United States; to understand the systemic impact of racism on contemporary social processes; to consider popular views about race in the light of emerging scholarship in the field; and to develop an ability to connect personal experiences to larger, collective realities. We will engage several questions as a group: What are the historical and sociological foundations of racial categories? When does focusing on race make someone racist? What is white privilege, and why does it matter? All students will be asked to think and write about their own racial identity. This course, or its equivalent, is required for majors and minors. (4 credits)"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
## [2] "In this discussion-based and residential course, we will explore the paradox of a society in which people are increasingly aware of patterns of racism and yet still unable to see or explain how those systems and patterns are connected to everyday life. As awareness increases, why are we not able to develop effective or meaningful responses?Our interdisciplinary and integrative approach will employ multiple methods of inquiry and expression, including: self-reflective essays and maps; a scavenger hunt along University Avenue; library research; and deep, critical analysis of arguments about race/ethnicity/assimilation/multiculturalism.We will practice engaging in open-ended conversations so that we might discover the questions that truly matter to each of us. To fulfill the WA general education writing requirement, this course will invite you to produce at least 20 pages of college-level writing through various assignments. Each writing assignment will strengthen your use of evidence and argumentation, and will involve drafts, feedback, in person conference, and revision.Class meets MWF, 9:40 am - 10:40 am in Neill Hall 111Writing designation: WALiving arrangements: Single gender rooms, co-ed floor."                                                                                                                                                                                                                                                
## [3] "This course will introduce students to interdisciplinary research approaches to the study of race, ethnicity, and other categories of difference. Students will learn to conceptualize and design research projects, and will obtain hands-on experience in executing different methods. The course will also consider the critiques of systems of knowledge production and research approaches that have been informed by scholars from fields such as African American history, gender studies, and critical race studies, as well as from the disciplines. The goal is to develop an understanding of the assumptions embedded in many fields of inquiry, and to learn to apply critical approaches to important research questions."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
## [4] "Americans, at least since the Founding era, have cherished the ideal of political equality. Unlike European nations, the United States did not inherit economic class distinctions from a feudal past. But time and again, American social reformers and mass movements have highlighted inconsistencies between the value of equality and the actual practice of democracy. Through the extension of rights to citizens who were previously excluded or treated as second-class citizens, such as women and African Americans, the polity has become more inclusive. But over the last three decades American citizens have grown increasingly unequal in terms of income and wealth. The central question posed by this course is the implications of such vast economic inequality for American democracy. Do these disparities between citizens curtail, limit, and perhaps threaten the functioning of genuinely representative governance? In this course will 1) Explore what other social scientists, mostly economists and sociologists, know about contemporary inequality, particularly in terms of its causes, manifestation, and socio-economic effects; 2) Consider the concept of inequality in political theory and in American political thought, and; 3) Examine the current relationship between economic inequality and each of three major aspects of the American political system: political voice, representation, and public policy. Cross-listed as Political Science 203. (4 Credits)"
## [5] "In Motion is an introduction to modern African American History from slavery to contemporary times. In Motion emphasizes the idea that both African Americans and the stories of their lives in the United States are fluid, varied and continually being reinterpreted. Rather than a strict chronological survey, this course is organized thematically. Some of the important themes include movement/mobility/migration; work/labor; resistance to systems of oppression; gender/sexuality/culture/performance; politics/citizenship; and sites of (re)memory. While the course is geographically situated in the United States, we will also consider African American life, culture, thought and resistance in global perspectives. In this course, students will read important historical texts, both primary and secondary, engage in discussion, and write essays that ask them to critically engage the history of African Americans in the US. Cross-listed with History 219. 4 credits."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
## [6] "This course examines traditions of 20th century African American women’s activism and the ways in which they have changed over time. Too often, the narrative of the “strong black woman” infuses stories of African American women’s resistance which, coupled with a culture of dissemblance, makes the inner workings of their lives difficult to imagine. This course, at its heart, seeks to uncover the motivations, both personal and political, behind African American women’s activism. It also aims to address the ways in which African American women have responded to the pressing social, economic, and political needs of their diverse communities. The course also asks students to consider narrative, voice and audience in historical writing, paying particular attention to the ways in which black women’s history has been written over the course of the twentieth century. Cross-listed with History 229 and Women's and Gender Studies 229. 4 credits."
```


```r
course_df <- tibble(number=course_numbers, name=course_names, day=course_day, 
                    time=course_time, room=course_room, instructor=course_instructor,
                    availability=course_avail, generaled=course_gen, description=course_desc)
head(course_df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["number"],"name":[1],"type":["chr"],"align":["left"]},{"label":["name"],"name":[2],"type":["chr"],"align":["left"]},{"label":["day"],"name":[3],"type":["chr"],"align":["left"]},{"label":["time"],"name":[4],"type":["chr"],"align":["left"]},{"label":["room"],"name":[5],"type":["chr"],"align":["left"]},{"label":["instructor"],"name":[6],"type":["chr"],"align":["left"]},{"label":["availability"],"name":[7],"type":["chr"],"align":["left"]},{"label":["generaled"],"name":[8],"type":["chr"],"align":["left"]},{"label":["description"],"name":[9],"type":["chr"],"align":["left"]}],"data":[{"1":"AMST 101-01","2":"Explorations of Race and Racism","3":"W","4":"07:00 pm-10:00 pm","5":"ARTCOM 102","6":"Gutierrez, Harris","7":"Closed -5 / 25","8":"U.S. Identities and Differences","9":"The main objectives of this introductory course are: to explore the historical construction of racial categories in the United States; to understand the systemic impact of racism on contemporary social processes; to consider popular views about race in the light of emerging scholarship in the field; and to develop an ability to connect personal experiences to larger, collective realities. We will engage several questions as a group: What are the historical and sociological foundations of racial categories? When does focusing on race make someone racist? What is white privilege, and why does it matter? All students will be asked to think and write about their own racial identity. This course, or its equivalent, is required for majors and minors. (4 credits)"},{"1":"AMST 103-01","2":"The Problems of Race in US Social Thought and Policy","3":"MWF","4":"09:40 am-10:40 am","5":"NEILL 111","6":"Karin Aguilar-San Juan","7":"0 / 16","8":"U.S. Identities and Differences Writing WA","9":"In this discussion-based and residential course, we will explore the paradox of a society in which people are increasingly aware of patterns of racism and yet still unable to see or explain how those systems and patterns are connected to everyday life. As awareness increases, why are we not able to develop effective or meaningful responses?Our interdisciplinary and integrative approach will employ multiple methods of inquiry and expression, including: self-reflective essays and maps; a scavenger hunt along University Avenue; library research; and deep, critical analysis of arguments about race/ethnicity/assimilation/multiculturalism.We will practice engaging in open-ended conversations so that we might discover the questions that truly matter to each of us. To fulfill the WA general education writing requirement, this course will invite you to produce at least 20 pages of college-level writing through various assignments. Each writing assignment will strengthen your use of evidence and argumentation, and will involve drafts, feedback, in person conference, and revision.Class meets MWF, 9:40 am - 10:40 am in Neill Hall 111Writing designation: WALiving arrangements: Single gender rooms, co-ed floor."},{"1":"AMST 200-01","2":"Critical Methods for American Studies Research","3":"MWF","4":"02:20 pm-03:20 pm","5":"OLRI 205","6":"Nathan Titman","7":"13 / 20","8":"","9":"This course will introduce students to interdisciplinary research approaches to the study of race, ethnicity, and other categories of difference. Students will learn to conceptualize and design research projects, and will obtain hands-on experience in executing different methods. The course will also consider the critiques of systems of knowledge production and research approaches that have been informed by scholars from fields such as African American history, gender studies, and critical race studies, as well as from the disciplines. The goal is to develop an understanding of the assumptions embedded in many fields of inquiry, and to learn to apply critical approaches to important research questions."},{"1":"AMST 203-01","2":"Politics and Inequality: American Welfare State","3":"MWF","4":"09:40 am-10:40 am","5":"CARN 204","6":"Lesley Lavery","7":"Closed 0 / 25","8":"U.S. Identities and Differences Writing WP","9":"Americans, at least since the Founding era, have cherished the ideal of political equality. Unlike European nations, the United States did not inherit economic class distinctions from a feudal past. But time and again, American social reformers and mass movements have highlighted inconsistencies between the value of equality and the actual practice of democracy. Through the extension of rights to citizens who were previously excluded or treated as second-class citizens, such as women and African Americans, the polity has become more inclusive. But over the last three decades American citizens have grown increasingly unequal in terms of income and wealth. The central question posed by this course is the implications of such vast economic inequality for American democracy. Do these disparities between citizens curtail, limit, and perhaps threaten the functioning of genuinely representative governance? In this course will 1) Explore what other social scientists, mostly economists and sociologists, know about contemporary inequality, particularly in terms of its causes, manifestation, and socio-economic effects; 2) Consider the concept of inequality in political theory and in American political thought, and; 3) Examine the current relationship between economic inequality and each of three major aspects of the American political system: political voice, representation, and public policy. Cross-listed as Political Science 203. (4 Credits)"},{"1":"AMST 219-01","2":"In Motion: African Americans in the United States","3":"MWF","4":"01:10 pm-02:10 pm","5":"MAIN 010","6":"Crystal Moten","7":"2 / 20","8":"U.S. Identities and Differences","9":"In Motion is an introduction to modern African American History from slavery to contemporary times. In Motion emphasizes the idea that both African Americans and the stories of their lives in the United States are fluid, varied and continually being reinterpreted. Rather than a strict chronological survey, this course is organized thematically. Some of the important themes include movement/mobility/migration; work/labor; resistance to systems of oppression; gender/sexuality/culture/performance; politics/citizenship; and sites of (re)memory. While the course is geographically situated in the United States, we will also consider African American life, culture, thought and resistance in global perspectives. In this course, students will read important historical texts, both primary and secondary, engage in discussion, and write essays that ask them to critically engage the history of African Americans in the US. Cross-listed with History 219. 4 credits."},{"1":"AMST 229-01","2":"Narrating Black Women's Resistance","3":"MWF","4":"10:50 am-11:50 am","5":"MAIN 001","6":"Crystal Moten","7":"Closed 4 / 14","8":"","9":"This course examines traditions of 20th century African American women’s activism and the ways in which they have changed over time. Too often, the narrative of the “strong black woman” infuses stories of African American women’s resistance which, coupled with a culture of dissemblance, makes the inner workings of their lives difficult to imagine. This course, at its heart, seeks to uncover the motivations, both personal and political, behind African American women’s activism. It also aims to address the ways in which African American women have responded to the pressing social, economic, and political needs of their diverse communities. The course also asks students to consider narrative, voice and audience in historical writing, paying particular attention to the ways in which black women’s history has been written over the course of the twentieth century. Cross-listed with History 229 and Women's and Gender Studies 229. 4 credits."}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


7. Create a graph that shows the number of sections offered per department. Hint: The department is a substring of the course number - there are `str_XXX()` functions that can help. Yes, COMP and MATH are the same department, but for this exercise you can just show the results by four letter department code, e.g., with COMP and MATH separate.


```r
course_df %>%
  mutate(department = substr(number, 1, 4)) %>% 
  ggplot() +
    geom_bar(aes(y = department)) +
  labs(x = "Department", 
       y = "", 
       title = "Number of Sections Offered Per Department in Fall of 2017")
```

![](06_exercises_files/figure-html/unnamed-chunk-21-1.png)<!-- -->


8. Analyze the typical length of course names by department. To do so, create a new data table based on your courses data table, with the following changes:
  
  * New columns for the length of the title of a course and the length of the description of the course. Hint: `str_length`.  
  * Remove departments that have fewer than 10 sections of courses. To do so, group by department, then remove observations in groups with fewer than 10 sections (Hint: use filter with n()). Then `ungroup()` the data.  
  * Create a visualization of the differences across groups in lengths of course names or course descriptions. Think carefully about the visualization you should be using!


```r
modified_data <- course_df %>%
  mutate(department = substr(number, 1, 4)) %>% 
  mutate(title_length = str_length(name)) %>% 
  mutate(desc_length = str_length(description)) %>% 
  group_by(department) %>%
  mutate(count = n()) %>% 
  filter(count > 10) %>% 
  ungroup()

modified_data %>% 
  ggplot(aes(x = title_length,
             y = department)) +
  geom_boxplot() +
  stat_summary(fun.x = mean, geom = "point", shape = "circle", size = 1, 
               color = "blue", fill = "blue") +
  labs(title = "Course Title Length by Academic Department",
       x = "Length (characters)", 
       y = "Department")
```

![](06_exercises_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
