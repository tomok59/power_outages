# My Project Title

by Tom Hocquet and Julia Ma

## Introduction (Report on the Website)

--

The topic of electricity is important to understand, as it is something that is now essential to daily life. Electricity is used to maintain machinery, electronics, public transportation systems, etc. Electricity also serves as a basis for satisfying fundamental human needs, such as food production, clean water, sanitation, education services, health care, and social services. When power outages occur, these fundamental human needs become at risk. We explored a data set that reports "major outages witnessed by different states in the continental U.S. during January 2000–July 2016." [link](https://www.sciencedirect.com/science/article/pii/S2352340918307182#bib6).

In this project, we want to specifically examine **what attributes are correlated with longer duration of major power outages in the U.S.?** Understanding this question will be crucial in finding out how to minimize the number of future power outages. 

### Dataset Introduction

The dataset we will be using provides information on the major power outages that occured in the U.S. from January 2000 to July 2016. Stats-wise, our raw dataset contains 1535 rows, which represent the nunber of major power outage reports, and 56 columns, which represent the number of power outage properties that were recorded. 

The following columns will be revelant to our dataset (descriptions were taken from [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S2352340918307182#t0005)): 

| Column Names                       | Description                                                        |
| ---------------------------------- | ------------------------------------------------------------------ |
| `YEAR`                             | Indicates the year when the outage event occurred                  |
| `MONTH`                            | Indicates the month when the outage event occurred                 |
| `U.S._STATE`                       | Represents all the states in the continental U.S.                  |
| `POSTAL.CODE`                      | Represents the postal code of the U.S. states                      | 
| `NERC.REGION`                      | The North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| (maybe use) `CLIMATE.REGION`       | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)    |
| `ANOMALY.LEVEL	`                | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W)                      |
| (maybe use) `CLIMATE.CATEGORY	`    | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)                      |
| `OUTAGE.START`                     | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region) (in Timedelta) |
| `OUTAGE.RESTORATION.DATE`          | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)                  |
| (maybe use) `OUTAGE.RESTORATION`   | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region) (in TimeDelta)                   |
| `CAUSE.CATEGORY`                   | Categories of all the events causing the major power outages                      | 
| `CAUSE.CATEGORY.DETAIL`            | Detailed description of the event categories causing the major power outages                     |
| `OUTAGE.DURATION`                  | Duration of outage events (in minutes)                    |
| `CUSTOMERS.AFFECTED`               | Number of customers affected by the power outage event                      |
| `TOTAL.PRICE`                      | Average monthly electricity price in the U.S. state (cents/kilowatt-hour)                     |

### Project Overview 
1. we will first pre-process the dataset by removing any unneccessary columns & rows and observe the data contained in each column  
2. we will then use exploratory data analysis to explore the relationship between the columns with each other. this includes creating univariate graphs, bivariate graphs, and creating interesting aggregate tables to evaluate the relationships between the major power outage properties. 
3. (provide more detail) we will then assess the missingness of the data in the dataset, which includes categorizing the data into MAR, MCAR, or NMAR
4. (ask tom to fill in) Finally, in our hypothesis test, we will mainly analyze the relationship betwen `OUTAGE.DURATION` and the other property columns (put specific columns)

## Cleaning and EDA

--

The data features 1534 rows and 55 columns. That means that their were 1,534 power outages in the continental U.S. in between the dates of January 2000 and July 2016. A lot of rows have some missing values depending what caused the power outages (for example the column `HURRICANE.NAMES` is mostly filled with np.nan as only 72 of the power outages were caused by hurricanes which is about ≈4.6% of the data).

> Dropping unneccessary columns

Some columns, such as the `OBS` and `variables` column, did not contain any information on the actual properties of major power outages, so they were dropped as a result. Some of the outages were also missing the time in which they started and ended. Due to our central question being around the outage duration and their only being 9 rows with missing dates, we decided to drop those rows. 

