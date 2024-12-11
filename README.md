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
First, we had to read our data, which uncovered quirks in the construction of the data table. To properly convert the data into a Panda’s DataFrame we had to account for the empty header rows, the repeated indices, and the unnecessary column labels. Once we sliced the raw data to only start from the 5th row (where the data actually began), we began to construct our DataFrame with the considerations of removing unneeded columns, improving column naming conventions, and creating robust time data. To address these issues we first dropped the columns that were unnecessary to our research question: `'ANOMALY.LEVEL'`, `'HURRICANE.NAMES'`, `'PCT_LAND'`, `'PCT_WATER_TOT'`,`'PCT_WATER_INLAND'`. Next, we updated the column names to fit our desired naming conventions, making all column names lowercase with underscores representing the spaces between words. Moving on, we combined the date and time columns for both outage start and outage restoration to create a comprehensive `‘outage_start’` and `‘outage_restoration’` DateTime columns. Our last form of cleaning came via column manipulation, where we created a new column `'outage_duration_hours'`, as we felt it was hard to understand the current form of the duration column in minutes. This would prove to be immensely helpful for our analysis. 

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

The first form of univariate analysis we carried out was a simple one - visualzing the number of power outages by state using a Folium heatmap. It is clear that the trend in our data shows that most power outages happen in states with more developed industrial sectors overall - such as California, Texas, and Michigan. 


<iframe
  src="assets/num_outages_per_state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We were chiefly concerned between the relationship of outages and gross state product, with our assumption being that less developed states would suffer more frequent outages. This initial assumption was not met by our state count of outages, so we were less confident it would stand once we visualized on gross state product. The geographical distribution is presented below:

<iframe
  src="assets/total_real_gsp_by_state.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot backs up our skepticism, as it shows that states with a higher average real GSP typically have more frequent outages. The two heatmaps overlap in which states are typically seeing more outages as well - leading us to believe that this correlation does exist in some capacity. However, these states also contain numerous large cities, leaving us to wonder about the role of how urban an area is affecting its power outage count. 

In addition to visualizing where power outages are typically occurring, we wanted to look into how they were caused. The following bar chart displays Power Outages by Cause Category, where it is increasingly clear that both severe weather and surprisingly, intentional attacks make up the most power outage causes in our dataset. We were particularly intrigued by the intentional attack category containing such a large make-up of outages, setting the stage for a future hypothesis test. 

<iframe
  src="assets/count_outages_cause_category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Bivariate Analysis

Pursuing the line of thought on highly-populated cities potentially impacting power outages we then created a plot with two variables: Population vs. Total Real GSP. We colored our data points to better illustrate the gradual increase in GSP as population simeltaneously increased, even finding that a quadratic fit line was very strong in explaining the trend of the data. 

<iframe
  src="assets/pop_state_gsp_quadratic.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

#### Aggregation
To further understand the utility experience residents of urban areas underwent we explored the cause category column. We created a pivot table indexed by both state and its urban population percent, and then displayed the outage duration proportions for each cause category. Longer outages were almost always dominated by severe weather instances, which is stagnant with our initial univariate analysis of cause category. A solid amount of overall outage durations were made up of intentional attacks - which were common in areas of high urban population percentage. We also noticed that many states had their overall outage duration dominated by a specific category, leading us to conclude that the data had many large outliers, such as the fuel supply emergency outages making up 46% of California's outage duration. This aggregation combined a lot of the economic markers we were interested in, and set us up to delve into these with further testing. The first 10 rows of this aggregation are provided here.

