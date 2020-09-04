+++
layout =  "post"
title =  "Your first Power BI report from scratch"
description ="Intro Power BI dashboard tutorial"
tags = ["Power BI", "Business Intelligence", "Tutorial"]
date = "2020-08-26"
share = true
comments = true
+++

Power BI is Microsoft's solution for creating powerful business intelligence reports. It provides tools for creating Extract, Transform and Load (ETL) pipelines as well as impressive visualizations which contain animations and allow user interaction. The steepness of Power BI's learning curve depends a bit on the user's background. With some practice though one can manage to create a solid report within one day. So this is what we're going to do in this tutorial, creating a dashboard in one day!

The material I'm going to present here is based on a Power BI workshop that took place in Irvine, CA in early 2019. Power BI underwent some substantial changes since then so it makes sense to look at some new features as well. Are you ready? So let's get started!

# Spoiler alert

In this tutorial we will create a powerful Power BI dashboard. We will analyze revenue data for the fictive company Van Ardsel and its competitors. We will gain sales insights and find that Van Ardsel sales were stronger for certain product segments relative to its competition. Power BI will allow us to to interact with the dashboard and analyze the data by country, year, and product segments. We will dive deeper into the data by performing drill-down/up operations based on a relational data model we will create. 

{{< figure src="/images/Dashboard_Intro_02.gif" caption="The dashboard we will create for analyzing revenue data for the fictive company Van Ardsel and competition." width="100%" >}}

# Dataset

## Disclaimer 

The dataset we'll be using represents real anonymized data using fictive company names and is provided by **ObviEnce LLC**. Visit their site to learn about their services: www.obvience.com . Please use the following attribution to ObviEnce, LLC for any usage of this data. **This data is property of ObviEnce LLC and has been shared for the purpose of demonstrating PowerBI functionality with industry sample data.**

## Examining the raw data

Let's have a first look at the raw data we'll be working with. The data is located in folders `USSales` and `InternationalSales` and is an mix of Microsoft Excel worksheets and comma separated value (CSV) files. Let's examine some of them real quick in Excel. Pro tip: When working with delimiter-separated data it is sometimes recommended to open Excel first and then load the data using **Data** -> **From Text/CSV**. 

First, let's have a look at `sales.csv` in the `USSales` folder. 

{{< figure src="/images/sales_01.png" caption="Purchase records with revenue for various products (sales.csv)." width="55%" >}}

So what we have here is a listing of purchase records by date, quantity, resulting revenue etc. for certain products which are represented by a **ProductID**. The reason for using a **ProductID** instead of e.g. the actual product name is related to concepts used in relational databases where different tables are linked through unique identifiers. We will see other **...ID** columns in other spreadsheets which have the exact same function. 

In the `bi_dimensions.xlsx` (also located in the `USSales` folder) we can find 3 spreadsheets: **product**, **manufacturer** and **geo**. In **product**, all products are listed by id and name plus other data such as **ManufacturerID** and **Price**. 

{{< figure src="/images/product_01.png" caption="Listing of all products (bi_dimensions.xlsx)." width="70%" >}}

One thing to note is that in the **product** table we will find each **ProductID** and associated **Product** appear only once! This will be important when linking tables later on thereby creating so called `1-to-many` relationships. We also see that entries in the **Category** column only appear somehow sporadically. We'll take care of that later. Now that we talked a little bit about how IDs are linked to names you may already have certain expectations on what the **manufacturer** spreadsheet contains and voila! ... **ManufacturerID** and **Manufacturer**. 

{{< figure src="/images/product_manu_01.png" caption="Manufacturer spreadsheet snippet (bi_dimensions.xlsx). Different data is represented row-wise and not column-wise as in the previous tables." width="100%" >}}

However, the table looks somewhat odd compared to the other tables we looked at so far because it seems like it is flipped and the data is contained row-wise rather than column-wise. Again, we will fix that later and create unified table formats. 

Last but not least, we have the **geo** spreadsheet that lists zip-codes, cities, states etc. per country. The format looks more familiar i.e. data is represented column-wise. However, we see that there is some extra info in the trailing rows which we may want to get rid of later. 

{{< figure src="/images/product_geo_02.png" caption="Geo spreadsheet snippet (bi_dimensions.xlsx)." width="100%" >}}

