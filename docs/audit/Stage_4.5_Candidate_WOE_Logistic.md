# Stage 4.5 — Candidate WOE Logistic Regression Task Report

## Task Identification

- Project: Credit Card Default Risk
- Stage: `Stage 4.5 — Candidate WOE Logistic Regression`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Authorized Notebook: `/Users/yu/2.工作/Risk Analysis Project/notebooks/08_candidate_woe_logistic.ipynb`
- Authorized Task Report: `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_4.5_Candidate_WOE_Logistic.md`
- Implementation status: `COMPLETE — awaiting independent Reviewer validation`
- Reviewer verdict: not prewritten; Reviewer section is reserved below.

## Stage Purpose and Boundaries

Stage 4.5 fits and diagnoses one fixed ten-variable WOE Logistic model on the full Train sample. It assesses simultaneous multivariate coefficient direction, uncertainty, and apparent development-sample discrimination. It does not perform final variable selection, cross-validation, Holdout evaluation, threshold selection, calibration, scorecard construction, or challenger modeling.

## Git Pre-flight Baseline

- Repository root: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD: `afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`
- Parent: `5b36512f317b532378977e66b90f1d379693bc4a`
- HEAD message: `Finalize WOE variable screening`
- Stage 4.4 final Reviewer verdict: `PASS — final Stage 4.4 milestone accepted after independent revalidation of the documentation/provenance repair.`
- Pre-flight `git status --short`: empty.
- Pre-flight tracked diff and stat: empty.
- Pre-flight staged diff: empty.
- Pre-flight untracked list: empty.
- Pre-flight remotes: none.
- Existing Stage 4.5 artifacts: none.

No restore, reset, stash, clean, checkout, amend, rebase, add, commit, fetch, pull, push, or remote operation was performed.

## Protected Inputs — Pre-flight Fingerprints

The within-run baseline used raw file bytes, file size, and SHA-256 for all 24 tracked files before either Stage 4.5 artifact was created.

| Protected tracked input | Pre-flight bytes | Pre-flight SHA-256 |
|---|---:|---|
| `.gitignore` | 534 | `300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1` |
| `README.md` | 2,245 | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` |
| `data/README.md` | 1,004 | `c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0` |
| `data/raw/UCI_Credit_Card.csv` | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |
| `notebooks/01_baseline_replication.ipynb` | 1,057 | `ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20` |
| `notebooks/01_luca_audit_and_data_understanding.ipynb` | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` |
| `notebooks/02_train_test_split_and_eda.ipynb` | 937,648 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` |
| `notebooks/03_feature_engineering_and_sanity_checks.ipynb` | 142,371 | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` |
| `notebooks/04_woe_iv_binning_method_design.ipynb` | 52,998 | `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9` |
| `notebooks/05_pay_categorical_binning.ipynb` | 971,553 | `79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796` |
| `notebooks/06_continuous_binning.ipynb` | 1,901,709 | `97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b` |
| `notebooks/07_woe_iv_variable_screening.ipynb` | 850,894 | `1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce` |
| `references/README.md` | 1,109 | `a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c` |
| `reports/figures/.gitkeep` | 60 | `73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63` |
| `requirements.txt` | 95 | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` |
| `task_records/Stage_1_Project_Foundation.md` | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` |
| `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md` | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` |
| `task_records/Stage_3.1_Train_Test_Split_and_EDA.md` | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` |
| `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md` | 77,812 | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` |
| `task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md` | 50,459 | `2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851` |
| `task_records/Stage_4.2_PAY_Categorical_Binning.md` | 102,407 | `9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60` |
| `task_records/Stage_4.3_Continuous_Variable_Binning.md` | 154,540 | `a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5` |
| `task_records/Stage_4.4_WOE_IV_Variable_Screening.md` | 185,676 | `637fd696317a9f18620052dbb45a140e8aa8a196e9ed36767091d8e225fcc288` |
| `task_records/TASK_REPORT_Task_0.1_Current-State_Audit.md` | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` |

Additional protected local reference:

- `references/credit-card-default-a-very-pedagogical-notebook.ipynb`: 805,686 bytes; SHA-256 `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd`.
- Git index baseline: 2,845 bytes; SHA-256 `25179533fa26d7f039315aca9a7b7a2c15660f1a684ed2747f60638f672146e7`.

Relevant frozen sources were inspected read-only before implementation:

- Stage 3 feature engineering: `notebooks/03_feature_engineering_and_sanity_checks.ipynb` — SHA-256 `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256`.
- Stage 4.2 PAY/categorical binning: `notebooks/05_pay_categorical_binning.ipynb` — SHA-256 `79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796`.
- Stage 4.3 continuous binning: `notebooks/06_continuous_binning.ipynb` — SHA-256 `97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b`.
- Stage 4.4 screening Notebook: `notebooks/07_woe_iv_variable_screening.ipynb` — SHA-256 `1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce`.
- Stage 4.4 Task Report: `task_records/Stage_4.4_WOE_IV_Variable_Screening.md` — SHA-256 `637fd696317a9f18620052dbb45a140e8aa8a196e9ed36767091d8e225fcc288`.

## Environment Verification

No package was installed, upgraded, removed, or otherwise changed.

- Project `.venv` metadata check: Python 3.9.6; pandas 2.3.3; numpy 2.0.2; scipy 1.13.1; scikit-learn 1.6.1; matplotlib 3.9.4; seaborn 0.13.2; `statsmodels` not installed in this interpreter.
- Existing project Jupyter `python3` kernel used by the Stage 4 notebooks and this Stage 4.5 clean-run: `/opt/anaconda3/bin/python`, Python 3.11.7.
- Kernel packages: pandas 2.1.4; numpy 1.26.4; scipy 1.11.4; scikit-learn 1.2.2; matplotlib 3.8.0; seaborn 0.12.2; statsmodels 0.14.0.
- Model implementation therefore used the already-installed `statsmodels.Logit`; no requirements or environment file was modified.

## Frozen Split Identity and Test Sealing

| Check | Actual | Result |
|---|---|---|
| Raw rows | 30,000 | PASS |
| Train rows | 24,000 | PASS |
| Test rows | 6,000 | PASS — technical identity only |
| Train ID SHA-256 | `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` | PASS — exact |
| Test ID SHA-256 | `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f` | PASS — exact |
| ID overlap | 0 | PASS |
| ID union coverage | complete | PASS |
| Train Good / Bad | 18,691 / 5,309 | PASS |
| Train bad rate | 22.120833% | PASS |
| Test seal | `SEALED — Test deleted after row/ID/union/hash identity checks only` | PASS |

The Test object was deleted immediately after row count, ID overlap, ID union coverage, and ID-hash checks. No Test predictor value, target distribution, WOE, bin occupancy, probability, score, risk relationship, AUC, KS, calibration, ROC, or threshold metric was inspected.

## Frozen Candidate Predictor List

Canonical order, unchanged from Stage 4.4:

1. `PAY_0`
2. `severe_delinquent_months`
3. `PAY_AMT1`
4. `PAY_AMT2`
5. `PAY_AMT3`
6. `PAY_AMT4`
7. `PAY_AMT5`
8. `PAY_AMT6`
9. `LIMIT_BAL`
10. `avg_utilization`

No predictor was added, deleted, substituted, or reordered.

## Frozen Feature, Binning, and WOE Reconstruction

- `severe_delinquent_months = Σ 1(PAY_i >= 2)` over `PAY_0`, `PAY_2`, ..., `PAY_6`.
- `avg_utilization = mean(max(BILL_AMT_i, 0)) / LIMIT_BAL` over six billing months; values above 1 remain uncapped.
- PAY_0, severe-delinquency, PAY_AMT1–6, LIMIT_BAL, and avg-utilization bins exactly reproduce the frozen Stage 4.2/4.3 mappings.
- The final IV of each candidate was mechanically checked against the Stage 4.4 frozen full-Train IV anchor at absolute tolerance `5e-12`.
- WOE convention: `ln(DistGood / DistBad)`, Good = target 0, Bad = target 1.
- Positive WOE is safer. With Logistic target `Default=1`, the expected coefficient sign is negative.
- WOE is estimated on full Train under the frozen bins; all bins contain both classes, so smoothing is not required and was not used.