> Converting datatypes 

Cleaning also included making sure each of the columns were the right type, and if they were not, updating them to their intended types. For instance, the `YEAR` and `MONTH` columns were originally floats, but through cleaning they were converted to ints. 

> Converting to correct datetimes

Certain columns such as the `OUTAGE.START.TIME` and `OUTAGE.RESTORATION.TIME` needed to be converted to the right datetimes using `pd.to_timedelta`  

Below shows the data types of the newly cleaned dataframe.

|                         | 0       |
|:------------------------|:--------|
| YEAR                    | float64 |
| MONTH                   | float64 |
| U.S._STATE              | object  |
| POSTAL.CODE             | object  |
| NERC.REGION             | object  |
| CLIMATE.REGION          | object  |
| ANOMALY.LEVEL           | float64 |
| CLIMATE.CATEGORY        | object  |
| OUTAGE.START.DATE       | int64   |
| OUTAGE.START.TIME       | object  |
| OUTAGE.RESTORATION.DATE | int64   |
| OUTAGE.RESTORATION.TIME | object  |
| CAUSE.CATEGORY          | object  |
| CAUSE.CATEGORY.DETAIL   | object  |
| HURRICANE.NAMES         | object  |
| OUTAGE.DURATION         | float64 |
| DEMAND.LOSS.MW          | float64 |
| CUSTOMERS.AFFECTED      | float64 |
| RES.PRICE               | float64 |
| COM.PRICE               | float64 |
| IND.PRICE               | float64 |
| TOTAL.PRICE             | float64 |
| RES.SALES               | float64 |
| ...                     | ...     |
| PCT_LAND                | float64 |
| PCT_WATER_TOT           | float64 |
| PCT_WATER_INLAND        | float64 |
| SQUARE.MILES.AFFECTED   | float64 |


Below is the head of the DataFrame

|    |   OBS |   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | OUTAGE.START.DATE   | OUTAGE.START.TIME   | OUTAGE.RESTORATION.DATE   | OUTAGE.RESTORATION.TIME   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |
|---:|------:|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:--------------------|:--------------------|:--------------------------|:--------------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|
|  0 |     1 |   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | 2011-07-01 00:00:00 | 17:00:00            | 2011-07-03 00:00:00       | 20:00:00                  | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |     2.30874e+06 |          276286 |           10673 |       2.5957e+06  |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |  5.34812e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|  1 |     2 |   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | 2014-05-11 00:00:00 | 18:38:00            | 2014-05-11 00:00:00       | 18:39:00                  | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |     2.34586e+06 |          284978 |            9898 |       2.64074e+06 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |  5.45712e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|  2 |     3 |   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | 2010-10-26 00:00:00 | 20:00:00            | 2010-10-28 00:00:00       | 22:00:00                  | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |     2.30029e+06 |          276463 |           10150 |       2.58690e+06 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |  5.3109e+06  |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|  3 |     4 |   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | 2012-06-19 00:00:00 | 04:30:00            | 2012-06-20 00:00:00       | 23:00:00                  | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |     2.31734e+06 |          278466 |           11010 |       2.60681e+06 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |  5.38044e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|  4 |     5 |   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | 2015-07-18 00:00:00 | 02:00:00            | 2015-07-19 00:00:00       | 07:00:00                  | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |     2.37467e+06 |          289044 |            9812 |       2.67353e+06 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |  5.48959e+06 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |

## Exploratory Data Analysis

--

### Univariate Analysis

---

For the univariate analysis, we decided to look at the columns that were relevent to you question. In this graph, we see the total number of outages based on their duration. 
Note: each grouping of the histogram represents a 12 hour period.

<iframe src="assets/outage-duration-hist4.png" width=800 height=600 frameBorder=0></iframe>

We also looked at whether in different months we see outage of different durations. Here we graphed the mean duration of the outages based on the month they occured in. We can see that outages in the summer last longer on average than the spring and the fall.

