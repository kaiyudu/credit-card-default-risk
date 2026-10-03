# Stage 4.7 — Final Logistic Freeze + Full-Train Refit

## Execution Status

`IMPLEMENTATION COMPLETE — AWAITING INDEPENDENT REVIEWER VALIDATION`

Codex completed the authorized Stage 4.7 implementation. This report records no Reviewer PASS and does not execute Stage 5.

## Prompt-Source Marker Validation

- Exact standalone BEGIN marker count: `1`.
- Exact standalone END marker count: `1`.
- BEGIN precedes END: yes.
- Both marker lines begin directly with `=`: yes.
- Leading backslash or whitespace: none.
- Blockquote, Markdown bullet, or inline-backtick prefix: none.
- Extracted body begins with the required Stage 4.7 heading and continues through complete Section 48: yes.
- Placeholder inside immutable block: none.
- Authoritative extraction: `23,488` UTF-8 bytes; SHA-256 `2be5b2dd43277591e5bbd25c1ccc8ba424b134aa18eaa26ed7e2b0b8a2ca0581`; exact boundary is the first byte of the unique standalone BEGIN line through the final byte of the unique standalone END line, inclusive, excluding any following line terminator.

## Stage Purpose

Stage 4.7 synthesizes accepted Stage 4.4–4.6 evidence, records the frozen Human decision, freezes the primary ten-variable WOE Logistic specification, and refits it once on the full 24,000-row Development Train for Stage 5 handoff. It is not screening, model search, cross-validation, re-binning, challenger modeling, or Holdout evaluation.

## Git Baseline and Stage 4.6 Milestone

- Root: `/Users/yu/2.工作/Risk Analysis Project`.
- Branch: `main`.
- HEAD: `bac1d2f0af6693b9443b80322eb49891992ccfab`.
- Parent: `1e9c95f022de590eec7eb4b3300e01be3ce57f03`.
- HEAD message: `Validate candidate WOE model stability`.
- Stage 4.6 final focused re-review verdict: `PASS`.
- Pre-flight tracked diff: empty.
- Pre-flight staged diff: empty.
- Pre-flight untracked list: empty.
- Pre-flight remote list: empty.
- Both Stage 4.7 artifacts were absent; no Stage 4.7 commit existed.

Recorded read-only commands included `git status --short`, both tracked/staged diff summaries, untracked listing, remote listing, branch/HEAD/parent/message checks, and the Stage 4.6 report tail.

## Frozen Outer Split Identity and Holdout Sealing