The files in the `InternationalSales` folder contain tables with the same format as the table in `sales.csv` except for an additional `Country` column. 

# Installing and launching Power BI

Follow this link [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) to get the free version of the application. You'll find a download link in the **Microsoft Power BI Desktop** section which will link you to the Microsoft Store. After downloading and installing you should be ready to launch **Power BI**. Upon launch you'll see this intro screen:

{{< figure src="/images/intro_screen_01.png" caption="Intro screen upon launching Power BI Desktop." width="100%" >}}

If you don't want to see this screen in future sessions you can un-check the `Show this screen on startup` checkbox in the bottom of the screen. Close the screen manually for the time being. 

# Things to be aware of

* apply changes window
* main power bi desktop window and query editor
* some things are counter-intuitive (add column feature appear in home)
* Power BI seems to be under constant development and therefore may have bugs
* Words table and query will be used interchangeably 
* Applied step can be modified if possible (gear icon)

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

Next, we will load the `bi_dimensions.xlsx` file. We will do that from the `Query Editor` using `Home` -> `New Source` -> `Excel`. Navigate to the `bi_dimensions.xlsx` file and hit `Open`. You'll see the `Navigator` window showing a table named **Product_Table** and the already discussed **product**, **manufacturer**, and **geo** spreadsheets. You may be wondering where the **Product_Table** is coming from but if you revisit `bi_dimensions.xlsx` you'll see that in the **product** spreadsheet the product listing was formatted as a Table. In Excel, if you select the **ProductID** column header in this table you'll see the `Table Design` menu item appear. If you click on `Table Design` you'll see that `Table Name` was indeed set to **Product_Table**. So there is no magic happening here just straight forward parsing.

Switching our focus back to Power BI and the Navigator window, upon further inspection we see that the **Product_Table** was parsed according to our expectations. Similar to the `sales` table, the table column header names were correctly propagated. So let's check its checkbox in the Navigator window. The **geo** spreadsheet parsing indicates that there are some unnecessary leading rows in the data which we will take care of soon. Select the **geo** checkbox. As we have seen before, the **manufacturer** spreadsheet will also need some cleaning operations since as we already have seen its entries are somewhat transposed. Select the **manufacturer** checkbox. Your `Navigator` window should look like this:

{{< figure src="/images/navigator_01.png" >}}

Hit OK in the `Navigator` window. The 3 selected tables should now appear as queries in the `Query Editor`. 

# Loading international sales data

Last but not least, we will load international sales data. In the `Query Editor` select `Home` -> `New Source` -> `Folder`, navigate to the folder `InternationalSales` containing those csv files: 

* Australia.csv
* Canada.csv
* Germany.csv
* Japan.csv
* Mexico.csv
* Nigeria.csv

and hit **OK**. You'll see a pop-up window listing those files with some additional meta-data. Click on **Combine & Transform Data**. This will open the **Combine Files** dialog allowing us to specify transformation steps for those files. The suggested default settings with **Comma** selected as the **Delimiter** work for us so let's go ahead and click on **OK**. Now, `InternationalSales` appears as an additional query in out `Queries`:

{{< figure src="/images/queries_01.png" >}}

Feel free to collapse the `Transform File from InternationalSales` query, we'll not use it. 

Select the `InternationalSales` query. A quick examination reveals that the `Zip` column has data-type `Whole Number` similar to what we have seen before. Let's change it to `Text` which will fix missing leading zeros. You may see a pop-up dialog asking you whether you want to replace the current step or add a new one. Select `Replace current` but in general you'll always need to decide for yourself which choice make most sense here. Next, we see that the query contains the `Source.Name` column indicating the filename which in turn contains the country name. Since we already have a country column we can remove the `Source.Name` column. Simply right-click on the column header and select `Remove` column:

{{< figure src="/images/remove_column.png" >}}

# Transforming the queries

We will now apply various transformations to columns

## Renaming the queries

Now that we have all of our baseline data loaded into our `Query Editor` let's prepare the data to be suitable for visualization and interactive analysis. We want to start with the obvious namely giving our queries adequate names. We see from the previous figure that the query names are somewhat all over the place, some are in lowercase, some are in CamelCase, some contain underscores to separate words. We should fix that and ake it more consistent and meaningful. Proper naming is crucial and should not be underestimated e.g. if you take a break from your Power BI session and come back to it a week later or if you intend to share findings with other which is one of the main purposes of Power BI. To rename a query, select the query in the `Queries` display and either right-click the query and select `Rename` or with the query selected press `F2` and type in the new name. Rename the queries as follows:

