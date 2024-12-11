# Economic Analysis of Power Outages
**Authors: Tejas Mani & Willem de Haan**

## Project Overview
Originally for DSC80 at the University of California San Diego, this project works to analyze major power outages in the continental United States from 2000-2016 through an economic lens. The original dataset can be found [here](https://www.sciencedirect.com/science/article/pii/S2352340918307182).

___
## Introduction
This project examines a dataset of major power outages from January 2000 to July 2016 provided by Purdue’s Laboratory for Advancing Sustainable Critical Infrastructure. The goal of our analysis is to provide an economic perspective in which to examine disparities within the utility industry. How economic factors explain the length and cause of power outages will be the central question motivating our analysis. Investigating the relationship between power outages, their frequency and duration, and economic situation provides critical insight into how inequities affect United States citizens. The economic situation is defined by the aspects that explain living conditions such as region,  state’s gross domestic product, urban population, and utility statistics.

The dataset utilized in this project is comprised of 1534 rows and 56 columns, with many features that helped us conduct our analysis such as such as geographical, state economic, and composition data. The columns that were used within this analysis are grouped and defined here: 


#### Geographic & Regional Climate Data

|Column	                 |Description|
|---                     |---        |
|`'state'`               |Represents all the states in the continental U.S.|
|`'nerc_region'`	     |The North American Electric Reliability Corporation (NERC) regions involved in the outage event.|
|`'Climate Region'`	     |U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)|
|`'Climate Category'`	 |This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI).|

#### Outage Event Data

|Column	                 |Description|
|---                     |---        |
|`'outage_duration_hours'`	            |The duration of the outage reported in hours.|
|`'cause_category'`	         |Categories of all the events causing the major power outages.|
|`'cause_category_detail'`	         |Detailed description of the event categories causing the major power outages.|
|`'Customers Affected'`	              |The number of customers affected by the outage|

#### Economic Data

|Column	                 |Description|
|---                     |---        |
|`'total_sales'`	          |Total electricity consumption in the U.S. state (megawatt-hour).|
|`'total_realgsp'`	          |Real GSP contributed by all industries (total) (measured in 2009 chained U.S. dollars).|
|`'population'`	              |Population in the U.S. state in a year.|
|`'poppct_urban'`	     |Percentage of the total population of the U.S. state represented by the urban population (in %).|
|`'popden_urban'`	     |Population density of the urban areas (persons per square mile).|


To fully utilize this data we must first clean the data. Using the cleaned data we can then perform exploratory data analysis to better understand any relations within our data pertaining to economic markers and power outages. After which we will then analyze missingness to diagnose dependency within the data. After handling missing data as necessary, we will attempt to answer this analysis's central question through hypothesis testing and predictive modeling. 

___
## Data Cleaning & Exploratory Data Analysis
### Data Cleaning
First, we had to read our data, which uncovered quirks in the construction of the data table. To properly convert the data into a Panda’s DataFrame we had to account for the empty header rows, the repeated indices, and the unnecessary column labels. Once we sliced the raw data to only start from the 5th row (where the data actually began), we began to construct our DataFrame with the considerations of removing unneeded columns, improving column naming conventions, and creating robust time data. To address these issues we first dropped the columns that were unnecessary to our research question: `'ANOMALY.LEVEL'`, `'HURRICANE.NAMES'`, `'PCT_LAND'`, `'PCT_WATER_TOT'`,`'PCT_WATER_INLAND'`. Next, we updated the column names to fit our desired naming conventions, making all column names lowercase with underscores representing the spaces between words. Finally, we combined the date and time columns for both outage start and outage restoration to create a comprehensive `‘outage_start’` and `‘outage_restoration’` DateTime columns.

While this is not the entire list of manipulation we performed on the data, it comprises all the steps we carried out before visualizing economic trends of interest to us. A preview of the cleaned dataset is shown here: 

|   year |   month | us_state   | postal_code   | nerc_region   | climate_region     | climate_category   | cause_category     | cause_category_detail   |   outage_duration |   demand_loss_mw |   customers_affected |   res_price |   com_price |   ind_price |   total_price |   res_sales |   com_sales |   ind_sales |   total_sales |   res_percen |   com_percen |   ind_percen |   res_customers |   com_customers |   ind_customers |   total_customers |   res_cust_pct |   com_cust_pct |   ind_cust_pct |   pc_realgsp_state |   pc_realgsp_usa |   pc_realgsp_rel |   pc_realgsp_change |   util_realgsp |   total_realgsp |   util_contri |   pi_util_ofusa |   population |   poppct_urban |   poppct_uc |   popden_urban |   popden_uc |   popden_rural |   areapct_urban |   areapct_uc | outage_start        | outage_restoration   |   outage_duration_hours |
|-------:|--------:|:-----------|:--------------|:--------------|:-------------------|:-------------------|:-------------------|:------------------------|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|:--------------------|:---------------------|------------------------:|
|   2011 |       7 | Minnesota  | MN            | MRO           | East North Central | normal             | severe weather     | nan                     |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              51         |
|   2014 |       5 | Minnesota  | MN            | MRO           | East North Central | normal             | intentional attack | vandalism               |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |               0.0166667 |
|   2010 |      10 | Minnesota  | MN            | MRO           | East North Central | cold               | severe weather     | heavy wind              |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |              50         |
|   2012 |       6 | Minnesota  | MN            | MRO           | East North Central | normal             | severe weather     | thunderstorm            |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |              42.5       |
|   2015 |       7 | Minnesota  | MN            | MRO           | East North Central | warm               | severe weather     | nan                     |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |              29         |



### Exploratory Data Analysis

#### Univariate Analysis

The first form of univariate analysis we carried out was a simple one - visualzing the number of power outages by state using a Folium heatmap. 

<iframe
  src="assets/num_outages_per_state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



___
## Assessment of Missingness


___
## Hypothesis Testing


___
## Framing a Prediction Problem

___
## Baseline Model

___
## Final Model

___
## Fairness Analysis

___