- Raw rows: `30,000`.
- Development Train: `24,000` rows.
- Holdout Test: `6,000` rows.
- Development Good / Bad: `18,691 / 5,309`; bad rate `22.1208%`.
- Train ID SHA-256: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`.
- Test ID SHA-256: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- ID overlap: zero; union: all 30,000 IDs.
- Holdout status: `SEALED — Holdout deleted after row/ID/union/hash identity checks only; Stage 5 only`.
- Holdout was not feature-engineered, binned, WOE-mapped, scored, evaluated, calibrated, thresholded, or inspected for risk relationships.

## Human Final-Variable Decision

The frozen Human decision is **KEEP ALL 10 PREDICTORS** in canonical order:

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

The decision combines Stage 4.4 signal/redundancy/governance evidence, Stage 4.5 multivariate behavior, Stage 4.6 fold stability, and business/temporal coherence. It is not an automatic p-value, CI, IV, VIF, or coefficient-SD decision.

## Stage 4.4–4.6 Evidence Synthesis

| Predictor | Stage 4.4 Decision | Stage 4.5 Status | Stage 4.6 Negative Folds / 5 | Stage 4.6 Directionally Clear Folds / 5 | Stage 4.6 CI-Crosses-Zero Folds / 5 | Structural / Interpretability Note | Stage 4.7 Final Decision |
| --- | --- | --- | --- | --- | --- | --- | --- |
| PAY_0 | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Recent/current delinquency status retained as a primary behavioral signal. | Retain |
| severe_delinquent_months | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Historical serious-delinquency persistence complements current status. | Retain |
| PAY_AMT1 | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Month-specific payment-history component in the coherent six-month family. | Retain |
| PAY_AMT2 | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Month-specific payment-history component in the coherent six-month family. | Retain |
| PAY_AMT3 | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Month-specific payment-history component in the coherent six-month family. | Retain |
| PAY_AMT4 | Keep / candidate approved | Directionally Clear | 5/5 | 4/5 | 1/5 | 5/5 negative and 4/5 clear; part of the continuum of monthly conditional strength. | Retain |
| PAY_AMT5 | Keep / candidate approved | Review in Stage 4.6 | 5/5 | 0/5 | 5/5 | Weaker conditional evidence but 5/5 negative; no defensible month-5 cutoff, so temporal-family coherence supports retention. | Retain |
| PAY_AMT6 | Keep / candidate approved | Directionally Clear | 5/5 | 2/5 | 3/5 | 5/5 negative and 2/5 clear; adjacent evidence confirms no clean month-5 cutoff. | Retain |
| LIMIT_BAL | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Distinct credit-capacity signal retained under approved governance screening. | Retain |
| avg_utilization | Keep / candidate approved | Directionally Clear | 5/5 | 5/5 | 0/5 | Distinct balance-to-limit burden signal; uncapped utilization above one preserved. | Retain |

Accepted Stage 4.6 context: pooled OOF AUC `0.771230165`, pooled OOF KS `0.429462039`, mean fold AUC `0.771415388` (sample SD `0.004852803`), mean fold KS `0.436219472` (sample SD `0.008255848`). The apparent-minus-OOF AUC/KS gaps were `0.001186933` and `0.000845828`. This supports stable discrimination under the accepted fold structure, but is not fully unbiased unseen-data evidence because supervised bins and candidate selection used full Development Train.

## PAY_AMT5 Retention and PAY_AMT4/PAY_AMT6 Context

`PAY_AMT5` is retained despite weaker conditional statistical evidence. Stage 4.5 recorded coefficient approximately `-0.097122437`, a zero-crossing 95% CI, and p≈`0.21635`. Stage 4.6 coefficients were `-0.125774`, `-0.057477`, `-0.098118`, `-0.041387`, and `-0.160361`: 5/5 negative, 0/5 positive, 0/5 Directionally Clear, 5/5 CI-crossing, mean approximately `-0.096623`, SD approximately `0.048729`. Thus the weakness is conditional strength, not directional instability.

Retention is supported by no sign instability, no defensible natural month-5 cutoff in Stage 4.4, the coherent six-month PAY_AMT family, lack of a defensible reason to remove month 5 while retaining adjacent months 4 and 6, and the project's rejection of automatic p-value/CI elimination. `PAY_AMT4` was 5/5 negative and 4/5 Directionally Clear; `PAY_AMT6` was 5/5 negative and 2/5 Directionally Clear. Together they show a continuum, not a new cutoff. No claim is made that `PAY_AMT5` is statistically strong or without weakness.

No 10-vs-9, recent-month-only, or any alternative subset model was fitted.

## Final Feature Definitions and Frozen Bins

- `severe_delinquent_months = sum(PAY_i >= 2)` across `PAY_0`, `PAY_2`–`PAY_6`.
- `avg_utilization = mean(max(BILL_AMT1,0), …, max(BILL_AMT6,0)) / LIMIT_BAL`; values above 1 remain uncapped.

Frozen bins:

- `PAY_0`: `[-2,-1,0]`, `[1]`, `[2+]`.
- `severe_delinquent_months`: `0`, `1`, `2`, `3`, `4+`.
- Each `PAY_AMT1`–`PAY_AMT6`: `=0`, `(0,5000]`, `(5000,20000]`, `(20000,+inf)`.
- `LIMIT_BAL`: `(-inf,30000]`, `(30000,140000]`, `(140000,240000]`, `(240000,+inf)`.
- `avg_utilization`: `=0`, `(0,0.2]`, `(0.2,0.6]`, `(0.6,1]`, `(1,+inf)`.

No new feature or re-binning operation occurred.

## Final WOE Convention and 41-Row Full-Train Mapping

WOE is `ln(DistGood / DistBad)`, Good=0, Bad=1. Positive WOE denotes safer bins. The mapping is computed from all 24,000 Development-Train rows. IV contribution is shown only for provenance and did not reopen selection.

| Predictor | Bin Order | Bin Key | Bin Label / Definition | Train Count | Train Share | Good Count | Bad Count | Bad Rate | DistGood | DistBad | Final Full-Train WOE | IV Contribution (provenance only) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PAY_0 | 1 | [-2,-1,0] | raw PAY_0 in {-2,-1,0} | 18506 | 77.1083% | 15966 | 2540 | 13.7253% | 0.854207908 | 0.478432850 | 0.579658749 | 0.217821300 |
| PAY_0 | 2 | [1] | raw PAY_0 = 1 | 3000 | 12.5000% | 1969 | 1031 | 34.3667% | 0.105344818 | 0.194198531 | -0.611642036 | 0.054346666 |
| PAY_0 | 3 | [2+] | raw PAY_0 in {2,…,8} | 2494 | 10.3917% | 756 | 1738 | 69.6872% | 0.040447274 | 0.327368619 | -2.091087561 | 0.599977656 |
| severe_delinquent_months | 1 | 0 | =0 | 17276 | 71.9833% | 15100 | 2176 | 12.5955% | 0.807875448 | 0.409870032 | 0.678567784 | 0.270073653 |
| severe_delinquent_months | 2 | 1 | =1 | 2799 | 11.6625% | 1887 | 912 | 32.5831% | 0.100957680 | 0.171783763 | -0.531535076 | 0.037646548 |
| severe_delinquent_months | 3 | 2 | =2 | 1279 | 5.3292% | 708 | 571 | 44.6443% | 0.037879193 | 0.107553212 | -1.043583747 | 0.072710673 |
| severe_delinquent_months | 4 | 3 | =3 | 861 | 3.5875% | 400 | 461 | 53.5424% | 0.021400674 | 0.086833679 | -1.400572127 | 0.091643642 |
| severe_delinquent_months | 5 | 4+ | in {4,5,6} | 1785 | 7.4375% | 596 | 1189 | 66.6106% | 0.031887004 | 0.223959314 | -1.949265861 | 0.374399997 |
| PAY_AMT1 | 1 | 0 | =0 | 4180 | 17.4167% | 2683 | 1497 | 35.8134% | 0.143545022 | 0.281974006 | -0.675166165 | 0.093462567 |
| PAY_AMT1 | 2 | (0,5k] | (0,5000] | 13723 | 57.1792% | 10739 | 2984 | 21.7445% | 0.574554598 | 0.562064419 | 0.021978662 | 0.000274517 |
| PAY_AMT1 | 3 | (5k,20k] | (5000,20000] | 5055 | 21.0625% | 4323 | 732 | 14.4807% | 0.231287786 | 0.137879073 | 0.517285740 | 0.048318995 |
| PAY_AMT1 | 4 | >20k | (20000,+inf) | 1042 | 4.3417% | 946 | 96 | 9.2131% | 0.050612594 | 0.018082501 | 1.029255747 | 0.033481785 |
| PAY_AMT2 | 1 | 0 | =0 | 4324 | 18.0167% | 2887 | 1437 | 33.2331% | 0.154459365 | 0.270672443 | -0.560978337 | 0.065193019 |
| PAY_AMT2 | 2 | (0,5k] | (0,5000] | 13854 | 57.7250% | 10766 | 3088 | 22.2896% | 0.575999144 | 0.581653795 | -0.009769243 | 0.000055242 |
| PAY_AMT2 | 3 | (5k,20k] | (5000,20000] | 4740 | 19.7500% | 4043 | 697 | 14.7046% | 0.216307314 | 0.131286495 | 0.499318228 | 0.042452445 |
| PAY_AMT2 | 4 | >20k | (20000,+inf) | 1082 | 4.5083% | 995 | 87 | 8.0407% | 0.053234177 | 0.016387267 | 1.178195988 | 0.043412881 |
| PAY_AMT3 | 1 | 0 | =0 | 4780 | 19.9167% | 3236 | 1544 | 32.3013% | 0.173131454 | 0.290826898 | -0.518677083 | 0.061045930 |
| PAY_AMT3 | 2 | (0,5k] | (0,5000] | 14043 | 58.5125% | 11009 | 3034 | 21.6051% | 0.589000054 | 0.571482388 | 0.030192608 | 0.000528904 |
| PAY_AMT3 | 3 | (5k,20k] | (5000,20000] | 4168 | 17.3667% | 3539 | 629 | 15.0912% | 0.189342464 | 0.118478056 | 0.468829593 | 0.033223332 |
| PAY_AMT3 | 4 | >20k | (20000,+inf) | 1009 | 4.2042% | 907 | 102 | 10.1090% | 0.048526029 | 0.019212658 | 0.926531006 | 0.027159747 |
| PAY_AMT4 | 1 | 0 | =0 | 5141 | 21.4208% | 3563 | 1578 | 30.6944% | 0.190626505 | 0.297231117 | -0.444193967 | 0.047353126 |
| PAY_AMT4 | 2 | (0,5k] | (0,5000] | 14201 | 59.1708% | 11176 | 3025 | 21.3013% | 0.597934835 | 0.569787154 | 0.048218899 | 0.001357250 |
| PAY_AMT4 | 3 | (5k,20k] | (5000,20000] | 3693 | 15.3875% | 3101 | 592 | 16.0303% | 0.165908726 | 0.111508759 | 0.397334653 | 0.021614992 |
| PAY_AMT4 | 4 | >20k | (20000,+inf) | 965 | 4.0208% | 851 | 114 | 11.8135% | 0.045529934 | 0.021472970 | 0.751575049 | 0.018080614 |
| PAY_AMT5 | 1 | 0 | =0 | 5387 | 22.4458% | 3812 | 1575 | 29.2371% | 0.203948424 | 0.296666039 | -0.374739917 | 0.034744991 |
| PAY_AMT5 | 2 | (0,5k] | (0,5000] | 13941 | 58.0875% | 10923 | 3018 | 21.6484% | 0.584398909 | 0.568468638 | 0.027637667 | 0.000440276 |
| PAY_AMT5 | 3 | (5k,20k] | (5000,20000] | 3746 | 15.6083% | 3132 | 614 | 16.3908% | 0.167567278 | 0.115652665 | 0.370793498 | 0.019249601 |
| PAY_AMT5 | 4 | >20k | (20000,+inf) | 926 | 3.8583% | 824 | 102 | 11.0151% | 0.044085389 | 0.019212658 | 0.830559086 | 0.020658273 |
| PAY_AMT6 | 1 | 0 | =0 | 5787 | 24.1125% | 4112 | 1675 | 28.9442% | 0.219998930 | 0.315501978 | -0.360542268 | 0.034432885 |
| PAY_AMT6 | 2 | (0,5k] | (0,5000] | 13662 | 56.9250% | 10689 | 2973 | 21.7611% | 0.571879514 | 0.559992466 | 0.021005000 | 0.000249687 |
| PAY_AMT6 | 3 | (5k,20k] | (5000,20000] | 3513 | 14.6375% | 2976 | 537 | 15.2861% | 0.159221015 | 0.101148992 | 0.453698670 | 0.026347200 |
| PAY_AMT6 | 4 | >20k | (20000,+inf) | 1038 | 4.3250% | 914 | 124 | 11.9461% | 0.048900540 | 0.023356564 | 0.738910375 | 0.018874709 |
| LIMIT_BAL | 1 | <=30k | (-inf,30000] | 3256 | 13.5667% | 2067 | 1189 | 36.5172% | 0.110587984 | 0.223959314 | -0.705652968 | 0.080000816 |
| LIMIT_BAL | 2 | (30k,140k] | (30000,140000] | 8957 | 37.3208% | 6660 | 2297 | 25.6447% | 0.356321224 | 0.432661518 | -0.194123070 | 0.014819412 |
| LIMIT_BAL | 3 | (140k,240k] | (140000,240000] | 6178 | 25.7417% | 5095 | 1083 | 17.5299% | 0.272591087 | 0.203993219 | 0.289886068 | 0.019885566 |
| LIMIT_BAL | 4 | >240k | (240000,+inf) | 5609 | 23.3708% | 4869 | 740 | 13.1931% | 0.260499706 | 0.139385948 | 0.625355039 | 0.075739098 |
| avg_utilization | 1 | 0 | =0 | 771 | 3.2125% | 495 | 276 | 35.7977% | 0.026483334 | 0.051987192 | -0.674481734 | 0.017201886 |
| avg_utilization | 2 | (0,0.2] | (0,0.2] | 10021 | 41.7542% | 8372 | 1649 | 16.4554% | 0.447916109 | 0.310604634 | 0.366085130 | 0.050267689 |
| avg_utilization | 3 | (0.2,0.6] | (0.2,0.6] | 5820 | 24.2500% | 4612 | 1208 | 20.7560% | 0.246749773 | 0.227538143 | 0.081056872 | 0.001557235 |
| avg_utilization | 4 | (0.6,1] | (0.6,1] | 6880 | 28.6667% | 4882 | 1998 | 29.0407% | 0.261195228 | 0.376342061 | -0.365230339 | 0.042055117 |
| avg_utilization | 5 | >1 | (1,+inf) | 508 | 2.1167% | 330 | 178 | 35.0394% | 0.017655556 | 0.033527971 | -0.641329527 | 0.010179449 |

## WOE Integrity

- Specification rows: `41`.
- Matrix shape: `24,000 × 10`.
- Canonical order: exact.
- Missing WOE: `0`.
- Non-finite WOE: `0`.
- Zero-support bins: `0`.
- Positive-support zero-class bins: `0`.
- Smoothing occurrences: `0`.
- Row, ID, and target alignment: exact.
- Mapping reconstruction check: byte-for-value exact DataFrame equality.

The Stage 4.1 0.5 fallback was implemented but not triggered. A zero-support bin would have blocked execution rather than changed binning or borrowed WOE.

## Final Logit Specification and Numerical Sanity

- Implementation: `statsmodels.Logit unpenalized maximum likelihood with explicit intercept`.
- Substantive model count: `1`.
- Development rows: 24,000.
- Converged: `true` in `6` Newton iterations.
- Design rank: `11/11`.
- Final intercept: `-1.254276175288`.
- Probability range: `[0.038073397741, 0.867665478338]`.
- Covariance minimum eigenvalue: `0.000187694824845`.
- Covariance condition number: `39.626825713`.
- Captured warnings: `none`.

| Check | Result |
| --- | --- |
| All standard errors finite and positive | PASS |
| Covariance condition number finite | PASS |
| Covariance finite | PASS |
| Covariance positive definite | PASS |
| Covariance symmetric | PASS |
| Design rank = 11 | PASS |
| Intercept and all coefficients finite | PASS |
| Linear predictor finite | PASS |
| Model converged | PASS |
| No invalidating fit warning | PASS |
| No separation warning in model summary | PASS |
| Predicted probabilities finite | PASS |
| Probabilities strictly within (0,1) | PASS |

No singular-matrix, inference-invalidating separation, overflow, underflow, non-finite parameter/SE/covariance/probability, or rank failure occurred.

## Stage 4.5 Deterministic Reproduction

All values were independently recomputed from raw CSV before comparison. Absolute tolerance: `1.0e-09`.

- Intercept absolute difference: `2.884e-10`.
- Maximum ten-coefficient absolute difference: `4.663e-10`.
- Maximum intercept/coefficient absolute difference: `4.663e-10`.
- Final apparent Train AUC: `0.772417097808`; absolute difference from `0.772417098`: `1.916e-10`.
- Final apparent Train KS: `0.430307867280`; absolute difference from `0.430307867`: `2.802e-10`.
- Result: PASS within tolerance for intercept, all ten coefficients, apparent AUC, and apparent KS.

These apparent Development-Train metrics are reproducibility diagnostics, not unseen-data performance and not selection criteria.

## Final Coefficient Table and Ten Sign Checks

| Predictor | Final Coefficient | Expected Sign | Sign Check | Standard Error | z-statistic | p-value | 95% CI Lower | 95% CI Upper | Odds Ratio | Stage 4.5 Reference Coefficient | Absolute Refit Difference | Final Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PAY_0 | -0.556303702 | Negative | Expected | 0.023615124 | -23.557094 | 1.06187e-122 | -0.602588494 | -0.510018909 | 0.573324330 | -0.556303702 | 4.201e-10 | Retained in Final Logistic |
| severe_delinquent_months | -0.513060046 | Negative | Expected | 0.024386248 | -21.038909 | 2.88949e-98 | -0.560856213 | -0.465263879 | 0.598660844 | -0.513060046 | 2.797e-10 | Retained in Final Logistic |
| PAY_AMT1 | -0.193755569 | Negative | Expected | 0.048840636 | -3.967098 | 7.27532e-05 | -0.289481457 | -0.098029681 | 0.823859256 | -0.193755569 | 9.636e-11 | Retained in Final Logistic |
| PAY_AMT2 | -0.222092688 | Negative | Expected | 0.054644384 | -4.064328 | 4.81711e-05 | -0.329193713 | -0.114991663 | 0.800841133 | -0.222092688 | 6.814e-11 | Retained in Final Logistic |
| PAY_AMT3 | -0.245401439 | Negative | Expected | 0.062020875 | -3.956755 | 7.59746e-05 | -0.366960120 | -0.123842757 | 0.782390394 | -0.245401439 | 4.663e-10 | Retained in Final Logistic |
| PAY_AMT4 | -0.165041794 | Negative | Expected | 0.072425635 | -2.278776 | 0.0226804 | -0.306993429 | -0.023090159 | 0.847858268 | -0.165041794 | 1.822e-10 | Retained in Final Logistic |
| PAY_AMT5 | -0.097122437 | Negative | Expected | 0.078559534 | -1.236291 | 0.21635 | -0.251096294 | 0.056851420 | 0.907444894 | -0.097122437 | 1.494e-10 | Retained in Final Logistic |
| PAY_AMT6 | -0.164349102 | Negative | Expected | 0.078064710 | -2.105293 | 0.0352658 | -0.317353122 | -0.011345082 | 0.848445776 | -0.164349102 | 2.965e-10 | Retained in Final Logistic |
| LIMIT_BAL | -0.226160642 | Negative | Expected | 0.044957706 | -5.030520 | 4.89151e-07 | -0.314276128 | -0.138045157 | 0.797589965 | -0.226160642 | 2.011e-10 | Retained in Final Logistic |
| avg_utilization | -0.405621158 | Negative | Expected | 0.052465668 | -7.731173 | 1.0656e-14 | -0.508451978 | -0.302790337 | 0.666562642 | -0.405621158 | 2.415e-10 | Retained in Final Logistic |

All ten coefficients are negative under the project WOE convention and all ten statuses are `Retained in Final Logistic`. Higher safer WOE values therefore reduce modeled default log-odds, holding other WOE predictors constant. Odds ratios are WOE-unit effects, not raw business-unit effects; no causal claim is made.

## Final Fitted Equation

```text
logit(P(Default=1)) = -1.254276175 -0.556303702 × WOE(PAY_0) -0.513060046 × WOE(severe_delinquent_months) -0.193755569 × WOE(PAY_AMT1) -0.222092688 × WOE(PAY_AMT2) -0.245401439 × WOE(PAY_AMT3) -0.165041794 × WOE(PAY_AMT4) -0.097122437 × WOE(PAY_AMT5) -0.164349102 × WOE(PAY_AMT6) -0.226160642 × WOE(LIMIT_BAL) -0.405621158 × WOE(avg_utilization)
```

## FRZ-01

Status: `PRESENT — inline final ten-predictor coefficient forest plot with 95% CIs and zero line`. The inline plot was visually reviewed and shows every frozen coefficient, 95% CI, and zero line without Keep/Drop encoding. No ROC or KS plot was created in this freeze stage.

## Evidence Hierarchy

1. Stage 4.5: Full-Train candidate-model apparent diagnostics.
2. Stage 4.6: five-fold stability and pooled OOF evidence under frozen full-Train-designed bins and predictors.
3. Stage 4.7: Human final-variable freeze and deterministic Full-Train refit.
4. Stage 5: sealed Holdout Test evaluation and challenger comparison.

Stage 4.7 apparent metrics are not presented as unseen-data performance.

## Final Model Freeze Record

| Field | Frozen value |
| --- | --- |
| Final model name | Final 10-Variable WOE Logistic |
| Development sample | 24,000 |
| Target | default.payment.next.month |
| Good / Bad | 18,691 / 5,309 |
| WOE convention | ln(DistGood / DistBad) |
| Final predictor count | 10 |
| Final predictor list | PAY_0, severe_delinquent_months, PAY_AMT1, PAY_AMT2, PAY_AMT3, PAY_AMT4, PAY_AMT5, PAY_AMT6, LIMIT_BAL, avg_utilization |
| Bin definitions | Stage 4.2/4.3 frozen definitions; 41-row mapping below |
| WOE source | Complete 24,000-row Development Train |
| Logistic | Unpenalized MLE with explicit intercept |
| Final coefficients | const=-1.254276175288; PAY_0=-0.556303701580; severe_delinquent_months=-0.513060045720; PAY_AMT1=-0.193755569096; PAY_AMT2=-0.222092687932; PAY_AMT3=-0.245401438534; PAY_AMT4=-0.165041793818; PAY_AMT5=-0.097122437149; PAY_AMT6=-0.164349101704; LIMIT_BAL=-0.226160642201; avg_utilization=-0.405621157758 |
| Final apparent Train AUC / KS | 0.772417098 / 0.430307867 |
| Stage 4.6 pooled OOF AUC / KS | 0.771230165 / 0.429462039 |
| Holdout status | SEALED — Stage 5 only |
| Structure status | FROZEN after Stage 4.7 |

`FROZEN — no predictor/bin/WOE-definition change before Stage 5 absent an explicit Architect reopening for a genuine defect`

Model structure is frozen after Stage 4.7. No predictor, bin, or WOE-definition change is permitted before Stage 5 unless a genuine implementation defect is found and the Architect explicitly reopens the model.

## Stage 5 Handoff Package

| Component | Frozen handoff |
| --- | --- |
| 1. Final predictor list | PAY_0, severe_delinquent_months, PAY_AMT1, PAY_AMT2, PAY_AMT3, PAY_AMT4, PAY_AMT5, PAY_AMT6, LIMIT_BAL, avg_utilization |
| 2. Exact bin definitions | BIN_DEFINITIONS above; frozen Stage 4.2/4.3; 41 rows |
| 3. Full-Train WOE mapping | Complete final_woe_specification table above (41/41 rows) |
| 4. Final intercept | -1.254276175288 |
| 5. Final ten coefficients | PAY_0=-0.556303701580; severe_delinquent_months=-0.513060045720; PAY_AMT1=-0.193755569096; PAY_AMT2=-0.222092687932; PAY_AMT3=-0.245401438534; PAY_AMT4=-0.165041793818; PAY_AMT5=-0.097122437149; PAY_AMT6=-0.164349101704; LIMIT_BAL=-0.226160642201; avg_utilization=-0.405621157758 |
| 6. WOE convention | ln(DistGood / DistBad); Good=0, Bad=1 |
| 7. Feature definitions | severe_delinquent_months=sum(PAY_i>=2); avg_utilization=mean(max(BILL_AMTi,0))/LIMIT_BAL; uncapped |
| 8. Train/Test identity | Train=bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857; Test=d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f |
| 9. Holdout sealing | SEALED — Holdout deleted after row/ID/union/hash identity checks only; Stage 5 only |
| 10. Stage 4.5 reference | Apparent AUC=0.772417098; KS=0.430307867; rounded parameter anchors reproduced at atol=1e-9 |
| 11. Stage 4.6 reference | Pooled OOF AUC=0.771230165; pooled OOF KS=0.429462039; full-Train-design limitation applies |
| 12. Stage 4.7 reference | Apparent AUC=0.772417098; KS=0.430307867 |

Status: `COMPLETE — frozen primary-model specification recorded; not applied to Holdout`. The 41-row WOE mapping, exact features/bins, Train/Test hashes, intercept, coefficients, conventions, evidence anchors, and sealing record are embedded in this Notebook and report. Nothing was applied to Holdout.

## Hard-Boundary Confirmation

- All ten retained, including `PAY_AMT5`.
- No 10-vs-9 model, feature deletion, or alternative subset.
- No p-value/CI elimination, IV/VIF re-screening, or AIC/BIC/stepwise search.
- No re-binning or monotonicity search.
- No regularization.
- No class weighting, balancing, SMOTE, oversampling, or undersampling.
- No cross-validation, bootstrap, or OOF generation.
- No calibration.
- No operational threshold optimization.
- No Holdout predictive analysis.
- No Stage 5 execution.
- No external model, mapping, coefficient, prediction, figure, HTML/PDF, `src/`, or `tests/` artifact.

## Notebook Clean-Run

- Artifact: `notebooks/10_final_woe_logistic_freeze.ipynb`.
- Final pre-report clean-run from raw CSV: success.
- Python: `/opt/anaconda3/bin/python` `3.11.7`.
- Packages: `matplotlib 3.8.0, numpy 1.26.4, pandas 2.1.4, scikit-learn 1.2.2, scipy 1.11.4, seaborn 0.12.2, statsmodels 0.14.0`.
- Notebook format: `4.5`.
- Cells: `30` total; `15` code; `15` markdown.
- Execution counts: `1–15`; strictly sequential: `True`.
- Error/traceback outputs: `0`.
- Inline PNG figures: `1`.
- `statsmodels.Logit` construction sites: `1`; substantive model result count: `1`.
- Notebook bytes: `268129`.
- Notebook SHA-256: `590bd84b05c9a979a9c58c6423f27283a7f1312fb8f0c186e4da1e9734b02625`.
- Post-report final clean-run from raw CSV: success; 30 cells, 15 code cells with sequential execution counts 1–15, zero error/traceback outputs, one inline PNG, and all Stage 4.7 decision/WOE/model/reproduction/freeze/handoff gates true.
- Final Notebook bytes: `268129`.
- Final Notebook SHA-256: `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863`.

An intermediate development run reached the final machine-summary cell but failed to serialize NumPy boolean values to JSON. The summary values were explicitly cast to standard booleans; no feature, bin, WOE, predictor, model, metric, or analytical output changed. The subsequent complete clean-run succeeded with zero errors.

## Protected-Input Integrity

All 28 tracked files were fingerprinted before modification; this superset includes every protected input read during implementation.

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
| notebooks/09_stratified_5fold_model_stability.ipynb | 497238 | 4a5fc99b2c5418ce37191553d4958a7a7005c4ac208be096c94a64225bb4cf1b |
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
| task_records/Stage_4.6_Stratified_5Fold_Model_Stability.md | 74895 | 9c1c699bb1d7fbe11a2a21bc392c6a08791b4fa2b7ea832b9625cced8f0bcea7 |
| task_records/TASK_REPORT_Task_0.1_Current-State_Audit.md | 25094 | 615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8 |

- Protected manifest SHA-256: `1373be2628e9fe6e6acbef974cb4c4e61dbd85e7c6855bbde55251e72f74f707`.
- Ignored Luca reference Notebook baseline: 805,686 bytes; SHA-256 `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd`.
- Git index baseline: SHA-256 `5ae5b55629fc4ededb8147a946204f4813c7bf9c8ae7c305275b64481283bce6`.
- Final byte-for-byte comparison: all 28 protected tracked paths matched their pre-flight sizes and SHA-256 values; mismatches = `0`. The ignored Luca reference Notebook and Git index also retained their exact baseline fingerprints.

## Git Status and Artifact Scope

- Authorized created artifact: `notebooks/10_final_woe_logistic_freeze.ipynb`.
- Authorized created artifact: `task_records/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md`.
- Modified existing, moved, or deleted project files: none.
- Final root: `/Users/yu/2.工作/Risk Analysis Project`; branch: `main`; HEAD: `bac1d2f0af6693b9443b80322eb49891992ccfab`; parent and HEAD message unchanged.
- Final tracked diff: empty; staged diff: empty; Git index fingerprint unchanged.
- Final untracked list contains exactly `notebooks/10_final_woe_logistic_freeze.ipynb` and `task_records/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md`.
- Final remote list: empty; no Stage 4.7 commit exists.
- No `git add`, commit, push, pull, fetch, reset, restore, checkout, stash, clean, amend, rebase, or remote mutation was performed.

## Deviations, Limitations, Warnings, and Incomplete Items

- Specification deviation: none known.
- Model/numerical warning: none.
- Required methodological limitation: Stage 4.6 OOF evidence used full-Development-designed bins/predictors and is not final unbiased unseen-data evidence; documented above.
- Environment note: project `.venv` lacks `statsmodels`; the existing registered `python3` kernel at `/opt/anaconda3/bin/python` with `statsmodels 0.14.0` was used. No package changed.
- Implementation note: one intermediate machine-summary JSON serialization error was corrected as described above; final clean-run is error-free.
- Incomplete analytical item: none.
- Reviewer validation: pending and intentionally not prewritten.

## Immutable Architect Specification

The authoritative block below is reproduced byte-for-byte. Its mechanically derived UTF-8 fingerprint is `23,488` bytes and SHA-256 `2be5b2dd43277591e5bbd25c1ccc8ba424b134aa18eaa26ed7e2b0b8a2ca0581` using the exact standalone-marker boundary described in Prompt-Source Marker Validation.

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.7 ================

# Stage 4.7 — Final Logistic Freeze + Full-Train Refit

## 1. Stage Context

Current Stage 4 structure:

- Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
- Stage 4.2 — PAY + Categorical Binning ✅ CLOSED
- Stage 4.3 — Continuous Variable Binning ✅ CLOSED
- Stage 4.4 — WOE / IV Variable Screening ✅ CLOSED
- Stage 4.5 — Candidate WOE Logistic Regression ✅ CLOSED
- Stage 4.6 — Stratified 5-Fold Model Stability ✅ CLOSED
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit ← CURRENT

Stage 4.6 final Reviewer verdict:

> PASS

Stage 4.6 milestone commit:

`bac1d2f0af6693b9443b80322eb49891992ccfab`

Short:

`bac1d2f`

Commit message:

`Validate candidate WOE model stability`

Parent:

`1e9c95f022de590eec7eb4b3300e01be3ce57f03`

Branch:

`main`

Stage 4.6 post-commit state:

- working tree clean;
- staged changes none;
- untracked files none;
- remote none;
- no push.

---

# 2. Stage 4.7 Objective

Stage 4.7 is the final stage of Stage 4 WOE / Logistic model development.

It has four objectives:

1. synthesize the already-completed Stage 4.4 / 4.5 / 4.6 evidence;
2. formally record the Human final-variable decision;
3. freeze the final WOE Logistic specification;
4. refit the frozen final model once on the complete 24,000-row Development Train for Stage 5 handoff.

Stage 4.7 is NOT:

- another feature-screening stage;
- another model-search stage;
- another K-fold stage;
- a binning stage;
- a challenger-model stage;
- a Holdout Test evaluation stage.

The model-development decision has already been made by the Architect and Human.

---

# 3. Human Final-Variable Decision — FROZEN

The Human final-variable decision is:

> KEEP ALL 10 PREDICTORS.

Final predictor list, in canonical order:

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

This is now the final primary Logistic predictor set.

Do not:

- delete any predictor;
- add any predictor;
- substitute any predictor;
- reorder canonical model tables;
- reopen Stage 4.4 screening;
- fit an alternative subset.

---

# 4. Final Decision Rationale

Stage 4.7 must document that the final-variable freeze is based on combined evidence from:

- Stage 4.4 standalone signal / redundancy / governance screening;
- Stage 4.5 Full-Train multivariate conditional behavior;
- Stage 4.6 five-fold coefficient / predictive stability;
- business and temporal-structure interpretability.

The decision is NOT based on an automatic:

- p-value threshold;
- confidence-interval threshold;
- IV threshold;
- VIF threshold;
- coefficient-SD threshold.

---

# 5. PAY\_AMT5 Final Decision — REQUIRED

`PAY_AMT5` must remain in the final model.

The Stage 4.7 rationale must accurately distinguish:

> weak conditional strength

from:

> unstable direction.

Evidence entering Stage 4.7:

Stage 4.5:

- Full-Train coefficient approximately `-0.097122437`;
- expected negative direction;
- 95% CI crossed zero;
- p-value approximately `0.21635`;
- status = `Review in Stage 4.6`.

Stage 4.6:

- Fold 1 coefficient = `-0.125774`
- Fold 2 coefficient = `-0.057477`
- Fold 3 coefficient = `-0.098118`
- Fold 4 coefficient = `-0.041387`
- Fold 5 coefficient = `-0.160361`
- negative folds = `5/5`
- positive folds = `0/5`
- Directionally Clear folds = `0/5`
- CI-crosses-zero folds = `5/5`
- Fold Mean ≈ `-0.096623`
- Fold SD ≈ `0.048729`
- Full-Train coefficient ≈ `-0.097122`

Therefore Stage 4.7 must state conceptually:

> PAY\_AMT5 shows weaker incremental multivariate evidence, but its coefficient direction is stable across all five folds and its fold mean closely reproduces its Full-Train estimate.

It is retained because:

1. there is no sign instability;
2. Stage 4.4 found no defensible natural month cutoff;
3. PAY\_AMT1–6 form a coherent six-month payment-history family;
4. selectively removing only the fifth month while retaining adjacent fourth and sixth months lacks a defensible temporal/business rationale;
5. automatic p-value / CI elimination is not the project's model-governance rule.

Do NOT claim:

> PAY\_AMT5 is statistically strong.

Do NOT claim:

> PAY\_AMT5 has no weakness.

Correct conclusion:

> retained despite weaker conditional statistical evidence because the evidence supports stable direction and family/temporal structural coherence.

---

# 6. PAY\_AMT4 / PAY\_AMT6 Context

Stage 4.6 evidence:

`PAY_AMT4`:

- negative folds = 5/5
- Directionally Clear = 4/5
- CI-crosses-zero = 1/5

`PAY_AMT6`:

- negative folds = 5/5
- Directionally Clear = 2/5
- CI-crosses-zero = 3/5

These observations support the conclusion that the six PAY\_AMT variables show a continuum of conditional strength rather than a clean structurally meaningful cutoff at month 5.

Do not create a new cutoff.

Do not create a recent-month-only model.

Do not aggregate or redesign PAY\_AMT features.

---

# 7. Stage 4.6 Overall Stability Evidence

Stage 4.7 may summarize, but must not rerun, the accepted Stage 4.6 results.

Accepted pooled OOF metrics:

[
AUC\_{OOF}=0.771230165
]

[
KS\_{OOF}=0.429462039
]

Mean validation-fold AUC:

[
0.771415388
]

Fold AUC sample SD:

[
0.004852803
]

Mean validation-fold KS:

[
0.436219472
]

Fold KS sample SD:

[
0.008255848
]

Stage 4.5 apparent baseline:

[
AUC\_{Apparent}=0.772417098
]

[
KS\_{Apparent}=0.430307867
]

Apparent-minus-OOF gaps:

[
AUC\ Gap=0.001186933
]

[
KS\ Gap=0.000845828
]

Interpretation:

> the fixed ten-variable candidate model showed stable discrimination across the Stage 4.6 fold structure, with only a very small apparent-vs-OOF gap.

Do not overclaim Stage 4.6 as a fully unbiased generalization estimate because supervised bin design and candidate selection were developed on full Development Train.

Stage 5 remains the sealed unseen-data evaluation.

---

# 8. New Stage 4.7 Artifacts

Create exactly:

`notebooks/10_final_woe_logistic_freeze.ipynb`

and:

`task_records/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md`

Do not modify:

- Stage 4.6 Notebook;
- Stage 4.6 Task Report;
- any Stage 0–4.6 tracked artifact.

Do not create external model/data files.

---

# 9. Git Pre-flight

Expected project root:

`/Users/yu/2.工作/Risk Analysis Project`

Expected:

- branch = `main`
- HEAD =
  `bac1d2f0af6693b9443b80322eb49891992ccfab`
- HEAD message =
  `Validate candidate WOE model stability`
- tracked diff empty
- staged diff empty
- untracked empty
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

# 10. Outer Train / Holdout Test Identity

Replay the frozen split exactly from:

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

Expected Development Train:

- Good = 18,691
- Bad = 5,309
- Bad rate ≈ 22.1208%

Holdout Test may be used only for:

- row count;
- ID overlap;
- ID union;
- frozen ID hash.

Then delete/seal the Test object.

Do not inspect:

- Test predictor values;
- Test target distribution;
- Test bins;
- Test WOE;
- Test probabilities;
- Test AUC;
- Test KS;
- Test calibration;
- any Test risk relationship.

Stage 5 owns Holdout evaluation.

---

# 11. Frozen Feature Engineering

Reproduce only the already-approved deterministic features required for the final ten predictors.

Relevant definitions include:

[
severe\_delinquent\_months
==========================

\sum\_i1(PAY\_i\ge2)
]

and:

[
avg\_utilization
================

\frac{
mean(max(BILL\_AMT1,0),...,max(BILL\_AMT6,0))
}{
LIMIT\_BAL
}
]

Utilization above 1 remains uncapped.

Do not create any new feature.

---

# 12. Frozen Final Bin Definitions

Use the exact approved Stage 4.2 / 4.3 final bins.

Final model bins:

## PAY\_0

- `[-2,-1,0]`
- `[1]`
- `[2+]`

## severe\_delinquent\_months

- `0`
- `1`
- `2`
- `3`
- `4+`

## PAY\_AMT1–PAY\_AMT6

Each uses:

- `=0`
- `(0,5000]`
- `(5000,20000]`
- `(20000,+inf)`

## LIMIT\_BAL

- `(-inf,30000]`
- `(30000,140000]`
- `(140000,240000]`
- `(240000,+inf)`

## avg\_utilization

- `=0`
- `(0,0.2]`
- `(0.2,0.6]`
- `(0.6,1]`
- `(1,+inf)`

Do not:

- search new cut points;
- merge bins;
- split bins;
- optimize monotonicity;
- change definitions.

These bins are now part of the final primary Logistic model specification.

---

# 13. Final Full-Train WOE Estimation

Using all 24,000 Development-Train rows:

1. assign each final predictor to its frozen bins;
2. calculate Good / Bad counts by bin;
3. calculate final Full-Train WOE using the project convention;
4. construct the final 24,000 × 10 WOE matrix.

WOE convention:

[
WOE\_j
======

\ln\frac{DistGood\_j}{DistBad\_j}
]

where:

- Good = 0
- Bad = 1

Therefore:

- positive WOE = safer;
- negative WOE = riskier.

Expected final Logistic coefficient sign remains:

[
\beta\_j<0
]

for all ten predictors.

---

# 14. Zero-Event / Smoothing Rule

Stage 4.5 previously established that no smoothing was required on the complete 24,000-row Train under the frozen bins.

Recheck mechanically.

Expected:

- zero-support final bins = 0;
- zero-class final bins = 0;
- smoothing occurrences = 0.

If a positive-support bin unexpectedly contains one zero class:

> use only the exact Stage 4.1-approved 0.5 smoothing fallback and document it.

If any frozen final bin has zero total support on full Train:

> BLOCKED.

Do not alter the bin structure.

---

# 15. Final WOE Specification Table

Create one canonical final WOE specification table covering every final predictor/bin combination.

There should be 41 final predictor-bin rows:

- PAY\_0: 3
- severe\_delinquent\_months: 5
- PAY\_AMT1–6: 24
- LIMIT\_BAL: 4
- avg\_utilization: 5

Total:

[
3+5+24+4+5=41
]

Columns:

- Predictor
- Bin Order
- Bin Label / Definition
- Train Count
- Train Share
- Good Count
- Bad Count
- Bad Rate
- DistGood
- DistBad
- Final Full-Train WOE

Optional:

- IV contribution may be included for provenance/reference only.

Do not use IV to reopen selection.

This table becomes the frozen Train-derived WOE mapping to be used in Stage 5.

---

# 16. Final WOE Matrix Integrity

Verify:

- shape = `24,000 × 10`;
- canonical predictor order;
- missing = 0;
- non-finite = 0;
- exact row alignment;
- exact ID alignment;
- exact target alignment;
- WOE values are generated only from the final frozen Full-Train mapping.

---

# 17. Final Logistic Model

Fit exactly one final model:

> explicit-intercept, unpenalized maximum-likelihood Logistic Regression

using:

- all 24,000 Development-Train observations;
- exactly the ten final WOE predictors;
- target = Default 1.

Preferred implementation:

`statsmodels.Logit`

consistent with Stage 4.5 and Stage 4.6.

Model form:

[
\log
\frac{P(Default=1)}
{1-P(Default=1)}
================

\beta\_0
\+
\sum\_{j=1}^{10}\beta\_jWOE\_j
]

No:

- regularization;
- class weights;
- resampling;
- subset search;
- alternate model.

---

# 18. Stage 4.5 Reproduction Anchor

Because Stage 4.7 uses:

- the same 24,000 Development Train;
- the same ten predictors;
- the same frozen bins;
- the same Full-Train WOE convention;
- the same unpenalized Logistic form;

the final Stage 4.7 refit should reproduce the accepted Stage 4.5 candidate model numerically.

Stage 4.5 reference intercept:

`-1.254276175`

Stage 4.5 reference coefficients:

- `PAY_0` = `-0.556303702`
- `severe_delinquent_months` = `-0.513060046`
- `PAY_AMT1` = `-0.193755569`
- `PAY_AMT2` = `-0.222092688`
- `PAY_AMT3` = `-0.245401439`
- `PAY_AMT4` = `-0.165041794`
- `PAY_AMT5` = `-0.097122437`
- `PAY_AMT6` = `-0.164349102`
- `LIMIT_BAL` = `-0.226160642`
- `avg_utilization` = `-0.405621158`

Reference apparent Train AUC:

`0.772417098`

Reference apparent Train KS:

`0.430307867`

Treat these as reproducibility anchors, not as numbers to hard-code into the model.

Recompute independently from raw data.

Compare after refit.

Use a clearly documented tight numerical tolerance appropriate for deterministic floating-point reproduction, for example absolute tolerance `1e-9`.

If material discrepancy beyond tolerance occurs:

> BLOCKED.

Do not silently overwrite the reference or change model design.

---

# 19. Final Model Numerical Sanity

Verify:

- converged;
- intercept finite;
- ten coefficients finite;
- ten standard errors finite;
- covariance usable;
- design rank = 11;
- predicted Train probabilities finite;
- probabilities in valid range;
- no singular matrix;
- no invalidating perfect/quasi separation;
- no material numerical overflow/underflow.

If invalid:

> BLOCKED.

Do not solve by deleting predictors.

---

# 20. Final Coefficient Table

Create one canonical ten-row final coefficient table containing:

- Predictor
- Final Coefficient
- Expected Sign
- Sign Check
- Standard Error
- z-statistic
- p-value
- 95% CI Lower
- 95% CI Upper
- Odds Ratio
- Stage 4.5 Reference Coefficient
- Absolute Refit Difference
- Final Status

Final Status for every predictor should be:

> `Retained in Final Logistic`

This is not a new significance-screening table.

`PAY_AMT5` must remain retained regardless of its p-value / CI because the Human final decision is already frozen.

---

# 21. Final Sign Check

Expected sign:

[
\beta<0
]

for all ten predictors under the project WOE convention.

Compare Stage 4.7 signs with Stage 4.5 / Stage 4.6 evidence.

Expected result:

> all ten remain negative.

If any coefficient materially changes sign relative to deterministic Stage 4.5 reproduction:

> BLOCKED as a reproducibility/model-construction discrepancy.

Do not reinterpret this as a new variable-selection opportunity.

---

# 22. Final Logistic Equation

Document the final fitted model explicitly.

At minimum show:

[
logit(P(Default=1))
===================

\beta\_0
\+
\beta\_1WOE(PAY\_0)
\+
...
\+
\beta\_{10}WOE(avg\_utilization)
]

Also provide a readable final coefficient equation with the fitted numerical coefficients.

Do not convert the model into scorecard points.

Do not create PDO / base score / scaling.

Score scaling is outside Stage 4.7.

---

# 23. Final Model Interpretation

Document the model-level WOE interpretation:

Because:

[
WOE=\ln(DistGood/DistBad)
]

higher WOE corresponds to safer bins.

With negative Logistic coefficients:

> moving toward safer WOE values reduces predicted default log-odds, holding other predictors constant.

Do not overinterpret one-WOE-unit OR as a raw business-unit effect.

Do not claim causality.

---

# 24. Final Apparent Development Metrics

For reproducibility and final Development-Train model documentation, calculate:

[
AUC\_{Final,Train}
]

and:

[
KS\_{Final,Train}
]

using the final refit on the complete Development Train.

Expected to reproduce Stage 4.5:

- AUC ≈ `0.772417098`
- KS ≈ `0.430307867`

These remain:

> apparent development-sample diagnostics.

They are NOT final generalization evidence.

Do not use these metrics for variable selection.

Do not optimize the model based on them.

---

# 25. Stage 4.6 Stability Context

The final Task Report must preserve the evidence hierarchy:

## Stage 4.5

Full-Train candidate-model apparent diagnostics.

## Stage 4.6

Five-fold stability / pooled OOF evidence under frozen full-Train-designed bins and predictors.

## Stage 4.7

Human final-variable freeze + deterministic Full-Train refit.

## Stage 5

Sealed Holdout Test evaluation and challenger comparison.

Do not present Stage 4.7 apparent metrics as unseen-data performance.

---

# 26. No 10-vs-9 Comparison

Do NOT fit a model without `PAY_AMT5`.

Do NOT compare:

- 10-variable vs 9-variable AUC;
- 10-variable vs 9-variable KS;
- 10-variable vs 9-variable likelihood;
- 10-variable vs 9-variable AIC/BIC;
- any subset combination.

The Human decision to retain all ten is already final.

Stage 4.7 must not reopen selection using development performance.

---

# 27. No Feature Selection

Do not use:

- p-value elimination;
- CI elimination;
- stepwise;
- backward elimination;
- forward selection;
- LASSO;
- Ridge;
- Elastic Net;
- AIC search;
- BIC search;
- performance-based subset search.

Final predictor list is frozen.

---

# 28. No Re-binning

Do not:

- modify Stage 4.2 / 4.3 cut points;
- search alternate bins;
- merge bins;
- split bins;
- enforce new monotonicity;
- alter PAY\_AMT month representation.

Final bins are frozen.

---

# 29. No Cross-Validation

Do not rerun:

- StratifiedKFold;
- KFold;
- repeated CV;
- bootstrap;
- OOF predictions.

Stage 4.6 is closed.

Stage 4.7 may read/summarize accepted Stage 4.6 evidence only.

---

# 30. No Imbalance Intervention

Do not use:

- class weighting;
- sample weighting for balancing;
- SMOTE;
- oversampling;
- undersampling.

Use the natural 24,000-row Train.

---

# 31. No Calibration

Do not perform:

- calibration curve;
- Brier score;
- calibration slope;
- calibration intercept;
- Hosmer-Lemeshow decision.

Calibration is not part of Stage 4.7 final freeze.

---

# 32. No Operational Threshold Selection

Do not choose:

- classification cutoff;
- approval threshold;
- F1 threshold;
- recall threshold;
- cost-based cutoff;
- KS-max operational cutoff.

Any KS-max threshold is diagnostic only.

---

# 33. No Holdout Test Evaluation

The 6,000-row Test remains sealed throughout Stage 4.7.

Do not apply the final model to Test.

Do not compute:

- Test WOE;
- Test prediction;
- Test AUC;
- Test KS;
- Test calibration;
- Test threshold metrics;
- Test risk stratification.

Stage 5 owns the first predictive application to the sealed Holdout.

---

# 34. Final Model Freeze Record

Create a concise explicit freeze record containing:

- Final model name:
  `Final 10-Variable WOE Logistic`
- Development sample:
  `24,000`
- Target:
  `default.payment.next.month`
- Good:
  `18,691`
- Bad:
  `5,309`
- WOE convention:
  `ln(DistGood / DistBad)`
- Final predictor count:
  `10`
- Final predictor list:
  exact canonical list
- Bin definitions:
  frozen Stage 4.2 / 4.3
- WOE source:
  complete Development Train
- Logistic:
  unpenalized MLE with intercept
- Final coefficients:
  refitted Stage 4.7 values
- Final apparent Train AUC
- Final apparent Train KS
- Stage 4.6 pooled OOF AUC
- Stage 4.6 pooled OOF KS
- Holdout status:
  `SEALED — Stage 5 only`

State explicitly:

> Model structure is frozen after Stage 4.7.

No predictor / bin / WOE-definition changes are allowed before Stage 5 unless a genuine implementation defect is discovered and Architect explicitly reopens the model.

---

# 35. Stage 5 Handoff Package

Stage 4.7 must hand off the complete frozen primary-model specification required for Stage 5:

1. final ten predictor list;
2. exact frozen bin definitions;
3. final 41-row Full-Train WOE mapping;
4. final intercept;
5. final ten Logistic coefficients;
6. WOE convention;
7. feature-engineering definitions required to reproduce the final predictors;
8. Train/Test identity hashes;
9. explicit Holdout sealing record;
10. Stage 4.5 apparent reference;
11. Stage 4.6 OOF stability reference;
12. final Stage 4.7 apparent reference.

Do not apply the handoff model to Holdout yet.

---

# 36. No External Model Artifact

Do not create:

- pickle;
- joblib;
- JSON model file;
- YAML model file;
- coefficient CSV;
- WOE CSV;
- score CSV;
- prediction CSV;
- external PNG;
- HTML;
- PDF;
- `src/`;
- `tests/`.

The frozen model specification must remain embedded in:

- Notebook;
- Task Report.

Stage 5 may reconstruct from these frozen artifacts.

---

# 37. Notebook Figures

Stage 4.7 does not need to recreate all prior plots.

Keep visualization minimal.

Recommended:

`FRZ-01 — Final WOE Logistic coefficient forest plot`

Show:

- ten final coefficients;
- 95% CI;
- zero reference line;
- all ten marked as retained through surrounding text, not visual Keep/Drop encoding.

Do not create unnecessary ROC/KS figures if the numerical apparent metrics are already clearly reported.

The focus of Stage 4.7 is model freeze, not another performance-analysis stage.

---

# 38. Final Evidence-Synthesis Table

Create one concise ten-row evidence-synthesis table with:

- Predictor
- Stage 4.4 Decision
- Stage 4.5 Status
- Stage 4.6 Negative Folds / 5
- Stage 4.6 Directionally Clear Folds / 5
- Stage 4.6 CI-Crosses-Zero Folds / 5
- Structural / Interpretability Note
- Stage 4.7 Final Decision

Stage 4.4 Decision for all ten:

> Keep / candidate approved

Stage 4.7 Final Decision for all ten:

> Retain

For `PAY_AMT5`, the Structural / Interpretability Note must capture the frozen rationale from Section 5.

This table explains WHY the final model retains all ten predictors.

It must not reopen selection.

---

# 39. Notebook Structure

Create:

`notebooks/10_final_woe_logistic_freeze.ipynb`

Recommended structure:

1. Stage 4.7 objective and boundaries
2. imports / environment
3. Git / model baseline summary
4. frozen outer split replay
5. Test technical identity check and seal
6. Human final-variable decision
7. Stage 4.4–4.6 evidence synthesis
8. PAY\_AMT5 retention rationale
9. final ten predictor list
10. final frozen feature definitions
11. final frozen bin definitions
12. compute Full-Train final WOE mappings
13. 41-row final WOE specification table
14. final WOE matrix integrity
15. fit final ten-variable unpenalized Logit
16. numerical sanity
17. Stage 4.5 deterministic reproduction check
18. final coefficient table
19. FRZ-01 coefficient forest plot
20. final Logistic equation
21. apparent Train AUC / KS reproduction
22. final model freeze record
23. Stage 5 handoff package
24. explicit Holdout-sealed / no-selection boundary

Notebook must clean-run from raw CSV.

---

# 40. Task Report Structure

Create:

`task_records/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md`

Include:

- Stage purpose;
- Git baseline;
- Stage 4.6 milestone;
- frozen outer split identity;
- Holdout sealing;
- Human final-variable decision;
- final ten predictors;
- Stage 4.4 / 4.5 / 4.6 evidence synthesis;
- PAY\_AMT5 retention rationale;
- PAY\_AMT4 / PAY\_AMT6 contextual rationale;
- no 10-vs-9 comparison;
- final feature definitions;
- final frozen bins;
- final WOE convention;
- 41-row final Full-Train WOE mapping;
- WOE integrity checks;
- final Logit specification;
- convergence / numerical sanity;
- Stage 4.5 reproduction check;
- final coefficient table;
- final equation;
- FRZ-01;
- final apparent Train AUC / KS;
- Stage 4.6 OOF context;
- evidence hierarchy;
- final model freeze record;
- Stage 5 handoff package;
- no feature selection;
- no re-binning;
- no regularization;
- no CV;
- no imbalance intervention;
- no calibration;
- no threshold optimization;
- no Holdout predictive analysis;
- Notebook clean-run;
- protected-file integrity;
- Git status;
- Reviewer handoff.

---

# 41. Immutable Architect Specification

Append this entire Stage 4.7 Architect Specification verbatim to the Task Report between the exact standalone lines:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.7 ================`

