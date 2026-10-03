# Stage 4.6 — Stratified 5-Fold Model Stability

## Execution Status

`IMPLEMENTATION COMPLETE — AWAITING INDEPENDENT REVIEWER VALIDATION`

Codex has completed the authorized Stage 4.6 implementation. This report does not claim Reviewer approval and does not make a Stage 4.7 Keep/Drop decision.

## Stage Purpose

Stage 4.6 evaluates coefficient and predictive stability for the fixed ten-variable Candidate WOE Logistic model across one deterministic five-fold partition of the 24,000-row Development Train. It is not a feature-screening, re-binning, alternative-model, Holdout-evaluation, or final-selection stage.

## Git Baseline

- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD: `1e9c95f022de590eec7eb4b3300e01be3ce57f03`
- Parent: `afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`
- HEAD message: `Fit candidate WOE logistic model`
- Stage 4.5 Reviewer verdict: `PASS`, read from `task_records/Stage_4.5_Candidate_WOE_Logistic.md`.
- Pre-flight tracked diff: empty.
- Pre-flight staged diff: empty.
- Pre-flight untracked list: empty.
- Pre-flight remote list: empty.
- Neither authorized Stage 4.6 artifact existed before this execution.
- No Stage 4.6 commit existed.

The recorded pre-flight commands were `git status --short`, `git diff --name-status`, `git diff --stat`, `git diff --cached --name-status`, `git ls-files --others --exclude-standard`, and `git remote -v`; each produced no path or remote entry.

## Stage 4.5 Reference Model and Apparent Evidence

The reference is the fixed ten-variable, explicit-intercept, unpenalized maximum-likelihood WOE Logistic model from Stage 4.5.

| Predictor | Coefficient |
| --- | --- |
| PAY_0 | -0.556303702 |
| severe_delinquent_months | -0.513060046 |
| PAY_AMT1 | -0.193755569 |
| PAY_AMT2 | -0.222092688 |
| PAY_AMT3 | -0.245401439 |
| PAY_AMT4 | -0.165041794 |
| PAY_AMT5 | -0.097122437 |
| PAY_AMT6 | -0.164349102 |
| LIMIT_BAL | -0.226160642 |
| avg_utilization | -0.405621158 |

- Expected negative signs: 10/10.
- Directionally Clear: 9/10.
- Sole Stage 4.5 review variable: `PAY_AMT5`, coefficient `-0.097122437`, 95% CI approximately `[-0.251096, 0.056851]`, p-value approximately `0.21635`.
- Apparent full-Train AUC: `0.772417098`.
- Apparent full-Train KS: `0.430307867`.
- These are comparison anchors, not generalization estimates.

## Frozen Train/Test Identity and Holdout Sealing

