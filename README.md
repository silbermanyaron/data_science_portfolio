# Data Science Portfolio

Machine learning and analytics projects from the Data Science certification program at the UT McCombs School of Business (completed May 2024). Each project starts with a business problem, works through exploratory analysis and modeling, and ends with recommendations a non-technical stakeholder could act on.

**A note on provenance.** These are program projects. The course supplied the problem statements, the datasets, and a section outline. The analysis, modeling decisions, evaluation, and written recommendations in each notebook are my own work. The datasets are not included in this repository; each project's `data/README.md` lists the file it expects.

## Projects

| # | Project | Business question | Methods | Headline result |
|---|---------|-------------------|---------|-----------------|
| 1 | [Hotel Cancellation Prediction](projects/01-hotel-cancellation-prediction) | Which bookings are likely to cancel, and what policies would reduce lost revenue? | Logistic regression, decision trees (pre- and post-pruning) | Pre-pruned decision tree, test F1 0.75 on 36,275 bookings |
| 2 | [Visa Certification Prediction](projects/02-visa-certification-prediction) | Can a model help pre-screen a growing volume of work-visa applications? | Decision tree, bagging, random forest, AdaBoost, gradient boosting, XGBoost, stacking | Gradient boosting, test F1 0.82 and recall 0.88 on 25,480 cases |
| 3 | [Wind Turbine Failure Prediction](projects/03-wind-turbine-failure-prediction) | Can sensor data predict generator failures early enough to schedule maintenance? | Over- and undersampling, tuned random forest, scikit-learn pipeline | Test recall 0.87 on a 5.5% failure class |
| 4 | [Used Device Price Modeling](projects/04-used-device-price-modeling) | What drives the resale price of used phones and tablets? | OLS linear regression with full assumption checks | R-squared 0.84, test MAPE 4.6% |
| 5 | [Stock Segmentation](projects/05-stock-segmentation) | How can stocks be grouped by financial profile to support diversification? | K-means and hierarchical clustering | 340 stocks segmented into 8 K-means clusters |

## What the projects cover

- **Python data stack:** pandas, NumPy, Matplotlib, Seaborn, SciPy
- **Modeling:** scikit-learn, statsmodels, XGBoost, imbalanced-learn, Yellowbrick
- **Supervised learning:** regression and classification, including class imbalance handling (SMOTE and random undersampling), hyperparameter tuning (grid and randomized search), cross-validation, and pipelines
- **Evaluation tied to business cost:** choosing recall, precision, or F1 based on which kind of error is more expensive, and checking train versus test gaps for overfitting
- **Regression diagnostics:** multicollinearity (VIF), linearity, normality, and homoscedasticity checks
- **Unsupervised learning:** elbow and silhouette analysis, dendrograms and cophenetic correlation, cluster profiling
- **Communication:** each notebook closes with conclusions and recommendations written for the business audience

## Repository layout

```
.
├── README.md
├── requirements.txt
└── projects/
    ├── 01-hotel-cancellation-prediction/
    │   ├── README.md
    │   ├── notebook.ipynb
    │   └── data/README.md
    ├── 02-visa-certification-prediction/
    ├── 03-wind-turbine-failure-prediction/
    ├── 04-used-device-price-modeling/
    └── 05-stock-segmentation/
```

## Running the notebooks

The notebooks were written in Google Colab, and the saved outputs are from those runs. To re-run one:

1. Place the dataset in that project's `data/` folder using the filename in its `data/README.md`.
2. Use Python 3.10 or 3.11 and install the dependencies: `pip install -r requirements.txt`. The versions match the ones pinned inside the notebooks (the visa notebook pins pandas 1.5.3; the others use 2.0.3).
3. Open `notebook.ipynb` from inside the project folder so the relative `data/` path resolves.

Some notebooks begin with a `!pip install` cell that pins the course environment. It is safe to skip if you have already installed `requirements.txt`.
