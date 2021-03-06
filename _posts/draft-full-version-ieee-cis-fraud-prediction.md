---
title: "VESTA IEEE-CIS eCommerce Fraud Detection -- Full Version"
layout: post
date: 2019-10-04
fromdate: 2019-09-04
tag:
img:
projects: False
drafts: true
description: "Predict fraudulant transactions in eCommerce transactional data"
jemoji: '<img class="emoji" title=":graduation cap:" alt=":graduation cap:" src="https://github.githubassets.com/images/icons/emoji/unicode/1f697.png" height="20" width="20" align="absmiddle">'
---

## The Problem

Given anonymized eCommerce transaction data (tabular data) which were labelled "**Fraud**" or "**Not Fraud**", the task was to build a binary classifier model that will predict whether a given transaction is "Fraud" or "Not Fraud" in new data. The labeling logic for "Fraud" was as follows: If an eCommerce transaction was reported & and a chargeback was issued, then that transaction was labelled "Fraud". All the previous transactions by that person/account/card were also labelled "Fraud" for 120 days after the first report; subsequently removing the label if no more transactions were repoted by the customer.

The dataset for this Kaggle competetion was provided by Vesta Corporation. Vesta Corporation is a forerunner in guaranteed e-commerce payment solutions. Here's an exceprt from the competetion description:
<br>
> <p style="max-height:50px;text-align:center"><img src="{{ site.relrefurl }}/Site_Materials/figures/ieee-cis-logo.png" style="height:50px;display: inline; margin-right:20px"><img src="{{ site.relrefurl }}/Site_Materials/figures/Vesta-logo_200x.png" style="height:50px; display:inline; margin-left:20px" ></p>
>In this competition, you’ll benchmark machine learning models on a challenging large-scale dataset. The data comes from Vesta's real-world e-commerce transactions and contains a wide range of features from device type to product features. You also have the opportunity to create new features to improve your results.
>If successful, you’ll improve the efficacy of fraudulent transaction alerts for millions of people around the world, helping hundreds of thousands of businesses reduce their fraud loss and increase their revenue.


## Solution
I worked on this competetion along with Nishan Singh Mann (a Data Scientist based out of New York). We tried a couple approaches to this problem. The approaches that put us in the top 50% of the competitors (~0.91 on AUC-ROC metric), involved building Tree based models with careful preprocessing and feature engineering.

### My approach - Tree based predictive model with impetus on careful preprocessing and feature engineering.
The given dataset had 400+ features and lots of missing data. A majority of the input features were numeric and of the categorical features, many had a high cardinality. As many of the input features (columns) were anonymized, it was difficult to guess what was the data in many of the columns. However, some information about groups of columns was provided which helped towards preprocessing and feature engineering.  

#### Pre-Processing:
- Applying mean-imputation on a large group of numeric features, followed by Z-scaling and dimensionality reduction with PCA. 
- Missing values in the rest of the numeric features were filled a place holder (a large outlier). 
- Missing values in categorical features were filled with a place holder category and subsequently Label-Encoded. 
- Removing highly null columns and single valued columns from the dataset.

#### Feature Engineering:
- Interaction between specific categorical features. 
- Target Likelihood Encoding of high cardinality categorical variables.
- Frequency Encoding of some categorical variables.
- Given the nature of "Fraud" labelling logic, it was likely that a certain card-holder/customer had a higher likelihood of having fraudulant transactions for 120 days since fraud was reported on the account. Due to this, it made sense to uniquely identify users in the dataset. For this Proxy-IDs were created using groups of loose-identifiers. 
- Proxy-ID group based features like intra-group quantile-transformations, medians, counts etc.
- Rationalized some categorical features like device names, operating system, browser names and versions etc.
- Time based features like day of week and hour of day, and daily and hourly aggregations of certain numeric features.
- Finally downcasting features to reduce memory burden during modeling.

#### Predictive Model:
- A RandomForest based Tree model was fit along with class weighting to handle class-imbalance in the data (~96% of "Not Fraud"). The model was tuned to optimize for bias vs variance. 
- Multiple CV strategies were tried and compared to replicate the train/test split in training/validation so as to achieve the best model generalization. Finally, the model that performed best on the two key CV formulations was chosen.

## Project Website 