| WOE matrix check | Result |
|---|---|
| Shape | `24,000 × 10` |
| Canonical order | PASS |
| Missing | 0 |
| Non-finite | 0 |
| Row alignment | PASS |
| ID alignment | PASS |
| Target alignment | PASS |
| Deterministic exact rebuild | PASS |
| Smoothing | none |

## Model Specification and Rationale

Exactly one substantive predictive model is present: a fixed ten-variable `statsmodels.Logit` with an explicit intercept, fitted by unpenalized maximum likelihood on the natural 24,000-row Train sample. Unpenalized MLE is appropriate here because Stage 4.5 is diagnostic and requires direct coefficients, standard errors, z-statistics, p-values, confidence intervals, and a covariance matrix. No regularization, class weighting, resampling, subset search, or alternative model was used.

## Convergence and Numerical Sanity

| Check | Actual | Result |
|---|---:|---|
| Converged | True | PASS |
| Newton iterations | 6 | recorded |
| Intercept | -1.254276175 | finite |
| Design rank | 11 of 11 | PASS |
| All coefficients | finite | PASS |
| All standard errors | finite and positive | PASS |
| Covariance | finite, symmetric, positive definite | PASS |
| Covariance minimum eigenvalue | 0.000187694824845 | positive |
| Covariance condition number | 39.626826 | finite |
| Predicted probability range | [0.038073398, 0.867665478] | finite, strictly inside (0,1) |
| Singular-matrix failure | absent | PASS |
| Perfect/quasi-separation warning | absent | PASS |
| Material overflow/underflow | absent | PASS |
| Optimizer/model warnings | none | PASS |
| Inferential validity | numerically valid | PASS |

## Coefficient, Uncertainty, and Odds-Ratio Interpretation

- Coefficient: conditional change in default log-odds for one WOE-unit increase, holding the other nine WOE predictors constant.
- Standard error: estimated coefficient uncertainty.
- `z = coefficient / standard error`; z supports the p-value and is not a deletion rule.
- p-value: descriptive inferential evidence; `p > 0.05` does not imply Drop.
- 95% CI: direction/uncertainty evidence used only for the deterministic diagnostic status.
- `OR = exp(coefficient)`: multiplicative change in default odds for one WOE-unit increase, not the same percentage change in default probability and not a selection criterion.

| Predictor | Coefficient | Expected Sign | Sign Check | Standard Error | z-statistic | p-value | 95% CI Lower | 95% CI Upper | Odds Ratio | Full-Train Diagnostic Status | Stage 4.6 Follow-up Note |
|---|---:|---|---|---:|---:|---:|---:|---:|---:|---|---|
| PAY_0 | -0.556304 | Negative | Expected | 0.023615 | -23.5571 | 1.06187e-122 | -0.602588 | -0.510019 | 0.573324 | Directionally Clear | Continue to Stage 4.6 — verify current vs persistent delinquency stability. |
| severe_delinquent_months | -0.513060 | Negative | Expected | 0.024386 | -21.0389 | 2.88949e-98 | -0.560856 | -0.465264 | 0.598661 | Directionally Clear | Continue to Stage 4.6 — verify current vs persistent delinquency stability. |
| PAY_AMT1 | -0.193756 | Negative | Expected | 0.048841 | -3.9671 | 7.27532e-05 | -0.289481 | -0.098030 | 0.823859 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. |
| PAY_AMT2 | -0.222093 | Negative | Expected | 0.054644 | -4.0643 | 4.81711e-05 | -0.329194 | -0.114992 | 0.800841 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. |
| PAY_AMT3 | -0.245401 | Negative | Expected | 0.062021 | -3.9568 | 7.59746e-05 | -0.366960 | -0.123843 | 0.782390 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. |
| PAY_AMT4 | -0.165042 | Negative | Expected | 0.072426 | -2.2788 | 0.0226804 | -0.306993 | -0.023090 | 0.847858 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. |
| PAY_AMT5 | -0.097122 | Negative | Expected | 0.078560 | -1.2363 | 0.21635 | -0.251096 | 0.056851 | 0.907445 | Review in Stage 4.6 | Continue to Stage 4.6 — prioritize conditional-contribution stability; no deletion. |
| PAY_AMT6 | -0.164349 | Negative | Expected | 0.078065 | -2.1053 | 0.0352658 | -0.317353 | -0.011345 | 0.848446 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. |
| LIMIT_BAL | -0.226161 | Negative | Expected | 0.044958 | -5.0305 | 4.89151e-07 | -0.314276 | -0.138045 | 0.797590 | Directionally Clear | Continue to Stage 4.6 — verify coefficient and uncertainty stability. |
| avg_utilization | -0.405621 | Negative | Expected | 0.052466 | -7.7312 | 1.0656e-14 | -0.508452 | -0.302790 | 0.666563 | Directionally Clear | Continue to Stage 4.6 — verify coefficient and uncertainty stability. |

## Diagnostic Status Rules and Results

- `Directionally Clear`: coefficient < 0 and 95% CI entirely below 0.
- `Review in Stage 4.6`: coefficient non-positive and 95% CI includes 0, or effectively near-zero/weak inferential evidence.
- `Sign Concern`: coefficient > 0, with priority over ordinary Review status.

Observed counts:

- Expected negative signs: 10 of 10.
- Positive coefficients / Sign Concerns: 0.
- Directionally Clear: 9.
- Review in Stage 4.6: 1.
- Coefficient CIs crossing zero: 1.

These are observational flags only. No Keep/Drop decision is introduced.

## Family Findings

### PAY_0 and severe_delinquent_months

- `PAY_0`: coefficient -0.556304, 95% CI [-0.602588, -0.510019], p-value 1.06187e-122; Directionally Clear.
- `severe_delinquent_months`: coefficient -0.513060, 95% CI [-0.560856, -0.465264], p-value 2.88949e-98; Directionally Clear.
- Both retain the expected negative direction when entered together. Stage 4.6 should still verify fold stability of current delinquency versus persistence; neither is removed.

### PAY_AMT1–6

- `PAY_AMT1`, `PAY_AMT2`, `PAY_AMT3`, `PAY_AMT4`, and `PAY_AMT6` are Directionally Clear with negative coefficients and CIs below zero.
- `PAY_AMT5` remains negative (-0.097122) but its 95% CI [-0.251096, 0.056851] crosses zero and its p-value is 0.21635; status `Review in Stage 4.6`.
- All six continue. No monthly cutoff, average, total, trend, or replacement representation was created.

### LIMIT_BAL and avg_utilization

- `LIMIT_BAL`: coefficient -0.226161, 95% CI [-0.314276, -0.138045], p-value 4.89151e-07; Directionally Clear.
- `avg_utilization`: coefficient -0.405621, 95% CI [-0.508452, -0.302790], p-value 1.0656e-14; Directionally Clear.
- Both retain expected conditional direction. Dropped BILL/current/max-utilization representations were not reopened.

## Figures

- `LOG-01 — Candidate WOE Logistic coefficient forest plot`: present inline; all ten point estimates, 95% CIs, and zero reference line; no Keep/Drop encoding.
- `LOG-02 — Apparent full-Train ROC curve`: present inline; candidate model AUC and random baseline; explicitly apparent/development-sample.
- `LOG-03 — Apparent full-Train KS diagnostic`: present inline; TPR and FPR across probability thresholds with maximum separation highlighted.