|                                 |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:--------------------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| ('Alabama', 59.04)              |          0          |                0        |           0.0513761  | 0           |        0        |         0.948624 |                       0         |
| ('Arizona', 89.81)              |          0.00515079 |                0        |           0.0237866  | 0           |        0        |         0.956763 |                       0.0142995 |
| ('Arkansas', 56.16)             |          0.0237484  |                0        |           0.123906   | 0.000678526 |        0.240586 |         0.611081 |                       0         |
| ('California', 94.95)           |          0.0398765  |                0.467644 |           0.0719145  | 0.0163254   |        0.154102 |         0.222506 |                       0.0276324 |
| ('Colorado', 86.15)             |          0          |                0        |           0.037428   | 0.000639795 |        0        |         0.872441 |                       0.0894914 |
| ('Connecticut', 87.99)          |          0          |                0        |           0.0212504  | 0           |        0        |         0.97875  |                       0         |
| ('Delaware', 83.3)              |          0.0222974  |                0        |           0.0173558  | 0           |        0        |         0.960347 |                       0         |
| ('District of Columbia', 100.0) |          0.0322967  |                0        |           0          | 0           |        0        |         0.967703 |                       0         |
| ('Florida', 91.16)              |          0.048007   |                0        |           0.00432886 | 0           |        0.374013 |         0.555842 |                       0.0178089 |
| ('Georgia', 75.07)              |          0          |                0        |           0.0705537  | 0           |        0        |         0.929446 |                       0         |


___
## Assessment of Missingness

### NMAR Data

Before conducting tests using our cleaned data we needed to address missingness. Data can be defined as NMAR (Not Missing at Random) if the chance that a value is missing depends on the actual missing value. Essentially, it is non-ignorable, the fact that the data is missing data in and of itself that we cannot ignore. Let's delve into some columns we believed were NMAR within the dataset.

After querying to find missing values we found nearly all columns contained missing values. Upon analyzing the columns with missing values we believe that `'customers_affected'` and `'demand_loss_mw'` are NMAR. These columns accounted for a majority of the missing values within the dataset. When considering the possible ways these values could be missing we determined that the data was plausibly NMAR. This is because companies could fail to report the demand loss or customers affected by an outage if the values were deemed insignificant. However, to rule out the possibility that `'demand_loss_mw'` and `'customers_affected'`’s missingness is independent of any other statistic we would like to collect company-specific data such as locale and gross profit. This is due to our belief that smaller companies could be less compliant in providing this counting data as they lack the infrastructure needed to make these assessments.

### Missing Dependencies

Now, we will take a more nuanced approach in proving a missigness dependency. Our column of choice to examine missigness will be `'cause_category_detail'`. The following process involves constructing and performing a permutation test to determine whether the missing data within `'cause_category_detail'` is NMAR on another specific column or not. 

#### Permutation Test 1

__Null Hypothesis__:
- The missingness of `'cause_category_detail'` is independent of `'cause_category'`.(Missing values are evenly distributed across outage categories, indicating no relationship between `'cause_category'` and missingness.)

__Alternative Hypothesis__:
- The missingness of `'cause_category_detail'` is dependent on `'cause_category'`. (Missingness varies significantly across outage categories, indicating a relationship between `'cause_category'` and missingness.)

__Significance Level__<br>
For this permutation test, we will use a significance level of 0.05.

__Observed__ <br>
Below are the observed distributions of the `'cause_category_detail'` column with and without missing `'cause_category'` values.

<iframe
  src="assets/category_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We will use TVD (Total Variation Distance) as our test statistic for the permutation test since we are comparing two categorical distributions. 

__Simulated__ <br>
Below is a histogram representing the results of 1000 simulated TVDs under our defined null hypothesis.

<iframe
  src="assets/category_missing_perm_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

__Conclusion__ <br>
With a p-value of 0.0, we reject the null hypothesis in this permutation test, and determine that the missingness of `'cause_category_detail'` is highly likely to be MAR (Missing at Random) with respect to `'cause_category'`.

#### Permutation Test 2

__Null Hypothesis__:
- The missingness of `'cause_category_detail'` is independent of `'nerc_region'`.(Missing values are evenly distributed across outage categories, indicating no relationship between `'nerc_region'` and missingness.)

__Alternative Hypothesis__:
- The missingness of `'cause_category_detail'` is dependent on `'nerc_region'`. (Missingness varies significantly across outage categories, indicating a relationship between `'cause_category'` and missingness.)

__Significance Level__<br>
For this permutation test, we will use a significance level of 0.05.

__Observed__ <br>
Below are the observed distributions of the `'cause_category_detail'` column with and without missing `'nerc_region'` values.

