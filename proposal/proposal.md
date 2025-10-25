Project proposal
================
AI Hallunication Cases - Yuka, Brynn, & Sasha

``` r
library(tidyverse)
library(broom)
ai_data <- read_csv("../data/Charlotin-hallucination_cases.csv")
#view(ai_data)
```

## 1. Introduction

The introduction should introduce your general research question(s) and
your data (where it came from, how it was collected, what are the cases,
what are the variables, etc.).

Text goes here. Refer to the Markdown Quick Reference: Help -\> Markdown
Quick Reference.

## 2. Data

Text goes here. Place your data in the /data folder, and add dimensions
and codebook to the README in that folder. Then print out the output of
glimpse() or skim() of your data frame.

``` r
# Code goes here
# Read in your data file
# Print the output of glimpse() or skim()
ai_data <- read_csv("../data/Charlotin-hallucination_cases.csv")
```

    ## Rows: 426 Columns: 14
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (13): Case Name, Court, State(s), Party(ies), AI Tool, Hallucination, O...
    ## date  (1): Date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## 3. Data analysis plan

Text goes here. - What variables will you visualize to explore your
research questions? - Will there be any other data that you need to find
to help with your research question? - Very preliminary exploratory data
analysis, including some summary statistics and visualizations, along
with some explanation on how they help you learn more about your data.
(You can add to these later as you work on your project.) - The data
visualization(s) that you believe will be useful in exploring your
question(s). (You can update these later as you work on your project.)

``` r
# Code goes here
# Code to calculate summary statistics
# Code for a visualization
```