* sales -> Sales
* geo -> Geography
* manufacturer -> Manufacturer
* Product_Table -> Product
* InternationalSales -> International Sales

## Conformity of Product query and Fill feature

Select the **Product** query, click on the `ProductID` column header, and enable the `Column profile` from the `View` menu. Make sure the `Column profiling based on entire dataset` setting is active. What does the `Product` query represent? It specifies name, category, manufacturer and price for each product. Obviously, it should specify those parameters only once for each product. Furthermore, each product is represented by a unique ID. Knowing that, what do we in turn expect for the  `Count`, `Distinct` and `Unique` statistics of the `ProductID` column? We expect those three values values being the same! This is indeed true since all values amount to 2412. However, strictly speaking we would expect those values to also be equal for the `Product` column. However, this column only contains 2172 distinct values meaning that we have duplicates in that column. The item `Fama UE01|Extreme` for example appears multiple times once in row 280 and one more time in row 380. For the time being we will not worry too much about this. Those two items differ in price so there is some difference between them but the level of detail that would explain this difference is not accessible to us given the data we have. This is fine since we still have the `ProductID` which uniquely defines each product.   

Let's shift our focus and have a look at the `Category` column of the `Product` query. As mentioned before, entries in this column occur in a irregular fashion. The rule is that whenever there is a change in the segment-name that appears after the pipe in the `Product` column (i.e. All Season, Productivity, Select etc.) the category for products for that segment is specified followed by `null` entries for the remainder of that segment. The way I understand this labeling is that the specific category name applies to the remainder of that particular segment. So what we want to do is to fill up null entries with the preceding segment-name. Before we carry out this transformation we need to make sure that our **Product** query is sorted. Click on the down-arrow in the right corner of the `ProductId` column and select `Sort Ascending` and hit **OK**. 

{{< figure src="/images/sort_productid_01.png" >}}

Then select the `Category` column, in the ribbon select the `Transform` menu item and in the `Any column` section click on `Fill` and select `Fill down`. 

{{< figure src="/images/fill_category.png" >}}

The `null` entries in the `Category` column have now been replaced with the preceding category label which is what we want. 

## Split Column feature

As we have seen before, the `Product` column contains product-name and segment-name separated by a pipe. It therefore makes sense to extract the segment part and create a new column. We can do this using the `Split Column` feature which you can find in the `Home` menu item in the `Transform` section. Click on `Split Column` and select `By Delimiter`. In the pop-up dialog make sure `Custom` is selected from the drop-down list and enter the pipe sign `|` as the delimiter (remove the hyphen that was added by Power BI):

{{< figure src="/images/split_feature.png" >}}

Click **OK**. Power BI has automatically renamed the existing **Product** column to **Product.1** and the newly created column with the segment-name is called **Product.2**. Let's rename **Product.1** to **Product** and  **Product.2** to **Segment**. 

## Column From Examples feature

You may end up in situations where the rule for extracting column contents into new columns is not as clear as in the previous pipe example. For those cases, the `Column From Examples` feature may be helpful. The idea is to basically tell Power BI what part of a column entry you want to extract and Power BI will create the rule for you. Let's see that feature in action for the `Price` column (we're still looking at the `Product` query). Select the `Price` column, then from the `Add Column` menu item select `Column From Examples`, and then `From Selection`.     

{{< figure src="/images/column_from_examples.png" >}}

You'll see a new column being create on the right-hand side named `Column1`. In the first row of this new column type in the numerical value from the corresponding row from the `Price` column. In the example above I typed in the value 412.13:

{{< figure src="/images/column_from_ex_editing.png" >}}

Hit **Enter**. Power BI replaced the name of the column with `Text After Delimiter` which the name of the transformation that Power BI applied based on the example we have provided. You can apply that transformation manually using `Add Column` -> `Extract` -> `Text After Delimiter` with space used as delimiter. The transformation also was appended to our list of applied steps. 

## Apply remaining transformations to Product query