<iframe src="assets/outage_duration_month.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

---

For the bivariate Analysis, we looked once more at the outage duration but in relation to their location in the U.S. Below is a map that shows each of the states color coded depending on the average duration. Note that the bins are not even to better represent the data.

<iframe src="assets/map.html" width=800 height=600 frameBorder=0></iframe>

### AGG STUFF

---

| CAUSE.CATEGORY                |   DETAIL_MISSING = False |   DETAIL_MISSING = True |
|:------------------------------|-------------------------:|------------------------:|
| equipment failure             |                0.0418288 |               0.0267857 |
| fuel supply emergency         |                0.0243191 |               0.0290179 |
| intentional attack            |                0.347276  |               0.102679  |
| islanding                     |              nan         |               0.0982143 |
| public appeal                 |              nan         |               0.154018  |
| severe weather                |                0.551556  |               0.395089  |
| system operability disruption |                0.0350195 |               0.194196  |

## Assessment of Missingness

--

### NMAR Analysis

---

One column that could have NMAR data is the "CUSTOMERS.AFFECTED" column. This column measures the number of customers affected by the power outage event and contains 420 NaNs. This missingness could be due to customers not reporting their affectedness by the power outage event. If the severity of power outage cause was small, then there would be less damage done and thus customers would feel less affected and less inclined to report on their affectedness, explaining the NaNs.

If we wanted to explain the missingness, information on how the customers were affected (i.e. through a survey or head count of the area) would be needed. Additional information on the type and severity of the power outage cause could be correlated with how the customers affected was reported, thus making the missingness MAR. 

### Missingness Dependency

---

> Picking a column with non-trivial missingness 

To find a column with non-trivial missingness, we must first define what non-trivial missingness is. In this project, we defined a column to have non-trivial missingness as having 20% or more missing data values in that respective column. The following dataframe shows the top 5 columns that have a the greatest proportion of missing values. 

| column name             |   missing data amount |
|:------------------------|----------------------:|
| HURRICANE.NAMES         |                  0.99 |
| DEMAND.LOSS.MW          |                  0.48 |
| CAUSE.CATEGORY.DETAIL   |                  0.32 |
| CUSTOMERS.AFFECTED      |                  0.3  |
| OUTAGE.RESTORATION.DATE |                  0.04 |

After analyzing the table, the column we decided to base our missingness analysis on was the `CAUSE.CATEGORY.DETAIL` column, which has non-trivial missingness of approximately 32%.

Next, we will use permutation tests to analyze the dependency of the missingness in the `CAUSE.CATEGORY.DETAIL` column against the following columns: `CAUSE.CATEGORY` and `POPULATION` 


#### `CAUSE.CATEGORY.DETAIL` and `CAUSE.CATEGORY` (MAR)

Null hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `CAUSE.CATEGORY` 

Alternative hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does depend on `CAUSE.CATEGORY` 

Observed test-statistic: Total Variation Distance (TVD)

To recall, `CAUSE.CATEGORY` contains categories of all the events causing the major power outages, and `CAUSE.CATEGORY.DETAIL` contains detailed description of the event categories causing the major power outages. 

The vertical barplot below displays the distributions of `CAUSE.CATEGORY` with and without the missingness in `CAUSE.CATEGORY.DETAIL`. Analyzing the barplot, we notice the distributions are very different. 

<iframe src="assets/missigness1.html" width=800 height=600 frameBorder=0></iframe>

Next, we continue to conduct a permutation test using the Total Variation Distance (TVD) as our observed test statistic.

<iframe src="assets/dist-tvd.html" width=800 height=600 frameBorder=0></iframe>

After 500 permutations of shuffling the `CAUSE.CATEGORY` column and simulating the TVD results, the p-value comes to be 0.0. Our p-value of 0.0 is less than our significance level of 0.05, therefore **we reject the null hypthesis stating that the missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `CAUSE.CATEGORY`**, thus making it **MAR**. 