- Raw rows: 30,000.
- Development Train: 24,000 rows; 18,691 Good; 5,309 Bad; bad rate `22.1208%`.
- Holdout Test: 6,000 rows.
- Train ID SHA-256: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`.
- Test ID SHA-256: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Identity checks confirmed row counts, zero ID overlap, full 30,000-ID union, and both frozen hashes.
- Holdout status: `SEALED — Holdout deleted after row/ID/union/hash identity checks only`.
- No Holdout predictor inspection, target-distribution inspection, binning, WOE, scoring, AUC, KS, calibration, or risk analysis occurred.

## Frozen Predictors, Features, and Bins

Canonical order in every fold:

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

The only reproduced Stage 3 derived features were `severe_delinquent_months = sum(PAY_i >= 2)` and uncapped `avg_utilization = mean(max(BILL_AMT1,0), …, max(BILL_AMT6,0)) / LIMIT_BAL`. Feature construction was target-free.

| Predictor(s) | Frozen bins in order |
| --- | --- |
| PAY_0 | [-2,-1,0]; [1]; [2+] |
| severe_delinquent_months | 0; 1; 2; 3; 4+ |
| PAY_AMT1–PAY_AMT6 | 0; (0,5k]; (5k,20k]; >20k |
| LIMIT_BAL | <=30k; (30k,140k]; (140k,240k]; >240k |
| avg_utilization | 0; (0,0.2]; (0.2,0.6]; (0.6,1]; >1 |

The frozen bin definitions were transcribed from the approved Stage 4.2/4.3 implementation and were not searched, merged, split, or optimized. Across all folds, 205 predictor × frozen-bin checks were completed.

## Stratified Five-Fold Configuration and Membership Audit

- Exact configuration: `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)`.
- Fold membership was generated once and stored as one list of five train/validation index pairs; folds were not independently resampled.

| Fold | Fold-Train row count | Validation row count | Fold-Train Good | Fold-Train Bad | Fold-Train bad rate | Validation Good | Validation Bad | Validation bad rate | Validation ID SHA-256 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 19200 | 4800 | 14952 | 4248 | 22.1250% | 3739 | 1061 | 22.1042% | 77aa267a9e7aa92775407bf611fa544bfda52bc15dfdba9fbb9fb87d1f06beda |
| 2 | 19200 | 4800 | 14953 | 4247 | 22.1198% | 3738 | 1062 | 22.1250% | 9c90b6d13f3e4c963fbf131fa658591abcfe676719bc18be4f7cf9cb72858eb5 |
| 3 | 19200 | 4800 | 14953 | 4247 | 22.1198% | 3738 | 1062 | 22.1250% | 5b835279e92cb56f1c4d77ec01d4d9e0cba39c0d2f3f070e04ecb9212d5255ee |
| 4 | 19200 | 4800 | 14953 | 4247 | 22.1198% | 3738 | 1062 | 22.1250% | 1b474e892452313444dcb2f51811d8db2d96fda84cc0ef245ae1e317686a2378 |
| 5 | 19200 | 4800 | 14953 | 4247 | 22.1198% | 3738 | 1062 | 22.1250% | 2933e9d11716a8bfa294057f2d24a1f39938d6736fb2f45c1691736f6b1aca62 |

Audit conclusion:

- validation sets pairwise disjoint: `true`;
- validation union equals all 24,000 Development-Train IDs: `true`;
- every Development-Train ID is in Validation exactly once: `true`;
- no Validation ID appears in its own Fold-Train set;
- total Validation assignment and prospective OOF coverage: 24,000.

## Fold-Specific WOE Method and Leakage Controls

For each fold, fixed-bin Good/Bad counts and WOE values were estimated exclusively from Fold-Train rows and Fold-Train targets. The Fold-Train mapping was then applied to Fold-Train and its corresponding Validation rows. Validation targets were never supplied to WOE estimation, and the Stage 4.5 Full-Train WOE mapping was never used for Validation.

WOE convention: `ln(DistGood / DistBad)`, Good = 0 and Bad = 1. Positive WOE is safer, so the expected coefficient sign is negative.

The approved Stage 4.1 fallback was implemented exactly: if any positive-support bin in a predictor/fold table had one zero class, `0.5` would be added to every Good and Bad bin count in that entire predictor/fold table before class-distribution denominators and WOE were recomputed. A zero-total-support bin would stop execution rather than borrow or invent WOE.

- Zero-support frozen bins: `0`.
- Positive-support zero-class bins: `0`.
- Predictor/fold tables requiring smoothing: `0`.
- Smoothing occurrences recorded: `0`.
- Actual outcome: ordinary Fold-Train-only WOE was used for every predictor/bin/fold.
- Every Fold-Train WOE matrix was 19,200 × 10; every Validation WOE matrix was 4,800 × 10; canonical order, row/target alignment, finiteness, and absence of missing values were asserted.

## Fold Model Method and Numerical Sanity

Exactly five substantive models were fitted: one explicit-intercept, unpenalized maximum-likelihood `statsmodels.Logit` per fold, always using all ten Fold-Train WOE predictors. No alternative subset or estimator was fitted.

| Fold | Converged | Iterations | Design rank | Intercept finite | Coefficients finite | Standard errors finite/positive | Covariance usable | Covariance condition number | Validation probability min | Validation probability max | Warning count | Numerically valid |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | True | 6 | 11 | True | True | True | True | 37.619928 | 0.039165 | 0.867914 | 0 | True |
| 2 | True | 6 | 11 | True | True | True | True | 41.284472 | 0.037889 | 0.859319 | 0 | True |
| 3 | True | 6 | 11 | True | True | True | True | 40.163282 | 0.037026 | 0.864836 | 0 | True |
| 4 | True | 6 | 11 | True | True | True | True | 40.988279 | 0.039712 | 0.842635 | 0 | True |
| 5 | True | 6 | 11 | True | True | True | True | 39.024693 | 0.035189 | 0.876235 | 0 | True |

All five models converged in six iterations, had full design rank 11/11, finite coefficients/intercepts/positive standard errors, usable finite covariance, finite in-range Validation probabilities, no captured warning, no singular matrix, and no inference-invalidating separation indicator. Numerical warnings: none.

## Complete 50-Row Fold Coefficient Detail

Expected sign is negative. `Directionally Clear = Yes` means coefficient < 0 and the entire 95% CI is below zero. These diagnostics were not used for elimination.

| Fold | Predictor | Coefficient | Expected Sign | Sign Check | Standard Error | z-statistic | p-value | 95% CI Lower | 95% CI Upper | Directionally Clear? |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | PAY_0 | -0.571513 | Negative | Expected | 0.026353 | -21.686520 | 2.75019e-104 | -0.623165 | -0.519861 | Yes |
| 1 | severe_delinquent_months | -0.495213 | Negative | Expected | 0.027518 | -17.995798 | 2.10171e-72 | -0.549147 | -0.441278 | Yes |
| 1 | PAY_AMT1 | -0.226706 | Negative | Expected | 0.053644 | -4.226080 | 2.37797e-05 | -0.331847 | -0.121565 | Yes |
| 1 | PAY_AMT2 | -0.198144 | Negative | Expected | 0.061594 | -3.216914 | 0.00129577 | -0.318866 | -0.077421 | Yes |
| 1 | PAY_AMT3 | -0.230898 | Negative | Expected | 0.069868 | -3.304804 | 0.000950428 | -0.367836 | -0.093961 | Yes |
| 1 | PAY_AMT4 | -0.133884 | Negative | Expected | 0.082310 | -1.626587 | 0.103825 | -0.295209 | 0.027440 | No |
| 1 | PAY_AMT5 | -0.125774 | Negative | Expected | 0.086177 | -1.459474 | 0.144435 | -0.294678 | 0.043131 | No |
| 1 | PAY_AMT6 | -0.147006 | Negative | Expected | 0.085242 | -1.724572 | 0.0846046 | -0.314078 | 0.020065 | No |
| 1 | LIMIT_BAL | -0.260276 | Negative | Expected | 0.049228 | -5.287174 | 1.24221e-07 | -0.356760 | -0.163791 | Yes |
| 1 | avg_utilization | -0.376214 | Negative | Expected | 0.058908 | -6.386455 | 1.69775e-10 | -0.491672 | -0.260756 | Yes |
| 2 | PAY_0 | -0.553077 | Negative | Expected | 0.026593 | -20.797435 | 4.56586e-96 | -0.605199 | -0.500954 | Yes |
| 2 | severe_delinquent_months | -0.520510 | Negative | Expected | 0.027383 | -19.008675 | 1.44558e-80 | -0.574179 | -0.466841 | Yes |
| 2 | PAY_AMT1 | -0.168827 | Negative | Expected | 0.055668 | -3.032745 | 0.0024234 | -0.277934 | -0.059719 | Yes |
| 2 | PAY_AMT2 | -0.223427 | Negative | Expected | 0.061408 | -3.638428 | 0.000274307 | -0.343784 | -0.103071 | Yes |
| 2 | PAY_AMT3 | -0.257929 | Negative | Expected | 0.068663 | -3.756461 | 0.000172333 | -0.392505 | -0.123352 | Yes |
| 2 | PAY_AMT4 | -0.193975 | Negative | Expected | 0.080606 | -2.406474 | 0.0161074 | -0.351959 | -0.035991 | Yes |
| 2 | PAY_AMT5 | -0.057477 | Negative | Expected | 0.089118 | -0.644955 | 0.518956 | -0.232146 | 0.117192 | No |
| 2 | PAY_AMT6 | -0.203027 | Negative | Expected | 0.089467 | -2.269288 | 0.0232508 | -0.378379 | -0.027674 | Yes |
| 2 | LIMIT_BAL | -0.221058 | Negative | Expected | 0.050787 | -4.352669 | 1.3449e-05 | -0.320599 | -0.121518 | Yes |
| 2 | avg_utilization | -0.410717 | Negative | Expected | 0.059875 | -6.859598 | 6.90546e-12 | -0.528070 | -0.293365 | Yes |
| 3 | PAY_0 | -0.557679 | Negative | Expected | 0.026188 | -21.295469 | 1.25049e-100 | -0.609006 | -0.506353 | Yes |
| 3 | severe_delinquent_months | -0.512832 | Negative | Expected | 0.026971 | -19.014476 | 1.29423e-80 | -0.565694 | -0.459971 | Yes |
| 3 | PAY_AMT1 | -0.178492 | Negative | Expected | 0.055612 | -3.209606 | 0.00132917 | -0.287489 | -0.069495 | Yes |
| 3 | PAY_AMT2 | -0.217648 | Negative | Expected | 0.061344 | -3.548016 | 0.000388144 | -0.337880 | -0.097417 | Yes |
| 3 | PAY_AMT3 | -0.267996 | Negative | Expected | 0.068810 | -3.894721 | 9.83117e-05 | -0.402862 | -0.133131 | Yes |
| 3 | PAY_AMT4 | -0.169016 | Negative | Expected | 0.081203 | -2.081400 | 0.0373973 | -0.328171 | -0.009861 | Yes |
| 3 | PAY_AMT5 | -0.098118 | Negative | Expected | 0.088117 | -1.113494 | 0.265496 | -0.270824 | 0.074589 | No |
| 3 | PAY_AMT6 | -0.131453 | Negative | Expected | 0.087905 | -1.495395 | 0.134811 | -0.303743 | 0.040838 | No |
| 3 | LIMIT_BAL | -0.238291 | Negative | Expected | 0.049207 | -4.842661 | 1.28112e-06 | -0.334734 | -0.141848 | Yes |
| 3 | avg_utilization | -0.394047 | Negative | Expected | 0.058371 | -6.750726 | 1.47107e-11 | -0.508452 | -0.279642 | Yes |
| 4 | PAY_0 | -0.549288 | Negative | Expected | 0.026330 | -20.861968 | 1.18685e-96 | -0.600893 | -0.497683 | Yes |
| 4 | severe_delinquent_months | -0.523764 | Negative | Expected | 0.026997 | -19.401107 | 7.55272e-84 | -0.576676 | -0.470852 | Yes |
| 4 | PAY_AMT1 | -0.141375 | Negative | Expected | 0.056522 | -2.501242 | 0.0123758 | -0.252156 | -0.030594 | Yes |
| 4 | PAY_AMT2 | -0.232493 | Negative | Expected | 0.062031 | -3.748032 | 0.000178228 | -0.354071 | -0.110915 | Yes |
| 4 | PAY_AMT3 | -0.281390 | Negative | Expected | 0.069084 | -4.073164 | 4.63788e-05 | -0.416792 | -0.145988 | Yes |
| 4 | PAY_AMT4 | -0.169505 | Negative | Expected | 0.081304 | -2.084819 | 0.0370857 | -0.328858 | -0.010151 | Yes |
| 4 | PAY_AMT5 | -0.041387 | Negative | Expected | 0.091442 | -0.452607 | 0.650832 | -0.220611 | 0.137836 | No |
| 4 | PAY_AMT6 | -0.217217 | Negative | Expected | 0.085691 | -2.534878 | 0.0112486 | -0.385169 | -0.049265 | Yes |
| 4 | LIMIT_BAL | -0.195552 | Negative | Expected | 0.051111 | -3.825988 | 0.000130249 | -0.295729 | -0.095375 | Yes |
| 4 | avg_utilization | -0.440321 | Negative | Expected | 0.057406 | -7.670356 | 1.71519e-14 | -0.552834 | -0.327808 | Yes |
| 5 | PAY_0 | -0.548767 | Negative | Expected | 0.026540 | -20.676853 | 5.59751e-95 | -0.600785 | -0.496749 | Yes |
| 5 | severe_delinquent_months | -0.514139 | Negative | Expected | 0.027451 | -18.729014 | 2.87186e-78 | -0.567943 | -0.460335 | Yes |
| 5 | PAY_AMT1 | -0.248995 | Negative | Expected | 0.051681 | -4.817885 | 1.45088e-06 | -0.350288 | -0.147701 | Yes |
| 5 | PAY_AMT2 | -0.243629 | Negative | Expected | 0.058759 | -4.146230 | 3.37994e-05 | -0.358795 | -0.128463 | Yes |
| 5 | PAY_AMT3 | -0.186212 | Negative | Expected | 0.070041 | -2.658624 | 0.00784604 | -0.323490 | -0.048935 | Yes |
| 5 | PAY_AMT4 | -0.160817 | Negative | Expected | 0.079080 | -2.033595 | 0.0419924 | -0.315812 | -0.005823 | Yes |
| 5 | PAY_AMT5 | -0.160361 | Negative | Expected | 0.084187 | -1.904812 | 0.0568045 | -0.325365 | 0.004643 | No |
| 5 | PAY_AMT6 | -0.126516 | Negative | Expected | 0.087713 | -1.442396 | 0.149191 | -0.298430 | 0.045397 | No |
| 5 | LIMIT_BAL | -0.213213 | Negative | Expected | 0.051021 | -4.178948 | 2.9286e-05 | -0.313211 | -0.113214 | Yes |
| 5 | avg_utilization | -0.409804 | Negative | Expected | 0.058579 | -6.995734 | 2.63875e-12 | -0.524617 | -0.294991 | Yes |

## Ten-Row Coefficient Stability Summary

Fold coefficient SD uses sample standard deviation with `ddof=1`. Because WOE is re-estimated inside each Fold-Train sample, WOE scale may shift modestly; cross-fold magnitude comparisons are diagnostic stability evidence, not a formal identical-scale hypothesis test.

| Predictor | Stage 4.5 Full-Train Coefficient | Fold 1 Coefficient | Fold 2 Coefficient | Fold 3 Coefficient | Fold 4 Coefficient | Fold 5 Coefficient | Fold Mean Coefficient | Fold Coefficient SD | Fold Minimum | Fold Maximum | Negative Fold Count | Positive Fold Count | Directionally Clear Fold Count | CI-Crosses-Zero Fold Count | Maximum Absolute Deviation from Full-Train Coefficient | Stage 4.7 Review Note |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PAY_0 | -0.556304 | -0.571513 | -0.553077 | -0.557679 | -0.549288 | -0.548767 | -0.556065 | 0.009346 | -0.571513 | -0.548767 | 5 | 0 | 5 | 0 | 0.015209 | 5/5 negative; all fold CIs below zero; direction stable. |
| severe_delinquent_months | -0.513060 | -0.495213 | -0.520510 | -0.512832 | -0.523764 | -0.514139 | -0.513292 | 0.011063 | -0.523764 | -0.495213 | 5 | 0 | 5 | 0 | 0.017847 | 5/5 negative; all fold CIs below zero; direction stable. |
| PAY_AMT1 | -0.193756 | -0.226706 | -0.168827 | -0.178492 | -0.141375 | -0.248995 | -0.192879 | 0.043964 | -0.248995 | -0.141375 | 5 | 0 | 5 | 0 | 0.055239 | 5/5 negative; all fold CIs below zero; direction stable. |
| PAY_AMT2 | -0.222093 | -0.198144 | -0.223427 | -0.217648 | -0.232493 | -0.243629 | -0.223068 | 0.017046 | -0.243629 | -0.198144 | 5 | 0 | 5 | 0 | 0.023949 | 5/5 negative; all fold CIs below zero; direction stable. |
| PAY_AMT3 | -0.245401 | -0.230898 | -0.257929 | -0.267996 | -0.281390 | -0.186212 | -0.244885 | 0.037666 | -0.281390 | -0.186212 | 5 | 0 | 5 | 0 | 0.059189 | 5/5 negative; all fold CIs below zero; direction stable. |
| PAY_AMT4 | -0.165042 | -0.133884 | -0.193975 | -0.169016 | -0.169505 | -0.160817 | -0.165439 | 0.021568 | -0.193975 | -0.133884 | 5 | 0 | 4 | 1 | 0.031158 | 5/5 negative; 1/5 CIs cross zero; review conditional-strength stability. |
| PAY_AMT5 | -0.097122 | -0.125774 | -0.057477 | -0.098118 | -0.041387 | -0.160361 | -0.096623 | 0.048729 | -0.160361 | -0.041387 | 5 | 0 | 0 | 5 | 0.063239 | 5/5 negative; 5/5 CIs cross zero; review conditional-strength stability. |
| PAY_AMT6 | -0.164349 | -0.147006 | -0.203027 | -0.131453 | -0.217217 | -0.126516 | -0.165044 | 0.042139 | -0.217217 | -0.126516 | 5 | 0 | 2 | 3 | 0.052868 | 5/5 negative; 3/5 CIs cross zero; review conditional-strength stability. |
| LIMIT_BAL | -0.226161 | -0.260276 | -0.221058 | -0.238291 | -0.195552 | -0.213213 | -0.225678 | 0.024700 | -0.260276 | -0.195552 | 5 | 0 | 5 | 0 | 0.034115 | 5/5 negative; all fold CIs below zero; direction stable. |
| avg_utilization | -0.405621 | -0.376214 | -0.410717 | -0.394047 | -0.440321 | -0.409804 | -0.406221 | 0.023688 | -0.440321 | -0.376214 | 5 | 0 | 5 | 0 | 0.034700 | 5/5 negative; all fold CIs below zero; direction stable. |

Every predictor was negative in all five folds. Seven predictors were Directionally Clear in all five folds; `PAY_AMT4` was Directionally Clear in 4/5, `PAY_AMT6` in 2/5, and `PAY_AMT5` in 0/5. No automatic threshold or deletion rule was applied.

## STB-01 — Five-fold WOE Logistic coefficient stability

Status: `PRESENT — five fold estimates plus Stage 4.5 reference for all ten predictors`. The inline figure shows all five fold estimates, the distinguishable Stage 4.5 Full-Train reference, and the zero line for every canonical predictor. It encodes no Keep/Drop decision.

## PAY_AMT5 Focused Stability Review

- Stage 4.5 coefficient: `-0.097122`.
- Fold coefficients: `-0.125774`, `-0.057477`, `-0.098118`, `-0.041387`, `-0.160361`.
- Negative/positive folds: `5/5` negative, `0/5` positive.
- Directionally Clear folds: `0/5`.
- CI-crosses-zero folds: `5/5`.
- Fold mean: `-0.096623`.
- Fold sample SD: `0.048729`.
- Fold range: `[-0.160361, -0.041387]`.
- Maximum absolute deviation from Full-Train: `0.063239`.
- Observation only: all five point estimates remain negative, but every fold CI crosses zero. This is conditional-strength stability evidence for Stage 4.7 Human review, not a removal decision.

## Fold Validation AUC / KS

Positive class is Default = 1. KS is `max(TPR − FPR)`; its maximizing thresholds are diagnostic only and are not operational cutoffs.

| Fold | Validation N | Validation Bad Rate | AUC | KS | KS-max Threshold (diagnostic only) |
| --- | --- | --- | --- | --- | --- |
| 1 | 4800 | 22.1042% | 0.776925289 | 0.448388852 | 0.237424356 |
| 2 | 4800 | 22.1250% | 0.773681556 | 0.437295390 | 0.225276181 |
| 3 | 4800 | 22.1250% | 0.765525765 | 0.429064154 | 0.182234605 |
| 4 | 4800 | 22.1250% | 0.767137703 | 0.427995575 | 0.212391285 |
| 5 | 4800 | 22.1250% | 0.773806627 | 0.438353390 | 0.201542508 |

| Metric | Mean | Sample SD (ddof=1) | Minimum | Maximum |
| --- | --- | --- | --- | --- |
| Validation AUC | 0.771415388 | 0.004852803 | 0.765525765 | 0.776925289 |
| Validation KS | 0.436219472 | 0.008255848 | 0.427995575 | 0.448388852 |

The five-row table and its mean/sample-SD/min/max statistics are the primary predictive-stability view. No pass/fail dispersion threshold was introduced.

## OOF Definition, Coverage, and Pooled Metrics

Each OOF prediction was produced by the fold model and Fold-Train-derived WOE mapping that excluded that observation. The five Validation prediction frames were reassembled by original Development-Train identity.

- OOF rows: `24,000`.
- Unique OOF IDs: `24,000`.
- Missing predictions: `0`.
- Duplicate predictions per ID: `0`.
- Fold labels: 1–5.
- Target equality to original Development Train: exact.
- Coverage: `24,000 / 24,000`.
- Pooled OOF AUC: `0.771230165`.
- Pooled OOF KS: `0.429462039`.
- Pooled OOF KS-max diagnostic threshold: `0.194182078`; not an operational threshold.

Mean Fold AUC (`0.771415388`) averages the five within-fold ranking statistics and answers how well an individual Validation fold ranked Good/Bad observations on average. Pooled OOF AUC (`0.771230165`) is calculated once over all 24,000 OOF predictions and includes cross-fold ranking comparisons. They are related but not mathematically identical. The same distinction applies to mean Fold KS (`0.436219472`) and pooled OOF KS (`0.429462039`).

## STB-02 and STB-03

- STB-02: `PRESENT — pooled 24,000-row OOF ROC with AUC and random reference`.
- STB-03: `PRESENT — pooled OOF TPR/FPR separation with maximum KS highlighted`.
- Both are clearly labeled OOF evidence under frozen full-Train-designed bins and predictors; neither is labeled Holdout/Test performance.
- STB-03 uses valid probability thresholds and highlights the maximum OOF KS without selecting a business threshold.

## Apparent-vs-OOF Gap

- Signed AUC gap = `0.772417098 − 0.771230165 = 0.001186933`.
- Signed KS gap = `0.430307867 − 0.429462039 = 0.000845828`.

These are descriptive apparent-vs-OOF optimism diagnostics. No arbitrary pass/fail cutoff was imposed.

## Required OOF Methodological Limitation

Stage 4.6 OOF evidence is stronger than Stage 4.5 apparent evidence because each observation is excluded from its fold-specific WOE estimation and Logistic fitting. It is nevertheless not a fully nested, leakage-free model-development estimate: Stage 4.2/4.3 supervised bin boundaries and Stage 4.4 candidate selection were developed using the full 24,000-row Development Train, so a Validation-fold observation influenced earlier design decisions.

> Stage 4.6 OOF metrics are model-stability / cross-validated evidence under frozen full-Train-designed bins and predictors, not a final unbiased estimate of unseen-data performance.

The truly sealed unseen-data evaluation remains Stage 5 Holdout Test.

## Stage 4.7 Handoff — Variable Stability Table

| Predictor | Stage 4.5 Full-Train Coefficient | Fold Mean Coefficient | Fold Coefficient SD | Fold Min | Fold Max | Negative Folds / 5 | Positive Folds / 5 | Directionally Clear Folds / 5 | CI-Crosses-Zero Folds / 5 | Max Absolute Deviation from Full-Train Coefficient | Stage 4.5 Status | Stage 4.6 Stability Evidence Summary | Stage 4.7 Review Note | Continuation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PAY_0 | -0.556304 | -0.556065 | 0.009346 | -0.571513 | -0.548767 | 5/5 | 0/5 | 5/5 | 0/5 | 0.015209 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |
| severe_delinquent_months | -0.513060 | -0.513292 | 0.011063 | -0.523764 | -0.495213 | 5/5 | 0/5 | 5/5 | 0/5 | 0.017847 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |
| PAY_AMT1 | -0.193756 | -0.192879 | 0.043964 | -0.248995 | -0.141375 | 5/5 | 0/5 | 5/5 | 0/5 | 0.055239 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |
| PAY_AMT2 | -0.222093 | -0.223068 | 0.017046 | -0.243629 | -0.198144 | 5/5 | 0/5 | 5/5 | 0/5 | 0.023949 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |
| PAY_AMT3 | -0.245401 | -0.244885 | 0.037666 | -0.281390 | -0.186212 | 5/5 | 0/5 | 5/5 | 0/5 | 0.059189 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |
| PAY_AMT4 | -0.165042 | -0.165439 | 0.021568 | -0.193975 | -0.133884 | 5/5 | 0/5 | 4/5 | 1/5 | 0.031158 | Directionally Clear | 5/5 negative; 1/5 CIs cross zero; review conditional-strength stability. | 5/5 negative; 1/5 CIs cross zero; review conditional-strength stability. | Continue to Stage 4.7 |
| PAY_AMT5 | -0.097122 | -0.096623 | 0.048729 | -0.160361 | -0.041387 | 5/5 | 0/5 | 0/5 | 5/5 | 0.063239 | Review in Stage 4.6 | 5/5 negative; 5/5 CIs cross zero; review conditional-strength stability. | 5/5 negative; 5/5 CIs cross zero; review conditional-strength stability. | Continue to Stage 4.7 |
| PAY_AMT6 | -0.164349 | -0.165044 | 0.042139 | -0.217217 | -0.126516 | 5/5 | 0/5 | 2/5 | 3/5 | 0.052868 | Directionally Clear | 5/5 negative; 3/5 CIs cross zero; review conditional-strength stability. | 5/5 negative; 3/5 CIs cross zero; review conditional-strength stability. | Continue to Stage 4.7 |
| LIMIT_BAL | -0.226161 | -0.225678 | 0.024700 | -0.260276 | -0.195552 | 5/5 | 0/5 | 5/5 | 0/5 | 0.034115 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |
| avg_utilization | -0.405621 | -0.406221 | 0.023688 | -0.440321 | -0.376214 | 5/5 | 0/5 | 5/5 | 0/5 | 0.034700 | Directionally Clear | 5/5 negative; all fold CIs below zero; direction stable. | 5/5 negative; all fold CIs below zero; direction stable. | Continue to Stage 4.7 |

All ten predictors continue to Stage 4.7. This table contains no Keep/Drop field or decision.

## Hard-Boundary Confirmation

- No variable deletion.
- No fold-specific feature selection or alternate subset search.
- No new feature engineering.
- No re-binning, cut search, merge, or split.
- No Full-Train WOE leakage into Validation.
- No Validation-target leakage into WOE.
- No regularization.
- No class weighting, SMOTE, over/undersampling, or synthetic observations.
- No calibration analysis.
- No operational threshold optimization.
- No Holdout predictive analysis.
- No Stage 4.7 final selection.
- No external CSV, PNG, model, prediction, HTML, PDF, `src/`, or `tests/` artifact.

## Notebook Clean-Run and Output Verification

- Artifact: `notebooks/09_stratified_5fold_model_stability.ipynb`.
- First isolated clean-run from raw CSV: success.
- Python executable: `/opt/anaconda3/bin/python`.
- Python version: `3.11.7`.
- Packages: `matplotlib 3.8.0, numpy 1.26.4, pandas 2.1.4, scikit-learn 1.2.2, scipy 1.11.4, seaborn 0.12.2, statsmodels 0.14.0`.
- Notebook format: `4.5`.
- Cells: `39` total; `19` code; `20` markdown.
- Execution counts: `1–19`, strictly sequential = `True`.
- Error/traceback outputs: `0`.
- Inline PNG figures: `3` (STB-01 through STB-03).
- `statsmodels.Logit` construction sites in source: `1`; the one construction executes once per each of five frozen fold splits, producing exactly `5` substantive fits.
- Notebook bytes after first clean-run: `497238`.
- Notebook SHA-256 after first clean-run: `03ae57a221bc035c8c72e3269f8910f70585037c2d1f66338a89fbe9a926ff6f`.
- Final post-report isolated clean-run from raw CSV: success; 39 cells, 19 code cells with sequential execution counts 1–19, zero error/traceback outputs, three inline PNG figures, and all Stage 4.6 result gates true.
- Final Notebook bytes: `497238`.
- Final Notebook SHA-256: `4a5fc99b2c5418ce37191553d4958a7a7005c4ac208be096c94a64225bb4cf1b`.

## Protected-File Integrity

Before modification, every tracked file was fingerprinted; this superset covers all protected inputs read during implementation.

| Protected tracked path | Pre-flight bytes | Pre-flight SHA-256 |
| --- | --- | --- |
| .gitignore | 534 | 300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1 |
| README.md | 2245 | c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34 |
| data/README.md | 1004 | c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0 |
| data/raw/UCI_Credit_Card.csv | 2862995 | a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1 |
| notebooks/01_baseline_replication.ipynb | 1057 | ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20 |
| notebooks/01_luca_audit_and_data_understanding.ipynb | 78438 | 1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c |
| notebooks/02_train_test_split_and_eda.ipynb | 937648 | b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d |
| notebooks/03_feature_engineering_and_sanity_checks.ipynb | 142371 | 68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256 |
| notebooks/04_woe_iv_binning_method_design.ipynb | 52998 | 97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9 |
| notebooks/05_pay_categorical_binning.ipynb | 971553 | 79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796 |
| notebooks/06_continuous_binning.ipynb | 1901709 | 97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b |
| notebooks/07_woe_iv_variable_screening.ipynb | 850894 | 1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce |
| notebooks/08_candidate_woe_logistic.ipynb | 316111 | 48b79b025c31781fc4d38baa07778b67b5c2241e579237741f90e2921c7cf072 |
| references/README.md | 1109 | a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c |
| reports/figures/.gitkeep | 60 | 73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63 |
| requirements.txt | 95 | 5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b |
| task_records/Stage_1_Project_Foundation.md | 32398 | 0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d |
| task_records/Stage_2_Luca_Audit_and_Data_Understanding.md | 62868 | 3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7 |
| task_records/Stage_3.1_Train_Test_Split_and_EDA.md | 43454 | 62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0 |
| task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md | 77812 | b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b |
| task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md | 50459 | 2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851 |
| task_records/Stage_4.2_PAY_Categorical_Binning.md | 102407 | 9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60 |
| task_records/Stage_4.3_Continuous_Variable_Binning.md | 154540 | a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5 |
| task_records/Stage_4.4_WOE_IV_Variable_Screening.md | 185676 | 637fd696317a9f18620052dbb45a140e8aa8a196e9ed36767091d8e225fcc288 |
| task_records/Stage_4.5_Candidate_WOE_Logistic.md | 61711 | 89d2441c2873fc10968d365faadbd4318e252ca7b193634fac26d31642bca47e |
| task_records/TASK_REPORT_Task_0.1_Current-State_Audit.md | 25094 | 615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8 |

- Protected manifest SHA-256: `f7ca72abee35083037989e9fa0cb8f65000823244dfdd08cfc9ae5795ba1ab03`.
- Ignored Luca reference Notebook pre-flight: 805,686 bytes; SHA-256 `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd`.
- Git index pre-flight: 3,062 bytes; SHA-256 `85a54dc98d8dee911152ed54a6b1de2a7286148fac8a9aee609ceea852053d16`.
- Final byte-for-byte comparison against this baseline: all 26 protected tracked paths matched their pre-flight byte sizes and SHA-256 values; mismatches = `0`. The ignored Luca reference Notebook and Git index also retained the exact fingerprints above.

## Git Status and Artifact Scope

- Authorized created artifact: `notebooks/09_stratified_5fold_model_stability.ipynb`.
- Authorized created artifact: `task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md`.
- Modified pre-existing project files: none.
- Deleted, moved, staged, or committed files: none.
- Final repository root: `/Users/yu/2.工作/Risk Analysis Project`; branch: `main`; HEAD: `1e9c95f022de590eec7eb4b3300e01be3ce57f03`; parent and message unchanged.
- Final tracked diff: empty; staged diff: empty; Git index fingerprint unchanged.
- Final untracked list contains exactly `notebooks/09_stratified_5fold_model_stability.ipynb` and `task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md`.
- Final remote list: empty; no Stage 4.6 commit exists.
- No `git add`, commit, push, pull, fetch, reset, restore, checkout, stash, clean, amend, rebase, or remote mutation was performed.

## Deviations, Limitations, Warnings, and Incomplete Items

- Implementation deviation from the immutable Specification: none known.
- Numerical warning or blocked fold: none.
- Methodological limitation: the required full-Train design-level limitation is stated above.
- Environment limitation: the project `.venv` does not contain `statsmodels`; the existing registered `python3` Jupyter kernel at `/opt/anaconda3/bin/python` was therefore used, matching the established Stage 4 execution environment. No package was installed, upgraded, removed, or otherwise changed.
- Incomplete analytical item: none.
- Reviewer validation: pending and intentionally not prewritten.

## Immutable Architect Specification

The following authoritative UTF-8 block was extracted using its unique standalone markers and inserted without normalization, trimming, reformatting, correction, omission, or insertion. Mechanically derived fingerprint: `27,150` bytes; SHA-256 `216db767c8f7aad8d60e2df0b2f329c61b5f9ec9867ee34f92cba9fe34346029`.

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.6 ================

# Stage 4.6 — Stratified 5-Fold Model Stability

## 1. Stage Context

Current Stage 4 structure:

- Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
- Stage 4.2 — PAY + Categorical Binning ✅ CLOSED
- Stage 4.3 — Continuous Variable Binning ✅ CLOSED
- Stage 4.4 — WOE / IV Variable Screening ✅ CLOSED
- Stage 4.5 — Candidate WOE Logistic Regression ✅ CLOSED
- Stage 4.6 — Stratified 5-Fold Model Stability ← CURRENT
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit

Stage 4.5 final Reviewer verdict:

> PASS

Stage 4.5 milestone commit:

`1e9c95f022de590eec7eb4b3300e01be3ce57f03`

Short:

`1e9c95f`

Commit message:

`Fit candidate WOE logistic model`

Parent:

`afe8057a1d6532ebf4b5549ad2e4ce68ba48b709`

Branch:

`main`

Stage 4.5 post-commit state:

- working tree clean;
- staged changes none;
- untracked files none;
- remote none;
- no push.

---

# 2. Stage 4.6 Objective

Stage 4.6 answers two primary questions.

## A. Coefficient Stability

> If the Train sample changes modestly across five folds, do the ten fixed WOE predictors retain consistent multivariate coefficient direction and reasonably stable magnitude?

## B. Predictive Stability

> When each observation is predicted by a model that did not use that observation in that fold's model fitting or fold-specific WOE estimation, how stable are validation AUC / KS and pooled OOF AUC / KS?

Stage 4.6 is NOT:

- another feature-screening stage;
- final variable selection;
- a new binning stage;
- nested feature engineering;
- challenger modeling;
- Holdout Test evaluation.

Stage 4.6 produces stability evidence for Stage 4.7.

It does NOT delete predictors.

---

# 3. Frozen Candidate Predictor List

Use exactly these ten predictors in every fold, in canonical order:

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

Exactly ten predictors.

Every fold uses all ten.

No:

- additions;
- deletions;
- substitutions;
- fold-specific feature selection;
- alternate representations.

---

# 4. Stage 4.5 Full-Train Reference Model

Stage 4.6 must preserve the Stage 4.5 model as the full-Train comparison baseline.

Stage 4.5 model:

> fixed ten-variable, explicit-intercept, unpenalized maximum-likelihood WOE Logistic Regression.

Stage 4.5 full-Train coefficients approximately:

- `PAY_0` = -0.556304
- `severe_delinquent_months` = -0.513060
- `PAY_AMT1` = -0.193756
- `PAY_AMT2` = -0.222093
- `PAY_AMT3` = -0.245401
- `PAY_AMT4` = -0.165042
- `PAY_AMT5` = -0.097122
- `PAY_AMT6` = -0.164349
- `LIMIT_BAL` = -0.226161
- `avg_utilization` = -0.405621

Observed Stage 4.5 diagnostic status:

- Expected negative signs = 10 / 10
- Sign Concerns = 0
- Directionally Clear = 9
- Review in Stage 4.6 = 1

The sole Stage 4.5 Review variable:

`PAY_AMT5`

with approximately:

- coefficient = -0.097122
- 95% CI = [-0.251096, 0.056851]
- p-value = 0.21635

This is a stability-review item only.

Do not pre-assume it should be removed.

---

# 5. Stage 4.5 Apparent Performance Baseline

Stage 4.5 apparent full-Train metrics:

\[
AUC_{Apparent}=0.772417098
\]

\[
KS_{Apparent}=0.430307867
\]

These are comparison anchors only.

Stage 4.6 will compare them with OOF evidence.

Do NOT treat Stage 4.5 apparent metrics as generalization estimates.

---

# 6. New Stage 4.6 Artifacts

Create exactly:

`notebooks/09_stratified_5fold_model_stability.ipynb`

and:

`task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md`

Do not modify:

- Stage 4.5 Notebook;
- Stage 4.5 Task Report;
- any earlier tracked artifact.

Do not create additional data/model exports.

---

# 7. Git Pre-flight

Expected project root:

`/Users/yu/2.工作/Risk Analysis Project`

Expected:

- branch = `main`
- HEAD =
  `1e9c95f022de590eec7eb4b3300e01be3ce57f03`
- HEAD message =
  `Fit candidate WOE logistic model`
- tracked diff empty
- staged diff empty
- untracked empty before Stage 4.6 artifact creation
- remote none

If unrelated tracked/untracked changes exist:

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

# 8. Outer Train / Holdout Test Split

Replay the frozen project split exactly from:

`data/raw/UCI_Credit_Card.csv`

Required:

- Raw = 30,000
- Development Train = 24,000
- Holdout Test = 6,000
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

The 6,000-row Holdout Test may be used only for:

- row count;
- ID overlap;
- ID union coverage;
- frozen ID hash.

Then delete/seal Test.

Do not inspect:

- Test predictor values;
- Test target distribution;
- Test bin occupancy;
- Test WOE;
- Test prediction;
- Test AUC;
- Test KS;
- Test calibration;
- any Test risk relationship.

All Stage 4.6 work occurs only inside the 24,000-row Development Train.

---

# 9. K-Fold Design — FROZEN

Use exactly:

`StratifiedKFold(n_splits=5, shuffle=True, random_state=42)`

on the 24,000-row Development Train.

The target used for stratification is:

`default.payment.next.month`

The five fold assignments must be generated ONCE.

Do not randomly resample a new validation set on each round.

The five validation folds must satisfy:

\[
F_i\cap F_j=\varnothing
\]

for all:

\[
i\neq j
\]

and:

\[
F_1\cup F_2\cup F_3\cup F_4\cup F_5
=
Development\ Train
\]

Therefore:

> every Development-Train observation appears in exactly one validation fold and in the training portion of the other four rounds.

Expected validation size:

approximately / normally exactly:

`4,800`

per fold because 24,000 is divisible by five.

Do not hard-code class counts per fold.

Let `StratifiedKFold` determine the deterministic allocation.

---

# 10. Fold Assignment Audit

Before fitting models, create a five-row fold audit table containing:

- Fold
- Fold-Train row count
- Validation row count
- Fold-Train Good
- Fold-Train Bad
- Fold-Train bad rate
- Validation Good
- Validation Bad
- Validation bad rate
- Validation ID SHA-256

Verify:

- five validation sets pairwise disjoint;
- validation union = all 24,000 Train IDs;
- each ID appears in validation exactly once;
- no validation ID appears in its own fold-training set;
- total OOF coverage = 24,000.

Fold membership is deterministic because:

- `shuffle=True`;
- `random_state=42`.

Do not declare the newly derived fold hashes as external authorities before computing them.

Record them as reproducibility evidence.

---

# 11. Frozen Feature Engineering

For every fold, reproduce only the already-approved deterministic Stage 3 features needed for the ten predictors.

Relevant definitions include:

\[
severe\_delinquent\_months
=
\sum_i 1(PAY_i\ge2)
\]

and:

\[
avg\_utilization
=
\frac{
mean(max(BILL\_AMT1,0),...,max(BILL\_AMT6,0))
}{
LIMIT\_BAL
}
\]

Utilization above 1 remains uncapped.

Do not create any new features.

---

# 12. Frozen Bin Boundaries — CRITICAL

Stage 4.6 does NOT perform supervised re-binning.

Use the exact Stage 4.2 / Stage 4.3 frozen final bins in all five folds.

Conceptually:

\[
Bins = Fixed
\]

The cut points/categories must not depend on fold.

Do NOT:

- rerun supervised cut search;
- optimize bins within folds;
- merge bins because of fold performance;
- change bin definitions based on validation observations.

This preserves the frozen Stage 4 model design.

---

# 13. Fold-Specific WOE — CRITICAL

Although bin definitions remain fixed:

> WOE values MUST be recomputed separately inside each Fold-Train sample.

For each fold \(k\):

1. take only Fold-Train rows;
2. assign them to the frozen bins;
3. calculate Good / Bad bin counts using only Fold-Train target;
4. calculate fold-specific WOE;
5. map those Fold-Train-derived WOE values to:
   - Fold-Train rows;
   - corresponding Validation rows.

Never use Validation target to calculate WOE.

Never use the Stage 4.5 Full-Train WOE mapping directly as the fold mapping.

Conceptually:

\[
WOE^{(k)}
=
f(FoldTrain_k)
\]

not:

\[
WOE^{(k)}
=
f(FullTrain)
\]

This is required to prevent direct target leakage into fold validation predictions.

---

# 14. WOE Convention

Continue using:

\[
WOE_j
=
\ln\frac{DistGood_j}{DistBad_j}
\]

where:

- Good = 0
- Bad = 1

Therefore:

\[
WOE>0
\]

means safer.

Expected Logistic coefficient sign remains:

\[
\beta_j<0
\]

for every predictor.

---

# 15. Fold WOE Zero-Event Handling

The frozen bins are expected to retain observations from both classes in each Fold-Train sample.

Check every predictor × frozen bin × fold.

Three cases:

## Case A — both Good and Bad counts > 0

Use ordinary fold-specific WOE.

## Case B — total bin count > 0 but one class count = 0

Do not alter bin boundaries.

Use the exact Stage 4.1-approved 0.5 smoothing fallback.

The implementation must reproduce the Stage 4.1 smoothing convention exactly.

Record:

- fold;
- predictor;
- bin;
- Good count;
- Bad count;
- smoothing used.

Do not hide smoothing.

## Case C — frozen bin has zero total observations in Fold-Train

Fold-specific WOE cannot be empirically estimated from that training fold.

Do not invent a WOE value.

Do not borrow Full-Train WOE.

Return:

> BLOCKED — zero-support frozen bin in Fold-Train requires Architect review.

No bin modification is authorized in Stage 4.6.

Expected result:

> no zero-support frozen bin.

---

# 16. Fold WOE Integrity

For each fold, verify:

- Fold-Train WOE matrix shape = approximately 19,200 × 10;
- Validation WOE matrix shape = approximately 4,800 × 10;
- canonical column order;
- no missing;
- no non-finite;
- Fold-Train / Validation row alignment exact;
- target alignment exact;
- all Validation WOE values originate only from Fold-Train mappings.

Record whether smoothing occurred.

If none:

> smoothing occurrences = 0.

---

# 17. Fold Logistic Model

For each of the five folds fit exactly:

> one explicit-intercept, unpenalized maximum-likelihood Logistic Regression

using:

- all ten predictors;
- Fold-Train WOE matrix;
- Fold-Train target.

Use the same model family and implementation logic as Stage 4.5:

`statsmodels.Logit`

or exact equivalent unpenalized MLE implementation.

No:

- L1;
- L2;
- Elastic Net;
- class weights;
- sample balancing;
- predictor deletion.

Exactly five substantive fold Logistic fits.

---

# 18. Fold Model Numerical Sanity

For every fold verify:

- converged;
- intercept finite;
- all ten coefficients finite;
- all standard errors finite;
- covariance usable;
- design rank full;
- Fold-Validation predicted probabilities finite;
- probabilities within valid range;
- no singular matrix;
- no invalidating separation warning;
- no numerical overflow/underflow.

If any fold model is numerically invalid:

> BLOCKED.

Do not fix the issue by dropping predictors.

Return fold-specific evidence to Architect.

---

# 19. Fold Coefficient Detail Table

Create one complete 50-row table:

\[
5\ folds\times10\ predictors
\]

Columns:

- Fold
- Predictor
- Coefficient
- Expected Sign
- Sign Check
- Standard Error
- z-statistic
- p-value
- 95% CI Lower
- 95% CI Upper
- Directionally Clear? Yes/No

Definitions:

## Expected sign

\[
\beta<0
\]

## Sign Concern

\[
\beta>0
\]

## Directionally Clear

coefficient < 0 and entire 95% CI below 0.

Do not use these columns for automatic elimination.

---

# 20. Coefficient Stability Summary

Create one canonical ten-row predictor summary table containing:

- Predictor
- Stage 4.5 Full-Train Coefficient
- Fold 1 Coefficient
- Fold 2 Coefficient
- Fold 3 Coefficient
- Fold 4 Coefficient
- Fold 5 Coefficient
- Fold Mean Coefficient
- Fold Coefficient SD
- Fold Minimum
- Fold Maximum
- Negative Fold Count
- Positive Fold Count
- Directionally Clear Fold Count
- CI-Crosses-Zero Fold Count
- Maximum Absolute Deviation from Full-Train Coefficient
- Stage 4.7 Review Note

Use sample standard deviation across the five coefficients:

`ddof=1`

for the reported fold coefficient SD.

Do not create an arbitrary SD threshold.

---

# 21. Coefficient Comparison Caveat

Fold-specific WOE is recomputed in each Fold-Train sample.

Therefore the exact WOE scale can shift modestly between folds.

As a result:

> coefficient magnitude comparisons across folds are diagnostic stability evidence, not a formal identical-scale hypothesis test.

The most robust first-order evidence is:

- coefficient sign;
- whether CI is clearly below zero;
- whether magnitude changes materially or erratically;
- consistency relative to the Full-Train result.

Do not claim coefficient equality across folds.

---

# 22. Sign Stability

For each predictor report:

\[
Negative\ Fold\ Count
\]

out of five.

Ideal sign consistency:

\[
5/5
\]

If any fold coefficient becomes positive:

> flag `Sign Instability for Stage 4.7 Review`.

Do not automatically delete the predictor.

If positive in multiple folds:

> explicitly record repeated sign instability.

Still do not delete it in Stage 4.6.

---

# 23. Confidence-Interval Stability

For each predictor report:

\[
Directionally\ Clear\ Fold\ Count
\]

where a fold is Directionally Clear if:

\[
\beta<0
\]

and:

\[
CI_{Upper}<0
\]

Also report:

\[
CI\text{-}Crosses\text{-}Zero\ Fold\ Count
\]

Do not impose an automatic required number such as 3/5, 4/5, or 5/5.

These counts are evidence for Stage 4.7 Human decision.

---

# 24. PAY_AMT5 Priority Review

`PAY_AMT5` is the only variable entering Stage 4.6 with a Stage 4.5 Review flag.

Full-Train:

- coefficient ≈ -0.097122
- CI crosses zero
- p ≈ 0.21635

Stage 4.6 must specifically report:

- five coefficient values;
- negative/positive fold count;
- Directionally Clear fold count;
- CI-cross-zero fold count;
- fold coefficient mean / SD / range;
- comparison to Stage 4.5 coefficient.

Do NOT presuppose the conclusion.

Possible outcomes include:

- consistently negative but weak;
- sometimes clear, sometimes weak;
- sign instability;
- stronger fold evidence than Full-Train.

All remain observational until Stage 4.7.

---

# 25. PAY_0 / severe_delinquent_months Stability

These two variables were intentionally retained as:

- recent/current delinquency;
- historical serious-delinquency persistence.

Stage 4.5 showed both were strongly negative and Directionally Clear.

Stage 4.6 must verify:

- sign consistency;
- coefficient spread;
- CI consistency;
- whether one becomes unstable when fold WOE/multivariate fit varies.

Do not reopen dropped PAY variables.

---

# 26. Other PAY_AMT Stability

For:

- PAY_AMT1
- PAY_AMT2
- PAY_AMT3
- PAY_AMT4
- PAY_AMT6

record the same stability evidence.

Do not introduce an arbitrary "recent months only" rule.

Do not drop older months during Stage 4.6.

---

# 27. LIMIT_BAL / avg_utilization Stability

Evaluate:

- sign consistency;
- coefficient magnitude spread;
- confidence-interval consistency.

Do not reopen:

- BILL_AMT;
- current_utilization;
- max_utilization;
- demographics.

---

# 28. STB-01 — Coefficient Stability Plot

Create:

`STB-01 — Five-fold WOE Logistic coefficient stability`

For each of the ten predictors show:

- five fold coefficient estimates;
- Stage 4.5 Full-Train coefficient as a clearly distinguishable reference;
- zero reference line.

Optional:

95% fold CIs may be shown only if the figure remains readable.

The figure must emphasize:

> fold-to-fold coefficient direction and spread.

Do not encode Keep/Drop decisions.

---

# 29. Validation Predictions

For each fold:

1. fit WOE and Logistic using Fold-Train only;
2. predict probabilities on that fold's Validation rows;
3. store:
   - original Train row identity / ID;
   - fold number;
   - true target;
   - predicted probability.

Every Development-Train observation must receive exactly one Validation prediction.

These are:

> out-of-fold predictions.

Do not save them as an external CSV.

Keep them inside Notebook memory/output only as needed.

---

# 30. OOF Coverage Integrity

After all five folds, concatenate/reassemble the five validation prediction sets into one 24,000-row OOF structure.

Requirements:

- OOF rows = 24,000;
- unique IDs = 24,000;
- missing predictions = 0;
- duplicate OOF predictions per ID = 0;
- every Train ID appears exactly once;
- target exactly matches original Development-Train target;
- fold labels cover 1–5;
- each prediction came from a model whose Fold-Train did not contain that observation.

The OOF prediction vector is conceptually:

\[
(\hat p^{OOF}_1,...,\hat p^{OOF}_{24000})
\]

---

# 31. Fold Validation AUC

For each Validation fold compute:

\[
AUC_k
\]

using:

- positive class = Default 1;
- score = that fold model's predicted default probability.

Produce a five-row performance table containing:

- Fold
- Validation N
- Validation Bad Rate
- AUC
- KS

---

# 32. Fold Validation KS

Use the same deterministic definition as Stage 4.5:

\[
KS_k
=
\max(TPR-FPR)
\]

where:

- target positive class = Default 1;
- score = Validation predicted default probability.

Optionally record KS-max threshold for diagnostics.

Do not treat any KS threshold as an operational cutoff.

---

# 33. Fold Performance Summary

For the five fold AUC values report:

- mean;
- sample SD (`ddof=1`);
- minimum;
- maximum.

For five fold KS values report:

- mean;
- sample SD (`ddof=1`);
- minimum;
- maximum.

Purpose:

> assess fold-to-fold predictive stability.

Do not create an automatic acceptable-AUC-SD threshold.

---

# 34. Pooled OOF AUC

Using the complete 24,000-row OOF prediction structure calculate:

\[
AUC_{OOF}
=
AUC
(
y_{1:24000},
\hat p^{OOF}_{1:24000}
)
\]

This is NOT the arithmetic mean of the five fold AUC values.

The five fold AUCs measure fold-level performance.

The pooled OOF AUC evaluates all 24,000 OOF predictions together.

Report both.

---

# 35. Pooled OOF KS

Using the complete OOF target and OOF probability vectors calculate:

\[
KS_{OOF}
=
\max(TPR_{OOF}-FPR_{OOF})
\]

This is not the arithmetic mean of the five fold KS values.

Record:

- pooled OOF KS;
- OOF KS-max threshold for diagnostic reference only.

No threshold is operationally frozen.

---

# 36. Apparent vs OOF Gap

Compare Stage 4.5 apparent metrics with Stage 4.6 pooled OOF metrics.

Calculate signed:

\[
AUC\ Gap
=
AUC_{Apparent}-AUC_{OOF}
\]

and:

\[
KS\ Gap
=
KS_{Apparent}-KS_{OOF}
\]

Using Stage 4.5 anchors:

\[
AUC_{Apparent}=0.772417098
\]

\[
KS_{Apparent}=0.430307867
\]

Record the gaps.

Do not define an arbitrary pass/fail cutoff for the gaps.

Interpret them as apparent-vs-OOF optimism diagnostics.

---

# 37. OOF Methodological Limitation — REQUIRED

Stage 4.6 OOF predictions are stronger evidence than Stage 4.5 apparent predictions because each observation is excluded from:

- its fold-specific WOE estimation;
- its fold Logistic fitting.

However:

> Stage 4.6 is NOT a fully nested, leakage-free model-development estimate.

Reason:

Stage 4.2 / Stage 4.3 supervised bin boundaries and Stage 4.4 candidate selection were developed using the full 24,000-row Development Train.

Therefore Validation-fold observations influenced earlier development decisions at the design level.

The Task Report must state explicitly:

> Stage 4.6 OOF metrics are model-stability / cross-validated evidence under frozen full-Train-designed bins and predictors, not a final unbiased estimate of unseen-data performance.

The truly sealed unseen-data evaluation remains:

> Stage 5 Holdout Test.

Do not overclaim OOF performance.

---

# 38. OOF ROC Figure

Create:

`STB-02 — Pooled OOF ROC curve`

Requirements:

- 24,000 OOF predictions;
- pooled OOF AUC displayed;
- random reference line;
- clearly labeled as OOF under frozen full-Train-designed bins.

Do not label as Holdout/Test performance.

---

# 39. OOF KS Figure

Create:

`STB-03 — Pooled OOF KS diagnostic`

Use the same KS methodology as Stage 4.5.

Show:

- OOF TPR / FPR separation over valid probability thresholds;
- maximum KS highlighted;
- OOF KS value.

Do not optimize a business threshold.

---

# 40. Fold Performance Table Is Primary Stability View

The five-row Fold Validation AUC/KS table plus:

- mean;
- SD;
- min;
- max

is the primary predictive-stability evidence.

Do not create excessive extra plots simply to visualize five values.

STB-02 / STB-03 provide pooled OOF performance context.

---

# 41. Mean Fold AUC vs OOF AUC — Explicit Explanation

Task Report must explain:

## Mean Fold AUC

\[
\frac{AUC_1+\cdots+AUC_5}{5}
\]

answers:

> on average, how well did an individual validation fold rank Good/Bad observations?

## Pooled OOF AUC

calculates AUC once using:

> all 24,000 OOF predictions together.

It includes cross-fold ranking comparisons.

Therefore:

> Mean Fold AUC and OOF AUC are related but are not mathematically identical statistics.

The same conceptual distinction applies to:

- mean fold KS;
- pooled OOF KS.

---

# 42. No Variable Removal in Stage 4.6

This is a hard boundary.

Regardless of:

- sign instability;
- p-value;
- CI;
- coefficient dispersion;
- fold AUC;
- fold KS;
- OOF performance;

Stage 4.6 does NOT remove any predictor.

All ten predictors proceed to Stage 4.7 evidence review.

The only exception is methodological failure:

> if a fold cannot produce a numerically valid fixed ten-variable model, Stage 4.6 becomes BLOCKED.

Do not resolve this by silently dropping variables.

---

# 43. No Alternative Subset Search

Do not fit:

- 9-variable model;
- 8-variable model;
- model without PAY_AMT5;
- model with only PAY_AMT1–3;
- model without severe_delinquent_months;
- alternate utilization model;
- demographics;
- BILL model;
- any dropped Stage 4.4 variable.

Exactly the same ten-variable model architecture is used in all five folds.

---

# 44. No Re-binning

Do not:

- move cut points;
- merge bins for performance;
- split bins;
- search monotonicity again;
- optimize bins by fold.

Only fold-specific WOE values change.

Frozen bin definitions remain unchanged.

---

# 45. No Regularization

Do not use:

- L1;
- L2;
- Elastic Net;
- Ridge;
- LASSO.

All five models are unpenalized MLE Logistic.

---

# 46. No Imbalance Intervention

Do not use:

- class weighting;
- SMOTE;
- oversampling;
- undersampling;
- synthetic observations.

Stratification is allowed and required for fold construction.

Stratification is not class rebalancing.

---

# 47. No Calibration

Do not compute:

- calibration curve;
- Brier score;
- calibration slope/intercept;
- Hosmer-Lemeshow decision.

Stage 4.6 is stability-focused.

---

# 48. No Operational Threshold Selection

Do not choose:

- approval threshold;
- classification cutoff;
- optimized F1 threshold;
- cost-based threshold;
- operational KS threshold.

Fold / OOF KS-max thresholds may be reported only as metric diagnostics.

---

# 49. No Holdout Test Evaluation

The 6,000-row Test remains sealed.

Do not apply:

- frozen bins;
- WOE;
- Logistic coefficients;
- predictions

to Holdout Test in Stage 4.6.

Stage 5 owns Holdout evaluation.

---

# 50. Stage 4.7 Handoff — Variable Stability Table

At the end of Stage 4.6 create one final ten-row Stage 4.7 handoff table containing:

- Predictor
- Stage 4.5 Full-Train Coefficient
- Fold Mean Coefficient
- Fold Coefficient SD
- Fold Min
- Fold Max
- Negative Folds / 5
- Positive Folds / 5
- Directionally Clear Folds / 5
- CI-Crosses-Zero Folds / 5
- Max Absolute Deviation from Full-Train Coefficient
- Stage 4.5 Status
- Stage 4.6 Stability Evidence Summary
- Stage 4.7 Review Note
- Continuation

For every predictor:

> Continuation = `Continue to Stage 4.7`

Do not include Keep/Drop.

---

# 51. Stage 4.7 Review Notes

Stage 4.6 may identify evidence such as:

- `5/5 negative; direction stable`
- `negative in all folds but frequently CI crosses zero`
- `one fold sign reversal`
- `multiple-fold sign instability`
- `coefficient magnitude highly variable descriptively`

Do not turn these phrases into automatic deletion rules.

Stage 4.7 will make the Human final-variable decision using:

- Stage 4.4 screening evidence;
- Stage 4.5 Full-Train multivariate diagnostics;
- Stage 4.6 fold stability evidence.

---

# 52. Notebook Structure

Create:

`notebooks/09_stratified_5fold_model_stability.ipynb`

Recommended structure:

1. Stage 4.6 objective / boundaries
2. imports / environment
3. outer Train/Test split replay
4. Test identity verification and sealing
5. frozen 10 predictors
6. frozen feature / bin definitions
7. StratifiedKFold setup
8. generate folds once
9. fold membership audit
10. fold-loop methodology
11. Fold 1–5 frozen-bin / Fold-Train WOE calculation
12. zero-event / smoothing checks
13. Fold 1–5 Logit fitting
14. numerical sanity by fold
15. 50-row coefficient detail table
16. ten-row coefficient stability summary
17. STB-01 coefficient stability plot
18. fold validation AUC/KS table
19. fold performance summary
20. construct 24,000 OOF predictions
21. OOF coverage integrity
22. pooled OOF AUC
23. STB-02 OOF ROC
24. pooled OOF KS
25. STB-03 OOF KS
26. apparent vs OOF gap
27. methodological limitation
28. PAY_AMT5 focused stability review
29. Stage 4.7 handoff table
30. explicit no-selection / no-Holdout boundary

Notebook must clean-run from raw CSV.

---

# 53. Task Report Structure

Create:

`task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md`

Include:

- Stage purpose;
- Git baseline;
- Stage 4.5 reference model;
- Stage 4.5 apparent AUC / KS;
- frozen Train/Test identity;
- Holdout sealing;
- frozen 10 predictors;
- exact StratifiedKFold configuration;
- explanation that folds are generated once, not independently resampled each round;
- five-fold membership audit;
- pairwise disjoint / union coverage checks;
- fixed bins;
- fold-specific WOE methodology;
- zero-event/smoothing handling;
- fold model methodology;
- fold numerical sanity;
- complete 50-row coefficient table;
- coefficient stability summary;
- STB-01;
- PAY_AMT5 focused review;
- fold AUC/KS table;
- fold mean/SD/min/max;
- OOF definition;
- OOF coverage integrity;
- pooled OOF AUC;
- STB-02;
- pooled OOF KS;
- STB-03;
- mean-fold versus pooled-OOF explanation;
- apparent-vs-OOF gap;
- methodological limitation from full-Train-developed bins/selection;
- Stage 4.7 handoff;
- confirmation all ten continue;
- no feature deletion;
- no alternate subset;
- no re-binning;
- no regularization;
- no resampling;
- no calibration;
- no threshold optimization;
- no Holdout predictive analysis;
- clean-run;
- protected-file integrity;
- Git status;
- Reviewer handoff.

---

# 54. Immutable Architect Specification

Append this entire Stage 4.6 Architect Specification verbatim to the Task Report between standalone lines:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.6 ================`