and:

`================ END ARCHITECT SPECIFICATION — STAGE 4.7 ================`

The marker lines appearing above inside inline backticks are descriptive references only.

The actual embedded authoritative block markers must be genuine standalone unescaped lines.

Provenance rule:

> Architect source text is authoritative.

Do not invent or predeclare a byte count or SHA-256 as an independent authority.

If fingerprinting is recorded:

- derive it mechanically from the actual embedded UTF-8 authoritative block;
- use the exact unique standalone BEGIN through END boundary inclusive;
- document whether any following line terminator is excluded;
- never alter Specification text to satisfy a fingerprint.

---

# 42. Reviewer Workflow

After Codex completes Stage 4.7:

> send the Stage 4.7 Notebook and Task Report to Reviewer.

Reviewer independently validates:

- Git baseline;
- Stage 4.6 PASS baseline;
- outer split identity;
- Holdout sealing;
- Human final-variable decision;
- exact ten predictors;
- PAY\_AMT5 retention rationale;
- no alternative subset;
- frozen feature definitions;
- frozen bin definitions;
- final 41-row WOE mapping;
- WOE matrix integrity;
- unpenalized Full-Train Logistic;
- convergence / numerical sanity;
- Stage 4.5 deterministic reproduction;
- final coefficient table;
- final equation;
- FRZ-01;
- final apparent AUC / KS;
- model freeze record;
- Stage 5 handoff package;
- no feature deletion;
- no re-binning;
- no CV;
- no regularization;
- no Holdout predictive analysis;
- Notebook clean-run;
- protected-file integrity;
- immutable Architect Specification;
- Git scope.

