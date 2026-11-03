# Hotel Booking Cancellation Prediction

## Business problem

A large share of hotel bookings are canceled or become no-shows, and last-minute cancellations are the most costly because the room often cannot be resold. INN Hotels Group wanted a model that predicts which bookings are likely to cancel, along with a view of which factors drive cancellations, so it could design better cancellation and refund policies.

## Data

36,275 bookings with 17 usable features: lead time, average room price, market segment, meal plan, room type, number of adults and children, weekday and weekend nights, parking request, special requests, arrival date, repeat-guest flag, and prior cancellation history. The target is booking status, with roughly one in three bookings canceled.

## Approach

1. **Exploratory analysis** of every feature on its own and against booking status (market segment, special requests, price, lead time, repeat guests).
2. **Preprocessing:** dropped the booking ID, encoded categorical variables, and made a stratified 70/30 train/test split (25,392 / 10,883 rows).
3. **Logistic regression** (statsmodels): checked multicollinearity with VIF, removed high p-value predictors, converted coefficients to odds for interpretation, then tuned the decision threshold using both the ROC curve and the precision-recall curve.
4. **Decision trees:** a default tree, a class-weighted tree, a pre-pruned tree tuned with grid search, and a post-pruned tree using cost-complexity pruning.
5. **Model selection** based on F1 and on the gap between training and test performance.

## Results (test set)

| Model | Accuracy | Recall | Precision | F1 |
|-------|----------|--------|-----------|-----|
| Logistic regression (threshold 0.43) | 0.799 | 0.688 | 0.694 | 0.691 |
| Decision tree, pre-pruned (selected) | 0.832 | 0.769 | 0.732 | 0.750 |
| Decision tree, post-pruned | 0.860 | 0.840 | 0.759 | 0.798 |

The post-pruned tree scores highest on the test set, but it is much deeper and showed a larger train/test gap, so I selected the pre-pruned tree: it generalizes well and is simple enough to explain and implement. The notebook suggests running the two models side by side, treating agreement as high confidence and falling back to the tree when they disagree.

The most important predictors were lead time, online booking channel, number of special requests, and average price per room.

## Recommendations from the analysis

- Lead time is the strongest driver of cancellation. For bookings made far in advance, add a confirmation schedule (for example at 151, 60, and 20 days before arrival).
- Offer discounted rates for early bookings paired with a non-refundable or partially refundable payment at booking, so revenue is secured while the lead-time risk is offset.
- Consider a cancellation fee for bookings canceled within 10 days.
- Focus policy changes on online bookings, the segment most associated with cancellations.
- Collect data over a longer period to test seasonal and yearly effects.

## Files

- `notebook.ipynb`: full analysis with saved outputs
- `data/README.md`: where to put the dataset to re-run the notebook
