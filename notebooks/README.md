# Notebook Guide

This repository preserves the complete notebook history of the credit-risk model development. The six notebooks below form the recommended portfolio reading path; the remaining notebooks retain intermediate methodology, diagnostics, and validation evidence for transparency and reproducibility.

## Recommended Reading

1. **Data Preparation & EDA — [Stage 3.1: Train/Test Split & Train-only EDA](02_train_test_split_and_eda.ipynb)**  
   Establishes the frozen Development/Holdout split and performs structured EDA without using Holdout outcomes for development decisions.

2. **Feature Engineering — [Stage 3.3: Feature Engineering & Sanity Checks](03_feature_engineering_and_sanity_checks.ipynb)**  
   Implements and validates the frozen category groupings, delinquency summaries, and utilization features later reused by both model families.

3. **WOE / Binning / Variable Screening — [Stage 4.4: WOE/IV Variable Screening](07_woe_iv_variable_screening.ipynb)**  
   Reconstructs the approved bins, compares IV, correlation, and VIF evidence, and freezes the ten-variable candidate set used by the Logistic pipeline.

4. **Final WOE Logistic Primary Model — [Stage 4.7: Final Logistic Freeze](10_final_woe_logistic_freeze.ipynb)**  
   Records the final Human variable decision, full-Development WOE specification, unpenalized Logistic refit, and frozen Primary handoff.

5. **XGBoost Challenger Development — [Stage 5: XGBoost Challenger Development](11_xgboost_challenger_development.ipynb)**  
   Develops the fixed-feature Challenger using deterministic five-fold validation, the authorized search sequence, and a pre-Holdout freeze record.

6. **Final Holdout Model Comparison — [Stage 5: Final Holdout Comparison](12_final_holdout_model_comparison.ipynb)**  
   Verifies both frozen models and performs their single, same-sample comparison on the sealed Holdout using AUC and KS.

## Supporting Notebooks

The other notebooks remain available as the historical development record. They cover the Luca reference audit, early baseline framing, WOE/IV method design, detailed PAY/categorical and continuous bin construction, candidate Logistic diagnostics, and five-fold Logistic stability. They support deeper technical or model-validation review but are not required for the shortest portfolio reading path.