No external PNG, HTML, PDF, CSV, model, pickle, joblib, source, or test artifact was created.

## Apparent Full-Train Performance Diagnostics

- Apparent development-sample AUC: `0.772417098`.
- Apparent development-sample KS: `0.430307867`.
- KS definition: maximum `TPR − FPR`, with `Default=1` and predicted default probability as score.
- KS-max threshold: `0.192435439`; diagnostic location only, not an operational or optimized threshold.

These values are not estimates of generalization performance because the frozen bins, WOE values, and Logistic model were developed on the same Train sample. They were not used for feature deletion or alternative-model search.

## Performance-Evidence Hierarchy

`Stage 4.5 full-Train apparent → Stage 4.6 out-of-fold stability/predictive evidence → Stage 5 sealed Holdout evaluation`

Stage 4.5 does not make a final expected-performance claim.

## Stage 4.6 Handoff Table

| Predictor | Full-Train Coefficient | Sign | 95% CI | p-value | Stage 4.5 Diagnostic Status | Stage 4.6 Follow-up Priority / Note | Continuation |
|---|---:|---|---|---:|---|---|---|
| PAY_0 | -0.556304 | Negative | [-0.602588, -0.510019] | 1.06187e-122 | Directionally Clear | Continue to Stage 4.6 — verify current vs persistent delinquency stability. | Continue to Stage 4.6 |
| severe_delinquent_months | -0.513060 | Negative | [-0.560856, -0.465264] | 2.88949e-98 | Directionally Clear | Continue to Stage 4.6 — verify current vs persistent delinquency stability. | Continue to Stage 4.6 |
| PAY_AMT1 | -0.193756 | Negative | [-0.289481, -0.098030] | 7.27532e-05 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. | Continue to Stage 4.6 |
| PAY_AMT2 | -0.222093 | Negative | [-0.329194, -0.114992] | 4.81711e-05 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. | Continue to Stage 4.6 |
| PAY_AMT3 | -0.245401 | Negative | [-0.366960, -0.123843] | 7.59746e-05 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. | Continue to Stage 4.6 |
| PAY_AMT4 | -0.165042 | Negative | [-0.306993, -0.023090] | 0.0226804 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. | Continue to Stage 4.6 |
| PAY_AMT5 | -0.097122 | Negative | [-0.251096, 0.056851] | 0.21635 | Review in Stage 4.6 | Continue to Stage 4.6 — prioritize conditional-contribution stability; no deletion. | Continue to Stage 4.6 |
| PAY_AMT6 | -0.164349 | Negative | [-0.317353, -0.011345] | 0.0352658 | Directionally Clear | Continue to Stage 4.6 — verify monthly-payment coefficient stability. | Continue to Stage 4.6 |
| LIMIT_BAL | -0.226161 | Negative | [-0.314276, -0.138045] | 4.89151e-07 | Directionally Clear | Continue to Stage 4.6 — verify coefficient and uncertainty stability. | Continue to Stage 4.6 |
| avg_utilization | -0.405621 | Negative | [-0.508452, -0.302790] | 1.0656e-14 | Directionally Clear | Continue to Stage 4.6 — verify coefficient and uncertainty stability. | Continue to Stage 4.6 |

All ten are marked `Continue to Stage 4.6`. This is not a final variable freeze and contains no Keep/Drop decision.

## Hard-Boundary Confirmation

- No automatic deletion based on sign, p-value, CI, z, OR, AUC, or KS.
- No alternative subset or raw-variable Logistic model.
- No regularization or penalty tuning.
- No class weighting, SMOTE, oversampling, or undersampling.
- No threshold optimization; KS threshold is diagnostic only.
- No calibration analysis.
- No KFold, StratifiedKFold, CV, bootstrap, or OOF predictions.
- No Test predictive analysis.
- No final variable freeze.
- No Stage 4.6 or Stage 5 work.

## Notebook Execution and Verification

- Notebook structure: 33 cells, including 16 code cells.
- Required tables and exactly three required figures are inline.
- The final clean-run evidence, final Notebook fingerprint, Task Report fingerprint, protected-input comparison, immutable-block comparison, and final Git state are recorded in the post-implementation verification section below.

## Implementation Notes, Warnings, Deviations, and Limitations

- Model-fit warnings: none.
- Environment change: none. The existing Python 3.11.7 Jupyter kernel supplied `statsmodels 0.14.0`; the project `.venv` itself does not contain statsmodels, and no installation was attempted.
- The first sandboxed clean-run attempt was blocked before kernel start because local Jupyter ports were not permitted. The authorized local-kernel run was then used.
- One execution attempt reached the final summary cell and stopped on an unmatched display parenthesis in that newly created cell. Only the new Stage 4.5 Notebook line was corrected; predictors, bins, WOE values, model form, model results, and protected inputs were not changed. The failed run was not saved as final evidence.
- The only analytical limitation is intentional: AUC and KS are apparent full-Train diagnostics, not cross-validated or Holdout performance.
- Incomplete by design: independent Reviewer validation and the Reviewer-owned milestone commit remain pending.
- Analytical deviations from the immutable specification: none.

## Acceptance Criteria Check

1. [x] Correct project root
2. [x] Branch `main`
3. [x] HEAD `afe8057a…`
4. [x] Clean Stage 4.4 baseline
5. [x] Only two authorized Stage 4.5 artifacts
6. [x] Frozen split reproduced
7. [x] Train = 24,000
8. [x] Test = 6,000
9. [x] Train ID hash exact
10. [x] Test ID hash exact
11. [x] Test sealed
12. [x] Good = 18,691
13. [x] Bad = 5,309
14. [x] Exact ten predictors
15. [x] Canonical order preserved
16. [x] Frozen Stage 4 binning preserved
17. [x] No new feature engineering
18. [x] WOE convention = ln(DistGood / DistBad)
19. [x] 24,000×10 WOE matrix
20. [x] No missing WOE
21. [x] No non-finite WOE
22. [x] Exact target/row alignment
23. [x] No smoothing required
24. [x] Exactly one substantive predictive model
25. [x] Unpenalized MLE Logistic
26. [x] Intercept included
27. [x] Model converged
28. [x] Coefficients finite
29. [x] Standard errors finite
30. [x] Inference numerically valid
31. [x] Coefficient table complete
32. [x] Expected negative sign documented
33. [x] Sign Check for all ten
34. [x] z-statistics reported
35. [x] p-values reported
36. [x] 95% CIs reported
37. [x] ORs reported
38. [x] OR interpreted as odds rather than probability
39. [x] No z-based elimination
40. [x] No p-value-based elimination
41. [x] No sign-based elimination
42. [x] Deterministic diagnostic statuses
43. [x] LOG-01 produced
44. [x] PAY_0 / severe_delinquent_months reviewed
45. [x] PAY_AMT1–6 reviewed
46. [x] LIMIT_BAL / avg_utilization reviewed
47. [x] Apparent Train AUC computed
48. [x] AUC labeled apparent
49. [x] LOG-02 produced
50. [x] Apparent Train KS computed
51. [x] KS definition documented
52. [x] LOG-03 produced
53. [x] No feature selection from AUC/KS
54. [x] No alternate subset model
55. [x] No regularization
56. [x] No class weighting
57. [x] No SMOTE/resampling
58. [x] No threshold optimization
59. [x] No calibration analysis
60. [x] No CV
61. [x] No Test predictive analysis
62. [x] No final variable freeze
63. [x] All ten continue to Stage 4.6
64. [x] Stage 4.6 handoff table
65. [x] Notebook clean-runs
66. [x] Task Report complete
67. [x] Architect Specification preserved verbatim
68. [x] Protected tracked files unchanged
69. [x] Staged diff empty
70. [x] No commit
71. [x] No push
72. [x] Final Reviewer handoff reached

## Final Post-Implementation Verification

### Artifact scope