---

# 43. Reviewer FAIL

If Reviewer finds a substantive or documentation failure:

- no commit;
- return Required Fixes;
- do not begin Stage 5.

---

# 44. Reviewer PASS

If Stage 4.7 passes:

In the same Reviewer round:

1. append final Stage 4.7 Reviewer Result;
2. stage exactly:
   - `notebooks/10_final_woe_logistic_freeze.ipynb`
   - `task_records/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md`
3. commit Stage 4.7;
4. verify post-commit Git state.

Recommended commit message:

`Freeze final WOE logistic model`

No push.

After successful Reviewer PASS + commit:

> Stage 4 is CLOSED.

Stage 5 may then begin discussion.

---

# 45. Protected Scope

Do not modify:

- raw data;
- Stage 0–4.6 Notebooks;
- Stage 0–4.6 Task Reports;
- README;
- requirements;
- references;
- any other tracked file.

Only create:

- `notebooks/10_final_woe_logistic_freeze.ipynb`
- `task_records/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md`

---

# 46. Codex Git Rules

Codex:

- no git add;
- no commit;
- no push.

Expected Codex completion state:

- HEAD remains:
  `bac1d2f0af6693b9443b80322eb49891992ccfab`
- branch = `main`
- tracked diff empty
- staged diff empty
- exactly two authorized Stage 4.7 artifacts untracked
- remote none
- no push

