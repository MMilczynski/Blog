+++
layout =  "post"
title =  "Your first Power BI report from scratch"
description ="Intro Power BI dashboard tutorial"
tags = ["Power BI", "Business Intelligence"]
date = "2020-08-26"
share = true
comments = true
+++

Power BI is Microsoft's solution for creating powerful business intelligence reports. It provides tools for creating Extract, Transform and Load (ETL) pipelines as well as impressive visualizations which contain animations and allow user interaction. The steepness of Power BI's learning curve depends a bit on the user's background but I would generally state that one can manage to create a solid report within one day. The material I'm going to present here is based on a Power BI workshop that took place in Irvine, CA in early 2019. Power BI underwent some substantial changes since then so it makes sense to look at some new features as well. Are you ready? So let's get started!

# Spoiler alert

In this tutorial we will create a powerful dashboard that supports user interactions.

{{< figure src="/images/Dashboard_Intro_02.gif" caption="Dashboard we will create." width="100%" >}}

# Dataset

## Disclaimer 

The dataset I'll be using represents real anonymized data and is provided by **ObviEnce LLC**. Visit their site to learn about their services: www.obvience.com .
**This data is property of ObviEnce LLC and has been shared for the purpose of demonstrating PowerBI functionality with industry sample data.** Please use this attribution to ObviEnce, LLC for any usage of this data.

## Examining the raw data

Let's have a first look at the raw data we'll be working with. The data is located in folders `USSales` and `InternationalSales` and is an mix of Microsoft Excel worksheets and comma separated value (CSV) files. Let's examine some of them real quick in Excel. Pro tip: When working with delimiter-separated data it is sometimes recommended to open Excel first and then load the data using **Data** -> **From Text/CSV**. 

First, let's have a look at `sales.csv` in the `USSales` folder. 

{{< figure src="/images/sales_01.png" caption="Purchase records with revenue for various products (sales.csv)." width="55%" >}}

So what we have here is a listing of purchase records by date, quantity, resulting revenue etc. for certain products which are represented by a **ProductID**. The reason for using a **ProductID** instead of e.g. the actual product name is related to concepts used in relational databases where different tables are linked through unique identifiers. We will see other **...ID** columns in other spreadsheets which have the exact same function. 

In the `bi_dimensions.csv` (also located in the `USSales` folder) we can find 3 spreadsheets: **product**, **manufacturer** and **geo**. In **product**, all products are listed by id and name plus other data such as **ManufacturerID** and **Price**. 

{{< figure src="/images/product_01.png" caption="Listing of all products (bi_dimensions.csv)." width="70%" >}}

One thing to note is that in the **product** table we will find each **ProductID** and associated **Product** appear only once! This will be important when linking tables later on thereby creating so called `1-to-many` relationships. We also see that entries in the **Category** column only appear somehow sporadically. We'll take care of that later. Now that we talked a little bit about how IDs are linked to names you may already have certain expectations on what the **manufacturer** spreadsheet contains and voila! ... **ManufacturerID** and **Manufacturer**. 

{{< figure src="/images/product_manu_01.png" caption="Manufacturer spreadsheet snippet (bi_dimensions.csv). Different data is represented row-wise and not column-wise as in the previous tables." width="100%" >}}

However, the table looks somewhat odd compared to the other tables we looked at so far because it seems like it is flipped and the data is contained row-wise rather than column-wise. Again, we will fix that later and create unified table formats. 

Last but not least, we have the **geo** spreadsheet that lists zip-codes, cities, states etc. per country. The format looks more familiar i.e. data is represented column-wise. 

{{< figure src="/images/product_geo_01.png" caption="Geo spreadsheet snippet (bi_dimensions.csv)." width="100%" >}}


The remaining tables in the `InternationalSales` folder have the same format as the `sales.csv` table except for an additional `Country` column. 

# Installing and launching Power BI

