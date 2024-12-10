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
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
First, we had to read our data, which uncovered quirks in the construction of the data table. To properly convert the data into a Panda’s DataFrame we had to account for the empty header rows, the repeated indices, and the unnecessary column labels. Once we sliced the raw data to only start from the 5th row (where the data actually began), we began to construct our DataFrame with the considerations of removing unneeded columns, improving column naming conventions, and creating robust time data. To address these issues we first dropped the columns that were unnecessary to our research question: 'ANOMALY.LEVEL', 'HURRICANE.NAMES', 'PCT_LAND', 'PCT_WATER_TOT','PCT_WATER_INLAND'. Next, we updated the column names to fit our desired naming conventions, making all column names lowercase with underscores representing the spaces between words. Finally, we combined the date and time columns for both outage start and outage resotatino to create a comprehensive ‘outage_start’ and ‘outage_restoration’ DateTime column.


### Exploratory Data Analysis



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