- Created: `notebooks/08_candidate_woe_logistic.ipynb`.
- Created: `task_records/Stage_4.5_Candidate_WOE_Logistic.md`.
- Modified protected or prior-stage files: none.
- Other Stage 4.5 project artifacts: none.
- External CSV, PNG, HTML, PDF, model, pickle, joblib, source, or test artifacts: none.
- Both authorized artifacts remain untracked. No third artifact exists.

### Final Notebook verification

- Final Notebook size: 316,111 bytes.
- Final Notebook SHA-256: `48b79b025c31781fc4d38baa07778b67b5c2241e579237741f90e2921c7cf072`.
- Notebook JSON: valid nbformat 4.5; 33 cells; all 33 cells have IDs.
- Clean-run: PASS in a fresh `python3` kernel from the raw CSV.
- Code cells: 16; execution counts exactly 1 through 16.
- Error outputs / tracebacks: 0.
- Inline PNG outputs: 3, corresponding exactly to LOG-01, LOG-02, and LOG-03.
- Visual QA: PASS for all three figures; LOG-03 displays only valid probability thresholds in `[0,1]` and excludes the ROC implementation's initial sentinel threshold without altering KS.
- Substantive model calls: one `statsmodels.Logit` construction and one `.fit` call; no sklearn Logistic and no alternate model.
- Frozen IV-anchor checks: all ten matched Stage 4.4 within absolute tolerance `5e-12`.
- Split hashes, Test sealing, 24,000×10 WOE integrity, model convergence, inferential validity, coefficient table, apparent metrics, and all-ten Stage 4.6 handoff assertions: PASS.

### Immutable-specification exact comparison

- Authoritative source markers: one standalone BEGIN and one standalone END, correctly ordered.
- Embedded Task Report markers: one standalone BEGIN and one standalone END, correctly ordered.
- Encoding: UTF-8; source raw LF bytes; no Unicode, newline, whitespace, or semantic normalization.
- Boundary: exact unique standalone BEGIN marker through exact unique standalone END marker inclusive, excluding a following line terminator.
- Authoritative block: 28,332 bytes; SHA-256 `f90a59ee9c28938efcfae11cc5198181a5c0beeff6e8f1eff4315e975d6d7b67`.
- Embedded block: 28,332 bytes; SHA-256 `f90a59ee9c28938efcfae11cc5198181a5c0beeff6e8f1eff4315e975d6d7b67`.
- Byte-for-byte equality: PASS.
- Execution instruction inserted inside the immutable block: none.

### Protected-input comparison

- All 24 tracked protected inputs exactly match the pre-flight file sizes and SHA-256 values.
- Pre-flight and final 24-file manifest SHA-256 are both `c00b1b3c35523f1cca5a128acfbf46eb9dbe563d19f85cd0d4f62b8076cdb553`.
- Luca local reference Notebook remains 805,686 bytes with SHA-256 `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd`.
- Git index remains 2,845 bytes with SHA-256 `25179533fa26d7f039315aca9a7b7a2c15660f1a684ed2747f60638f672146e7`.
- Raw CSV, Stage 0–4.4 notebooks/reports, README, requirements, references, and every other tracked file remain unchanged.
- Package/Python environment changes: none.
- Prior Notebook execution: none; only the new Stage 4.5 Notebook was executed.

### Final Git state

- Repository root: `/Users/yu/2.工作/Risk Analysis Project`.
- Branch: `main`.
- HEAD: `afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`.
- Parent: `5b36512f317b532378977e66b90f1d379693bc4a`.
- HEAD message: `Finalize WOE variable screening`.
- `git status --short --untracked-files=all`: exactly the two authorized untracked Stage 4.5 artifacts.
- `git diff --name-status`: empty.
- `git diff --stat`: empty.
- `git diff --cached --name-status`: empty.
- `git ls-files --others --exclude-standard`: exactly the two authorized Stage 4.5 artifacts.
- `git remote -v`: empty.
- Stage, commit, history, and remote changes: none.
- No `git add`, commit, or push was performed.

Final implementation conclusion: the fixed ten-variable model is numerically valid, all ten predictors remain candidates for Stage 4.6, and this artifact pair is awaiting independent Reviewer validation.

## Immutable Architect Specification

The following marked block is the authoritative Stage 4.5 source copied as raw UTF-8 text without normalization. Its mechanically derived boundary fingerprint is 28,332 bytes and SHA-256 `f90a59ee9c28938efcfae11cc5198181a5c0beeff6e8f1eff4315e975d6d7b67`. The boundary is the unique standalone BEGIN marker through the unique standalone END marker, inclusive, excluding a following line terminator.

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.5 ================

# Stage 4.5 — Candidate WOE Logistic Regression

## 1. Stage Context

Current Stage 4 structure:

- Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
- Stage 4.2 — PAY + Categorical Binning ✅ CLOSED
- Stage 4.3 — Continuous Variable Binning ✅ CLOSED
- Stage 4.4 — WOE / IV Variable Screening ✅ CLOSED
- Stage 4.5 — Candidate WOE Logistic Regression ← CURRENT
- Stage 4.6 — Stratified 5-Fold Model Stability
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit

Stage 4.4 final Reviewer verdict:

> PASS

Stage 4.4 milestone commit:

`afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`

Short:

`afe8057`

Commit message:

`Finalize WOE variable screening`

Parent:

`5b36512f317b532378977e66b90f1d379693bc4a`

Branch:

`main`

Stage 4.4 post-commit working tree was clean.

No push was performed.

---

# 2. Stage 4.5 Objective

Stage 4.5 answers one central question:

> How do the 10 Human-approved WOE predictors behave when they enter one multivariate Logistic Regression simultaneously?

Stage 4.5 is the first predictive Logistic model in the project.

It is NOT:

- another Stage 4.4 feature-screening round;
- final variable selection;
- cross-validation;
- holdout evaluation;
- scorecard construction;
- challenger modeling.

The purpose is:

1. fit one fixed 10-variable unpenalized WOE Logistic model on full Train;
2. confirm numerical/model-fit sanity;
3. inspect multivariate coefficient behavior;
4. inspect coefficient direction under the project's WOE convention;
5. quantify coefficient uncertainty;
6. identify variables that require stability attention in Stage 4.6;
7. establish apparent full-Train AUC / KS baseline;
8. carry all 10 predictors forward to Stage 4.6 unless the model itself is numerically invalid.

---

# 3. Frozen Candidate Predictor List

Use exactly the Stage 4.4 frozen list, in this canonical order:

1. `PAY_0`
2. `severe_delinquent_months`
3. `PAY_AMT1`
4. `PAY_AMT2`
5. `PAY_AMT3`
6. `PAY_AMT4`
7. `PAY_AMT5`
8. `PAY_AMT6`
9. `LIMIT_BAL`
10. `avg_utilization`

Exactly 10 predictors.

No additions.

No deletions.

No substitutions.

No alternative representation.

No reordering in canonical model tables unless technically necessary.

---

# 4. Stage 4.4 Validation Baseline

Stage 4.4 already established that the 10-variable candidate set is acceptably nonredundant in WOE space.

Verified reduced-set results:

- Shape = `24,000 × 10`
- Missing = 0
- Non-finite = 0
- maximum absolute off-diagonal WOE correlation = `0.692029`
- number of pairs with `|r| >= 0.70` = 0
- number of pairs with `|r| >= 0.85` = 0
- all 10 VIF = Low
- maximum VIF = `2.040957`

Therefore Stage 4.5 must NOT repeat feature elimination based on:

- IV;
- correlation;
- VIF.

Those screening decisions are closed.

---

# 5. New Stage 4.5 Artifacts

Create exactly:

`notebooks/08_candidate_woe_logistic.ipynb`

and:

`task_records/Stage_4.5_Candidate_WOE_Logistic.md`