Commit belongs to Reviewer after PASS.

---

# 47. Stage 4.7 Completion Criteria

All must pass:

1. correct project root;
2. branch `main`;
3. HEAD = `bac1d2f...`;
4. clean Stage 4.6 baseline;
5. exactly two authorized Stage 4.7 artifacts;
6. frozen 24,000 / 6,000 outer split reproduced;
7. Train hash exact;
8. Test hash exact;
9. Holdout sealed;
10. Train Good = 18,691;
11. Train Bad = 5,309;
12. Human final decision documented;
13. exactly ten predictors retained;
14. canonical order preserved;
15. PAY\_AMT5 explicitly retained;
16. PAY\_AMT5 rationale accurately documented;
17. PAY\_AMT4 / PAY\_AMT6 context documented;
18. no 10-vs-9 model;
19. no alternative subset;
20. frozen feature definitions preserved;
21. frozen Stage 4.2 / 4.3 bins preserved;
22. no re-binning;
23. final Full-Train WOE computed;
24. zero-support check complete;
25. zero-class check complete;
26. smoothing occurrence documented;
27. 41-row final WOE table complete;
28. 24,000 × 10 final WOE matrix;
29. no missing WOE;
30. no non-finite WOE;
31. exact row / ID / target alignment;
32. exactly one final substantive model;
33. model = ten-variable unpenalized MLE Logistic;
34. intercept included;
35. model converged;
36. design rank full;
37. coefficients finite;
38. SEs finite;
39. covariance usable;
40. probabilities finite;
41. no invalidating separation/numerical failure;
42. Stage 4.5 coefficient reproduction checked;
43. Stage 4.5 intercept reproduction checked;
44. reproduction within documented tolerance;
45. final coefficient table complete;
46. all ten final statuses = Retained;
47. final sign checks complete;
48. final Logistic equation documented;
49. FRZ-01 produced;
50. final apparent Train AUC computed;
51. final apparent Train KS computed;
52. AUC / KS reproduce Stage 4.5 within tolerance;
53. apparent metrics not used for selection;
54. Stage 4.6 OOF evidence summarized;
55. OOF limitation preserved;
56. evidence-synthesis table complete;
57. final model freeze record complete;
58. Stage 5 handoff package complete;
59. model structure explicitly frozen;
60. no p-value elimination;
61. no CI elimination;
62. no IV/VIF re-screening;
63. no regularization;
64. no class weighting;
65. no resampling;
66. no CV;
67. no calibration;
68. no operational threshold optimization;
69. no Holdout predictive analysis;
70. no Stage 5 execution;
71. Notebook clean-runs;
72. Task Report complete;
73. immutable Architect Specification preserved verbatim;
74. protected files unchanged;
75. staged diff empty;
76. no commit;
77. no push;
78. final Reviewer handoff reached.

