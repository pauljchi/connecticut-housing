# connecticut-housing

## Dataset

The dataset contains residential real estate transactions recorded in Connecticut from 2001 through the third quarter of 2023. Because residential type is largely unavailable before late 2006, the analysis focuses on transactions recorded from 2006 onward.

- 1,141,722 original transactions
- 14 original columns
- Property location, recording date, assessed value, sale amount, sales ratio, and residential type
- Modeling subset restricted to condos and single-family homes
- `sale_amount`: target variable representing the recorded sale price
- `sales_ratio`: tax-assessed value divided by sale amount

Dataset source: [Connecticut Real Estate Sales on Data.gov](https://catalog.data.gov/dataset/real-estate-sales-2001-2018)

## Data Preparation and Exploratory Analysis

The full analysis is available in [`connecticut-housing.ipynb`](connecticut-housing.ipynb).

The notebook covers:

- Missing-value and schema review
- Removal of sparsely populated and unnecessary columns
- Date and street-address cleaning
- Restriction to condo and single-family transactions recorded from 2006 onward
- Removal of sales below $5,000 and implausible sales-ratio values
- Transaction volume and median sale-price trends over time
- Town-level transaction frequency and median sale-price analysis
- Seasonal transaction patterns
- Sale amount, assessed value, and sales-ratio distributions
- Comparisons between condos and single-family homes

## Preliminary Findings

- Transaction volume declined after the 2000s housing boom, recovered gradually, and peaked during the pandemic-era buying surge.
- Stamford had the most transactions among Connecticut towns in the cleaned data.
- Town transaction frequency and median sale price were nearly unrelated, with an R-squared of approximately 0.01.
- Sales activity was seasonal, increasing during spring and peaking in summer.
- Tax-assessed value and sale amount had a strong relationship, with an R-squared of approximately 0.80.
- The median sales ratio was approximately 0.60, indicating that sale prices were generally higher than tax-assessed values.
- Median sale prices were $185,000 for condos and $280,000 for single-family homes.
- Single-family sale prices had a heavier right tail and greater variability than condo prices.
- Home prices fell after the 2008 financial crisis, remained relatively flat for roughly a decade, and then rose sharply before and during the pandemic period. Condo prices showed more muted changes.

## Model Development and Evaluation

The modeling section tests the hypothesis that smaller and less expensive homes are harder to price accurately.

The notebook covers:

- Sale-price prediction using `year`, `town`, `tax_assessed_value`, and `residential_type`
- One-hot encoding of town and residential type
- An 80/20 training and test split with a fixed random seed
- Comparison of linear regression and random forest regression
- Random forest training with 100 estimators and a maximum depth of 10
- Evaluation using R-squared and absolute residuals
- Error comparisons by residential type and assessed-value quintile
- Residual analysis for heteroscedasticity and high-value outliers

## Model Results

- Linear regression achieved an R-squared of 0.861 on the test set.
- Random forest regression performed slightly better, with an R-squared of 0.866.
- Random forest produced lower mean absolute residuals than linear regression for both residential types and across every assessed-value quintile.
- For the random forest model, mean absolute residuals were approximately $44,693 for condos and $81,705 for single-family homes.
- Random forest mean absolute residuals ranged from approximately $37,975 to $57,097 across the first four assessed-value quintiles, then increased to approximately $191,455 in the highest quintile.
- Both models were substantially less reliable for the most expensive properties and tended to underpredict many sales above $1 million.
- The findings did not support the original hypothesis: lower-value properties were generally easier to predict, while the luxury segment showed much larger errors.
- Residential type and assessed value are imperfect proxies for property size. Square footage, bedroom and bathroom counts, temporal validation, and analysis of additional housing markets would strengthen future work.