and

`================ END ARCHITECT SPECIFICATION — STAGE 4.6 ================`

Provenance rule:

> Architect source text is authoritative.

Do not predeclare any byte count or SHA-256 as independent authority.

If fingerprinting is recorded:

- derive it mechanically from the authoritative embedded UTF-8 block;
- document exact standalone-marker boundary;
- do not modify Specification text to fit a hash.

---

# 55. Reviewer Workflow

After Codex completes Stage 4.6:

> send the Stage 4.6 Notebook and Task Report to Reviewer.

Reviewer independently validates:

- Git baseline;
- outer split identity;
- Holdout Test sealing;
- exact ten predictors;
- fold configuration;
- deterministic five-fold membership;
- disjointness and complete OOF coverage;
- frozen bin definitions;
- fold-specific WOE;
- no Validation-target leakage into WOE;
- zero-event handling;
- five unpenalized Logistic models;
- fold numerical sanity;
- coefficient detail table;
- stability summary;
- STB-01;
- fold AUC/KS;
- OOF predictions;
- OOF AUC/KS;
- STB-02 / STB-03;
- apparent-vs-OOF gap;
- methodological limitation wording;
- PAY_AMT5 review;
- all ten continue to Stage 4.7;
- no variable deletion;
- no re-binning;
- no regularization;
- no subset search;
- no Holdout evaluation;
- clean-run;
- protected-file integrity;
- immutable Specification;
- Git scope.