---

# 48. Codex Completion Report

Return a concise Completion Report containing:

- pre-flight result;
- branch / HEAD;
- outer Train/Test identity;
- Holdout sealing;
- Human final-variable decision;
- exact final ten predictors;
- PAY\_AMT5 final rationale confirmation;
- frozen-bin confirmation;
- final WOE specification row count;
- final WOE matrix shape;
- missing/non-finite counts;
- zero-support / zero-class / smoothing counts;
- final model implementation;
- convergence result;
- numerical warnings;
- final intercept;
- final ten coefficients;
- ten sign checks;
- Stage 4.5 coefficient/intercept reproduction max absolute difference;
- final apparent Train AUC;
- final apparent Train KS;
- difference from Stage 4.5 reference AUC / KS;
- FRZ-01 status;
- final model freeze status;
- Stage 5 handoff completeness;
- confirmation:
  - all ten retained
  - PAY\_AMT5 retained
  - no 10-vs-9 model
  - no feature deletion
  - no re-binning
  - no alternative subset
  - no regularization
  - no class weighting/resampling
  - no CV
  - no calibration
  - no operational threshold optimization
  - no Holdout predictive analysis
- Notebook clean-run;
- protected-file integrity;
- final Git state;
- no commit;
- no push.

If successful, final line:

