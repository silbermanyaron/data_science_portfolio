# Used Device Price Modeling

## Business problem

ReCell, a refurbished-device business, wants to price used and refurbished phones and tablets competitively. This project builds a linear regression model of used-device price and identifies which device attributes most strongly drive it, so the business can price inventory and decide what to source.

## Data

3,454 devices with attributes such as brand, operating system, screen size, 4G and 5G support, main and selfie camera megapixels, internal memory, RAM, battery, weight, release year, days used, and the price of the device when new. The target is the normalized used price.

## Approach

1. **Exploratory analysis:** distributions, relationships to price, and outlier review.
2. **Preprocessing:** missing hardware values imputed with medians grouped by brand and release year, a years-since-release feature (relative to 2021, when the data was collected), outlier review of camera and memory values, and one-hot encoding of categorical variables. A 70/30 split (2,417 / 1,037 rows).
3. **Ordinary least squares** with statsmodels. Multicollinearity was checked with VIF and high p-value predictors were removed. Screen size was dropped for multicollinearity, leaving 13 predictors.
4. **Assumption checks:** linearity of residuals, normality (Shapiro-Wilk), and homoscedasticity (Goldfeld-Quandt).
5. **Evaluation** on train and test data.

## Results

| Metric | Train | Test |
|--------|-------|------|
| R-squared | 0.839 | 0.837 |
| Adjusted R-squared | | 0.835 |
| RMSE | | 0.242 |
| MAE | | 0.188 |
| MAPE | | 4.58% |

Train and test performance are nearly identical, so the model is not overfitting. The model explains about 84% of the variation in used price, and predictions are off by under 5% on average.

## Findings

- Main camera resolution has a stronger relationship with price than selfie camera resolution.
- RAM and the price of the device when new both raise the expected used price.
- Price falls as the device ages (years since release).
- Devices running an uncommon operating system (the "Others" group) sell for less than those on mainstream systems, controlling for the other attributes.
- Residuals look close to normal in plots, but the Shapiro-Wilk test rejects strict normality, which the notebook notes as a limitation.

## Takeaways

- A simple, interpretable linear model gives dependable price estimates, and its coefficients translate directly into business guidance on what buyers pay for.
- ReCell can use the model to set prices and to prioritize the device specifications that command a premium.

## Files

- `notebook.ipynb`: full analysis with saved outputs
- `data/README.md`: where to put the dataset to re-run the notebook