Follow this link [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) to get the free version of the application. You'll find a download link in the **Microsoft Power BI Desktop** section which will link you to the Microsoft Store. After downloading and installing you should be ready to launch **Power BI**. Upon launch you'll see this intro screen:

{{< figure src="/images/intro_screen_01.png" caption="Intro screen upon launching Power BI Desktop." width="100%" >}}

If you don't want to see this screen in future sessions you can un-check the `Show this screen on startup` checkbox in the bottom of the screen. Close the screen manually for the time being. 

# Loading US-sales data

The first thing we want to do is to load the data we have seen and examined in the previous section into Power BI. We'll start with `sales.csv`. In the ribbon click on **Get Data** to expand possible sources of data that are supported by Power BI and select **Text/CSV**.

{{< figure src="/images/get_data_01.png" >}}

Power BI will show a preview of what it thinks the format of the loaded table is. 

{{< figure src="/images/data_preview.png" >}}

It looks like Power BI wants to parse the table the way we want. However, instead of hitting `Load` we want to go to `Transform Data`. This will open the so called `Query Editor` in another Power BI window. The `Query Editor` will be the go-to place for us for applying various transformations to the data for the purpose of shaping the data to our liking and making it suitable for our final dashboard. For example, one common transformation we will use often it to apply the correct data type to a column to e.g. make sure that a column has a numerical data type if we want to perform calculations on that column. 

## Column profiling

Now that our sales table is loaded in the `Query Editor` let's engage one useful feature of Power BI that will provide us with additional metadata and even some descriptive statistics on each column. From the ribbon of the `Query Editor` go to `View` and check the `Column distribution` and `Column profile` boxes. Now each column is enhanced by a row showing a histogram of the column data right below the column header (that's `Column distribution`) and whenever you select the column header a new panel in the bottom of the screen will appear showing some descriptive statistics (max, min, mean etc.) of the data in the current column as shown below. 

{{< figure src="/images/column_profiling_01.png" >}}

In the bottom-left corner one can select whether the profiling data is based on the first 1000 rows or the entire dataset. To me personally is makes more sense to extract the profile based on the entire dataset especially if your data contains millions of rows so let's make this selection. However, beware that it may take a while for the profile to be fully calculated if you have a relatively large dataset. From the profiling, I consider the  `Count`, `Distinct` and `Unique` statistics as most relevant for the time being. `Count` gives the total number of entries (or rows) for the selected column. `Distinct` shows how many distinct values the column contains. So if we e.g. have the value 1076 appear 10 times and the value 1077 appear 5 times we would get a `Count` of 15 but the `Distinct` measure would indicate 2. Finally, `Unique` gives the number of elements that only appear once in the column. With those 3 values we will be able to quickly check a dataset for conformity. We'll see how soon.

## First data transformation: Data type change

We will carry out our first data transformation now, namely changing the **data type** of zip-codes in the `sales` table from `Whole Number` to `Text`. Click on the ![WholeNumberType](/images/whole_number_sign_01.png) sign in the column header of the `Zip` column and select ![TextType](/images/text_sign_01.png) `Text`. This change just makes sense for several reasons. Although zip-codes are represented as a sequence of numbers they are not intended for numerical operations such as addition. Also, with the `Whole Number` type leading zeros are discarded which would result in incorrect or non-existing zip-codes. 

## Applied steps

The `Query Editor` of Power BI keeps track of your actions in the `Applied Steps` panel on the right-hand side. This section is quite powerful since you can:

* Click on one particular step in the list and check the state of your table before and after a sequence of steps
* Rename an applied step (don't underestimate this possibility)
* Modify a step by clicking on the settings ![AppStepSettings](/images/app_step_settings_01.png) icon next to the step
* Remove a step 
* Move a step up/down in the sequence of `Applied Steps`
  
# Loading data on products, manufacturers, and location

Next, we will load the `bi_dimensions.csv` file. We will do that from the `Query Editor` using `Home` -> `New Source` -> `Excel`. Navigate to the `bi_dimensions.csv` file. 