Do not modify the Stage 4.4 Notebook or Task Report.

Do not modify any prior tracked artifact.

Do not create additional model/data files.

---

# 6. Git Pre-flight

Expected project root:

`/Users/yu/2.工作/Risk Analysis Project`

Expected:

- branch = `main`
- HEAD =
  `afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`
- commit message =
  `Finalize WOE variable screening`
- staged diff empty
- tracked working-tree diff empty
- no remote
- no pre-existing Stage 4.5 artifacts unless they are clearly authorized unfinished artifacts from the current execution context

If unrelated tracked changes exist:

> BLOCKED.

Do not automatically:

- restore;
- reset;
- stash;
- clean;
- checkout;
- amend;
- rebase.

---

# 7. Frozen Train / Test Split

Replay the existing split exactly from:

`data/raw/UCI_Credit_Card.csv`

Required:

- Raw = 30,000
- Train = 24,000
- Test = 6,000
- `random_state=42`
- stratify on `default.payment.next.month`

Train ID SHA-256:

`bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`

Test ID SHA-256:

`d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

Expected Train target:

- Good = 18,691
- Bad = 5,309
- Bad rate ≈ 22.1208%

Test may be used only for technical identity checks:

- row count;
- ID overlap;
- ID union coverage;
- frozen ID hash.

Then seal/delete the Test object.

Do not inspect:

- Test predictor values;
- Test WOE;
- Test bin occupancy;
- Test target distribution;
- Test probabilities;
- Test scores;
- Test AUC;
- Test KS;
- Test calibration;
- any Test risk relationship.

Holdout remains sealed until Stage 5.

---

# 8. Frozen Feature Engineering and Binning

Reproduce only the already-approved deterministic feature engineering required for the 10 variables.

Do not invent new features.

Use the frozen Stage 3 / Stage 4 feature definitions and bin structures.

Relevant frozen predictors include:

- PAY\_0
- severe\_delinquent\_months
- PAY\_AMT1–6
- LIMIT\_BAL
- avg\_utilization

Definition:

[
severe\_delinquent\_months
==========================

\sum\_i 1(PAY\_i \ge 2)
]

Definition:

[
avg\_utilization
================

\frac{
mean(max(BILL\_AMT1,0),\ldots,max(BILL\_AMT6,0))
}{
LIMIT\_BAL
}
]

No cap on utilization above 1.

Do not change any Stage 4.2 / 4.3 frozen bins.

---

# 9. WOE Convention — CRITICAL

The project uses:

[
WOE\_j
======

\ln
\frac{DistGood\_j}{DistBad\_j}
]

where:

- Good = target 0
- Bad = target 1

Therefore:

[
WOE > 0
]

means relatively safer.

And:

[
WOE < 0
]

means relatively riskier.

The Logistic target is:

[
Default=1
]

Therefore the expected coefficient direction for a conventionally behaving WOE predictor is:

[
\boxed{\beta\_j < 0}
]

because higher/safer WOE should reduce default log-odds.

This expected sign applies to all 10 predictors.

---

# 10. Build Exact 24,000 × 10 WOE Matrix

Construct the full-Train WOE matrix for exactly the 10 frozen predictors.

Requirements:

- Shape = `24,000 × 10`
- exact canonical column order
- all finite
- no missing values
- row alignment preserved
- target alignment preserved
- same frozen bin structures as Stage 4.4
- WOE estimated on full Train under the frozen bins
- no smoothing unless a frozen prior-stage rule unexpectedly requires it

The Stage 4.4 final bins were already verified to avoid zero-class issues.

Expected:

> no smoothing required.

Do not recreate the 29-variable screening exercise.

Stage 4.5 needs only the frozen 10-variable candidate matrix.

---

# 11. Model Form

Fit exactly one model:

[
\log
\frac{P(Default=1)}
{1-P(Default=1)}
================

\beta\_0
\+
\sum\_{j=1}^{10}
\beta\_j WOE\_j
]

Use:

> unpenalized maximum-likelihood Logistic Regression.

Preferred implementation:

`statsmodels` Logit or an equivalent unpenalized MLE implementation that exposes standard inferential statistics.

Include an intercept.

Do NOT use a default sklearn penalized Logistic without explicitly removing the penalty.

---

# 12. Why Unpenalized Logistic

The purpose of Stage 4.5 is model diagnosis and interpretability.

The project needs direct estimates of:

- coefficient;
- standard error;
- z-statistic;
- p-value;
- confidence interval.

Regularization would intentionally shrink coefficients and could obscure their unpenalized multivariate behavior.

The candidate model has:

- n = 24,000
- p = 10
- low reduced-set VIF
- no Stage 4.4 high-correlation flags

Therefore no regularization is required for this diagnostic stage.

Do not use:

- L1;
- L2;
- Elastic Net;
- automatic shrinkage.

---

# 13. Model Fit Sanity Check

Before interpreting coefficients, verify:

- model converged;
- intercept finite;
- all 10 coefficients finite;
- all standard errors finite;
- covariance matrix usable;
- predicted probabilities finite;
- predicted probabilities in valid probability range;
- no singular-matrix failure;
- no perfect-separation failure;
- no quasi-separation warning that invalidates standard inference;
- no numerical overflow/underflow that compromises results.

Record convergence and any optimizer/model warnings explicitly.

If the model does not converge or inferential statistics are invalid:

> BLOCKED.

Do not change predictors automatically.

Return evidence to Architect.

---

# 14. Core Coefficient Diagnostic Table

Produce one canonical 10-row table in frozen predictor order with:

- Predictor
- Coefficient
- Expected Sign
- Sign Check
- Standard Error
- z-statistic
- p-value
- 95% CI Lower
- 95% CI Upper
- Odds Ratio
- Full-Train Diagnostic Status
- Stage 4.6 Follow-up Note

Use:

[
OR=e^\beta
]

Odds Ratio is auxiliary only.

Do not treat OR as a new feature-selection criterion.

---

# 15. Role of z-statistic

Use:

[
z
=

\frac{\beta}{SE(\beta)}
]

z-statistic is standard inferential output.

It indicates how many standard errors the coefficient estimate lies from zero.

It mainly supports the p-value.

Do not create an independent z-based feature-elimination rule.

---

# 16. Role of Odds Ratio

Use:

[
OR=e^\beta
]

Interpretation:

> multiplicative change in default odds for a one-unit increase in the WOE predictor, holding the other predictors constant.

Because the unit is one WOE unit, this is less directly intuitive than an odds ratio on an ordinary raw business variable.

Therefore:

- report OR;
- explain it correctly;
- do not overemphasize it;
- do not use OR as an automatic selection criterion.

Do not claim OR represents the same percentage change in default probability.

---

# 17. Sign Sanity Check

Expected sign for every predictor:

> Negative.

Classify:

## Expected

[
\beta < 0
]

## Sign Concern

[
\beta > 0
]

If coefficient is numerically equal or effectively indistinguishable from zero, treat it as a Review case rather than claiming a meaningful direction.

A positive coefficient means:

> after controlling for the other nine predictors, a higher/safer WOE is associated with higher predicted default log-odds.

This conflicts with the univariate WOE direction and requires Stage 4.6 attention.

Do NOT automatically drop a Sign Concern variable.

Sign reversal can arise from multivariate overlap or suppression.

It must be investigated through stability analysis.

---

# 18. Conditional Contribution Interpretation

Stage 4.4 asked:

> Does this predictor have useful standalone information and an acceptable redundancy profile?

Stage 4.5 now asks:

> After controlling for the other nine predictors, what conditional information does this predictor retain?

Therefore inspect:

- coefficient direction;
- coefficient magnitude;
- standard error;
- confidence interval;
- p-value.

Do not confuse:

> weak multivariate conditional contribution

with:

> weak Stage 4.4 standalone IV.

A variable can have useful IV but limited conditional contribution once related predictors enter the same model.

---

# 19. Full-Train Diagnostic Status

Do NOT assign Keep / Drop.

Use diagnostic status only.

Recommended deterministic classification:

## Directionally Clear

Use when:

- coefficient < 0;
- 95% confidence interval is entirely below 0.

This means full-Train multivariate evidence is directionally clear.

It does NOT prove cross-sample stability.

## Review in Stage 4.6

Use when:

- coefficient is non-positive;
- but 95% CI includes 0;

or the coefficient estimate is effectively near zero / statistically weak.

Do not create a new arbitrary coefficient-magnitude cutoff.

The CI/p-value evidence should drive the formal Review flag.

## Sign Concern

Use when:

- coefficient > 0.

Sign Concern has priority over ordinary Review status.

Even if statistically non-significant, record that its point estimate is directionally unexpected.

All diagnostic statuses are:

> observational flags only.

None cause automatic deletion.

---

# 20. p-value Policy

Report p-values.

Use conventional 0.05 only as descriptive inferential context.

Do NOT implement:

[
p>0.05 \Rightarrow Drop
]

Do NOT:

- stepwise-remove variables;
- refit reduced alternatives;
- search for lower p-values;
- repeatedly fit models until all predictors become significant.

p-value is supporting evidence for Stage 4.6 / 4.7.

---

# 21. Coefficient Forest Plot

Create:

`LOG-01 — Candidate WOE Logistic coefficient forest plot`

Show:

- all 10 predictors;
- point estimate;
- 95% CI;
- vertical zero reference line;
- canonical predictor order or clearly documented readable ordering.

Purpose:

> visualize coefficient direction, magnitude, and uncertainty.

Do not encode Keep/Drop decisions.

---

# 22. Specific PAY / Delinquency Diagnostic

Pay particular attention to:

- `PAY_0`
- `severe_delinquent_months`

Stage 4.4 intentionally retained:

- `PAY_0` as recent/current delinquency information;
- `severe_delinquent_months` as historical serious-delinquency persistence.

Their Stage 4.4 WOE correlation was approximately:

`0.692029`

Stage 4.5 must inspect whether both retain:

- negative coefficient direction;
- interpretable conditional contribution;
- acceptable uncertainty.

Do not compare them against dropped alternative PAY representations.

Do not reopen:

- delinquent\_months;
- max\_delinquency;
- PAY\_2–PAY\_6.

---

# 23. PAY\_AMT1–6 Diagnostic

All six PAY\_AMT predictors remain in the candidate model.

Stage 4.4 did not identify a defensible month cutoff.

Stage 4.5 must inspect their joint multivariate behavior.

For each PAY\_AMT variable record:

- coefficient;
- sign;
- SE;
- CI;
- p-value;
- diagnostic status.

This is the first stage that can reveal whether some PAY\_AMT variables have little incremental contribution after controlling for the other payment months and risk predictors.

Do NOT delete any PAY\_AMT variable in Stage 4.5.

Do NOT create:

- PAY\_AMT averages;
- PAY\_AMT totals;
- PAY\_AMT trends;
- recent-3-month summaries;
- any replacement representation.

---

# 24. LIMIT\_BAL / avg\_utilization Diagnostic

Inspect:

- `LIMIT_BAL`
- `avg_utilization`

Stage 4.4 established that they are not highly redundant in WOE space.

Stage 4.5 now examines whether each retains a reasonable conditional contribution after controlling for:

- delinquency;
- severe-delinquency history;
- payment behavior.

Do not reopen:

- current\_utilization;
- max\_utilization;
- BILL\_AMT variables.

---

# 25. Apparent Full-Train AUC

Calculate:

[
AUC\_{Train}
]

using the fitted probabilities from the same full Train sample.

This metric must be labeled:

> apparent development-sample AUC

or equivalent language.

Explicitly state:

> This is not an estimate of generalization performance because the frozen bins, WOE values, and Logistic model were developed using the Train sample.

Do not use Train AUC for:

- feature elimination;
- alternative-model search;
- final model claims.

---

# 26. Apparent Full-Train KS

Calculate full-Train KS using a deterministic definition.

Preferred implementation:

derive ROC quantities from:

- target = Default
- positive class = 1
- score = predicted default probability

Then:

[
KS
==

\max(TPR-FPR)
]

Record:

- KS value;
- threshold at maximum KS if available.

Label:

> apparent development-sample KS

Do not use it for variable selection.

---

# 27. ROC Figure

Create:

`LOG-02 — Apparent full-Train ROC curve`

Requirements:

- full Train only;
- candidate 10-variable Logistic;
- AUC shown;
- clearly labeled apparent/development-sample;
- random baseline reference if appropriate.

Do not imply holdout performance.

---

# 28. KS Figure

Create:

`LOG-03 — Apparent full-Train KS diagnostic`

Show a deterministic KS representation, such as:

- TPR and FPR separation over model threshold;
  or
- equivalent cumulative Good/Bad score-distribution separation.

State exact implementation.

Highlight the maximum KS value.

Do not optimize or freeze a classification threshold.

---

# 29. Performance Evidence Hierarchy

The Task Report must explicitly distinguish:

## Stage 4.5

Full-Train apparent metrics.

## Stage 4.6

Out-of-fold stability / predictive evidence.

## Stage 5

Sealed Holdout Test evaluation.

Conceptually:

[
Full\ Train\ apparent
\rightarrow
Cross\ Validation
\rightarrow
Final\ Holdout
]

The Stage 4.5 AUC/KS values must not be presented as the final expected performance of the model.

---

# 30. No Variable Removal in Stage 4.5

This is a hard rule.

Regardless of:

- p-value;
- confidence interval;
- coefficient magnitude;
- coefficient sign;

Stage 4.5 does NOT automatically delete predictors.

At the end of Stage 4.5:

> all 10 frozen candidate predictors remain candidates for Stage 4.6.

The only exception is not feature selection but model failure:

> if the fixed 10-variable model is numerically invalid and cannot support meaningful inference, stop and return BLOCKED evidence to Architect.

Do not solve numerical pathology by silently dropping variables.

---

# 31. No Alternative Model Search

Fit exactly one substantive predictive model.

Do not fit:

- 9-variable model;
- 8-variable model;
- PAY\_AMT1–3-only model;
- no-PAY\_AMT6 model;
- no-severe-delinquency model;
- alternate-utilization model;
- demographic model;
- raw-variable Logistic;
- regularized Logistic;
- stepwise model.

No subset performance comparisons.

---

# 32. No Regularization

Do not use:

- L1;
- L2;
- Elastic Net;
- Ridge Logistic;
- LASSO Logistic.

Do not tune:

- C;
- lambda;
- regularization strength.

Stage 4.5 is deliberately unpenalized.

---

# 33. No Imbalance Intervention

Do not use:

- `class_weight='balanced'`;
- sample weights for class balancing;
- SMOTE;
- oversampling;
- undersampling;
- synthetic observations;
- threshold adjustment to compensate for class imbalance.

Use the natural Train sample.

Class imbalance remains an observed dataset property, not a Stage 4.5 intervention.

---

# 34. No Threshold Optimization

Do not choose an operational classification threshold.

Do not optimize:

- accuracy;
- precision;
- recall;
- F1;
- Youden index;
- cost function;
- confusion matrix threshold.

The KS-max threshold may be reported only as part of the KS diagnostic.

It is NOT an approved decision threshold.

---

# 35. No Calibration Analysis

Do not calculate or plot:

- calibration curve;
- reliability diagram;
- Brier score;
- calibration slope;
- calibration intercept;
- Hosmer-Lemeshow-style acceptance decisions.

Calibration belongs later, after final model structure is determined.

---

# 36. No Cross-Validation

Do not run:

- KFold;
- StratifiedKFold;
- repeated CV;
- bootstrap stability;
- fold-specific WOE;
- fold-specific Logistic;
- OOF predictions.

Stage 4.6 owns cross-sample stability.

---

# 37. No Test Evaluation

Do not calculate anything predictive on Test.

No:

- Test WOE mapping inspection;
- Test probability;
- Test AUC;
- Test KS;
- Test ROC;
- Test calibration;
- Test coefficient assessment;
- Test threshold metrics.

Test remains sealed.

---

# 38. No Final Variable Freeze

Stage 4.5 must not state:

> final Logistic predictor list.

Instead state:

> Stage 4.5 fixed 10-variable Candidate WOE Logistic.

The final variable decision belongs to Stage 4.7 after considering:

- Stage 4.5 full-Train multivariate evidence;
- Stage 4.6 fold stability evidence.

---

# 39. Stage 4.6 Handoff Table

At the end of Stage 4.5 create a concise handoff table containing:

- Predictor
- Full-Train Coefficient
- Sign
- 95% CI
- p-value
- Stage 4.5 Diagnostic Status
- Stage 4.6 Follow-up Priority / Note

This table does not contain Keep / Drop.

All 10 must be marked:

> Continue to Stage 4.6

unless the complete model itself was BLOCKED for numerical validity.

---

# 40. Notebook Structure

Create:

`notebooks/08_candidate_woe_logistic.ipynb`

Recommended structure:

1. Stage 4.5 objective and boundaries
2. imports / environment
3. frozen split replay
4. Train/Test technical identity checks
5. Test seal
6. frozen 10-predictor list
7. reproduce required deterministic features
8. apply frozen binning
9. compute full-Train WOE mappings
10. construct 24,000×10 WOE matrix
11. WOE matrix integrity
12. WOE convention and expected coefficient sign
13. fit unpenalized MLE Logistic
14. convergence / numerical sanity
15. full coefficient diagnostic table
16. LOG-01 coefficient forest plot
17. PAY / delinquency interpretation
18. PAY\_AMT interpretation
19. LIMIT\_BAL / avg\_utilization interpretation
20. apparent full-Train AUC
21. LOG-02 ROC
22. apparent full-Train KS
23. LOG-03 KS diagnostic
24. Stage 4.6 handoff table
25. explicit no-selection / no-Test / no-CV boundary

Notebook must clean-run from raw data.

---

# 41. Task Report Structure

Create:

`task_records/Stage_4.5_Candidate_WOE_Logistic.md`

Include:

- Stage purpose;
- Git baseline;
- split identity;
- Test sealing;
- frozen 10-variable list;
- WOE convention;
- why negative coefficient is expected;
- model specification;
- why unpenalized MLE is used;
- convergence and numerical sanity;
- complete coefficient table;
- explanation of coefficient / SE / z / p-value / CI / OR;
- diagnostic status rules;
- PAY\_0 / severe\_delinquent\_months findings;
- PAY\_AMT1–6 findings;
- LIMIT\_BAL / avg\_utilization findings;
- LOG-01;
- apparent full-Train AUC;
- LOG-02;
- apparent full-Train KS;
- LOG-03;
- explicit performance-evidence hierarchy;
- Stage 4.6 handoff table;
- confirmation all 10 continue to Stage 4.6;
- no automatic feature deletion;
- no regularization;
- no subset search;
- no imbalance intervention;
- no threshold optimization;
- no calibration;
- no CV;
- no Test predictive analysis;
- Notebook clean-run;
- protected-file integrity;
- Git status;
- final Reviewer handoff.

---

# 42. Immutable Architect Specification

Append this entire Stage 4.5 Architect Specification verbatim to the Task Report between standalone lines:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.5 ================`

