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

While I already presented the columns of the DataFrame that will be valuable to my analysis, the DataFrame required significant cleaning to arrive at a usable state. These are the first five rows of the raw DataFrame __(Make sure to scroll!)__

|    |   OBS |   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | OUTAGE.START.DATE         | OUTAGE.START.TIME   | OUTAGE.RESTORATION.DATE    | OUTAGE.RESTORATION.TIME   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND |
|---:|------:|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:--------------------------|:--------------------|:---------------------------|:--------------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|
|  0 |     1 |   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | Friday, July 1, 2011      | 5:00:00 PM          | Sunday, July 3, 2011       | 8:00:00 PM                | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |         0.4112 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|  1 |     2 |   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | Sunday, May 11, 2014      | 6:38:00 PM          | Sunday, May 11, 2014       | 6:39:00 PM                | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |         0.3748 |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|  2 |     3 |   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | Tuesday, October 26, 2010 | 8:00:00 PM          | Thursday, October 28, 2010 | 10:00:00 PM               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |         0.3924 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|  3 |     4 |   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | Tuesday, June 19, 2012    | 4:30:00 AM          | Wednesday, June 20, 2012   | 11:00:00 PM               | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |         0.4224 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |
|  4 |     5 |   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | Saturday, July 18, 2015   | 2:00:00 AM          | Sunday, July 19, 2015      | 7:00:00 AM                | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |         0.367  |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |

While a daunting DataFrame to clean, I split the process into four steps:
- Remove redundant or valueless columns,
- Perform necessary datatype transformations,
- Create new features using existing data,
- Rename existing columns for readability.