---

# 56. Reviewer FAIL

If Reviewer finds a failure:

- no commit;
- return Required Fixes;
- no Stage 4.7.

---

# 57. Reviewer PASS

If Stage 4.6 passes:

In the same Reviewer round:

1. append final Stage 4.6 Reviewer Result;
2. stage exactly:
   - `notebooks/09_stratified_5fold_model_stability.ipynb`
   - `task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md`
3. commit Stage 4.6;
4. verify post-commit Git state.

Recommended commit message:

`Validate candidate WOE model stability`

No push.

---

# 58. Protected Scope

Do not modify:

- raw data;
- Stage 0–4.5 notebooks;
- Stage 0–4.5 Task Reports;
- README;
- requirements;
- references;
- other tracked project files.

Only create:

- `notebooks/09_stratified_5fold_model_stability.ipynb`
- `task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md`

---

# 59. No External Artifacts

Do not create:

- OOF prediction CSV;
- coefficient CSV;
- fold CSV;
- WOE CSV;
- model pickle;
- joblib;
- external PNG;
- HTML;
- PDF;
- `src/`;
- `tests/`.

All analytical outputs remain embedded in Notebook / Task Report.

---

# 60. Codex Git Rules

Codex:

- no git add;
- no commit;
- no push.

Expected completion Git state:

