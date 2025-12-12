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

Lastly, I plotted the distribution of power outages by year. As shown, there seems to be a significant spike in power outages from 2010 to 2011 (the year with the most power outages).

<iframe
  src="assets/outages-by-year.html"
  width="830"
  height="500"
  frameborder="0"
></iframe>

### Bivariate Analysis

This scatterplot of outage duration in minutes by month can help identify which months tend to have longer outage durations. However, after inspection, there doesn't seem to be any apparent pattern.

<iframe
  src="assets/outage-duration-by-month.html"
  width="830"
  height="550"
  frameborder="0"
></iframe>

In this scatterplot, I wanted to examine the relationship between cause category and outage duration. It seems that outages due to severe weather and fuel supply tend to last longer than outages caused by other reasons.

<iframe
  src="assets/outage-duration-by-cause-category.html"
  width="830"
  height="600"
  frameborder="0"
></iframe>

This stacked bar chart displays the distribution of outage causes by NERC region. Upon examination, it looks like severe weather is the dominant cause for power outages in most regions, whereas, causes like public appeal and fuel supply emergency seem to be the least dominant.

<iframe
  src="assets/outage-cause-by-nerc-region.html"
  width="950"
  height="600"
  frameborder="0"
></iframe>

### Aggregrate Analysis

This pivot table allows us to understand how power outage durations vary across both climate regions and cause categories by taking the median power outage duration for each combination of the two variables, which also ultimately ties back to my research question above. Here are a couple of trends I picked up from the pivot table below:

- fuel supply emergencies seem to lead to the longest outages overall
- equipment failure seems to cause shorter outages
- high amount of variability across both regions and causes
- NaN values suggest gaps in reporting

| cause category (hor) / climate region (vert) | equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption |
| :------------------------------------------- | ----------------- | --------------------- | ------------------ | --------- | ------------- | -------------- | ----------------------------- |
| Central                                      | 149.0             | 7500.5                | 198.0              | 96.0      | 1410.0        | 1695.0         | 65.0                          |
| East North Central                           | 761.0             | 13564.0               | 1046.0             | 1.0       | 733.0         | 4005.0         | 2694.0                        |
| Northeast                                    | 267.5             | 12240.0               | 30.0               | 881.0     | 2760.0        | 3189.0         | 234.5                         |
| Northwest                                    | 702.0             | 1.0                   | 292.0              | 21.0      | 898.0         | 3507.0         | 157.5                         |
| South                                        | 227.0             | 20160.0               | 110.0              | 493.5     | 430.0         | 2027.5         | 396.5                         |
| Southeast                                    | 308.5             | NaN                   | 108.0              | NaN       | 4320.0        | 1355.0         | 129.0                         |
| Southwest                                    | 35.0              | 76.0                  | 57.0               | 2.0       | 2275.0        | 2425.0         | 337.5                         |
| West                                         | 269.0             | 882.5                 | 118.0              | 128.5     | 420.0         | 962.0          | 199.0                         |
| West North Central                           | 61.0              | NaN                   | 47.0               | 56.0      | 439.5         | 83.0           | NaN                           |

# Assessment of Missingness

### NMAR Analysis

The column `OUTAGE.RESTORATION.DATE`, which contains 58 missing values, can be classified as Not Missing At Random (NMAR) because the missingness could be inherently linked to the nature of the outage event itself. For example, if the power was not restored at the time the data was reported, there would be no date or time to record, which directly ties the missing value to the ongoing status of the event. Similarly, for outages that were minor or very short, restoration details could have been deemed unnecessary to log, which means that the absence of this data is again related to the characteristics of the outage rather than occurring randomly. In both cases, the probability of a missing value is dependent on the unobserved value or the specific circumstances of the event rather than other observed data, making this column likely NMAR.

### Missing Dependency

In this section, I investigated whether the missingness of the outage duration column depends on other attributes in the dataset. I chose cause category as my dependent column (missingness depends on this column) and NERC region (missingness does NOT depend on this column) as my independent column. For both permutation tests, we use a significance level of α = 0.01, and we use Total Variance Distance (TVD) as the test statistic to quantify differences between distributions.

**Testing Dependency Between Missing Outage Duration and Cause Category**

Null Hypothesis (H₀): The distribution of cause category when outage duration is missing is the same as when outage duration is not missing.

Alternative Hypothesis (H₁): The distributions are not the same.

<iframe
  src="assets/missingness-cause-category.html"
  width="830"
  height="650"
  frameborder="0"
></iframe>

After running the permutation test, I obtained the following statistics:

- Observed TVD: 0.469
- p-value: 0.000

Because the p-value is less than α = 0.01, I reject the null hypothesis. The missingness of outage duration depends on the cause category column, which means that outages of certain causes are more or less likely to have missing duration values.

