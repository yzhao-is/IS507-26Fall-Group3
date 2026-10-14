# MoMA Collection Metadata Project

## Midterm Project Report

### Team Members

- LIU Shiqi
- WANG Yuling
- ZHAO Yang

### Project Links

- **Data source:** [MoMA Collection Dataset](https://github.com/MuseumofModernArt/collection) (Based on the Sept. 22, 2026 update)
- **Presentation:** [Google Slides](https://docs.google.com/presentation/d/1bQAFA9kRCh8y8M3GlJiRZnAVYaBDGwUgR_FPOiNd2iY/edit?usp=sharing)
- **Project repository:** this repository

---

## 1. Motivation

Museum collection databases contain large amounts of structured metadata, including information about artists, creation dates, media, dimensions, acquisition dates, departments, and cataloging status. These records support collection management, research, and public access, but museum metadata usually also contain missing, inconsistent, or difficult-to-standardize information.

Our original project question was:

> **Can we identify artwork records that are most likely to require cataloguing review?**

This question was initially based on an older MoMA dataset available through [Kaggle](https://www.kaggle.com/datasets/momanyc/museum-collection), which contained information related to curator approval and cataloging status.

However, after switching to the current dataset published by the Museum of Modern Art, we found that the structure of the data had changed. The current dataset does not contain an explicit variable indicating whether a record has been rejected or requires curator review, and based on the disclaimer: "Much of the information included in this dataset is not complete and has not been curatorially approved".

We therefore adjusted our approach. Rather than directly predicting which records need review, we first investigate whether statistical models can reliably describe museum metadata and what limitations these models may have.

For the midterm, we focus on **artwork creation year** as a case study. It is a numerical variable suitable for probability modeling, but its original metadata includes uncertain dates, ranges, and inconsistent formats.

Our midterm serves as a methodological pilot. We examine how sampling affects estimation, establish a simple probabilistic baseline using Maximum Likelihood Estimation (MLE), and explore how prior assumptions influence estimation through Maximum A Posteriori (MAP).

We also conduct an initial comparison of creation years by cataloging status to explore how temporal patterns may relate to our broader research interest.

This allows us to evaluate the usefulness and limitations of simple statistical methods before extending the analysis to additional metadata and more substantive cataloging questions.

---

## 2. Research Questions

For the midterm, we focus on the following questions:

### RQ1

> **How are artwork creation years distributed in MoMA's collection?**

### RQ2

> **How does sample size affect the accuracy and stability of estimates of the creation-year distribution?**

### RQ3

> **Does a Normal distribution adequately describe the creation years of artworks in MoMA's collection?**

These questions allow us to examine the behavior of real museum metadata using probability and statistical methods introduced in the course, including random sampling, the Law of Large Numbers, the Central Limit Theorem, and Maximum Likelihood Estimation.

We use a Normal distribution as our baseline model, estimate its parameters using MLE, and examine the influence of prior assumptions through MAP estimation.

This midterm analysis also provides a foundation for the broader final project, where we plan to investigate whether statistical characteristics of metadata can support more targeted analysis of cataloging patterns.

---

## 3. Data

We use the current open-access MoMA collection dataset published by the Museum of Modern Art:

[https://github.com/MuseumofModernArt/collection](https://github.com/MuseumofModernArt/collection)

The primary file used in our analysis is:

`Artworks.csv` (Sept. 22, 2026 version)

The dataset contains **160,700 artwork records** and includes 30 variables such as:

- Title
- Artist
- ConstituentID
- ArtistBio
- Nationality
- BeginDate
- EndDate
- Gender
- Date (of the artwork)
- Medium
- Dimensions
- OnView
- Department
- Acquisition date
- Cataloged status
- Object ID
- URL and image information

Each row represents an artwork record. No duplicated entry.

---

## 4. Initial Data Exploration

We first examined overall metadata completeness.

The initial results were:

```Missing_Count	Missing_Percent
Seat Height (cm)	160700	100.000000
Circumference (cm)	160690	99.993777
Weight (kg)	160398	99.812072
Length (cm)	159962	99.540759
OnView	159442	99.217175
Diameter (cm)	159300	99.128811
Duration (sec.)	158710	98.761668
Depth (cm)	141962	88.339764
ImageURL	65122	40.523958
URL	56587	35.212819
Width (cm)	31339	19.501556
Height (cm)	30492	18.974487
Medium	8717	5.424393
Dimensions	8325	5.180460
ArtistBio	6684	4.159303
DateAcquired	5462	3.398880
Date	2016	1.254512
CreditLine	1283	0.798382
Artist	1258	0.782825
ConstituentID	1258	0.782825
BeginDate	1258	0.782825
EndDate	1258	0.782825
Nationality	1258	0.782825
Gender	1258	0.782825
Title	48	0.029869
Classification	1	0.000622
AccessionNumber	0	0.000000
Cataloged	0	0.000000
ObjectID	0	0.000000
Department	0	0.000000
```

The "Cataloged" field is a binary with only "Y" and "N", the results are as follows:

```Cataloged	
Y	104113
N	56587
```

The date of artworks should be all before 2026, we identified one with a number "3000", manually checked it, and determined it to be a testing entry and should be removed. (It should a picture of chopsticks with date 3000 B.C.E.)


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

X̄ → μ

as sample size increases.

We therefore compare the absolute error:

|X̄ − μ|

for different sample sizes.

We also study the sampling distribution of the sample mean using 500 repeated samples.

According to the Central Limit Theorem, the distribution of sample means should become increasingly concentrated around the population mean as sample size increases.

The standard error is expected to decrease approximately according to:

SE(X̄) = σ / √n

---

## 9. Maximum Likelihood Estimation

We also use Maximum Likelihood Estimation to estimate parameters of the creation-year distribution.

As a baseline probability model, we initially assume that creation years follow a Normal distribution:

Xᵢ ~ N(μ, σ²)

For a Normal model, the maximum likelihood estimate of the mean is:

μ̂_MLE = X̄

and the maximum likelihood estimate of the variance is:

σ̂²_MLE = (1/n) · Σᵢ₌₁ⁿ(Xᵢ − X̄)²

We estimate these parameters separately for samples of different sizes and compare their stability.

---

## 10. MAP Estimation and Sensitivity Analysis

In addition to Maximum Likelihood Estimation (MLE), we use **Maximum A Posteriori (MAP) estimation** to examine how prior assumptions influence the estimated mean creation year of artworks.

While MLE estimates the parameters using only the observed data, MAP combines sample information with a prior distribution.

The purpose of MAP in this project is **not to determine whether artwork creation years follow a Normal distribution**, but to evaluate the sensitivity and stability of parameter estimates.

### Prior Selection

We assume a **Normal prior distribution** for the mean creation year (μ).

To examine the influence of prior strength, we compare two hypothetical priors:

- **Weak Prior:** Normal distribution with mean = 1950 and standard deviation = 20
- **Strong Prior:** Normal distribution with mean = 1950 and standard deviation = 3

Both priors have the same mean but different levels of uncertainty. These priors are experimental assumptions rather than independently verified historical information.

We combine each prior with an approximately Normal likelihood for the sample mean, using the sample variance to estimate sampling uncertainty.

### MLE vs. MAP Comparison

We compare MLE and MAP across three sample sizes:

- n = 100
- n = 1,000
- n = 3,000

For each sample size, we repeat random sampling 500 times.

For each sample, we compare:

- MLE and MAP estimates of the mean creation year
- Mean Absolute Error (MAE) relative to the full cleaned dataset mean
- Average absolute difference between MAP and MLE
- The influence of weak and strong priors across sample sizes

This comparison allows us to examine whether increasing the sample size reduces the influence of prior assumptions.

### Interpretation

This analysis helps us distinguish between two separate questions:

1. How sensitive are the estimated parameters to prior assumptions?
2. Does increasing the sample size make MAP estimates converge toward MLE estimates?

As sample size increases, we expect the observed data to have greater influence on MAP estimates, reducing the effect of the prior.

However, **agreement between MAP and MLE does not necessarily imply that the Normal distribution adequately represents the observed creation years**.

Therefore, MAP serves as a **supplementary sensitivity analysis**, while the evaluation of Normal model fit relies primarily on the observed distribution, fitted Normal curve, and Q–Q plot.

--

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

- `data_cleaning/` contains different iterations of the data-cleaning process.
- `data_analysis/` contains sampling and repeated-sampling experiments, MLE and probability-model fitting.

---

## 14. Team Workflow

All team members participate in:

- data cleaning
- statistical analysis
- interpretation
- visualization
- report preparation
- presentation preparation

Individual notebooks focus on different sample sizes or analytical tasks, but results are reviewed and compared jointly by the team.

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

## 16. Related Work

Bylund, C., & Näslund, A. (2026). Beyond numbers: visual and textual diversity in the online art database. Museum Management and Curatorship, 41(2), 318–338. https://doi.org/10.1080/09647775.2024.2375994

Zhitomirsky-Geffet M, Kizhner I, Minster S (2023), "What do they make us see: a comparative study of cultural bias in online databases of two large museums". Journal of Documentation, Vol. 79 No. 2 pp. 320–340, doi: https://doi.org/10.1108/JD-02-2022-0047 


## 17. AI Disclosure

We used Claude and ChatGPT to support brainstorming during dataset selection. We also used ChatGPT to assist with Python coding and to help draft and revise parts of the report. All AI-assisted code, analyses, and written content were reviewed, tested, and discussed by the team, and all team members are expected to understand the methods and results presented in this project. 