- HEAD remains:
  `1e9c95f022de590eec7eb4b3300e01be3ce57f03`
- branch = `main`
- tracked diff empty
- staged diff empty
- exactly two authorized Stage 4.6 artifacts untracked
- remote none

Commit belongs to Reviewer after PASS.

---

# 61. Stage 4.6 Completion Criteria

All must pass:

1. correct project root;
2. branch `main`;
3. HEAD = `1e9c95f...`;
4. clean Stage 4.5 baseline;
5. exactly two Stage 4.6 artifacts created;
6. outer 24,000 / 6,000 split exact;
7. Train hash exact;
8. Test hash exact;
9. Holdout Test sealed;
10. Train Good = 18,691;
11. Train Bad = 5,309;
12. exact ten predictors;
13. exact canonical order;
14. `StratifiedKFold(5, shuffle=True, random_state=42)`;
15. folds generated once;
16. five validation folds pairwise disjoint;
17. validation union = 24,000 Train IDs;
18. every Train ID validation exactly once;
19. fold audit table complete;
20. frozen Stage 4 bins preserved;
21. no re-binning;
22. fold-specific WOE computed only from Fold-Train;
23. no Validation target used in WOE estimation;
24. Fold-Train WOE mapped to corresponding Validation;
25. no Full-Train WOE leakage into Validation mapping;
26. zero-bin-support checks complete;
27. smoothing, if needed, uses exact Stage 4.1 fallback;
28. smoothing occurrences documented;
29. five fixed ten-variable unpenalized Logistic models;
30. intercept in every model;
31. all five converge;
32. all coefficient estimates finite;
33. all SEs finite;
34. all fold probabilities finite;
35. no invalidating separation/numerical failures;
36. complete 50-row coefficient table;
37. ten-row coefficient stability summary;
38. negative/positive fold counts;
39. Directionally Clear fold counts;
40. CI-cross-zero fold counts;
41. fold coefficient mean;
42. fold coefficient sample SD;
43. fold coefficient min/max;
44. max absolute deviation from Full-Train coefficient;
45. STB-01 produced;
46. PAY_AMT5 specifically reviewed;
47. five Validation AUC values;
48. five Validation KS values;
49. fold performance mean/SD/min/max;
50. OOF predictions = 24,000;
51. no missing OOF predictions;
52. no duplicated OOF IDs;
53. every OOF prediction comes from excluding that observation from fold model/WOE estimation;
54. pooled OOF AUC computed;
55. pooled OOF KS computed;
56. STB-02 produced;
57. STB-03 produced;
58. mean-fold vs pooled-OOF distinction documented;
59. apparent AUC gap computed;
60. apparent KS gap computed;
61. OOF methodological limitation documented;
62. no claim of fully unbiased generalization performance;
63. all ten continue to Stage 4.7;
64. no feature deletion;
65. no alternate subset;
66. no new feature engineering;
67. no regularization;
68. no class weighting;
69. no SMOTE/resampling;
70. no calibration;
71. no operational threshold optimization;
72. no Holdout predictive analysis;
73. Stage 4.7 handoff table complete;
74. Notebook clean-runs;
75. Task Report complete;
76. immutable Architect Specification preserved;
77. protected files unchanged;
78. staged diff empty;
79. no commit;
80. no push;
81. final Reviewer handoff reached.

