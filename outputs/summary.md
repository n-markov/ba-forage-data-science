# Predicting Booking Completion — Summary

## Objective
Predict whether a customer completes a booking (`booking_complete`), and identify which factors drive that outcome, using British Airways' customer booking data.

## Data
- 50,000 bookings, 14 features (passenger count, sales channel, trip type, purchase lead time, length of stay, flight time/route/duration, ancillary preferences).
- Cleaned: removed 719 exact duplicate rows and merged a duplicate country label (`"Czech Republic"`/`"Czechia"`).
- Target is imbalanced: only **15% of bookings complete** (7,478 of 50,000) — accuracy alone is not a meaningful metric here.

## Model
- **RandomForestClassifier** (300 trees, class-balanced) — chosen for its built-in feature importance output, so results can be interpreted, not just predicted.
- Stratified train/test split (80/20) and 5-fold cross-validation, evaluated on ROC-AUC, precision, recall, and F1 rather than accuracy.

## Results

| Metric | Baseline | + Region grouping |
|---|---|---|
| ROC-AUC | 0.752 | 0.753–0.754 |
| Precision | 0.39 | 0.39 |
| Recall | 0.29 | 0.31 |
| F1 | 0.33 | 0.35 |

- ROC-AUC of ~0.75 means: given one random completed booking and one random non-completed booking, the model ranks the completed one higher about 75% of the time — meaningfully better than chance (0.5), but far from perfect separation.
- Precision of ~0.39 is roughly **2.6x better than the ~15% base rate** of guessing at random, but still means most positive predictions are wrong — the data only weakly separates completers from non-completers.
- Adding a `booking_origin` → continent-region feature (in place of relying on country frequency alone) gave a small, consistent improvement, mainly in recall/F1, confirmed on both cross-validation and the held-out test set.

## What drives the model
Top 5 features by importance: `purchase_lead`, `booking_origin_freq`, `route_freq`, `flight_hour`, `length_of_stay` — together these dominate. `trip_type` and `sales_channel` contribute almost nothing.

See `outputs/feature_importance_region.png` for the full ranked chart, and `outputs/numeric_distributions.png` for the underlying feature distributions.

## Takeaway
The available features are **moderately, not strongly, predictive** of booking completion. The model is directionally useful (e.g. for prioritizing follow-up on likely-to-complete bookings), but shouldn't be treated as a reliable yes/no gate on its own. The biggest lever for improvement would likely be richer behavioral/intent data (e.g. session activity, prior booking history) rather than further tuning of the current feature set.