<iframe
  src="assets/nerc_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We will once again use TVD as our test statistic for the permutation test since we are comparing two categorical distributions. 

__Simulated__ <br>
Below is a histogram representing the results of 1000 simulated TVDs under our defined null hypothesis.

<iframe
  src="assets/nerc_missing_perm_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

__Conclusion__ <br>
With a p-value of 0.069, we we fail to reject the null hypothesis in this permutation test, and conclude that the missingness of the `'cause_category_detail'` column is not likely to be MAR with respect to `'nerc_region'`.

Understanding our data and its missingness mechanisms creates deeper insights into the relationships between columns. Using dependency within columns we can better account for missingness within the dataset, and better understand how to utilize data to avoid skewing the results of future models and tests.


___
## Hypothesis Testing

Building on the framework developed in our exploratory statistical analysis, we have decided to examine the economic factors underlying the causes of power outages. Further pursuing how an economic situation affects the cause of the power outage we sought to answer whether an outage caused by an intentional attack has a higher likelihood of occurring in an urban or rural setting. Acts of sabotage or vandalism fall under the umbrella of intentional attacks. By analyzing the setting these attacks take place we can better understand the living conditions that would spur this behavior.

__Null Hypothesis__:
- An outage that is caused by an intentional attack is equally likely to occur in an urban or rural setting. 

__Alternative Hypothesis__:
- An outage that is caused by an intentional attack has a higher liklihood of occuring in an urban setting.

__Significance Level__:
- This permutation test used a significance level of 0.01. This choice is stricter than the standard 0.05 level, but it is valid in this scenario because we are handling a problem that could create stringest real-world affects (increased intentional attack protection) so it is important to be very certain in handling our hypothesis.

__Test Statistic__:
- This permutation test used the test statistic of difference in means. Since we hypothesize that outage caused by intentional attacks occur more in urban areas in our alternate hypothesis (backed by findings from our initial EDA), the test statistic must point in a direction. Therefore, the valid statistic to use is difference in means, which is perfect for illustrating a directional result. 

The empiricial distribution for this permutation test is below. 

<iframe
  src="assets/hyp_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

__Conclusion__:

The test yielded a significant result, with a p-value of 0.0002, which suggests that intentional attacks occur at a higher frequency in urban settings versus rural ones. However, this finding is not comprehensive, as other confounding factors, such as population density, infrastructure differences, and regional law enforcement presence, could also contribute to the observed frequency and may need to be considered in further analysis.


___
## Framing a Prediction Problem

### Motivation
After establishing connections between characteristics of power outages, such as frequency and type, with economic situations we were interested in better understanding how economic situations affected power restoration. Our goal was to develop a model that would use aspects that define the economic situation, such as state, utility, and region data available at the time of the outage, to predict if an outage would be short (0 to 6 hours), medium (6 to 15 hours), or long (15 plus hours). The idea behind these classification bins is the following: a short prediction is designed for residential outages and can help families prepare to be without power for a part of their day, a medium prediction is meant for low-level buisnesses and abnormally long residential outages, and a long prediction is meant to forecast necessary precautions to be without power for more than a day in the case of severe weather or other pertinent causes.

### Specifications
To evaluate the accuracy of our baseline and final model we will use a weighted F-1 average. F-1 score provides a metric that balances both precision and recall when measuring accuracy, which is critical as we want to reduce both the amounts of false positives and false negatives. Since we are constructing a multi-class classifier it is important to use a weighted average as the classes are imbalanced, and each of their proportions should be accounted for. By creating this model we hope to provide a means of diagnosing how quickly power outages are dealt with given economic factors. 

We employed some mild data manipulation prior to training and tuning our classifier: creating a new column `'outage_category'` with the aforementioned outage duration bins and probabalistically imputing missing values for features that we would be using. We chose to use this approach of imputation over other measures like mean/median imputation or dropping the columns outright to keep the integrity of our data and not cause the model to be too reliant on predicting a median value for instance. 
___
## Baseline Model

___
## Final Model

___
## Fairness Analysis

___

