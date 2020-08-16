+++
layout =  "post"
title =  "Creating a powerful dashboard with PowerBI"
description = "Full workflow in PowerBI from zero to dashboard"
tags = ["BI", "Visualization", "PowerBI"]
date = "2020-08-09"
share = true
comments = true
+++

# Get data

- load sales as csv
- fix zip
- load bi_dimensions.xlxs
- unselect product sheet from query window view
- select remaining sheets as they look correctly parsed
- load international sales as folder  
- country column drop-down for international sales is incomplete so hit 'load more'

# Data cleaning

- fill down in Product -> Category
- Product -> Split Product column by delimiter |
- Product -> Rename Product.1 to Product and Product.2 to Segment
- Separate Currency and Price -> Add column from examples with From all columns
- Fix Geography and Manufacturer tables 
- Append sales and international sales
- Add USA to Country column by creating temp column
- 