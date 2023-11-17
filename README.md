# My Project Title

by Tom Hocquet and Julia Ma

---

## Introduction

Electricity serves as a basis for satisfying fundamental human needs, such as food production, clean water, sanitation, education services, health care, and social services. When power outages occur, these fundamental human needs become at risk, which is something we want to minimize. In this project, we explored a data set that reports "major outages witnessed by different states in the continental U.S. during January 2000–July 2016." [link](https://www.sciencedirect.com/science/article/pii/S2352340918307182#bib6).

The data features 1534 rows and 55 columns. That means that their were 1,534 power outages in the continental U.S. in between the dates of January 2000 and July 2016. A lot of rows have some missing values depending what caused the power outages (for example the column 'HURRICANE.NAMES' is mostly filled with np.nan as only 72 of the power outages were caused by hurricanes (≈4.6% of the data)

---

## Cleaning and EDA

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(pd.DataFrame(df.groupby('NERC.REGION')['YEAR'].count()).to_markdown(index=False))
```


