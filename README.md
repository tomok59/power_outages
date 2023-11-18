# My Project Title

by Tom Hocquet and Julia Ma

---

## Introduction (Report on the Website)

---

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

---

## Cleaning and EDA

---

The data features 1534 rows and 55 columns. That means that their were 1,534 power outages in the continental U.S. in between the dates of January 2000 and July 2016. A lot of rows have some missing values depending what caused the power outages (for example the column 'HURRICANE.NAMES' is mostly filled with np.nan as only 72 of the power outages were caused by hurricanes which is about ≈4.6% of the data). 

Some of the outages were missing the time in which they started and ended. Due to our central question being around the outage duration and their only being 9 rows with missing dates, we decided to drop those rows. Cleaning also included making sure each of the columns were the right type, and if they were not, updating them to their intended types. Below is shown the types of variables after the cleaning.

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
...
| PCT_LAND                | float64 |
| PCT_WATER_TOT           | float64 |
| PCT_WATER_INLAND        | float64 |
| SQUARE.MILES.AFFECTED   | float64 |

---

## Exploratory Data Analysis

---

### Univariate Analysis

For the univariate analysis, we decided to look at the columns that were relevent to you question. In this graph, we see the total number of outages based on their duration. 
Note: each grouping of the histogram represents a 12 hour period.

<iframe src="assets/outage-duration-hist4.png" width=800 height=600 frameBorder=0></iframe>

We also looked at whether in different months we see outage of different durations. Here we graphed the mean duration of the outages based on the month they occured in. We can see that outages in the summer last longer on average than the spring and the fall.

<iframe src="assets/outage_duration_month.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

For the bivariate Analysis, we looked once more at the outage duration but in relation to their location in the U.S. Below is a map that shows each of the states color coded depending on the average duration. Note that the bins are not even to better represent the data.

<iframe src="assets/map.html" width=800 height=600 frameBorder=0></iframe>

### AGG STUFF

| CAUSE.CATEGORY                |   DETAIL_MISSING = False |   DETAIL_MISSING = True |
|:------------------------------|-------------------------:|------------------------:|
| equipment failure             |                0.0418288 |               0.0267857 |
| fuel supply emergency         |                0.0243191 |               0.0290179 |
| intentional attack            |                0.347276  |               0.102679  |
| islanding                     |              nan         |               0.0982143 |
| public appeal                 |              nan         |               0.154018  |
| severe weather                |                0.551556  |               0.395089  |
| system operability disruption |                0.0350195 |               0.194196  |

---

## Assessment of Missingness

---

### NMAR Analysis

One column that could have NMAR data is the "CUSTOMERS.AFFECTED" column. This column measures the number of customers affected by the power outage event and contains 420 NaNs. This missingness could be due to customers not reporting their affectedness by the power outage event. If the severity of power outage cause was small, then there would be less damage done and thus customers would feel less affected and less inclined to report on their affectedness, explaining the NaNs.

If we wanted to explain the missingness, information on how the customers were affected (i.e. through a survey or head count of the area) would be needed. Additional information on the type and severity of the power outage cause could be correlated with how the customers affected was reported, thus making the missingness MAR. 

### Missingness Dependency

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