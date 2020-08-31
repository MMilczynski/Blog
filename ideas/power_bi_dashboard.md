+++
layout =  "post"
title =  "Creating a powerful dashboard with PowerBI"
description = "Full workflow in PowerBI from zero to dashboard"
tags = ["BI", "Visualization", "PowerBI"]
date = "2020-08-09"
share = true
comments = true
+++

# General 
- new features include
  - Edit Query is now called Transform Data

# Get data

- Start with Query Editor
- load sales as csv
- draw attention to Power BI's auto format feature based on first 200 rows
- Use Transform Data (to open Query Editor) rather than Load
- Having proper naming and data-type (data-type even more) is very crucial!!
- fix zip
- load bi_dimensions.xlxs
- unselect product sheet from query window view
- select remaining sheets as they look correctly parsed
- load international sales as folder  
- country column drop-down for international sales is incomplete so hit 'load more'
- no panic: Query Settings to show/hide applied steps
- Query Editor -> Column distribution + etc. (Nice!)
 
# Data cleaning

- fill down in Product -> Category
- Product -> Split Product column by delimiter |
- Product -> Rename Product.1 to Product and Product.2 to Segment
- Separate Currency and Price -> Add column from examples with From all columns
- Fix Geography and Manufacturer tables 
- Append sales and international sales
- Add USA to Country column by creating temp column

# Data Modeling and Exploration
- create 1 to many relation-ship between geography and sales using new ZipCountry column
- stacked bar chart to display revenue by country and manufacturer
- grouping to see relations between manufacturers more clearly -> VanArschGel 50 % share in Australia
- briefly show cross filtering (?)
- Edit interactions
- display two-way interactions
- drill down/up capability
- add new Date column
- create relationship Sales <-> Date
- create hierarchy Product -> Category -> Segment
- Explore VanArdsel revenue across segments and year for Australia -> Extreme segment prominent
- Create calculated measures (DAX)
- wait and wait and wait for applied steps (close all other apps when reloading doc for enhanced metadata bullshit)

# Data Viz\
- add date slicer
- add logo
- gauge visual for target vs. actual revenue