`Stage 4.7 implementation complete — final 10-variable WOE Logistic model frozen and refitted on the complete Development Train; Holdout remains sealed; awaiting Reviewer validation.`

If deterministic reproduction or final model validity fails, final line:

`Stage 4.7 BLOCKED by final-model reproducibility or numerical-validity issue — frozen predictor/bin structure was not altered and Architect review is required.`

================ END ARCHITECT SPECIFICATION — STAGE 4.7 ================

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

- Core Stage 4.7 model-freeze requirements: satisfied. The frozen Human decision retains all ten canonical predictors, including `PAY_AMT5`; the required Stage 4.4–4.6 evidence synthesis and PAY_AMT5/PAY_AMT4/PAY_AMT6 rationale are complete and do not reopen selection.
- Final Full-Train WOE and Logistic requirements: satisfied. The implementation preserves the approved features and bins, produces the complete 41-row WOE mapping and aligned 24,000 × 10 WOE matrix, and fits exactly one explicit-intercept unpenalized `statsmodels.Logit` model on the complete Development Train.
- Reproduction, numerical, freeze, and handoff requirements: satisfied. The final model reproduces the Stage 4.5 parameter/AUC/KS anchors within `1e-9`, passes numerical-validity checks, records the fitted equation and model freeze, and provides the complete Stage 5 handoff while keeping Holdout sealed.
- Scope requirements: satisfied. Exactly the two authorized Stage 4.7 artifacts are present; protected files are unchanged; no alternative subset, re-binning, CV, regularization, resampling, calibration, operational-threshold optimization, Holdout prediction, Stage 5 execution, staging, commit, remote, or push occurred.
- Immutable Architect Specification content: satisfied. The embedded unique standalone-marker block is byte-for-byte identical to the authoritative attachment.
- Immutable Specification fingerprint statement: not satisfied. The Task Report records `30,661` UTF-8 bytes and SHA-256 `0092dad718d3a9edbac7de88319193742b5128334dd54928d67d31ef6f23e4d9`, but the actual inclusive standalone-marker block—excluding the following line terminator as stated—is `23,488` UTF-8 bytes with SHA-256 `2be5b2dd43277591e5bbd25c1ccc8ba424b134aa18eaa26ed7e2b0b8a2ca0581`.