and

`================ END ARCHITECT SPECIFICATION — STAGE 4.5 ================`

Important provenance rule:

> The Architect source text is authoritative.

Do not invent or predeclare a byte count or SHA-256 as an independent authority.

If implementation records a fingerprint, it must be mechanically derived from the embedded authoritative UTF-8 block under an explicitly documented boundary.

A fingerprint must never be used to modify the specification text.

---

# 43. Reviewer Workflow

After Codex completes Stage 4.5:

> send the Stage 4.5 Notebook and Task Report to Reviewer.

Reviewer independently validates:

- Git baseline;
- frozen 10-variable list;
- Train/Test identity;
- Test sealing;
- frozen binning / WOE convention;
- 24,000×10 matrix integrity;
- unpenalized Logistic implementation;
- convergence;
- coefficient table;
- sign logic;
- inferential statistics;
- LOG-01;
- apparent Train AUC;
- apparent Train KS;
- LOG-02 / LOG-03;
- diagnostic-status logic;
- no feature deletion;
- all 10 continue to 4.6;
- no regularization;
- no subset search;
- no CV;
- no Test predictive analysis;
- clean-run;
- protected-file integrity;
- immutable Architect Specification;
- Git scope.

---

# 44. Reviewer FAIL

If Reviewer finds a substantive or documentation failure:

