
# Walmart Black Friday Sales Analysis

## Project overview
This project analyzes Walmart Black Friday transaction data to understand how customer spending changes by **gender, age group, marital status, and city**.  
We ask: do women spend more than men? Which age groups spend the most? How should Walmart use these answers?

## Problem statement
Walmart wants to analyze customer purchase behavior during Black Friday sales to check whether spending habits differ by **gender**, **marital status**, and **age group**. The goal is to use these insights to make better marketing and stocking decisions.

## Goal
- Explore the dataset and clean basic issues.  
- Compare average spending across groups (gender, marital status, age).  
- Use **confidence intervals** and **Central Limit Theorem (CLT)** to estimate population averages and their uncertainty.  
- Give simple, actionable recommendations for Walmart.

## Dataset
Main columns used:
- `User_ID` — unique customer id  
- `Product_ID` — product code  
- `Gender` — M or F  
- `Age` — group labels (0-17, 18-25, 26-35, 36-45, 46-50, 51-55, 55+)  
- `Occupation` — masked id  
- `City_Category` — A / B / C  
- `Stay_In_Current_City_Years` — 0,1,2,3,4+  
- `Marital_Status` — 0 = unmarried, 1 = married  
- `Product_Category` — masked id  
- `Purchase` — purchase amount (target)

Dataset shape used in analysis: **550,068 rows × 10 columns**

## Key analysis steps (simple)
1. Load the CSV and inspect structure (`df.shape`, `df.info()`, `df.head()`).
2. Convert text-like columns to category type for faster grouping:
   ```python
   for col in ['Gender','Age','City_Category','Stay_In_Current_City_Years','Product_Category']:
       df[col] = df[col].astype('category')

3. Check for missing values and outliers (df.isnull().sum(), sns.boxplot(df['Purchase'])).

4.Compute basic summary statistics (df['Purchase'].describe()).

5.Group by categories and compute averages (e.g., df.groupby('Gender')['Purchase'].mean()).

6.Calculate confidence intervals (bootstrap and analytic) for group means.

7.Use CLT: draw repeated samples (n = 300, 3000, 30000) and compare the sampling distribution of the mean.

8.Plot results: histogram, barplots with CI, boxplots, countplots, heatmap for numeric correlation.

## Main results (numbers from the analysis)
Basic purchase stats

Count: 550,068 transactions

Mean purchase: about ₹9,264

Median (50%): ₹8,047

Purchase range: ₹12 (min) to ₹23,961 (max)

Distribution: right-skewed (most transactions are medium sized; a few are very high)

Gender — averages and 95% CI

Average female spending: ₹8,734.57

Average male spending: ₹9,437.53

95% CI (bootstrap example):

Male: (₹9,422.69, ₹9,452.23)

Female: (₹8,709.55, ₹8,758.93)

Conclusion: males spend more per transaction than females. The 95% confidence intervals do not overlap, so the difference is statistically meaningful.

## Married vs Unmarried

Average married: ₹9,261.17

Average unmarried: ₹9,265.91

95% CIs overlap → marital status has little effect on spending.

Age groups (95% CI for each)

0–17: (₹8,851.95, ₹9,014.98)

18–25: (₹9,138.41, ₹9,200.92)

26–35: (₹9,231.73, ₹9,273.65)

36–50: (₹9,270.46, ₹9,320.20)

51+: (₹9,423.17, ₹9,504.16)

Conclusion: people aged 26–50 are the most valuable group (highest average spend).

## Central Limit Theorem (CLT) and sample-size effect

As sample size increases (300 → 3,000 → 30,000), confidence intervals get narrower (estimates become more precise).

CLT: the sampling distribution of the mean becomes tighter and more stable with larger samples.

## Correlation

Numeric columns (Occupation, Marital_Status, User_ID) show very low correlation with Purchase.

Demographics (Gender, Age, City) are more informative for spending differences.

Simple insights (what Walmart should know)

Men spend about ~₹700 more per transaction than women. Market premium/high-value categories more toward male shoppers.

Marital status does not significantly change average spend; combined campaigns are fine.

The 26–50 age range is the highest spending segment — prioritize them for premium promotions.

City B customers contribute the most transactions and should get extra marketing and inventory focus.

Most customers are medium spenders (₹5k–₹12k). A small group are high spenders — create loyalty or VIP programs for them.

## Recommendations (short, actionable)

Target premium electronics and bundles to male customers.

Use discounts, cashback, and loyalty programs to increase basket value for female customers.

Focus advertising and stock planning on the 26–50 age group.

Increase marketing and inventory in City Category B.

Keep a balanced product mix (budget + premium) and use dynamic pricing during big sale events.

Short example code snippets (ready to use)

## Calculate average by gender:

Always show details
avg_gender = df.groupby('Gender')['Purchase'].mean()
print(avg_gender)


## Analytic 95% CI (normal approx):

Always show details
import numpy as np
def ci(data, confidence=0.95):
    mean = np.mean(data)
    std = np.std(data, ddof=1)
    n = len(data)
    z = 1.96  # for 95% CI
    moe = z * (std / np.sqrt(n))
    return (mean - moe, mean + moe)

male = df[df['Gender']=='M']['Purchase']
female = df[df['Gender']=='F']['Purchase']
print("95% CI for Male:", ci(male))
print("95% CI for Female:", ci(female))


## Bootstrap CI:

Always show details
import numpy as np
def bootstrap_ci(data, n_bootstrap=2000, ci=95):
    data = np.array(data)
    means = [np.mean(np.random.choice(data, size=len(data), replace=True))
             for _ in range(n_bootstrap)]
    lower = np.percentile(means, (100 - ci)/2)
    upper = np.percentile(means, 100 - (100 - ci)/2)
    return (lower, upper)


## CLT sample experiment:

Always show details
for n in [300, 3000, 30000]:
    male_s = np.random.choice(male, size=n, replace=True)
    female_s = np.random.choice(female, size=n, replace=True)
    print(n, "Male CI:", ci(male_s), "Female CI:", ci(female_s))

## How to reproduce

Put walmart_data.csv and this notebook in the same folder.

Open the notebook (or run the script) and run cells top-to-bottom.

Required Python libraries: pandas, numpy, matplotlib, seaborn, scipy (optional).

## Files in this repo

Walmart_analysis.ipynb — main notebook (code + plots)

walmart_data.csv — dataset (not included here, put your file)

wallMart.pdf — solution export (your PDF)

<!-- portfolio-navigation -->
## Repository guide

- **Code and notebooks** · [wallmart solution.pynb](wallmart%20solution.pynb)
- **Data** · [walmart_data.csv](walmart_data.csv)
- **Papers and reports** · [wallmart solution.pdf](reports/wallmart%20solution.pdf)

<!-- /portfolio-navigation -->