---

# 62. Codex Completion Report

Return a concise Completion Report containing:

- pre-flight result;
- branch / HEAD;
- outer Train/Test identity;
- Test sealing;
- exact ten predictors;
- StratifiedKFold configuration;
- five Validation row counts;
- five Validation Good/Bad counts;
- five Validation ID hashes;
- fold disjointness / union result;
- frozen-bin confirmation;
- fold-specific WOE confirmation;
- zero-support-bin count;
- zero-class-bin count;
- smoothing occurrence count;
- five model convergence results;
- numerical warnings, if any;
- five-fold coefficient sign summary for each predictor;
- Directionally Clear fold count for each predictor;
- Fold Mean / SD for each coefficient;
- PAY_AMT5 stability summary;
- five Validation AUC values;
- Fold AUC mean / SD / min / max;
- five Validation KS values;
- Fold KS mean / SD / min / max;
- OOF coverage = 24,000 / 24,000;
- pooled OOF AUC;
- pooled OOF KS;
- Stage 4.5 apparent-minus-OOF AUC gap;
- Stage 4.5 apparent-minus-OOF KS gap;
- STB-01 status;
- STB-02 status;
- STB-03 status;
- confirmation OOF limitation documented;
- confirmation all ten continue to Stage 4.7;
- confirmation:
  - no variable deletion
  - no re-binning
  - no Full-Train WOE leakage into validation
  - no alternative subset search
  - no regularization
  - no class weighting/resampling
  - no calibration
  - no operational threshold optimization
  - no Holdout predictive analysis