Then, I divided the dataset into components for the data cleaning process to familiarize myself with all of the information conveyed in the dataset. For this, I generally defaulted to the categorizations used by Sayanti Mukherjee, Roshanak Nateghi, and Makarand Hastak in their explanation of the variables in their article [Data on major power outage events in the continental U.S.](https://www.sciencedirect.com/science/article/pii/S2352340918307182). However, this came with numerous adjustments. Within each component, I walked through the four steps I described above.

- __Geographic / Climate Data__: The climate data in the raw dataset was generally very well organized and formatted. In cleaning these columns, I first dropped redundancies present in the `'U.S._STATE'` and `'ANOMALY.LEVEL'` columns. They contained the same information as the `'State'` and `'Climate Category'` columns of the cleaned DataFrame, respectively, and were unnecessary to include. Then, I aggregated the time-related columns in the raw DataFrame into the `'Year'` column of the cleaned DataFrame. This entailed converting one of the columns to a DateTime datatype, extracting its year, and removing the resulting redundancies. I renamed the columns for readability and moved forward.
- __Event-Specific Data__: There was no mechanically challenging data cleaning on any event-specific columns in the raw DataFrame. I removed repetitive columns, renamed the existing ones to the versions you were introduced to, and moved on.
- __Economic Data__: Once again, the economic data in the raw DataFrame was presented very well. There were unnecessary redundancies in the data, so I removed them, renamed the columns, and concluded cleaning the data.

__Note__: Originally, I divided the DataFrame into seven components, and performed data cleaning on all 58 features of the original DataFrame. This resulted in a 36-column DataFrame, but it is unnecessary to include details on the entire data cleaning process for this analysis. As such, I only included data cleaning on portions relevant to my following analysis. But don't worry, _no stone was left unturned!_

The first five rows of the cleaned DataFrame look like this:

|    | State   | NERC Region   | Climate Region     | Climate Category   |   Year |   Duration | Cause Category     | Hurricane Name   |   Customers Affected |   Demand Loss |   GSP Relative to USA |   Change in GSP |   Utility Percent of GSP |   Residential Price |
|---:|:--------|:--------------|:-------------------|:-------------------|-------:|-----------:|:-------------------|:-----------------|---------------------:|--------------:|----------------------:|----------------:|-------------------------:|--------------------:|
|  0 | MN      | MRO           | East North Central | normal             |   2011 |       3060 | severe weather     | None             |                70000 |           nan |               1.07738 |             1.6 |                  1.75139 |               11.6  |
|  1 | MN      | MRO           | East North Central | normal             |   2014 |          1 | intentional attack | None             |                  nan |           nan |               1.08979 |             1.9 |                  1.79    |               12.12 |
|  2 | MN      | MRO           | East North Central | cold               |   2010 |       3000 | severe weather     | None             |                70000 |           nan |               1.06683 |             2.7 |                  1.70627 |               10.87 |
|  3 | MN      | MRO           | East North Central | normal             |   2012 |       2550 | severe weather     | None             |                68200 |           nan |               1.07148 |             0.6 |                  1.93209 |               11.79 |
|  4 | MN      | MRO           | East North Central | warm               |   2015 |       1740 | severe weather     | None             |               250000 |           250 |               1.09203 |             1.7 |                  1.6687  |               13.07 |

### Exploratory Data Analysis

To further our understanding of the information, let's examine some visualizations related to our dataset.

##### Outages over Time
The most fundamental unit of our dataset is power outages, and understanding how their frequency has changed over time is a crucial piece of context to have when conducting any form of analysis on this data. Below, we see the frequency of power outages in every month in our dataset. There is a visible spike in the summer of 2011, when the United States experienced one of the most active hurricane seasons in its history. Climaxing with Hurricane Irene in August but containing a total of 19 named storms, this spike in our dataset makes sense.

<iframe src="assets/outages_over_time.html" width="800" height="600" frameborder="0"></iframe>
  
##### Cause Distribution
Another critical feature in our dataset is the `'Cause Category'` of a power outage. The visualization below displays the distribution of `'Cause Category'`, showing `'Severe Weather'` as the most common cause. While this is intuitive, it is interesting that `'Intentional Attack'` was the second leading cause of major power outages from 2000-2016 in the United States.

<iframe src="assets/cause_distribution.html" width="800" height="600" frameborder="0"></iframe>

With the more basic, yet critical, visualizations completed, let's explore a few more nuanced relationships before continuing.

##### Cause by Climate Region
This table displays the distribution of `'Cause Category'` between the different `'Climate Region'`s present in our DataFrame. While much of the information is intuitive, it is interesting to note that the West North Central region has a markedly low number of major power outages. Further, I found it interesting that in the South, the second-leading cause of major power outages was `'Public Appeal'`.

<iframe src="assets/climate_region_cause.html" width="800" height="600" frameborder="0"></iframe>

##### Power Outages per State per Year
This interactive choropleth displays the number of major power outages per state per year. There are lots of interesting features in this visualization (Look at Washington in 2011, for example), and I welcome you to explore the plot on your own and observe the trends over time!

<iframe src="assets/outages_by_year.html" width="800" height="600" frameborder="0"></iframe>

Now, let's continue with our analysis.

___
## Assessment of Missingness

### NMAR Data
Data that is Not Missing At Random (NMAR) is data in which the missingness of a value is likely dependent on the value itself. A common example of NMAR data is missing data in a survey about education level. In this example, individuals with lower education levels are less likely to reply to the survey, and as such, the missingness of a value likely depends on what the value would have been.

Let's apply this definition to our dataset to determine whether we have any NMAR data.

First, let's look at the columns that contain missing values:

|Column	                 |Description|
|---                     |---        |
|`'Climate Region'`            |The U.S. Climate region, as specified by National Centers for Environmental Information, in which the outage occurred|
|`'Hurricane Name'`	           |The name of the hurricane that caused the outage, if applicable|
|`'Duration'`	           |The length of the outage|
|`'Demand Loss'`	         |The peak electricity demand loss during the outage|
|`'Customers Affected'`	           |The number of customers affected by the outage|

Now, let's discuss the possibility of NMAR data in each of these columns.

- `'Climate Region'`: In the dataset, all of the of missing `'Climate Region'` values have a corresponding `'State'` value of `'HI'`, meaning that Hawaii does not have a `'Climate Region'` in this dataset. While the missing value certainly "depends" on other columns (Specifically, the `'State'` column), thinking critically about its missingness reveals that this data does not truly depend on other columns or the missing value itself. With some domain knowledge about the National Centers for Environmental Information, one understands that the U.S. Climate regions in this dataset only encapsulate the contiguous United States. As such, any row concerning `'Hawaii'` cannot have a value in the `'Climate Region'` column- it wouldn't make sense.
- `'Hurricane Name'`: The missingness of the `'Hurricane Name'` column is self-explanatory. As these values are only filled when applicable, it makes sense that many of these values would be missing in our dataset. It would not make sense to include a Hurricane Name for a power outage caused by an intentional attack, or public appeal. As in `'Climate Region'`, this data is missing by design, and not dependent on the value of the data.
- `'Duration'`, `'Demand Loss'`, `'Customers Affected'`: The missingness mechanism of the values in these three columns is interesting to discuss. Intuitively, it seems as though more severe outages would be more likely to produce metrics that are hard or impossible to accurately capture- who knows, for example, the true duration of an outage caused by Hurricane Katrina, when the first power was lost and the last power was restored. And with this logic, the data in all three of these columns is NMAR. Higher values of `'Duration'`, `'Demand Loss'`, and `'Customers Affected'` would be significantly harder to confidently measure and accurately report, thus making them more likely to be missing. However, this fails to consider confounding variables that are also correlated with outage severity. As I will show in the next section of the analysis, many other featuers indirectly influence the likelihood that the data in these columns are missing.

__Conclusion__: None of the columns in the dataset are truly NMAR. Despite valid logic supporting their classification as such, further analysis will show that other features impact the likelihood that data, otherwise labeled as NMAR, is missing. As such, these data are truly Missing At Random (MAR).

### Missingness Dependency

As previously mentioned, I will show that the missing data in the `'Customers Affected` column of the dataset is not `'NMAR'`, and has a dependency on other features in the dataset.

This process involves constructing and executing a permutation test to determine whether unrelated columns could produce what we observe in our dataset.

#### Permutation Test 1

__Null Hypothesis__:
- The missingness of the `'Customers Affected`' column is not dependent on the `'Cause Category'` column

__Alternative Hypothesis__:
- The missingness of the `'Customers Affected'` column is dependent on the `'Cause Category'` column.

__Significance Level__<br>
For this permutation test, we will use the standard significance level of 0.05.

__Observed__ <br>
Below are the observed distributions of the `'Cause Category'` column with and without missing `'Customers Affected'` values.

<iframe src="assets/file-name.html" width="800" height="600" frameborder="0"></iframe>
Visually, these distributions look significantly different, but it is inappropriate to make judgments before conducting a permutation test to determine dependency. As we are comparing the distribution of two categorical variables, the test statistic for this permutation test is TVD.

__Simulated__ <br>
Below is a histogram representing the results of 1000 simulated TVDs under the Null Hypothesis.

<iframe src="assets/cause_category_simulated.html" width="800" height="600" frameborder="0"></iframe>

__Conclusion__ <br>
With a p-value of 0.0, we reject the Null Hypothesis in this permutation test, and determine that the missingness of `'Customers Affected'` is highly likely to be MAR with respect to `'Cause Category'`.

#### Permutation Test 2

__Null Hypothesis__:
- The missingness of the `'Customers Affected`' column is not dependent on the `'Climate Category'` column

__Alternative Hypothesis__:
- The missingness of the `'Customers Affected'` column is dependent on the `'Climate Category'` column.

__Significance Level__:
- For this permutation test, we will use the standard significance level of 0.05.

__Observed__ <br>
Below are the observed distributions of the `'Climate Category'` column with and without missing `'Customers Affected'` values.

<iframe src="assets/climate_category_observed_real.html" width="800" height="600" frameborder="0"></iframe>
 These distributions look significantly more similar than the `'Cause Category'` distributions did, but as before, we will wait to pass judgment until we successfully conduct a permutation test. Once again, we are comparing the distribution of two categorical variables, and so the test statistic for this permutation test is TVD.
 
__Simulated__ <br>
Below is a histogram representing the results of 1000 simulated TVDs under the Null Hypothesis.

<iframe src="assets/climate_category_simulated.html" width="800" height="600" frameborder="0"></iframe>

__Conclusion__ <br>
With a p-value of 0.3 - 0.4, we fail to reject the Null Hypothesis in this permutation test, and conclude that the missingness of the `'Customers Affected'` column is not likely to be MAR with respect to `'Climate Category'`.

___
## Hypothesis Testing

### Introduction:

One would be remiss to avoid tackling the potential economic inequity underlying the information we wrestle with. As such, the remainder of this analysis works to determine whether economic inequity impacts the nature of major power outages and vice versa.

Let's take a look at the economic data we're working with, in the context of a few other columns.

|    | State   |   Duration | Cause Category     |   GSP Relative to USA |   Change in GSP |   Utility Percent of GSP |
|---:|:--------|-----------:|:-------------------|----------------------:|----------------:|-------------------------:|
|  0 | MN      |       3060 | severe weather     |               1.07738 |             1.6 |                  1.75139 |
|  1 | MN      |          1 | intentional attack |               1.08979 |             1.9 |                  1.79    |
|  2 | MN      |       3000 | severe weather     |               1.06683 |             2.7 |                  1.70627 |
|  3 | MN      |       2550 | severe weather     |               1.07148 |             0.6 |                  1.93209 |
|  4 | MN      |       1740 | severe weather     |               1.09203 |             1.7 |                  1.6687  |

While `Change in GSP` and `Utility Percent of GSP` are valuable pieces of information, we will explore them further later. For this Hypothesis Test, I deemed it fitting to generalize the economic wellbeing of a state to its `GSP Relative to USA`. 
- This column contains information on "Relative per capita real GSP as compared to the total per capita real GDP of the U.S. (expressed as fraction of per capita State real GDP & per capita US real GDP)" [source](https://www.sciencedirect.com/science/article/pii/S2352340918307182)


__Null Hypothesis__:
- There is no statistically significant difference in the mean duration of major power outages between states grouped by their GSP per capita relative to the United States' GDP per capita.

__Alternative Hypothesis__:
- There is a statistically significant difference in the average duration of major power outages between states grouped by their GSP per capita relative to the United States' GDP per capita.

__Significance Level__:
- For this hypothesis test, it is appropriate to use 0.005 as the significance level. As we are handling complicated data that relates to important economic factors, we must be certain that inequities exist in order to justify acting on them or investigating them further.

To set up, I added a column to our DataFrame that breaks the `Relative GSP Category` column into quartiles. Using the numerical values of the original column is a far too granular approach, and I decided on using quartiles for the sake of convention. Given unlimited time, I would likely perform further research into the appropriate number of groups to split the category into.

Although the below DataFrame is not what we will use to conduct our Hypothesis Test, it is valuable to have a rudimentary understanding of our grouped data. Below, I displayed the mean Duration of major power outages per Relative GSP Category.

| Relative GSP Category   |   Duration |
|:------------------------|-----------:|
| (0.639, 0.894]          |    2637.46 |
| (0.894, 1.014]          |    3612.17 |
| (1.014, 1.11]           |    1791.83 |
| (1.11, 3.561]           |    2524.95 |

__Important!__
While this DataFrame looks similar to a population distribution, allowing for the use of TVD, it is not. This represents multiple group means, and none of these values need to sum to anything specific (Like the total duration, average duration, etc.).

### Defining a Test Statistic
It is not immediately obvious what test statistic we should use to approach this hypothesis test. 

#### Intuitive Idea

It makes intuitive sense to compare the average unsigned distance of each group's mean to the global mean. This is a vague analogy to how TVD operates and attempts to compare the distribution to some "null" distribution.

__However__, this does not consider the variability within groups themselves, which is crucial to understanding the variability of our permuted samples under the null hypothesis.
- In other words, one must consider both distances from the global mean __and__ variance within groups to gain an accurate understanding of the data under the null hypothesis.

#### Solution: ANOVA F-Statistic:

The ANOVA F-Statistic measures the ratio of two variances:

- __Between-Group Variance__: How much the means of different groups vary from the global mean of the data. This reflects the variability in data due to the categorical variable we group by.

- __Within-Group Variance__: How much individual data points within each group vary from their own group mean. This reflects the variability in data that is due to randomness or inherent variation within each group.

Below is the formula for the ANOVA F-Statistic:

<img src="https://latex.codecogs.com/svg.latex?F%20%3D%20%5Cfrac%7B%5Ctext%7BBetween-Group%20Variance%7D%7D%7B%5Ctext%7BWithin-Group%20Variance%7D%7D" alt="F = \frac{\text{Between-Group Variance}}{\text{Within-Group Variance}}">
 
Here's what the it indicates:

A __high__ F-value suggests that the between-group variance is significantly larger than the within-group variance. This implies that the group means are not all the same; in other words, at least one group mean is significantly different from the others. The differences observed between groups are likely not due to chance alone.

A __low__ F-value suggests that the between-group variance is not much larger than the within-group variance, indicating that any differences in group means could likely be due to random variation rather than an effect of the categorical variable.

With this in mind, we will conduct a permutation test, using the ANOVA F-statistic as our test-statistic, and comparing our simulated tests with our observed statistic.

__Simulated__
Below is a histogram representing the results of 1000 simulated F-Statistics under the Null Hypothesis
<iframe src="assets/simulated_f_statistic.html" width="800" height="600" frameborder="0"></iframe>


__Conclusion__
With a p-value of 0, we reject the null hypothesis. We conclude that the economic well-being of a state, as measured by its GSP relative of the GDP of the United States, likely impacts the duration of the state's major power outages.
- This does not tell us about the true cause of lengthier power outages.

___
## Framing a Prediction Problem
### Introduction / Motivation

To continue with the theme of examining pertinent economic data, I feel it appropriate to frame a prediction problem that works to answer a larger economic question. There are a multitude of factors to consider and prediction problems to pursue, but my attention was captured by the `Change in GSP` column in our DataFrame.

This column represents "Percentage change of per capita real GSP from the previous year (in %)," in the [words](https://www.sciencedirect.com/science/article/pii/S2352340918307182) of the researchers who provided this dataset. This immediately sparked my interest, and made me wonder whether the GSP of a state who had a major power outage would decline. This idea simply leverages the intuitive premise that significant disruptions in power supply could have tangible impacts on economic activity and productivity, which, in turn, could affect the GSP.

This question is as fascinating as it is important, and further explores the potential economic inequity of our country through the lens of major disruptances in power supply.

__Note__:
There are a few things to consider when approaching this problem head-on.
- GSP is influenced by a wide array of factors beyond power outages, including fiscal policies, global economic conditions, technological advancements, and more. Isolating the impact of power outages from these variables is extremely challenging, and may only provide part of the picture.
- However, even if the predictive power for GSP changes directly is limited, the exploration could yield valuable insights into the resilience of different sectors or regions to power outages and contribute to broader economic resilience and planning discussions.

In short, this prediction problem is worth pursuing regardless of the outcome, and we should not shy away from the potential shortcoming of our predictive model.

### Problem Specifications:
- This prediction problem is a regression problem, as the change in GSP a state experiences year-over-year is a quantitative and continuous 
variable. 

- I will use RMSE as my metric of choice.

     - RMSE is sensitive to large outliers, which is desirable when predicting potentially negative economic shifts. I want to weight outliers heavily, as ignoring larger outliers in our predictive model could lead to lowballing the economic impact of power outages in specific circumstances.
    - Additionally, RMSE is expressed in the same units as the target variable, making it relatively easy to interpret.

These specifications frame our problem, and will inform our model's decisions.

### Available Data
As a result of predicting loss in GSP over an entire year, all of the data in our dataset will be available to us at the time of prediction except for data relating to the GSP of the state. This is due to the fact that in practice, our model will be used after a major power outage has already occurred in order to predict the potentially detrimental economic effects it will have.

___
## Baseline Model
### Defining Baseline Models for Comparison

To begin the predictive process, I will compare possible baseline approaches to predicting a state's change in GSP in a year they experienced a severe power outage.

__Single Value Prediction__
- As a reference point, I looked at the RMSE of a predictive model that predicts the mean of the `Change in GSP` column. Its RMSE was around 2.4%.

__Linear Regression__
- This model used the features `Duration`, `Customers Affected`, `NERC Region`, and `GSP Relative to USA` to create a Linear Regression model that predicted a state's `Change in GSP` the year that they have a major power outage. Its RMSE was around 2.3%.

__Random Forest Regression__
- This model performed Random Forest Regression using the `Duration`,  `Customers Affected`, `NERC Region`, and `GSP Relative to USA` columns of the DataFrames to predict the `Change in GSP` values. Its RMSE was around 1.9%.

### Analysis
Comparing our baseline models against each other, the __Random Forest Regressor__ was better at predicting the GSP change than the single-value estimator and the Linear Regression model. Let's discuss the approach I took when implementing the baseline model.

#### Features
- `Duration` and `Customers Affected`. Intuitively, these features encapsulate information related to the severity of the power outage. As such, it makes sense to include them in the implementation of our baseline models. They are both quantitative, and I did not alter them or engineer them in any way before feeding them to the model.
     - Alternatively, features such as `Demand Loss` could have been included to include the same information. This will not be overlooked, and may be a preferable alternative or addition to the model.
- `NERC Region`. The NERC Region represents the geographic area that the state is in. I one-hot encoded this variable, as it is a nominal variable- that is, it is categorical with no inherent ordering. 
     - This is another arbitrary choice of which feature is most accurate for capturing geographic data. `Climate Region` could also be considered.
- `GSP Relative to USA`. I included this feature to include auxiliary economic data in my model. As with `Duration` and `Customers Affected`, this feature is quantitative. However, I binned the values into 10 deciles to discourage the potential memorization of economic indicators in specific states.


#### Limitations

- Black box. The Random Forest Regressor is a __black box__. While we could observe the weights of coefficients in our Linear Regression model and determine the variables with the most significant impact (after normalizing), this is not the case with Random Forest Regressors. As such, it is more difficult to reason why it displays the behavior it does.
- Hyperparameters. Our model performed best on a set of hyperparameters that raises suspicions about its methodology. Its lowest RMSE of around 1.9% was achieved with unlimited tree depth and a minimum of just one leaf in each leaf node of each tree.
     - Possible Explanation: The model may contain a multitude of trees that are "memorizing" some characteristic of states for every year in the dataset. This could be mitigated by binning quantitative data, similar to what I did with `GSP Relative to USA`. However, this must be balanced to maintain sufficient granularity in our data. 
          - Note: It's also possible that our model is simply very good at predicting the percent change in `GSP` year over year with the given variables.
- The Data. The actual values we're trying to predict are difficult to interpret. While the minimum value of this Series (-9.1%), is quite low, the median and mean are greater than 0! This may seem like it negates the purpose of our analysis but it is merely a limitation. 
     - First, this data does __not__ include the context of the states that did __not__ experience a major power outage in a given year. It could be that while the states in our dataset saw their `GSP`s increase, they increased at a much lower rate than states that did not experience major power outages. 
     - Second, regardless of other states, seeing an increase in `GSP` year over year, regardless of major power outages, would be a largely positive result of our analysis. "Negative" results or a lack of strong predictive power can be as enlightening as positive results, offering evidence that states may possess resilience against the economic impacts of power outages.
 
### Plan for Refinement
- One-hot encoding and including features such as `Cause Category` may be helpful, as some types of power outages may cause more harm than others. For example, an intentional attack that affects thousands of people may cause far less economic downturn than a hurricane that affects thousands of people.
- Testing the inclusion of features such as `Demand Loss`, `Climate Category`, and `Urban Population` to ensure the robustness of our model and that no stones are left unturned.

___
## Final Model

### Philosophy
Although it was addressed in our discussion of the baseline model's performance, I chose to use __Random Forest Regression__ because its baseline model showed more promise than an approach using Linear Regression. I used a __GridSearch__ to determine the highest-performing parameters, and the ones that performed the best in the final model were a max depth of 20 for each tree in the random forest, a minimum of 1 sample per leaf (And thus 2 samples allowed per split of a Node), and 200 estimators (Or trees).

### Features
- `Customers Affected`. In my baseline model, the inclusion of `Customers Affected` and `Duration` was unnecessary. These features are collinear, and when using Random Forest Regression, may have diluted the importance of other features. This "dilution" effect can lead to less accurate predictions because the model spends part of its capacity on less informative aspects of the data. __In building my final model, I excluded `Duration` and retained `Customers Affected`. I also used a Standard Scaler for normalization, although this should not impact my model's decision-making.__
- `NERC Region`. The NERC Region represents the geographic area that the state is in. I one-hot encoded this variable, as it is a nominal variable- that is, it is categorical with no inherent ordering.
     - __In building my final model, I also tested the inclusion of features such as `Climate Region`, but they were not as effective.__
- `GSP Relative to USA`. As in the baseline model, I included `GSP Relative to USA` as a feature in my final model. __However, to maximize granularity and allow the model full access to the data in the column, I unbinned the data.__
- `Cause Category`. Including `Cause Category` proved beneficial to the model. As mentioned in the analysis of the previous section, some types of power outages may cause more harm than others. The example I provided compared the economic impacts of Hurricanes to those of intentional attacks, highlighting potential discrepancies. __I one-hot encoded this column, as it is qualitative and nominal.__

The adjustments I made to feature engineering and selection are bolded in the analysis above.

### Performance

Our final model's RMSE was around 1.2%, about 65% better than our baseline model and over twice as low as the single-value predictor. This is because of one of two reasons;

- Effectiveness
     - It is possible that our model is truly very effective at predicting the change in a state's GSP given the features provided in the dataset. This is more of a "Null Hypothesis" approach, and assumes that there are no confounding variables when analyzing model performance with RMSE.

- Granularity
     - The granularity of the data may be too fine. By allowing the regressor unbinned access to some of the quantitative columns, it may have memorized characteristics of states within years to make artificially accurate predictions on our dataset. This is difficult to balance, because lowering granularity also dilutes or eliminates important information about an event.
 
### Conclusion

In this section of the project, I developed and refined a predictive model to understand the economic impact of major power outages on states, as measured by the change in Gross State Product (GSP) per capita. And, through a rigorous process of feature engineering and hyperparameter tuning, I arrived at a final model that demonstrated a significant improvement over our baseline model.

The enhanced performance of our RandomForestRegressor model can, in part, be attributed to its ability to handle the granularity and complexity inherent in our dataset. By incorporating a wide variety of potentially impactful features, the model was able to capture nuanced relationships between power outages and economic outcomes. However, it is essential to consider that the model's success may also stem from its capacity to "memorize" certain state-specific characteristics within a given year, potentially leading to artificially high predictions. This highlights the challenge of distinguishing between genuine predictive insights and overfitting, especially without access to an even larger and more diverse dataset for validation.

This analysis reveals the importance of including data from states that have not experienced major power outages. This would offer a more comprehensive view of the economic impact of power outages by providing a contrast with states who did not experience such disruptions. For example, in our current dataset, the mean change in GSP was positive, suggesting resilience or even progress in the face of power outages. However, this observation is limited by the lack of comparative data from states without major power outages. It is plausible that the economic performance of these unaffected states could be markedly better, but this is outside the scope of this dataset.

In conclusion, while the final model represents a significant advancement over more simplistic approaches to predicting the economic impact of power outages, it also opens avenues for further investigation. To truly grasp the full economic consequences of major power outages, future research should incorporate a more extensive dataset that includes a wider population of states, both affected and unaffected by power outages. This exploration is essential and could make headway in proactively handling failures in infrastructure.

___
## Fairness Analysis
### Philosophy

As our model focuses on predicting economic outcomes, it is fitting to determine its performance on different economic groups.
For example, there might be inherent economic resilience or vulnerability affecting the model's predictions, and a fairness analysis could reveal if the model underestimates or overestimates the impact of power outages on weaker vs. stronger economies.

For this fairness analysis, I will compare the model's RMSE on states in the lower quartile of `GSP Relative to USA` with states in the upper quartile of `GSP Relative to USA`.

### Permutation Test

__Null Hypothesis:__
- The model is fair between states in the lower quartile of `GSP Relative to USA` and states in the upper quartile of `GSP Relative to USA`- the observed difference in RMSE is due to random chance.

__Alternative Hypothesis:__
- The model is not fair between states in the lower quartile of `GSP Relative to USA` and states in the upper quartile of `GSP Relative to USA`- the observed difference in RMSE is not due to random chance.

We will conduct this test using the test statistic `np.abs(rmse_lower - rmse_upper)`, or the absolute difference between the RMSEs of states in the lower quartile of `GSP Relative to USA` and states in the upper quartile of `GSP Relative to USA`. We will use a significance level of 0.01, so that we only reject the Null Hypothesis with compelling evidence.

__Simulated__<br>
Below is a histogram representing the results of 1000 simulated F-Statistics under the Null Hypothesis
<iframe src="assets/fairness_distribution.html" width="800" height="600" frameborder="0"></iframe>

__Conclusion__<br>
With this p-value, we fail to reject the null hypothesis. We conclude that it is highly likely that our model is fair between states in the lower quartile of `GSP Relative to USA` and states in the upper quartile of `GSP Relative to USA`.

___
# Thank You!
I hope you enjoyed my analysis!
