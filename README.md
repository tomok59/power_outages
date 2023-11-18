# My Project Title

by Tom Hocquet and Julia Ma

---

## Introduction (Report on the Website)
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

The data features 1534 rows and 55 columns. That means that their were 1,534 power outages in the continental U.S. in between the dates of January 2000 and July 2016. A lot of rows have some missing values depending what caused the power outages (for example the column 'HURRICANE.NAMES' is mostly filled with np.nan as only 72 of the power outages were caused by hurricanes which is about ≈4.6% of the data). 

Some of the outages were missing the time in which they started and ended. Due to our central question being around the outage duration and their only being 9 rows with missing dates, we decided to drop those rows.

---

### Exploratory Data Analysis

### Univariate Analysis

For the univariate analysis, we decided to look at the columns that were relevent to you question. In this graph, we see the total number of outages based on their duration. 
Note: each grouping of the histogram represents a 12 hour period.

<iframe src="assets/outage-duration-hist4.png" width=800 height=600 frameBorder=0></iframe>

We also looked at whether in different months we see outage of different durations. Here we graphed the mean duration of the outages based on the month they occured in. We can see that outages in the summer last longer on average than the spring and the fall.

<iframe src="assets/outage_duration_month.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

For the bivariate Analysis, we looked once more at the outage duration but in relation to their location in the U.S. Below is a map that shows each of the states color coded depending on the average duration. Note that the bins are not even to better represent the data.

<iframe src="assets/map.html" width=800 height=600 frameBorder=0></iframe>

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(pd.DataFrame(df.groupby('NERC.REGION')['YEAR'].count()).to_markdown(index=False))
```