- Notebook clean-run;
- protected-file integrity;
- final Git state;
- no commit;
- no push.

If successful, final line:

`Stage 4.6 implementation complete — fixed ten-variable Candidate WOE Logistic evaluated under deterministic stratified five-fold stability analysis; all ten predictors continue to Stage 4.7 final-variable review; awaiting Reviewer validation.`

If any fold model / WOE construction is methodologically invalid, final line:

`Stage 4.6 BLOCKED by fold-specific WOE/model validity issue — no predictor was automatically removed and Architect review is required.`

================ END ARCHITECT SPECIFICATION — STAGE 4.6 ================

# Reviewer Result

## Status

`AWAITING INDEPENDENT REVIEWER VALIDATION`

## Verdict

_Reserved for Reviewer. Codex has not prewritten PASS or FAIL._

## Reviewer Notes

_To be completed by Reviewer._

# REVIEWER RESULT

## Verdict

PASS WITH FIXES

## Requirement Check

- Core Stage 4.6 analytical requirements: satisfied. The implementation uses the frozen ten predictors and frozen bins, deterministic `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)`, Fold-Train-only WOE estimation, five explicit-intercept unpenalized `statsmodels.Logit` fits, complete coefficient/performance/OOF evidence, the required methodological limitation, and continuation of all ten predictors to Stage 4.7 without selection.
- Holdout and scope boundaries: satisfied. Holdout use is limited to the authorized split-identity checks; protected files remain unchanged; exactly the two authorized Stage 4.6 artifacts are present; no staging, commit, remote, or push occurred.
- Notebook execution and numerical validity: satisfied. Independent clean execution completed with 19 sequential code-cell executions, zero error outputs, five converged full-rank models, no zero-support or zero-class fold bins, and complete 24,000-row OOF coverage.
- Immutable Architect Specification content: satisfied. The embedded standalone-marker block is byte-for-byte identical to the authoritative attachment.
- Immutable Specification fingerprint statement: not satisfied. The Task Report claims that the embedded marker-delimited block is `34,795` bytes with SHA-256 `8197c634a115afe19ca56809fad7d08f913f513227d7d51025597a50eb19179d`, but the actual inclusive standalone-marker UTF-8 block is `27,150` bytes with SHA-256 `216db767c8f7aad8d60e2df0b2f329c61b5f9ec9867ee34f92cba9fe34346029`.