<iframe
  src="assets/tvd-missingness-cause-category.html"
  width="830"
  height="550"
  frameborder="0"
></iframe>

**Testing Dependency Between Missing Outage Duration and NERC Region**

Null Hypothesis (H₀): The distribution of NERC region when outage duration is missing is the same as when outage duration is not missing.

Alternative Hypothesis (H₁): The distributions are not the same.

<iframe
  src="assets/missingness-nerc-region.html"
  width="830"
  height="650"
  frameborder="0"
></iframe>

After running the permutation test, I obtained the following statistics:

- Observed TVD: 0.143
- p-value: 0.045

Because the p-value is greater than α = 0.01, I fail to reject the null hypothesis. The missingness of outage duration does not depend on NERC region, which means missing duration values seem to be unrelated to the NERC region responsible for grid oversight in the area of the power outage.

<iframe
  src="assets/tvd-missingness-nerc-region.html"
  width="830"
  height="550"
  frameborder="0"
></iframe>

# Hypothesis Testing

In the following hypothesis, I am testing whether the average duration of power outages in the South climate region is greater than the average duration of outages in the Northeast climate region. This investigation is important for understanding whether certain climate regions experience longer outages on average, which can have implications for resource allocation, infrastructure planning, and reliability measures in the power grid.

**Null Hypothesis (H₀)**: The average outage duration in the South climate region is the same as the average outage duration in the Northeast climate region.

**Alternative Hypothesis (H₁)**: The average outage duration in the South climate region is greater than the average outage duration in the Northeast climate region.

**Test Statistic**: Difference in mean outage duration between South and Northeast (Mean South − Mean Northeast)

**Observed Mean Difference**: -458.06

**Significance Level**: 5%

**P-value**: 0.8331

Here is a histogram containing the distribution of the test statistics generated from the permutation test, with the observed mean difference indicated as a red vertical line:

<iframe
  src="assets/hyp-test-fig.html"
  width="900"
  height="650"
  frameborder="0"
></iframe>

Based on the hypothesis test I performed, the p-value is 0.8331. Since the p-value is much greater than the significance level of 0.05, I fail to reject the null hypothesis. This suggests that there is no significant evidence to conclude that the average outage duration in the South is greater than in the Northeast. And actually, the negative observed mean difference indicates that, on average, outages in the South were slightly shorter than in the Northeast in the dataset, although this difference is not statistically significant.

# Framing a Prediction Problem

For my prediction component, I will build a model that predicts the cause category of a major power outage at the moment the outage is reported. This keeps the overall project cohesive as everything I’ve done so far has revolved around understanding why outages happen and how different factors influence them. Since `cause category` has multiple possible outcomes, this is a multiclass classification problem. I’m using only information that would be available at the time of prediction, like the `month`, `NERC region`, `climate region`, and `climate category`. These features reflect the broader environmental and regional context surrounding each outage, and because they’re known immediately, they avoid the pitfall of using future variables like outage duration or restoration time.

To evaluate the model, I’m focusing on F1-score alongside accuracy. Accuracy alone can be misleading here because outage causes are noticeably imbalanced as weather-related outages dominate the dataset, while other categories appear far less frequently. The F1-score treats each cause category equally, which gives me a much clearer sense of whether the model is doing well across the board rather than just memorizing the majority class. In a setting like this, where understanding minority causes is just as important, the F1-score provides a more honest and well-rounded assessment of how the model performs.

# Baseline Model

For my baseline model, I used a simple XGBoost classifier wrapped in a single sklearn Pipeline that handled all preprocessing and model training end-to-end. The model uses four features: `month` (quantitative), `climate category` (nominal), `climate region` (nominal), and `NERC region` (nominal). Since the only quantitative feature (month) doesn’t require encoding, I left it as-is and standardized it using a StandardScaler. All three nominal features were transformed using a OneHotEncoder so the model could work with them properly.

In terms of performance, the baseline model achieved an accuracy of 0.60 and a F1-score of 0.315 on the test set. While 60% accuracy might look decent at first glance, the F1-score tells a different story: the model is struggling significantly on the minority cause categories, sometimes failing to predict them at all. Most of the performance is being carried by the dominant class, severe weather, which is consistent with the warnings in the classification report. Because this baseline is intentionally simple, I wouldn’t consider it “good” yet, but it definitely established a starting point that highlights the current weaknesses of the model and set a very clear direction for improvement in the final model.

# Final Model

For my final model, I expanded the baseline by adding two new features: `year` (quantitative) and `US state` (nominal), on top of the original features (month, NERC region, climate region, and climate category). The year feature provides more long term temporal context than month (as month provides more seasonal context), allowing the model to capture long-term trends or policy/technology changes that could influence outage causes. Including the US state gives more granular geographic information than NERC region or climate region alone, which helps the model differentiate between areas that share a broad climate zone but have different infrastructure or operational practices. In terms of preprocessing, year was standardized using StandardScaler, while US state (like the other categorical features) was one-hot encoded. Then, I integrated this preprocessing and model training into my baseline sklearn Pipeline.