Now that you know some common transformations try to carry out those steps:

* Rename the `Text After Delimiter` to `MSRP` (a.k.a. Manufacturer Suggested Retail Price)
* Change the data-type of the `MSRP` column to **Fixed Decimal**
* Extract the currency portion from the `Price` column into a new column called `Currency` with data-type **Text**
* Remove the `Price` column

The `Product` query should look like this now:

{{< figure src="/images/Product_query_transformed.png" >}}

## Cleaning up the Geography query

Select the `Geography` query. As mentioned before, the original data contains some not very useful meta-data in the trailing rows which was propagated to the current query. Let's get rid of those rows and also make sure that the column names `Zip`, `City` etc. will be correctly applied to the query. From the **Home** menu click on **Remove Rows** in the **Reduce Rows** section. Select **Remove Top Rows**. In the pop-up dialog enter 2 for the **Number of Rows** parameter and hit **OK**. 

{{< figure src="/images/Remove_top_rows_02.png" >}}

Next, from the **Home** menu item click on **Use First Rows as Headers** in the **Transform** section. Now our column headers have proper names. In a final step, make sure that all columns have **Text** as data-type i.e. change the type if necessary. This also applies to columns indicating **ABC 123** as data-type which is the data-type **Any** (propagated from Excel).  

## Applying the Transpose feature to the Manufacturer query 

Select the `Manufacturer` query. We already have seen in the beginning of this tutorial that the `Manufacturer` data is somewhat flipped. So what we want to do first is to turn rows into columns. From the **Transform** menu item select **Transpose** from the **Table** section. Now, similar to the `Geography` query, the column names are contained in the first row so let's fix that using the **Use First Rows as Headers**. The last step we want to perform on the `Manufacturer` query is to remove all columns beyond the `Logo` column. Use {{< kbd Ctrl>}} + `Click` to select the 3 rightmost columns to speed this up.

## Combining Sales and International  queries

The last transformation we want to perform in the `Query Editor` is to combine the `Sales` and `International Sales` queries. This makes sense since later on we will want to analyze the data across countries. What we will do is to append the `International Sales` query to the `Sales` query. We will then perform a number of transformations on the `Sales` query. 

Select the `Sales` query. Select the `Sales` query and from the **Home** menu item select **Append Queries** from the **Combine** section. A pop-up dialog will show up allowing you to select how many tables you want to append and which ones to append. We leave the **Two Tables** setting selected and will select `International Sales` from the **Table to Append** drop-down list. 

{{< figure src="/images/append_queries.png" >}}

A new `Country` column in the `Sales` table with `null` values for US products was created. This makes sense since `International Sales` had an extra `Country` column. Now, we want to replace the `null` values with `USA`. But before we do that we need to make sure that `null` values only appear for US products. We can quickly ensure that this is the case by counting the number of rows in the `Sales` query before the merge with `International Sales` and check whether this number is equal to the number of null counts in the merged query. With the `Sales` query selected, select **Changed Type** from the **APPLIED STEPS** listing (i.e. one step before the **Appended Query** step) and enable **Column profile** from the **View** menu item and make sure that the entire dataset is used for profiling. After a couple of moments we count 4198753 rows in the `Sales` table. Now select the **Appended Query** step and select the `Country` column. Again, after a few moments of waiting for the calculation we see 4198753 **Empty** rows in the column statistics (`null` represents an empty entry). That's good news! So we can go ahead and apply the **Replace Values** feature to the `Country` column. Select the `Country` column, from the **Transform** menu item select **Replace Values** from the **Any column** section. In the pop-up dialog enter `null` in the **Value To Find** field and `USA` in the **Replace With** field and hit **OK**. 

Let's make sure that all columns in `Sales` have the correct data-type. Therefore, confirm the following data-types (and change if necessary):

* `ProductID` -> **Whole Number**
* `Date` -> **Date**
* `Zip` -> **Text**
* `Units` -> **Whole Number**
* `Revenue` -> **Fixed Decimal**
* `Country` -> **Text**

Last but not least, we want to exclude the `International Sales` table from being part of reloading the entire dataset since we don't need this table anymore. Right-click on the `International Sales` table and make sure that **Enable load** is unchecked. The font of the `International Sales` table now should change to italic indicating that the table is not part of the dataset anymore. 

