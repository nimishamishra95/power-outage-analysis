# Power Outage Analysis

By Nimisha Mishra

If you are viewing this repository, access the report [here](https://nimishamishra95.github.io/power-outage-analysis/).

# Introduction

Major power outages are disruptive events that affect daily life, compromise public safety, and create significant economic burdens across the United States. Understanding not just when and where outages occur, but also why they last as long as they do, is essential for improving grid resilience and preparing infrastructure for a changing climate. This project uses a dataset from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure (LASCI), which tracks major power outages across the continental U.S. The dataset includes 1,534 rows detailing the geographical, climatic, and operational characteristics of each event, offering a rich view of outage patterns, impacts, and regional factors.

The central question guiding this analysis is:
How do anomaly level and climate category impact the duration of a major power outage?

This question matters because climate variability is playing an increasingly important role in electrical grid stability. As extreme temperatures, seasonal anomalies, and shifting climate patterns become more frequent, understanding how they relate to outage duration can help policymakers, energy providers, and communities better anticipate vulnerabilities and allocate resources effectively. By analyzing these data, we can see whether certain climatic conditions contribute to longer outages, and if so, in what ways.

Relevant Columns

To answer this question, several key variables from the dataset are used:

YEAR, MONTH – When each outage occurred, giving seasonal and historical context.

U.S.\_STATE, POSTAL.CODE – The geographical location of each outage.

NERC.REGION – The North American Electric Reliability Corporation region responsible for grid oversight in the affected area.

CLIMATE.REGION – Groups states into broad climate regions (e.g., Southeast, Northwest), enabling comparisons across zones.

ANOMALY.LEVEL – Measures the temperature anomaly at the time of the outage, showing how much warmer or cooler it was compared to historical averages.

CLIMATE.CATEGORY – Classifies the general climate conditions during the outage (e.g., dry, wet, warm), supporting analysis of climate impacts on outage duration.

OUTAGE.START.DATE / OUTAGE.START.TIME – When the outage began.

OUTAGE.RESTORATION.DATE / OUTAGE.RESTORATION.TIME – When power was restored.

CAUSE.CATEGORY / CAUSE.CATEGORY.DETAIL – Information on what triggered the outage, such as weather events, equipment failure, or other factors.

OUTAGE.DURATION (min) – The primary outcome variable, measuring how long the outage lasted in minutes.

Together, these columns provide a detailed view of how climatic factors—specifically anomaly level and climate category—may influence the duration of major power outages across the United States.

# Data Cleaning and Exploratory Data Analysis

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis
