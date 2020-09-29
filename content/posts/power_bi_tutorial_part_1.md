+++
layout =  "post"
title =  "Power BI tutorial part 1: Installation and setup"
description ="Power BI tutorial part 1"
tags = ["Power BI", "Business Intelligence", "Tutorial"]
date = "2020-08-26"
share = true
comments = true
+++

Power BI is Microsoft's solution for creating powerful business intelligence reports. It provides tools for creating Extract, Transform and Load (ETL) pipelines as well as impressive dashboards containing visualizations, animations and user-interaction support. The steepness of Power BI's learning curve depends a bit on the user's background but with some practice one can manage to create a solid report within one day. To help newcomers to Power BI with getting started and understanding some fundamental concepts I created a tutorial in which we will create the Power BI dashboard below. 

{{< figure src="https://github.com/MMilczynski/BlogRawData/blob/master/PowerBITutorial/images/Dashboard_Intro_02.gif?raw=true" caption="The dashboard we will create for analyzing revenue data for the fictive company Van Ardsel and competition." width="100%" >}}

We will analyze revenue data for the fictive company Van Ardsel and its competitors. Power BI will allow us to to interact with the dashboard and analyze the data by country, year, and product segments. We will e.g. find that Van Ardsel sales were stronger for certain product segments relative to its competition. By performing drill-down/up operations based on a relational data model we will obtain deeper insights into the data. 

The tutorial I'm going to present here is based on a Power BI workshop that took place in Irvine, CA in early 2019. Power BI underwent some substantial changes since then so it makes sense to look at some new features as well. Since the material is quite extensive I divided it up into a three-part series as follows:

* Part 1: Installation and setup (the current document)
* [Part 2: Data inspection, cleaning and transformations]({{< ref "/posts/power_bi_tutorial_part_2.md" >}})
* Part 3: Data model, visualizations and dashboard creation

<!-- {{< table_of_contents >}} -->

Are you ready? So let's get started!

# Installing, launching and configuring Power BI

Follow this link [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) to get the free version of the application. You'll find a download link in the **Microsoft Power BI Desktop** section which will link you to the Microsoft Store. After downloading and installing you should be ready to launch Power BI. Upon launch you'll see this intro screen:

{{< figure src="https://github.com/MMilczynski/BlogRawData/blob/master/PowerBITutorial/images/intro_screen_01.png?raw=true" caption="Intro screen upon launching Power BI Desktop." width="100%" >}}

If you don't want to see this screen in future sessions you can un-check the **Show this screen on startup** checkbox in the bottom of the screen. Close the intro screen manually. 

To ensure correct interpretation of the data we will deal with in [part 2]({{< ref "/posts/power_bi_tutorial_part_2.md" >}}) we need to make sure that we are using the correct regional settings. Click on the **File** menu item and select **Options and settings** -> **Options**. The **Options** pop-up dialog will show up. In the **CURRENT FILE** section click on **Regional settings** and make sure that **English (United States)** is selected as shown below. 
 
{{< figure src="https://github.com/MMilczynski/BlogRawData/blob/master/PowerBITutorial/images/options_regional_settings_01.png?raw=true" >}}

# Things to be aware of

Before we get to grips with the nitty-gritty details of Power BI it's important to mention a few things you will encounter. Most importantly, don't get frustrated! Instead, if you think you get stuck come back to this section and see whether you can resolve a particular issue. 

## Query editor vs. main power BI desktop window

In Power BI you will mainly work in two different windows namely:

* **Main Power BI desktop window**
* **Query Editor**

The **Main Power BI desktop window** can be recognized by the distinct `Three-icon bar` on the on the left-hand side and the **Visualization** and **Fields** sections: 

{{< figure src="https://github.com/MMilczynski/BlogRawData/blob/master/PowerBITutorial/images/main_power_bi_window_02.png?raw=true" >}}

This window is always present and will be in our main focus in part 3 of this tutorial. 

In [part 2]({{< ref "/posts/power_bi_tutorial_part_2.md" >}}), we will spend quite some time in the so called **Query Editor** in which we will perform data transformations on imported data. The **Query Editor** automatically opens once data is imported. Without any data imported it doesn't really make sense to make the **Query Editor** visible. With some data imported, the **Query Editor** contains a **Queries** listing on the left-hand side and the **Query Settings** on the right-hand side which in turn contains the **APPLIED STEPS** listing:  

{{< figure src="https://github.com/MMilczynski/BlogRawData/blob/master/PowerBITutorial/images/query_editor_02.png?raw=true" >}}

If you accidentally close the **Query Editor** while working on data the easiest way to restore it from the main window is to right-click on one of the tables in the **Fields** section and select **Edit query**:

{{< figure src="https://github.com/MMilczynski/BlogRawData/blob/master/PowerBITutorial/images/restore_query_editor_01.png?raw=true" >}}

Tables will also be referred to as `queries` and I will use those words interchangeably. In the **Query Editor**, if you accidentally close the the **Query Settings** you can restore them by clicking on **Query Settings** in the **View** menu item.   

## Power BI is not perfect

Despite it's powerful features Power BI sometimes can be counter-intuitive as far as e.g. which menus contain which buttons but there are only a few examples for that. Also, Power BI seems to be under constant development and therefore may contain bugs or some features may change in between updates. 
 
Congratulations for finishing part 1! Now you should be in good shape and ready to get your hands dirty with [part 2]({{< ref "/posts/power_bi_tutorial_part_2.md" >}}) of this tutorial.
