# MoMA Collection Metadata Project

## Midterm Project Report

### Team Members

- LIU Shiqi
- WANG Yuling
- ZHAO Yang

### Project Links

- **Data source:** [MoMA Collection Dataset](https://github.com/MuseumofModernArt/collection)
- **Presentation:** [Google Slides](https://docs.google.com/presentation/d/1bQAFA9kRCh8y8M3GlJiRZnAVYaBDGwUgR_FPOiNd2iY/edit?usp=sharing)
- **Project repository:** this repository

---

## 1. Motivation

Museum collection databases contain large amounts of structured metadata, including information about artists, creation dates, media, dimensions, acquisition dates, departments, and cataloging status. These records support collection management, research, and public access, but museum metadata can also contain missing, inconsistent, or difficult-to-standardize information.

Our original project question was:

> **Can we identify artwork records that are most likely to require cataloguing review?**

This question was initially based on an older MoMA dataset available through [Kaggle](https://www.kaggle.com/datasets/momanyc/museum-collection), which contained information related to curator approval and cataloging status.

However, after switching to the current dataset published by the Museum of Modern Art, we found that the structure of the data had changed substantially. The current dataset does not contain an explicit variable indicating whether a record has been rejected or requires curator review.

We therefore revised the scope of the midterm project to focus on the statistical properties of one important metadata field: **artwork creation year**.

---

## 2. Current Research Questions

For the midterm, we focus on the following questions:

### RQ1

> **How are artwork creation years distributed in MoMA's collection?**

### RQ2

> **How does sample size affect the accuracy and stability of estimates of the creation-year distribution?**

### RQ3

> **Does a Normal distribution adequately describe the creation years of artworks in MoMA's collection?**

These questions allow us to examine the behavior of real museum metadata using probability and statistical methods introduced in the course, including random sampling, the Law of Large Numbers, the Central Limit Theorem, and Maximum Likelihood Estimation.

This midterm analysis also provides a foundation for the broader final project, where we plan to investigate whether statistical characteristics of metadata can support more targeted analysis of cataloging patterns.

---

## 3. Data

We use the current open-access MoMA collection dataset published by the Museum of Modern Art:

[https://github.com/MuseumofModernArt/collection](https://github.com/MuseumofModernArt/collection)

The primary file used in our analysis is:

`Artworks.csv`

The dataset contains **160,700 artwork records** and includes variables such as:

- Title
- Artist
- Artist nationality
- Gender
- Artwork date
- Medium
- Dimensions
- Classification
- Department
- Acquisition date
- Cataloged status
- Object ID
- URL and image information

Each row represents an artwork record.

---

## 4. Initial Data Exploration

We first examined overall metadata completeness.

We constructed a metadata completeness score based on whether selected core fields were present for each artwork record.

The initial results were:

```text
count    160700.000000
mean          0.982345
std           0.050796
min           0.400000
25%           1.000000
50%           1.000000
75%           1.000000
max           1.000000
```

The average metadata completeness is approximately **98.2%**, while the median and both the 25th and 75th percentiles are 100%.

This suggests that simple missing fields status alone is unlikely to provide enough variation for the original catalog-review question.

---

## 5. Why We Focus on Creation Year

Creation year provides a useful variable for the midterm analysis for both substantive and statistical reasons.

First, time is an important dimension of an art museum collection. The distribution of creation years can reveal how strongly the collection is concentrated in particular historical periods.

Second, the raw date field presents genuine data-cleaning challenges. Examples include:

- exact years
- year ranges
- multiple years
- approximate dates
- missing dates
- B.C. dates
- textual date descriptions

For example, some records cannot be converted directly into a standard integer year without additional parsing.

Creation year therefore provides a useful case for studying both **real-world metadata cleaning** and **statistical estimation**.

---

## 6. Data Cleaning

Our cleaning process includes:

1. Checking duplicated records and Object IDs.
2. Inspecting missing values.
3. Cleaning text fields.
4. Converting acquisition dates into structured date variables.
5. Parsing artwork creation dates.
6. Converting valid creation years into numeric values.
7. Handling ranges, unusual formats, B.C. dates, and other ambiguous values.
8. Creating analysis-ready datasets for sampling and modeling.

Because the original `Date` field contains heterogeneous formats, we retain the original value while creating a separate numeric variable for statistical analysis.

Our cleaning notebooks are available in:

`IS507-26Fall-Group3/data_cleaning/`

---

## 7. Sampling Plan

We treat the full cleaned MoMA dataset as the population and draw random samples of different sizes.

The three main sample sizes are:

- \(n = 100\)
- \(n = 1,000\)
- \(n = 3,000\)

Each team member works with one sample size while all members participate in the overall analysis, interpretation, and comparison.

For each sample size, we record:

- sample mean
- sample standard deviation
- difference between sample mean and population mean
- absolute estimation error

We then repeat the random sampling procedure **500 times** for each sample size.

This allows us to compare the sampling distributions of the sample mean.

---

## 8. Law of Large Numbers and Central Limit Theorem

Using repeated sampling, we examine whether larger samples produce more stable estimates of the population mean.

According to the Law of Large Numbers, we expect:

\[
\bar{X} \rightarrow \mu
\]

as sample size increases.

We therefore compare the absolute error:

\[
|\bar{X} - \mu|
\]

for different sample sizes.

We also study the sampling distribution of the sample mean using 500 repeated samples.

According to the Central Limit Theorem, the distribution of sample means should become increasingly concentrated around the population mean as sample size increases.

The standard error is expected to decrease approximately according to:

\[
SE(\bar{X}) = \frac{\sigma}{\sqrt{n}}
\]

---

## 9. Maximum Likelihood Estimation

We also use Maximum Likelihood Estimation to estimate parameters of the creation-year distribution.

As a baseline probability model, we initially assume that creation years follow a Normal distribution:

\[
X_i \sim N(\mu,\sigma^2)
\]

For a Normal model, the maximum likelihood estimate of the mean is:

\[
\hat{\mu}_{MLE} = \bar{X}
\]

and the maximum likelihood estimate of the variance is:

\[
\hat{\sigma}^2_{MLE}
=
\frac{1}{n}
\sum_{i=1}^{n}(X_i-\bar{X})^2
\]

We estimate these parameters separately for samples of different sizes and compare their stability.

---

## 10. Baseline Model

Our baseline model is a **single Normal distribution** fitted to artwork creation years.

This model is intentionally simple.

Its purpose is not to assume that MoMA's collection must be Normally distributed, but to provide a baseline against which the observed distribution can be evaluated.

For each sample, we compare:

- the observed histogram of creation years
- the fitted Normal distribution
- estimated mean and standard deviation
- differences across sample sizes

This allows us to distinguish between two separate questions:

1. Are the estimated parameters stable?
2. Does the assumed Normal distribution actually fit the observed data?

Stable parameter estimates do not necessarily imply that the Normal model is a good representation of the data.

---

## 11. Expected Challenges

We have identified several challenges.

### Date parsing

The creation-date field contains heterogeneous formats, including year ranges, approximate dates, multiple dates, and B.C. dates.

### Historical outliers

Very early or very late creation years may be valid museum records rather than data errors. Statistical rarity therefore cannot automatically be interpreted as incorrect metadata.

### Distributional assumptions

Artwork creation years may be skewed, multimodal, or historically clustered rather than Normally distributed.

### Sampling variability

Small samples may give substantially different estimates from the full population, especially if the underlying distribution contains long tails or relatively rare historical periods.

---

## 12. Planned Analysis

For the midterm, we plan to:

1. Complete creation-year data cleaning.
2. Calculate population-level descriptive statistics.
3. Draw samples of sizes 100, 1,000, and 3,000.
4. Compare sample means with the population mean.
5. Repeat sampling 500 times for each sample size.
6. Compare sampling distributions and standard errors.
7. Estimate Normal-distribution parameters using MLE.
8. Plot fitted Normal curves against observed creation-year distributions.
9. Evaluate whether the Normal model provides an adequate baseline description.

For the final project, we expect to extend the analysis using additional metadata variables and more flexible models.

---

## 13. Repository Structure

```text
.
├── README.md
│
├── notebooks/
│   ├── data_cleaning/
│   ├── sampling/
│   ├── modeling/
│   └── final_analysis/
│
├── figures/
│   ├── midterm/
│   └── final/
│
├── data/
│   └── README.md
│
└── docs/
```

### Notebook organization

- `data_cleaning/` contains different iterations of the data-cleaning process.
- `sampling/` contains sampling and repeated-sampling experiments.
- `modeling/` contains MLE and probability-model fitting.
- `final_analysis/` will contain integrated analyses for the final project.

---

## 14. Team Workflow

All team members participate in:

- data cleaning
- statistical analysis
- interpretation
- visualization
- report preparation
- presentation preparation

Individual notebooks may focus on different sample sizes or analytical tasks, but results are reviewed and compared jointly by the team.

---

## 15. Current Status & Future Work

At the midterm stage, we have:

- selected the current official MoMA dataset
- completed initial data exploration
- identified limitations of the original research question
- examined metadata completeness
- developed the creation-year cleaning strategy
- designed the sampling experiment
- selected a Normal distribution as the initial baseline model
- developed an MLE-based estimation plan

The next step is to complete the repeated-sampling analysis and compare the fitted Normal models across sample sizes.

## 16. AI Disclosure

We used Claude and ChatGPT to support brainstorming during dataset selection. We also used ChatGPT to assist with Python coding and to help draft and revise parts of the report. All AI-assisted code, analyses, and written content were reviewed, tested, and discussed by the team, and all team members are expected to understand the methods and results presented in this project. 
