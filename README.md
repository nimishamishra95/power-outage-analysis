# Power Outage Analysis

By Nimisha Mishra

If you are viewing this repository, access the report **[here](https://nimishamishra95.github.io/power-outage-analysis/).**

# Introduction

### Context & Guiding Question

Major power outages are disruptive events that affect daily life, compromise public safety, and create significant economic burdens across the United States. Understanding not just when and where outages occur, but also why they last as long as they do, is essential for improving grid resilience and preparing infrastructure for a changing climate. This project uses a dataset from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure (LASCI), which tracks major power outages across the continental U.S. The dataset includes 1534 rows detailing the geographical, climatic, and operational characteristics of each event, offering a view of outage patterns, impacts, and regional factors.

The question I chose to guide my analysis is:
**How do climate region and the categorical cause of an outage influence the duration of major power outages?**

This question matters because climate variability is playing an increasingly important role in electrical grid stability. As extreme temperatures, seasonal anomalies, and shifting climate patterns become more frequent, understanding how they relate to outage duration can help policymakers, energy providers, and communities better anticipate vulnerabilities and allocate resources effectively. By analyzing these data, we can see whether certain climatic conditions contribute to longer outages, and if so, in what ways.

### Relevant Columns

To answer this question, I have narrowed down the scope of the dataset (56 variables) to the following 12 key variables from the dataset:

- `YEAR`, `MONTH` – When each outage occurred, giving seasonal and historical context.

- `U.S.\_STATE` – The geographical location of each outage.

- `NERC.REGION` – The North American Electric Reliability Corporation region responsible for grid oversight in the affected area.

- `CLIMATE.REGION` – Groups states into broad climate regions (Southeast, Northwest, etc.), enabling comparisons across zones.

- `CLIMATE.CATEGORY` – Classifies the general climate conditions during the outage (dry, wet, warm, etc.), supporting analysis of climate impacts on outage duration.

- `OUTAGE.START.DATE` / `OUTAGE.START.TIME` – When the outage began.

- `OUTAGE.RESTORATION.DATE` / `OUTAGE.RESTORATION.TIME` – When power was restored.

- `CAUSE.CATEGORY` – Information on what triggered the outage, such as weather events, equipment failure, or other factors.

- `OUTAGE.DURATION` – The primary outcome variable, measuring how long the outage lasted in minutes.

Together, these columns provide a detailed view of how climatic factors, may influence the duration of major power outages across the United States.

Note: To download the complete dataset from Purdue, please use this **[link](https://engineering.purdue.edu/LASCI/research-data/outages)**.

# Data Cleaning and Exploratory Data Analysis

### Data Cleaning

I began the data cleaning process by standardizing all column names, as the original dataset used uppercase letters and mixed punctuation such as periods and underscores, which made the variables difficult to read and work with. I reformatted each name to be lowercase and separated by spaces (for example, converting `U.S._STATE` to `us state`). Although this is a simple adjustment, it significantly improved the readability and usability of the dataset. After this, I reduced the dataset from the original 56 variables to the 13 variables relevant to my analysis that I mentioned above, allowing me to focus on the factors most tied to outage duration rather than carrying unnecessary fields through each step of my workflow.

Next, I cleaned the time and date related fields. Outage start information and restoration information were each split into two separate object columns (one for the date and one for the time). To fix this, I merged each pair into a single timestamp column and converted them into proper datetime objects. Although outage duration was already provided, creating these unified timestamps was important because it allowed me to use the timing information in more flexible ways such as aggregating outages by hour, examining patterns across days or months, and generating more accurate time-based visualizations. After merging, I removed the original date and time columns to avoid redundancy across the dataset.

I also addressed placeholder values used throughout the dataset. Several numeric columns, including month, year, and outage duration, incorrectly used 0 to represent missing data, and some categorical fields used empty strings. I replaced all such values with `np.nan` to explicitly indicate missingness and prevent inaccurate summaries, misclassifications, or distorted model outputs.

Preview of CLEANED DATASET:

| year | month | us state  | nerc region | climate region     | climate category | cause category     | outage duration | outage start datetime | outage restoration datetime |
| ---: | ----: | :-------- | :---------- | :----------------- | :--------------- | :----------------- | --------------: | --------------------: | --------------------------: |
| 2011 |   7.0 | Minnesota | MRO         | East North Central | normal           | severe weather     |          3060.0 |   2011-07-01 17:00:00 |         2011-07-03 20:00:00 |
| 2014 |   5.0 | Minnesota | MRO         | East North Central | normal           | intentional attack |             1.0 |   2014-05-11 18:38:00 |         2014-05-11 18:39:00 |
| 2010 |  10.0 | Minnesota | MRO         | East North Central | cold             | severe weather     |          3000.0 |   2010-10-26 20:00:00 |         2010-10-28 22:00:00 |
| 2012 |   6.0 | Minnesota | MRO         | East North Central | normal           | severe weather     |          2550.0 |   2012-06-19 04:30:00 |         2012-06-20 23:00:00 |
| 2015 |   7.0 | Minnesota | MRO         | East North Central | warm             | severe weather     |          1740.0 |   2015-07-18 02:00:00 |         2015-07-19 07:00:00 |

Note: This data will likely be manipulated further when I use it for hypothesis testing and modeling.

### Univariate Analysis

First, I graphed the frequency of outages by their cause category. As shown below, severe weathers seems to cause the most outages out of all cause categories.

<iframe
  src="assets/outages-by-cause-category.html"
  width="830"
  height="550"
  frameborder="0"
></iframe>

This is a pie chart of the distribution of power outages by climate region. It seems that the Northeast tends to have the most power outages, while the West North Central has the least.

<iframe
  src="assets/outages-by-climate-region.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, I plotted the the number of power outages by hour. It seems most power powerages occur from 12 PM to 5 PM.

<iframe
  src="assets/outages-by-hour.html"
  width="830"
  height="500"
  frameborder="0"
></iframe>

Lastly, I plotted the distribution of power outages by year. As shown, 2011 had the most power outages.

<iframe
  src="assets/outages-by-year.html"
  width="830"
  height="500"
  frameborder="0"
></iframe>

### Bivariate Analysis

<iframe
  src="assets/outage-duration-by-month.html"
  width="830"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/outage-duration-by-cause-category.html"
  width="830"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/outage-cause-by-nerc-region.html"
  width="830"
  height="500"
  frameborder="0"
></iframe>

### Aggregrate Analysis

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis
