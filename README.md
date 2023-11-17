# My Project Title

by Tom Hocquet and Julia Ma

---

## Introduction

Electricity serves as a basis for satisfying fundamental human needs, such as food production, clean water, sanitation, education services, health care, and social services. When power outages occur, these fundamental human needs become at risk, which is something we want to minimize. In this project, we explored a data set that reports "major outages witnessed by different states in the continental U.S. during January 2000–July 2016." [link](https://www.sciencedirect.com/science/article/pii/S2352340918307182#bib6).

The data features 1534 rows and 55 columns. That means that their were 1,534 power outages in the continental U.S. in between the dates of January 2000 and July 2016. A lot of rows have some missing values depending what caused the power outages (for example the column 'HURRICANE.NAMES' is mostly filled with np.nan as only 72 of the power outages were caused by hurricanes which is about ≈4.6% of the data). 

Some of the outages were missing the time in which they started and ended. Due to our central question being around the outage duration and their only being 9 rows with missing dates, we decided to drop those rows.

---

## Cleaning and EDA


---

### Exploratory Data Analysis

### Univariate Analysis

For the univariate analysis, we decided to look at the columns that were relevent to you question. In this graph, we see the total number of outages based on their duration. 
Note: each grouping of the histogram represents a 12 hour period.

<iframe src="assets/outage-duration-hist4.png" width=800 height=600 frameBorder=0></iframe>

We also looked at whether in different months we see outage of different durations. Here we graphed the mean duration of the outages based on the month they occured in. We can see that outages in the summer last longer on average than the spring and the fall.

<iframe src="assets/outage_duration_month.html" width=800 height=600 frameBorder=0></iframe>


## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(pd.DataFrame(df.groupby('NERC.REGION')['YEAR'].count()).to_markdown(index=False))
```