The modeling algorithm I chose was XGBoost, the same as in the baseline, but I improved performance through hyperparameter tuning using GridSearchCV. I tuned max_depth (to control tree complexity), learning_rate (to control how much each tree impacts the model), and n_estimators (the number of trees), and applied 3-fold cross-validation to optimize the F1-score. The best combination was a learning rate of 0.07, max depth of 6, and 600 trees. On the test set, the final model achieved accuracy of 0.71 and an F1-score of 0.48, which is definitely a significant improvement over the baseline model's 0.60 accuracy and 0.32 F1.

Here is the baseline classification report and final model classification report, so you can see the improvement in the model:

Baseline Classification Report:

|                               | precision |   recall | f1-score | support |
| ----------------------------: | --------: | -------: | -------: | ------: |
|             equipment failure |      0.00 |     0.00 |     0.00 |      11 |
|         fuel supply emergency |      0.33 |     0.14 |     0.20 |       7 |
|            intentional attack |      0.58 |     0.52 |     0.54 |      66 |
|                     islanding |      0.00 |     0.00 |     0.00 |       9 |
|                 public appeal |      0.56 |     0.36 |     0.43 |      14 |
|                severe weather |      0.66 |     0.82 |     0.73 |     148 |
| system operability disruption |      0.30 |     0.29 |     0.30 |      24 |
|                  **accuracy** |           |          | **0.60** | **279** |
|                 **macro avg** |  **0.35** | **0.30** | **0.32** | **279** |
|              **weighted avg** |  **0.55** | **0.60** | **0.57** | **279** |

Final Model Classification Report:

|                               | precision |   recall | f1-score | support |
| ----------------------------: | --------: | -------: | -------: | ------: |
|             equipment failure |      0.25 |     0.09 |     0.13 |      11 |
|         fuel supply emergency |      0.33 |     0.29 |     0.31 |       7 |
|            intentional attack |      0.79 |     0.82 |     0.81 |      66 |
|                     islanding |      0.50 |     0.33 |     0.40 |       9 |
|                 public appeal |      0.55 |     0.79 |     0.65 |      14 |
|                severe weather |      0.78 |     0.81 |     0.80 |     148 |
| system operability disruption |      0.27 |     0.25 |     0.26 |      24 |
|                  **accuracy** |           |          | **0.71** | **279** |
|                 **macro avg** |  **0.50** | **0.48** | **0.48** | **279** |
|              **weighted avg** |  **0.69** | **0.71** | **0.69** | **279** |

The final model shows a very clear improvement over the baseline across multiple metrics and classes. Overall accuracy increased from 0.60 to 0.71, and the F1-score improved from 0.32 to 0.48, indicating that the model is performing better not just on the majority class but across all classes. Smaller and previously under-predicted categories like islanding and equipment failure now have higher recall and F1-scores, showing that the model is more capable of identifying minority classes. Major classes like intentional attack and severe weather also have higher precision and F1-scores, which means the model is becoming more consistent and reliable at predicting.

Overall, the final model shows that adding thoughtful, engineered features and tuning hyperparameters can significantly improve both predictive accuracy and fairness across classes.

# Fairness Analysis

For my fairness analysis, I decided to compare outages in the South versus outages in the Northwest. I chose these groups because after inspection of the pivot table I made under the "Aggregate Analysis" section, these two regions seemed to have very distinct outage duration values, which could indicate possible regional differences. Since regional factors can influence outage patterns, I wanted to ensure that my model performs consistently across areas. Ensuring fairness here is important, since inaccurate predictions in one region could mislead energy companies about which outages to prioritize.

I used macro-averaged precision as my evaluation metric because it accounts for all cause categories and handles any class imbalance. To test for fairness, I ran a permutation test, shuffling the group labels 10,000 times to create a null distribution of precision differences. Then I compared my observed precision difference between the South and Northwest to this null distribution.

**Null Hypothesis**: The model is fair. Its precision is roughly the same for South and Northwest outages, and any differences are due to random chance.

**Alternative Hypothesis**: The model is unfair. Its precision for South outages is lower than for Northwest outages.

**Significance Level**: 5%

**P-value**: 0.1564

After running the test, I got a p-value of 0.1564. Since this is above the 0.05 significance level, I fail to reject the null hypothesis. This means there’s no evidence that the model performs worse in the South compared to the Northwest. The histogram below shows the null distribution of precision differences, with the observed difference highlighted for reference.

<iframe
  src="assets/distr_precision_diff.html"
  width="900"
  height="650"
  frameborder="0"
></iframe>
