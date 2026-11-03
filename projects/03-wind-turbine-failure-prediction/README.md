# Wind Turbine Failure Prediction

## Business problem

Wind turbines are expensive to repair when a component fails unexpectedly, and far cheaper to service when a failure is caught early. Using ciphered sensor readings from turbine generators, this project builds a classification model that flags likely failures so maintenance can be scheduled before they happen.

Errors have unequal costs. A missed failure means a full replacement, a false alarm means an inspection, and an inspection is cheaper than a replacement. The project therefore prioritizes recall (catching failures) while keeping precision at a workable level.

## Data

Sensor data with 40 predictor variables (V1 to V40; names are ciphered). The course supplied a training file of 20,000 rows and a test file of 5,000 rows. About 5.5% of observations are failures, so the classes are heavily imbalanced. Some values are missing and are imputed inside the modeling pipeline.

## Approach

1. **Exploratory analysis** of every variable, the target balance, and correlations.
2. **Train/validation split** of the training file (15,000 / 5,000), with the separate test file held out.
3. **Missing values** handled with `SimpleImputer` (median) fitted on training data only.
4. **Baseline models** (logistic regression, decision tree, bagging, random forest, AdaBoost, gradient boosting, XGBoost) evaluated with stratified cross-validation on three versions of the data: original, oversampled with SMOTE, and undersampled with `RandomUnderSampler`.
5. **Hyperparameter tuning** with `RandomizedSearchCV` on the four strongest candidates.
6. **Final model** built as a single scikit-learn `Pipeline` (imputer plus classifier) so the same preprocessing applies at prediction time.

## Results

The selected model is a tuned random forest trained on the undersampled data. It scored 0.885 recall on the validation set and 0.879 on the test file when evaluated during selection. The final pipeline on the held-out test data:

| Metric | Value |
|--------|-------|
| Recall | 0.872 |
| Accuracy | 0.945 |
| Precision | 0.507 |
| F1 | 0.641 |

The model misses 36 failures out of the test file's 5,000 rows. Precision of about 0.51 means roughly half of the flagged turbines would be false alarms; that is the cost of the high recall, and it is acceptable here because an inspection costs far less than a replacement. The most influential sensors were V36, V18, V39, V15, and V3.

## Takeaways

- With a 5.5% failure rate, accuracy alone is misleading. Recall and the replace/repair/inspect cost trade-off drive model choice.
- Resampling (SMOTE and undersampling) combined with cross-validation made a clear difference to recall on the minority class.
- Putting imputation and the model in one pipeline avoids leakage and makes deployment simpler.

## Files

- `notebook.ipynb`: full analysis with saved outputs
- `data/README.md`: where to put the datasets to re-run the notebook
