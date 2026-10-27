# BoHan_DS_Case_2026

### Kit End Date Prediction
The objective of this case study is to predict the kit cycle completion date using information available when the kit cycle begins. The baseline XGBoost model used predicts build duration, which is then used to estimate the completion date.

#### EDA Findings
1. Near 29% of products had unknown expected build, burn, and kit minutes. Imputation or other practices can be implemented.

2. 5 rows in training data had null download dates.

3. Duplicates in capacity data, with multiple dates having multiple rows.

4. The minimum date in calendar data is later than download dates and kit start dates in training data (for 5 rows).

#### Model Prediction Findings
1. Working Hour Effects

regular_hours accounts for 55.6% of the total feature importance, substantially more than any other feature, which is very intuitive and what we may predict to be the most influential feature.

Overtime hours (ot_hours, 4.1%) and the overtime indicator (ot_flag, 2.4%) also contribute to model gain.

2. Calendar and Seasonal Effects

fiscal_weeknum: 5.2%
day_of_week: 4.7%
calendar_month: 4.4%
fiscal_month: 2.8%
fiscal_qtr_weeknum: 1.8%
is_holiday: 1.7%

These results suggest that kit completion times may vary with the timing of an order, potentially reflecting differences in factory site schedules, working days, or seasonality in workload.

3. Product Feature Effects
Product-related variables such as family_desc (1.7%), family_parent_desc (1.3%), and expected_build_minutes (1.1%) have modest importance scores.

Other variables, including expected_burn_minutes, expected_kit_minutes, quantity_produced, and line_qty, each contribute less than 1%.

4. Some Features Contribute Little or Nothing

site, alt_type_desc, and calendar_year have zero reported importance in this model. They are among 23 features that contribute less than 1%.

These variables may be candidates for further investigation or removal.

#### Recommendations and Next Steps

Investigate regular_hours: Verify how this feature is constructed and whether it contains information available after kit_start. The high importance score can be a sign of data leakage.

Review calendar features: Test whether redundant calendar variables can be removed without reducing validation performance.

Evaluate feature importance with SHAP or permutation importance: Better understand feature effects and confirm whether the ranking is robust.

Analyze prediction errors: Compare model performance across factories, product families, order sizes, and calendar periods to identify business need.

Model validation: Retrain the model after feature selection and compare MAE and RMSE on a time-based validation set.

Explore additional features: If available at prediction time, investigate features such as historical queue length, factory workload, etc.

#### Things to Note

Feature importance does not establish causation, so more investigation needs to be done. As this is an initial baseline case study, more exploration with models, validation, and evaluation metric optimization is needed.

Several variables (especially in product features and calendar data) overlap in the information they represent, so individual feature importance scores should not be interpreted as fully independent effects. Addressing collinearity and feature selection is necessary.