- no commit;
- return Required Fixes;
- no Stage 4.6.

---

# 45. Reviewer PASS

If Stage 4.5 passes:

In the same Reviewer round:

1. append final Stage 4.5 Reviewer Result;
2. stage exactly:
   - `notebooks/08_candidate_woe_logistic.ipynb`
   - `task_records/Stage_4.5_Candidate_WOE_Logistic.md`
3. commit Stage 4.5;
4. verify post-commit Git state.

Recommended commit message:

`Fit candidate WOE logistic model`

No push.

---

# 46. Protected Scope

Do not modify:

- raw data;
- Stage 0–4.4 notebooks;
- Stage 0–4.4 Task Reports;
- README;
- requirements;
- references;
- other tracked project files.

Stage 4.5 may create only:

- `notebooks/08_candidate_woe_logistic.ipynb`
- `task_records/Stage_4.5_Candidate_WOE_Logistic.md`

---

# 47. No External Artifacts

Do not create:

- model pickle;
- joblib;
- coefficient CSV;
- prediction CSV;
- score CSV;
- WOE CSV;
- ROC CSV;
- KS CSV;
- external PNG;
- HTML;
- PDF;
- `src/`;
- `tests/`.

All outputs remain embedded in the Notebook and Task Report.

---

# 48. Codex Git Rules

Codex:

- no git add;
- no commit;
- no push.

At Codex completion expected:

- HEAD remains:
  `afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`
- branch remains `main`
- staged diff empty
- only the two authorized Stage 4.5 artifacts are untracked
- no remote
- no push

Commit is owned by Reviewer after PASS.

---

# 49. Stage 4.5 Completion Criteria

All must pass:

1. correct project root;
2. branch `main`;
3. HEAD = `afe8057a...`;
4. clean Stage 4.4 baseline;
5. only two authorized Stage 4.5 artifacts created;
6. frozen split reproduced;
7. Train = 24,000;
8. Test = 6,000;
9. Train ID hash exact;
10. Test ID hash exact;
11. Test sealed;
12. Good = 18,691;
13. Bad = 5,309;
14. exact 10 predictors;
15. canonical predictor order preserved;
16. frozen Stage 4 binning preserved;
17. no new feature engineering;
18. WOE convention = ln(DistGood / DistBad);
19. full-Train 24,000×10 WOE matrix created;
20. no missing WOE;
21. no non-finite WOE;
22. exact target / row alignment;
23. no smoothing required;
24. one and only one substantive predictive model fitted;
25. model = unpenalized MLE Logistic;
26. intercept included;
27. model converged;
28. coefficients finite;
29. standard errors finite;
30. inference numerically valid;
31. coefficient table complete;
32. expected sign = negative documented;
33. Sign Check completed for all 10;
34. z-statistics reported;
35. p-values reported;
36. 95% CI reported;
37. OR reported;
38. OR correctly interpreted as odds, not probability;
39. no z-based elimination;
40. no p-value-based elimination;
41. no sign-based elimination;
42. deterministic diagnostic statuses assigned;
43. LOG-01 produced;
44. PAY\_0 / severe\_delinquent\_months explicitly reviewed;
45. PAY\_AMT1–6 explicitly reviewed;
46. LIMIT\_BAL / avg\_utilization explicitly reviewed;
47. apparent Train AUC computed;
48. AUC labeled apparent;
49. LOG-02 produced;
50. apparent Train KS computed;
51. KS definition documented;
52. LOG-03 produced;
53. no feature selection based on AUC/KS;
54. no alternate subset model;
55. no regularization;
56. no class weighting;
57. no SMOTE/resampling;
58. no threshold optimization;
59. no calibration analysis;
60. no CV;
61. no Test predictive analysis;
62. no final variable freeze;
63. all 10 predictors continue to Stage 4.6;
64. Stage 4.6 handoff table completed;
65. Notebook clean-runs;
66. Task Report complete;
67. Architect Specification preserved verbatim;
68. protected tracked files unchanged;
69. staged diff empty;
70. no commit;
71. no push;
72. final Reviewer handoff reached.

