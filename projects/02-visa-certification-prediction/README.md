# Visa Certification Prediction

## Business problem

The Office of Foreign Labor Certification (OFLC) processes a rising number of work-visa applications each year, and reviewing each one by hand takes time and staff. The project builds a classification model that predicts whether an application will be certified or denied, and identifies which factors most influence the outcome, as a way to help OFLC prioritize its review effort.

## Data

25,480 applications. Features describe the employee (continent of origin, education level, job experience, whether job training is required) and the employer and job (company size, year established, region of employment, prevailing wage, wage unit, full-time position). The target is case status, with about two thirds of cases certified.

## Approach

1. **Exploratory analysis:** univariate review of every feature and bivariate analysis against case status (education, experience, region, continent, wage type, prevailing wage).
2. **Preprocessing:** outlier review, encoding, and a stratified 70/30 train/test split (17,836 / 7,644 rows).
3. **Metric choice:** both error types carry a cost. Certifying an application that should be denied takes an opportunity from a more deserving applicant, and denying one that should be certified loses a qualified candidate. I therefore optimized F1 and used balanced class weights.
4. **Models compared**, each at default settings and tuned with grid search: decision tree, bagging, random forest, AdaBoost, gradient boosting, and XGBoost, plus a stacking classifier.
5. **Selection** on test F1 together with the train/test gap.

## Results (test set)

| Model | Accuracy | Recall | Precision | F1 |
|-------|----------|--------|-----------|-----|
| Decision tree, tuned | 0.707 | 0.931 | 0.715 | 0.809 |
| Random forest, tuned | 0.738 | 0.899 | 0.755 | 0.821 |
| XGBoost, tuned | 0.745 | 0.878 | 0.772 | 0.821 |
| Gradient boosting, default (selected) | 0.745 | 0.876 | 0.772 | 0.821 |

Several models land within a point of each other on F1 (about 0.82). I selected default gradient boosting because it matched the best F1 with a modest train/test gap (train F1 0.830, test 0.821) and needed no tuning.

The strongest predictors were education level (the high school indicator in particular), prior job experience, and prevailing wage, followed by master's or doctorate education and the applicant's continent.

## Takeaways

- A model like this can serve as a pre-screening aid so that reviewers spend their time on the applications that need it most.
- Education, experience, and wage level account for most of the model's signal.
- Data should keep being collected year over year so the model can be refined.

## Caveats

This is a screening-aid exercise on a course dataset. The model uses attributes such as continent of origin because they are in the data, and any real deployment would need a fairness and legal review and human adjudicators making the decisions.

## Files

- `notebook.ipynb`: full analysis with saved outputs
- `data/README.md`: where to put the dataset to re-run the notebook