## Independent Verification

- Confirmed project root, branch `main`, Stage 4.6 PASS baseline HEAD `bac1d2f0af6693b9443b80322eb49891992ccfab`, empty tracked/staged diffs, exactly two authorized untracked Stage 4.7 artifacts, and no remote.
- Read the complete embedded Architect Specification and compared its unique marker-delimited bytes with `/Users/yu/.codex/attachments/d619b00c-f320-48ac-8c13-22f547bab562/pasted-text.txt`; exact content equality passed, while the recorded byte count and SHA-256 did not reproduce.
- Inspected all Notebook code for the outer split, Holdout sealing, frozen Human decision, feature/bin construction, Full-Train WOE calculation, single-model fit, numerical gates, Stage 4.5 reproduction, coefficient/equation output, freeze record, and Stage 5 handoff.
- Independently clean-ran the Notebook from raw CSV in an isolated kernel: 30 total cells, 15 code cells with execution counts 1–15, zero error outputs, one inline PNG, and all final machine-summary gates passed.
- Independently reconstructed the frozen features/bins, 41-row WOE mapping, 24,000 × 10 WOE matrix, and final Logit directly from the raw CSV. The refit reproduced intercept `-1.254276175288`, all ten reported coefficients, apparent Train AUC `0.772417097808`, apparent Train KS `0.430307867280`, design rank 11, zero-support bins = 0, zero-class bins = 0, and all ten negative coefficient signs.
- Visually checked FRZ-01; it contains all ten retained predictors, their 95% confidence intervals, and a zero reference line without Keep/Drop encoding.
- Recomputed all 28 protected tracked-file fingerprints plus the ignored Luca Notebook and Git index; all match the recorded pre-flight values.

## Issues Found

1. Both Task Report references to the mechanically derived Architect Specification fingerprint are incorrect. The embedded Specification itself is exact, so this is a limited audit-documentation defect and does not affect the frozen model or analytical results.

## Required Fixes

1. Correct both fingerprint statements—the `Prompt-Source Marker Validation` entry and the paragraph immediately before the embedded Specification—to `23,488` UTF-8 bytes and SHA-256 `2be5b2dd43277591e5bbd25c1ccc8ba424b134aa18eaa26ed7e2b0b8a2ca0581`, or remove the optional fingerprint claims. Preserve the stated inclusive standalone-marker boundary and do not modify the embedded Architect Specification text.
2. Return the corrected Task Report for focused Reviewer re-verification. No Notebook or analytical rerun is required unless other content changes.

No local commit was created because the Stage 4.7 artifact set requires this limited correction before a final `PASS` closeout.

## Architect Decision Needed

None

# REVIEWER RESULT — FOCUSED RE-REVIEW

## Verdict

PASS

## Requirement Check

- The sole issue from the prior `PASS WITH FIXES` review has been corrected in both required locations. The Task Report now records the actual inclusive standalone-marker Architect Specification fingerprint as `23,488` UTF-8 bytes with SHA-256 `2be5b2dd43277591e5bbd25c1ccc8ba424b134aa18eaa26ed7e2b0b8a2ca0581`, excluding the following line terminator exactly as documented.
- The embedded Architect Specification remains byte-for-byte identical to the authoritative source attachment.
- The Stage 4.7 Notebook is unchanged from the independently clean-run and recomputed artifact (SHA-256 `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863`). All final-variable decision, Full-Train WOE, final Logit, Stage 4.5 reproduction, numerical-validity, model-freeze, Holdout-sealing, and Stage 5 handoff requirements therefore retain their prior independently verified PASS status.
- Exactly the two authorized Stage 4.7 artifacts remain in scope; protected tracked files remain unchanged; no prohibited project or Git operation was introduced.

## Independent Verification

- Mechanically re-extracted the marker-delimited Architect Specification from the corrected Task Report and independently reproduced `23,488` bytes and SHA-256 `2be5b2dd43277591e5bbd25c1ccc8ba424b134aa18eaa26ed7e2b0b8a2ca0581`.
- Compared that block with `/Users/yu/.codex/attachments/d619b00c-f320-48ac-8c13-22f547bab562/pasted-text.txt`; exact byte equality passed.
- Confirmed the Notebook hash is unchanged from the prior isolated clean-run and independent full-model recomputation review.
- Recomputed all 28 protected tracked-file fingerprints; mismatches = `0`.
- Confirmed branch `main`, baseline HEAD `bac1d2f0af6693b9443b80322eb49891992ccfab`, empty tracked and staged diffs, exactly two authorized untracked Stage 4.7 artifacts, and no remote before Reviewer closeout.

## Issues Found

None. The prior limited fingerprint-documentation defect is resolved.

## Required Fixes

None

## Architect Decision Needed

None