---

# 50. Codex Completion Report

Return a concise Completion Report containing:

- pre-flight result;
- branch / HEAD;
- Train/Test identity verification;
- Test sealed confirmation;
- exact 10 predictor list;
- WOE matrix shape;
- missing/non-finite counts;
- model implementation;
- convergence result;
- any warnings;
- intercept;
- 10 coefficient signs;
- count of Expected-sign predictors;
- count of Sign Concerns;
- count of Directionally Clear predictors;
- count of Review-in-Stage-4.6 predictors;
- whether any coefficient CI crosses zero;
- whether any coefficient is positive;
- apparent Train AUC;
- apparent Train KS;
- LOG-01 status;
- LOG-02 status;
- LOG-03 status;
- confirmation all 10 remain candidates for Stage 4.6;
- confirmation:
  - no automatic deletion
  - no alternative subset search
  - no regularization
  - no class weighting/resampling
  - no threshold optimization
  - no calibration
  - no CV
  - no Test predictive analysis
- Notebook clean-run;
- protected-file integrity;
- final Git state;
- no commit;
- no push.

If numerically valid, final line:

`Stage 4.5 implementation complete — fixed 10-variable Candidate WOE Logistic fitted and diagnosed; all 10 predictors retained for Stage 4.6 stability analysis; awaiting Reviewer validation.`

If model inference is numerically invalid, final line:

`Stage 4.5 BLOCKED by candidate-model numerical validity issue — no predictor was automatically removed and Architect review is required.`

================ END ARCHITECT SPECIFICATION — STAGE 4.5 ================

# Reviewer Result

## Status

`AWAITING INDEPENDENT REVIEWER VALIDATION`

## Verdict

_Reserved for Reviewer. Codex has not prewritten PASS or FAIL._

## Reviewer Notes

_To be completed by Reviewer._

# REVIEWER RESULT

## Verdict

PASS — Stage 4.5 Candidate WOE Logistic Regression satisfies the immutable Architect Specification and is accepted for milestone closeout.

## Requirement Check

- PASS — Git pre-flight is correct: project root, branch `main`, Stage 4.4 HEAD `afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`, clean tracked/staged baseline, and no remote.
- PASS — Exactly the two authorized Stage 4.5 artifacts were created; no prior-stage tracked file or external project artifact was modified or added.
- PASS — The frozen split was reproduced as 24,000 Train / 6,000 sealed Test with exact Train/Test ID hashes, zero overlap, complete ID coverage, and Train totals of 18,691 Good / 5,309 Bad. Test was deleted immediately after the permitted technical identity checks.
- PASS — Exactly the ten frozen predictors were reconstructed in canonical order using the approved feature definitions and Stage 4 bins. WOE uses `ln(DistGood / DistBad)` with no smoothing, producing an aligned finite 24,000×10 matrix with no missing values.
- PASS — The Notebook contains exactly one substantive predictive model: an explicit-intercept, unpenalized maximum-likelihood `statsmodels.Logit` on the natural full Train sample. No regularization, class weighting, resampling, alternative subset model, or raw-variable model was fitted.
- PASS — The model converged in six Newton iterations. Design rank is 11/11; coefficients, standard errors, covariance, linear predictor, and probabilities are finite; covariance is symmetric and positive definite; probabilities lie strictly inside `(0,1)`; and no invalidating warning or separation failure is present.
- PASS — The canonical ten-row coefficient table contains all required coefficient, expected-sign, sign-check, SE, z, p-value, confidence interval, odds-ratio, diagnostic-status, and Stage 4.6 follow-up fields. Odds ratios are correctly interpreted as changes in odds, not probabilities.
- PASS — All ten coefficients have the expected negative sign. Nine are `Directionally Clear`; `PAY_AMT5` is correctly marked `Review in Stage 4.6` because its negative point estimate has a 95% CI crossing zero. No Sign Concern exists and no predictor was removed.
- PASS — LOG-01, LOG-02, and LOG-03 are present inline and correctly show the coefficient forest plot, apparent full-Train ROC/AUC, and apparent full-Train KS diagnostic. AUC `0.772417098` and KS `0.430307867` are accurately labeled development-sample diagnostics and were not used for selection or threshold optimization.
- PASS — The performance-evidence hierarchy and Stage 4.6 handoff are explicit. All ten predictors continue to Stage 4.6; no final variable freeze, CV, calibration, Holdout evaluation, or Stage 4.6/5 work was performed.
- PASS — The authoritative and embedded Stage 4.5 Architect Specification blocks each contain one valid standalone marker pair and are byte-for-byte equal: 28,332 UTF-8 bytes, SHA-256 `f90a59ee9c28938efcfae11cc5198181a5c0beeff6e8f1eff4315e975d6d7b67`.
- PASS — The Notebook clean-runs from raw data in a fresh isolated kernel: 33 cells, 16 code cells with sequential execution counts 1–16, 27 outputs, exactly three inline PNGs, and zero errors.
- PASS — All 24 protected tracked files, the Luca reference Notebook, and the Git index match their recorded pre-flight fingerprints.

## Independent Verification

- Read the complete Task Report, immutable Stage 4.5 Architect Specification, and referenced authoritative source prompt.
- Inspected every Notebook code cell, saved output, model call, frozen mapping, WOE construction, numerical-sanity assertion, coefficient-status rule, apparent-performance calculation, handoff record, and hard-boundary assertion.
- Replayed the complete Notebook in a clean local kernel without saving or modifying it; execution completed with zero errors.
- Independently refitted the same fixed design using the equivalent `statsmodels` Binomial GLM in an unsaved Reviewer process. It converged; maximum coefficient difference versus the saved Logit result was `2.51e-14`, maximum probability difference was `1.90e-14`, and independently calculated AUC/KS matched exactly.
- Visually inspected LOG-01 through LOG-03, including canonical predictor coverage, confidence intervals and zero line, apparent ROC labeling, valid probability-threshold axis, and maximum-KS highlighting.
- Verified source scope mechanically: one `Logit` construction, one `.fit` call, no alternate predictive estimator or CV call, no project-file write/export call, and Test data references only in the permitted split/seal cell; later Test mentions are explanatory strings/tables only.
- Independently extracted and hashed the authoritative and embedded specification blocks using whole standalone-line boundaries and confirmed exact equality.
- Recomputed protected-file, Notebook, reference, and index fingerprints; inspected Git tracked/staged/untracked state, HEAD, branch, and remotes immediately before closeout.

## Issues Found

No issue affecting Stage 4.5 acceptance. `PAY_AMT5` is a correctly documented Stage 4.6 stability-review item, not a Stage 4.5 defect or deletion decision.

## Required Fixes

None.

## Architect Decision Needed

None.