#### `CAUSE.CATEGORY.DETAIL` and `IND.PRICE` (MCAR)

Null hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `IND.PRICE` 

Alternative hypothesis: The missingness in `CAUSE.CATEGORY.DETAIL` does depend on `IND.PRICE` 

Observed test-statistic: Total Variation Distance (TVD)

To recall, `IND.PRICE` contains the monthly electricity price in the industrial sector (cents/kilowatt-hour), and `CAUSE.CATEGORY.DETAIL` contains detailed description of the event categories causing the major power outages. 

**Note*: Since `IND.PRICE` contains numerical data, we decided to bin the prices into 5 categories to make it categorical, and thus be able to use the Total Variation Distance to run our permutation tests. 

| QcutBin                    |   DETAIL_MISSING = False |   DETAIL_MISSING = True |
|:---------------------------|-------------------------:|------------------------:|
| (3.1990000000000003, 5.48] |               0.224708   |               0.162946  |
| (5.48, 6.3]                |               0.197471   |               0.1875    |
| (6.3, 7.32]                |               0.190661   |               0.212054  |
| (7.32, 9.114]              |               0.189689   |               0.209821  |
| (9.114, 27.85]             |               0.191634   |               0.214286  |
| nan                        |               0.00583658 |               0.0133929 |

The vertical barplot below displays the distributions of `IND.PRICE` with and without the missingness in `CAUSE.CATEGORY.DETAIL`. Analyzing the barplot, we notice the distributions are more similar. 

<iframe src="assets/customers-missingness.html" width=800 height=600 frameBorder=0></iframe>

Next, we continue to conduct a permutation test using the Total Variation Distance (TVD) as our observed test statistic.

<iframe src="assets/dist-tvd2.html" width=800 height=600 frameBorder=0></iframe>

After 500 permutations of shuffling the `IND.PRICE` column and simulating the TVD results, the p-value comes to be 1.0. Our p-value of 1.0 is greater than our significance level of 0.05, therefore **we fail to reject the null hypthesis stating that the missingness in `CAUSE.CATEGORY.DETAIL` does not depend on `IND.PRICE`**, thus making it **MCAR**. 

## Hypothesis Testing

--

For our hypothesis testing we have chosen two variables that appear to have a correlation with out question to test whether that is true or not. We looked to see if any of our columns had a graphical correlation with `OUTAGE.DURATION`. After many, many graphes, which all seemed to have very little correlation, started to make different statistics from the column we had. One such statistic we made is `SQUARE.MILES.AFFECTED`, made from dividing `CUSTOMERS.AFFECTED` by `POPDEN_UC`

Here is a graph of the two variables, with the line of best fit graphed on it as well. 

<iframe src="assets/hypo1.png" width=800 height=600 frameBorder=0></iframe>

From this we construced the following:

Null Hypothesis ($H_0$): There is no correlation between the two variables.

Alternative Hypothesis ($H_A$): There is a correlation between the two variables.

To test for this, we used the Pearson correlation coefficient to measure our hypothesis. This test the linear relationsip between two numerical variables. We set the alpha to equal 0.01

```py
observed_correlation, _ = scipy.stats.pearsonr(df['OUTAGE.DURATION'], df['SQUARE.MILES.AFFECTED'])
```
This returned a p-value of 0.2419610934765804

### Permutation Test

---

<iframe src="assets/hypo2.html" width=800 height=600 frameBorder=0></iframe>

We ran the permutation test 100,000 times. The blue stack is our distribution and the red line is the observed value.

### Hypothesis Testing Conclusion

---

The P-Value for this hypothesis test is of 0.000, which is less than our alpha of 0.01. This means that we can reject the null hypothesis.

This could be explained by a mutlitude of factors and unknown variables, so we cannot draw any conclusion other that the variables are correlated. 