## Independent Verification

- Confirmed project root, branch `main`, Stage 4.5 baseline HEAD `1e9c95f022de590eec7eb4b3300e01be3ce57f03`, empty tracked/staged diffs, exactly two authorized untracked artifacts, and no remote.
- Read the complete embedded Architect Specification and compared its marker-delimited bytes with `/Users/yu/.codex/attachments/32d5ab01-0420-4ffc-b7ca-4eadd7d6ea75/pasted-text.txt`; content equality passed, while the recorded fingerprint did not reproduce.
- Inspected the Notebook's fold construction, feature/bin mappings, Fold-Train-only WOE helper, model loop, coefficient/OOF assembly, performance calculations, and hard-boundary assertions.
- Independently reran the Notebook from raw CSV in an isolated kernel: 39 total cells, 19 code cells with execution counts 1–19, zero errors, and three inline PNG outputs.
- Independently recomputed the split, fold membership hashes, frozen-bin Fold-Train WOE, five Logit fits, fold AUC/KS, and pooled OOF metrics from the raw CSV. Results reproduced the report: fold AUCs `0.776925289`, `0.773681556`, `0.765525765`, `0.767137703`, `0.773806627`; fold KS values `0.448388852`, `0.437295390`, `0.429064154`, `0.427995575`, `0.438353390`; pooled OOF AUC `0.771230165`; pooled OOF KS `0.429462039`; all 50 predictor coefficients were negative; zero-support bins = 0; zero-class bins = 0.
- Visually checked STB-01, STB-02, and STB-03. They show the required coefficient stability, pooled OOF ROC, and pooled OOF KS evidence with appropriate OOF/full-Train-design labeling and no Keep/Drop or operational-threshold decision.
- Recomputed all 26 protected tracked-file hashes plus the ignored Luca Notebook and Git index fingerprints; all match the recorded pre-flight values.

## Issues Found

1. The Task Report's mechanically derived byte count and SHA-256 for the embedded Architect Specification are incorrect, despite the embedded Specification text itself being exact. This is a limited audit-documentation defect and does not change the analytical results.

## Required Fixes

1. Correct the fingerprint statement in the Task Report to the actual inclusive standalone-marker block values — `27,150` UTF-8 bytes and SHA-256 `216db767c8f7aad8d60e2df0b2f329c61b5f9ec9867ee34f92cba9fe34346029` — or remove the optional fingerprint claim. Do not modify the embedded Architect Specification text.
2. Return the corrected Task Report for focused Reviewer re-verification. No Notebook or analytical rerun is required unless other content changes.

No local commit was created because the Stage 4.6 artifact set requires this limited correction before a final `PASS` closeout.

## Architect Decision Needed

None

# REVIEWER RESULT — FOCUSED RE-REVIEW

## Verdict

PASS

## Requirement Check

- The sole issue from the prior `PASS WITH FIXES` review has been corrected: the Task Report now records the actual inclusive standalone-marker Architect Specification fingerprint as `27,150` UTF-8 bytes with SHA-256 `216db767c8f7aad8d60e2df0b2f329c61b5f9ec9867ee34f92cba9fe34346029`.
- The embedded Architect Specification remains byte-for-byte identical to the authoritative source attachment.
- The Stage 4.6 Notebook is unchanged from the independently validated artifact (SHA-256 `4a5fc99b2c5418ce37191553d4958a7a7005c4ac208be096c94a64225bb4cf1b`). All analytical, Holdout-sealing, scope, and Stage 4.7 handoff requirements therefore retain the prior independently verified PASS status.
- Exactly the two authorized Stage 4.6 artifacts remain in scope; protected tracked files remain unchanged; no prohibited project or Git operation was introduced.

## Independent Verification

- Mechanically re-extracted the marker-delimited Architect Specification from the corrected Task Report and independently reproduced `27,150` bytes and SHA-256 `216db767c8f7aad8d60e2df0b2f329c61b5f9ec9867ee34f92cba9fe34346029`.
- Compared that block with `/Users/yu/.codex/attachments/32d5ab01-0420-4ffc-b7ca-4eadd7d6ea75/pasted-text.txt`; exact byte equality passed.
- Confirmed the Notebook hash is unchanged from the prior clean-run and independent recomputation review.
- Recomputed all 26 protected tracked-file fingerprints; mismatches = `0`.
- Confirmed branch `main`, baseline HEAD `1e9c95f022de590eec7eb4b3300e01be3ce57f03`, empty tracked and staged diffs, exactly two authorized untracked Stage 4.6 artifacts, and no remote before Reviewer closeout.

## Issues Found

None. The prior limited fingerprint-documentation defect is resolved.

## Required Fixes

None

## Architect Decision Needed

None
