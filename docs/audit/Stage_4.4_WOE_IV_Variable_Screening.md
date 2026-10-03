# Stage 4.4A — WOE / IV Variable Screening Diagnostics

## Task Identification and Status

- Project: Credit Card Default Risk
- Repository root: `/Users/yu/2.工作/Risk Analysis Project`
- Stage: `Stage 4.4A — WOE / IV Variable Screening Diagnostics`
- Status: `Stage 4.4A complete — final candidate selection pending Architect/user Human Checkpoint.`
- Execution date: `2026-08-25T21:32:43-04:00`
- Authorized artifacts: this Task Report and `notebooks/07_woe_iv_variable_screening.ipynb` only.
- This report does not claim a Stage 4.4A Reviewer PASS, final candidate selection, Stage 4.4B completion, or Stage 4.4 closure.

## Immutable Architect Specification

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.4A ================

# Stage 4.4A — WOE / IV Variable Screening Diagnostics

## 1. Stage Context

Stage 4 固定结构：

- Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
- Stage 4.2 — PAY + Categorical Binning ✅ CLOSED
- Stage 4.3 — Continuous Variable Binning ✅ CLOSED
- Stage 4.4A — Screening Diagnostics ← CURRENT
- Stage 4.4B — Final Candidate Freeze
- Stage 4.5 — Candidate WOE Logistic Regression
- Stage 4.6 — Stratified 5-Fold Model Stability Test
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit

Stage 4.3 final Reviewer verdict:

> PASS

Stage 4.3 milestone commit:

`5b36512f317b532378977e66b90f1d379693bc4a`

Short:

`5b36512`

Commit message:

`Finalize continuous variable binning`

Parent:

`75c1ac1db6c76a7d80d1be3b13b8c8449ac92fd5`

Branch:

`main`

Stage 4.3 final Reviewer confirmed:

- all 17 continuous final mappings exact；
- all WOE / IV finite；
- no smoothing；
- Holdout Test sealed；
- no variable selection；
- no VIF；
- no Logistic；
- no KFold；
- no AUC / KS；
- final Stage 4.3 milestone commit created；
- working tree clean；
- staged diff empty；
- remote none；
- push none。

---

# 2. Stage 4.4 Overall Architecture — FROZEN

Stage 4.4 is split into exactly:

## Stage 4.4A

> Screening Diagnostics

Purpose:

> Generate evidence about individual predictor strength, WOE-space redundancy, multicollinearity, sparse-bin dependence, family overlap, and governance-review flags.

Stage 4.4A performs:

> **NO Keep / Drop decision.**

## Mandatory Human Checkpoint

Architect/user reviews evidence family by family and freezes final Keep / Drop / representative choices.

## Stage 4.4B

> Final Candidate Freeze

Only after explicit Architect authorization.

4.4B will:

- implement approved Keep / Drop decisions；
- recompute reduced WOE correlation；
- recompute reduced VIF；
- freeze Candidate Logistic Predictor List。

Stage 4.4A must not perform any Stage 4.4B work.

---

# 3. Stage 4.4 Philosophy — FROZEN

Stage 4.4 is NOT:

> “Rank variables by IV and keep the highest.”

It is NOT:

> “Automatically remove variables above correlation/VIF thresholds.”

The objective is:

> Identify a compact, interpretable, sufficiently informative set of predictors with limited unnecessary information duplication.

The final model is intended to be an interpretable WOE Logistic credit-risk model.

Selection philosophy:

> **Independent information + reasonable predictive evidence + stability/interpretability + limited redundancy**

has priority over:

> maximizing the number of high-IV predictors.

A small amount of information loss is acceptable if it materially improves:

- simplicity；
- coefficient interpretability；
- model stability；
- business narrative；
- portfolio / interview defensibility。

---

# 4. Stage 4.4A Objective

Stage 4.4A must answer five diagnostic questions:

1. How much standalone information does each candidate predictor contain?
2. Does any predictor's apparent IV depend disproportionately on a small/special bin?
3. Which pairs of WOE predictors provide highly overlapping risk representations?
4. Which predictors can be strongly explained by combinations of the other predictors?
5. Within structurally overlapping feature families, where does redundancy appear to exist?

Stage 4.4A does NOT answer:

> Which predictor should finally be retained?

That is Human Checkpoint work.

---

# 5. Git Pre-flight

Project root:

`/Users/yu/2.工作/Risk Analysis Project`

Expected:

- branch = `main`
- HEAD = `5b36512f317b532378977e66b90f1d379693bc4a`
- message = `Finalize continuous variable binning`
- parent = `75c1ac1db6c76a7d80d1be3b13b8c8449ac92fd5`
- working tree clean
- staged diff empty
- remote none

Verify Stage 4.3 commit scope contains exactly:

- `notebooks/06_continuous_binning.ipynb`
- `task_records/Stage_4.3_Continuous_Variable_Binning.md`

Verify Stage 4.3 Task Report contains final Reviewer:

> PASS

Any unexpected baseline mismatch:

> `BLOCKED`

Do not automatically:

- restore；
- reset；
- stash；
- clean；
- checkout；
- amend；
- rebase。

---

# 6. New Stage 4.4 Artifacts

Create exactly:

`notebooks/07_woe_iv_variable_screening.ipynb`

and:

`task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

Stage 4.4A creates these two artifacts.

Stage 4.4B will later extend the same two artifacts.

Do not modify Stage 0–4.3 artifacts.

---

# 7. Data Split and Holdout Protection

Load formal raw data:

`data/raw/UCI_Credit_Card.csv`

Replay frozen split exactly:

- Raw = 30,000
- Train = 24,000
- Test = 6,000
- `random_state=42`
- `stratify=default.payment.next.month`

Expected Train ID hash:

`bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`

Expected Test ID hash:

`d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

Test may only be used for:

- row count；
- ID overlap；
- ID union coverage；
- frozen Test ID hash。

Immediately afterward:

> delete / seal Test.

Forbidden:

- Test predictor transformation；
- Test bin assignment；
- Test WOE transformation；
- Test correlation；
- Test VIF；
- Test distribution；
- Test range；
- Test bin occupancy；
- Test target rate；
- Test WOE / IV；
- Test AUC / KS；
- Test model performance。

Holdout remains sealed until Stage 5.

---

# 8. Train Target Convention

Frozen:

- Good = target `0`
- Bad = target `1`

Expected Train totals:

- Count = 24,000
- Good = 18,691
- Bad = 5,309

WOE convention:

\[
WOE_j=
\ln
\left(
\frac{DistGood_j}{DistBad_j}
\right)
\]

Therefore:

- positive WOE = relatively safer；
- negative WOE = relatively riskier。

No convention change.

---

# 9. Exactly 29 Candidate Predictors

Stage 4.4A scope is exactly 29 predictors.

## A. Stage 4.2 PAY variables — 6

1. `PAY_0`
2. `PAY_2`
3. `PAY_3`
4. `PAY_4`
5. `PAY_5`
6. `PAY_6`

## B. Stage 4.2 delinquency summaries — 3

7. `delinquent_months`
8. `max_delinquency`
9. `severe_delinquent_months`

## C. Stage 4.2 categorical predictors — 3

10. `SEX`
11. `EDUCATION_GROUP`
12. `MARRIAGE_GROUP`

## D. Stage 4.3 demographic / capacity continuous predictors — 2

13. `AGE`
14. `LIMIT_BAL`

## E. Stage 4.3 BILL predictors — 6

15. `BILL_AMT1`
16. `BILL_AMT2`
17. `BILL_AMT3`
18. `BILL_AMT4`
19. `BILL_AMT5`
20. `BILL_AMT6`

## F. Stage 4.3 PAY_AMT predictors — 6

21. `PAY_AMT1`
22. `PAY_AMT2`
23. `PAY_AMT3`
24. `PAY_AMT4`
25. `PAY_AMT5`
26. `PAY_AMT6`

## G. Stage 4.3 utilization predictors — 3

27. `current_utilization`
28. `avg_utilization`
29. `max_utilization`

Exactly 29.

No ID predictor.

No raw EDUCATION predictor.

No raw MARRIAGE predictor.

No additional feature engineering.

---

# 10. Reproduce Frozen Stage 3 Derived Features

Reproduce exactly:

## EDUCATION_GROUP

- 1 → Graduate School
- 2 → University
- 3 → High School
- 0 / 4 / 5 / 6 → Other / Unknown

## MARRIAGE_GROUP

- 1 → Married
- 2 → Single
- 0 / 3 → Other / Unknown

## delinquent_months

\[
\sum_i1(PAY_i>0)
\]

## max_delinquency

\[
\max_i\max(PAY_i,0)
\]

## severe_delinquent_months

\[
\sum_i1(PAY_i\ge2)
\]

## current_utilization

\[
\frac{\max(BILL\_AMT1,0)}{LIMIT\_BAL}
\]

## avg_utilization

\[
\frac{
mean(\max(BILL\_AMT1,0),...,\max(BILL\_AMT6,0))
}{
LIMIT\_BAL
}
\]

## max_utilization

\[
\frac{
\max(\max(BILL\_AMT1,0),...,\max(BILL\_AMT6,0))
}{
LIMIT\_BAL
}
\]

Raw PAY and raw BILL columns remain unchanged.

No clipping/winsorization/capping.

Utilization >1 remains valid.

---

# 11. Reproduce Frozen Stage 4.2 Final Bins

Do not redesign.

## PAY_0

Exact final grouping:

- `[-2,-1,0]`
- `[1]`
- `[2+]`

## PAY_2 through PAY_6

Exact grouping:

- `[-2,-1,0,1]`
- `[2]`
- `[3+]`

The mapping definition includes code `1` even for months in which Train may not observe it.

Do not inspect Test.

## delinquent_months

- `0`
- `1`
- `2`
- `3`
- `4+`

## max_delinquency

- `0`
- `1`
- `2`
- `3+`

## severe_delinquent_months

- `0`
- `1`
- `2`
- `3`
- `4+`

## SEX

Use frozen original category treatment from Stage 4.2 final artifact.

No regrouping.

## EDUCATION_GROUP

Use:

- Graduate School
- University
- High School
- Other / Unknown

No regrouping.

## MARRIAGE_GROUP

Use:

- Married
- Single
- Other / Unknown

No regrouping.

Stage 4.4A must independently rebuild the final grouped WOE/IV tables from Train and verify they match the formal Stage 4.2 final artifacts.

No smoothing.

---

# 12. Reproduce Frozen Stage 4.3 Final Bins

No redesign.

## AGE

`<=25 / 26–35 / 36–49 / 50+`

Formally:

\[
(-\infty,25]
\]

\[
(25,35]
\]

\[
(35,49]
\]

\[
(49,+\infty)
\]

## LIMIT_BAL

`<=30k / (30k,140k] / (140k,240k] / >240k`

## BILL_AMT1–6

All six:

`Negative / Zero / (0,10k] / (10k,40k] / >40k`

## PAY_AMT1–6

All six:

`0 / (0,5k] / (5k,20k] / >20k`

## current / avg / max utilization

All three:

`0 / (0,0.2] / (0.2,0.6] / (0.6,1] / >1`

Do not reopen these bins.

---

# 13. Final WOE Transformation Matrix

Construct an in-memory matrix:

> 24,000 rows × 29 WOE predictors

Each predictor must be represented by its frozen final-bin WOE value.

Conceptually:

\[
X_{1,WOE},...,X_{29,WOE}
\]

Requirements:

- exactly 24,000 rows；
- exactly 29 predictor columns；
- no NaN；
- no ±inf；
- no missing bin mapping；
- no duplicated ID；
- row order / ID alignment preserved；
- target unchanged；
- all WOE values finite；
- no smoothing；
- deterministic under rerun。

The correlation and VIF diagnostics must use this WOE matrix.

Do not use raw-variable correlation as the primary screening metric.

---

# 14. Why WOE Correlation — Record in Notebook/Report

Document explicitly:

> Stage 4.5 Logistic will consume WOE-transformed predictors, so Stage 4.4 examines redundancy primarily in the predictor representation actually entering the model.

WOE correlation measures:

> similarity in risk representation,

not:

> equality of raw feature values.

Example interpretation:

If two WOE predictors move similarly across customers, they may repeatedly give the future Logistic model similar risk information even when their raw numerical values are different.

---

# 15. Layer 1 — Individual Predictor Diagnostics

Produce one fixed-order 29-variable summary table.

Columns:

- Predictor
- Family
- Final Bin Count
- Final IV
- Minimum Bin Count
- Minimum Bin Share
- Maximum Absolute WOE
- Largest Single-bin IV Contribution
- Largest Single-bin IV Share of Total IV
- Sparse-bin Flag
- Governance Review Flag
- Notes

Do not sort as a Keep/Drop ranking.

Use fixed 29-variable scope order.

---

# 16. Final IV

Use final grouped IV only.

Do NOT use:

- Stage 4.2 raw-state diagnostic IV；
- Stage 4.3 pre-bin IV；

as the main Stage 4.4 screening IV.

Only final frozen bins count.

Stage 4.3 final grouped examples already include approximately:

- AGE ≈ 0.0204
- LIMIT_BAL ≈ 0.1904
- BILL variables ≈ 0.012–0.017
- PAY_AMT1 ≈ 0.1755
- PAY_AMT2 ≈ 0.1511
- PAY_AMT3 ≈ 0.1220
- avg_utilization ≈ 0.1213

Codex must independently recompute exact values rather than rely on these rounded examples.

---

# 17. IV Interpretation Boundary

IV is evidence only.

Reference descriptors may be displayed:

\[
IV<0.02
\]

`very weak`

\[
0.02\le IV<0.10
\]

`weak`

\[
0.10\le IV<0.30
\]

`moderate`

Higher values may be described as strong diagnostic signal.

But Stage 4.4A must NOT implement:

> `IV < threshold → Drop`

or:

> `highest IV → Keep`

Correct interpretation:

> IV describes standalone discriminatory information, not unique information after redundancy is considered.

---

# 18. Sparse-bin Diagnostic

For every predictor identify:

> minimum final-bin Train share.

Define diagnostic:

`Sparse-bin Flag = Yes`

if any frozen final bin has:

\[
Train\ Share<3\%
\]

This is a diagnostic flag only.

Some sparse final bins were intentionally preserved under earlier Human-approved special-state rules.

Do not:

- merge；
- smooth；
- delete variable；

because of this flag.

---

# 19. Signal Concentration Diagnostic

For each predictor calculate:

\[
\frac{
\max_j IVContribution_j
}{
TotalIV
}
\]

when Total IV > 0.

Purpose:

> show whether much of a predictor's apparent signal is concentrated in one bin.

Also display the corresponding:

- bin label；
- bin Train share；
- bin IV contribution。

Do not set an automatic rejection threshold.

Do not interpret concentration alone as instability.

Human Review will decide whether sparse/special-bin concentration matters.

---

# 20. Governance Review Flag

This is an Architect-defined project review flag.

Flag these four demographic-family predictors for explicit Human governance discussion:

- AGE
- SEX
- EDUCATION_GROUP
- MARRIAGE_GROUP

The flag:

- is not a mathematical input；
- does not modify IV；
- does not modify correlation；
- does not modify VIF；
- does not automatically Keep/Drop；
- is not a legal conclusion。

Correct wording:

> `Governance Review Flag — Human policy/model-governance consideration required before final candidate freeze.`

Do not provide legal advice.

---

# 21. Layer 2 — Full 29×29 WOE Correlation Matrix

Compute:

> Pearson correlation across the 29 WOE predictor columns.

For predictors \(i,j\):

\[
Corr(X_{i,WOE},X_{j,WOE})
\]

Output:

1. full numeric 29×29 correlation matrix；
2. full 29×29 heatmap；
3. high-correlation pair table。

The diagonal = 1 and must not be treated as redundancy evidence.

---

# 22. Correlation Diagnostic Thresholds

Use absolute correlation:

\[
|r|
\]

Diagnostic interpretation:

## Review flag

\[
|r|\ge0.70
\]

Meaning:

> noticeable pairwise WOE-space redundancy requiring Human review.

## Strong redundancy flag

\[
|r|\ge0.85
\]

Meaning:

> very strong pairwise information overlap.

These thresholds:

> are flags only.

Never automatically remove either predictor.

---

# 23. Negative Correlation

Use:

\[
|r|
\]

rather than only positive correlation.

A pair with:

\[
r=-0.90
\]

is also highly redundant.

Because WOE convention is consistent across variables, many similar risk variables may show positive correlation, but negative high-magnitude correlation must still be flagged.

---

# 24. High-correlation Pair Table

Create a compact table containing every unique non-diagonal pair satisfying:

\[
|r|\ge0.70
\]

Columns:

- Predictor A
- Family A
- Predictor B
- Family B
- Correlation
- Absolute Correlation
- Flag Level:
  - `Review`
  - `Strong`
- Structural Relationship Note

Sorting by descending absolute correlation is allowed here because this is:

> redundancy inspection,

not:

> variable-selection ranking.

No Keep/Drop field.

---

# 25. Structural Relationship Notes

For high-correlation pairs identify factual structural relationships where applicable.

Examples:

- adjacent PAY months；
- PAY versus delinquency summary；
- PAY_AMT adjacent months；
- BILL adjacent months；
- current/avg/max utilization；
- LIMIT_BAL versus utilization；
- raw BILL versus utilization。

Do not infer causal relationships.

Do not write:

> Predictor A should therefore be dropped.

---

# 26. Layer 3 — Family-level WOE Correlation Diagnostics

Do not rely only on one giant 29×29 heatmap.

Create dedicated family diagnostics.

---

# 27. Family A — PAY / Delinquency

Exactly 9 variables:

- PAY_0
- PAY_2
- PAY_3
- PAY_4
- PAY_5
- PAY_6
- delinquent_months
- max_delinquency
- severe_delinquent_months

Produce:

- 9×9 numeric WOE correlation matrix；
- 9×9 heatmap；
- family-specific pair table for \(|r|\ge0.70\)。

Explicit questions:

1. How much do raw monthly PAY states overlap?
2. How strongly do the three summaries overlap with raw PAY?
3. Does a summary appear to encode information already distributed across several PAY months?
4. Does recent PAY appear distinguishable from broader historical persistence/severity information?

Do not answer:

> which one should be kept.

---

# 28. PAY / Delinquency Structural Note

Record exact deterministic relationships:

\[
\sum_i1(PAY_i>0)
\]

\[
\sum_i1(PAY_i\ge2)
\]

\[
\max_i\max(PAY_i,0)
\]

Therefore:

> these summaries are structurally derived from the six raw PAY fields.

Important:

Pairwise correlation below 0.70 does NOT prove independence.

Several PAY variables may jointly explain a summary.

This motivates the later VIF diagnostic.

---

# 29. Family B — PAY_AMT1–6

Exactly:

- PAY_AMT1
- PAY_AMT2
- PAY_AMT3
- PAY_AMT4
- PAY_AMT5
- PAY_AMT6

Produce:

- 6×6 WOE correlation matrix；
- heatmap；
- high-correlation pair table。

Questions:

1. How strongly do payment months repeat the same WOE risk representation?
2. Is recent payment behavior already capturing most of the same pattern as older months?
3. Do any older PAY_AMT variables retain visibly distinct WOE information?

Do not choose a representative.

---

# 30. Family C — LIMIT_BAL / BILL / Utilization

Exactly 10 variables:

- LIMIT_BAL
- BILL_AMT1
- BILL_AMT2
- BILL_AMT3
- BILL_AMT4
- BILL_AMT5
- BILL_AMT6
- current_utilization
- avg_utilization
- max_utilization

Produce:

- 10×10 WOE correlation matrix；
- heatmap；
- high-correlation pair table。

Questions:

1. How much do six raw BILL predictors repeat each other?
2. How much do BILL predictors overlap with utilization?
3. How redundant are current / avg / max utilization?
4. Does LIMIT_BAL retain a visibly distinct WOE representation despite appearing in utilization denominators?

No Keep/Drop.

---

# 31. Family D — Demographic Review

Exactly:

- AGE
- SEX
- EDUCATION_GROUP
- MARRIAGE_GROUP

Produce:

- 4×4 WOE correlation matrix；
- compact heatmap；
- Individual IV / bin-support / concentration evidence；
- Governance Review Flag。

Questions:

1. Is statistical signal weak/moderate?
2. Is signal concentrated in sparse/special bins?
3. Are these variables redundant with one another?
4. Which variables will require explicit governance discussion at Human Checkpoint?

Do not make governance-based Keep/Drop decisions.

---

# 32. Layer 4 — Full-candidate VIF

Calculate VIF on the exact:

> 24,000 × 29 WOE predictor matrix.

For each predictor \(j\):

\[
VIF_j=
\frac{1}{1-R_j^2}
\]

where \(R_j^2\) comes from regressing predictor \(j\)'s WOE values on all other 28 WOE predictors with an intercept.

This is an auxiliary multicollinearity diagnostic.

It is NOT Stage 4.5 predictive modeling.

---

# 33. VIF Implementation

Use a deterministic implementation available within the existing environment.

Do not install or upgrade packages.

It is acceptable to use:

- existing `statsmodels` if available；

or:

- deterministic NumPy / linear algebra implementation.

Include an intercept in each auxiliary regression.

Document method.

If exact or numerical perfect multicollinearity produces:

\[
R^2\approx1
\]

report:

> VIF = infinity / effectively infinite

rather than silently perturbing data.

Do not add noise.

Do not drop predictors to make computation succeed.

---

# 34. VIF Diagnostic Interpretation

Use:

## Low concern

\[
VIF<5
\]

## Review

\[
5\le VIF<10
\]

## Severe multicollinearity flag

\[
VIF\ge10
\]

These are diagnostic flags only.

Forbidden:

> repeatedly remove the highest-VIF variable until all VIF < 5.

Forbidden:

> automatic VIF elimination.

---

# 35. Full VIF Table

Fixed 29-variable order.

Columns:

- Predictor
- Family
- VIF
- Tolerance (=1/VIF), when finite
- Flag Level
- Structural Note

Flag Level:

- `Low`
- `Review`
- `Severe`
- `Infinite`, if applicable

No Keep/Drop.

---

# 36. Why Correlation and VIF Are Both Required

Notebook / Task Report must explain:

Pairwise WOE correlation asks:

> Are two individual predictors giving similar risk representations?

VIF asks:

> Can one predictor be strongly explained by a combination of all other predictors?

Therefore a predictor may have:

\[
\max|pairwise\ r|<0.70
\]

but still:

\[
VIF\ge10
\]

especially when structurally derived from several other predictors.

PAY/delinquency summaries are a particularly important example.

---

# 37. No Raw-Correlation Selection

Raw-variable correlations may be inspected only if needed as explanatory context.

They must not replace WOE correlation as the primary redundancy screen.

Do not make Keep/Drop decisions from raw correlation.

---

# 38. No Automated Composite Score

Do not build a formula such as:

\[
IV-CorrelationPenalty-VIFPenalty
\]

No feature-selection score.

No weighted ranking.

No automatic shortlist.

---

# 39. No Variable Selection — Mandatory

At the end of Stage 4.4A:

> all 29 predictors remain candidates.

Do not produce:

- Keep；
- Drop；
- Selected；
- Rejected；
- Final Candidate；
- Recommended Candidate；
- Representative chosen。

Candidate count must remain:

> 29 / 29

until Human Checkpoint.

---

# 40. Explicitly Forbidden Selection Methods

Do not run:

- IV threshold deletion；
- correlation threshold deletion；
- VIF elimination；
- forward selection；
- backward elimination；
- stepwise selection；
- LASSO；
- Elastic Net；
- Ridge-based feature selection；
- recursive feature elimination；
- permutation importance；
- tree feature importance；
- mutual-information selection；
- univariate p-value selection；
- coefficient significance screening。

---

# 41. No Logistic Modeling

Do not run predictive Logistic Regression.

Do not calculate:

- candidate Logistic coefficients；
- p-values；
- Odds Ratios；
- coefficient signs；
- model deviance；
- AIC；
- BIC；
- model likelihood tests。

The auxiliary linear regressions required solely for VIF are allowed.

Stage 4.5 owns Logistic modeling.

---

# 42. No Performance Metrics

Do not calculate:

- ROC；
- AUC；
- KS；
- PR-AUC；
- accuracy；
- precision；
- recall；
- F1；
- confusion matrix；
- calibration；
- threshold optimization。

Stage 4.4 is variable-screening methodology only.

---

# 43. No Cross-validation

Do not run:

- KFold；
- StratifiedKFold；
- bootstrap；
- repeated CV；
- fold-based IV；
- fold-based correlation；
- fold-based VIF。

Formal model stability work remains Stage 4.6.

---

# 44. Stage 4.4A Diagnostic Figures

Use independent numbering:

- `SCR-01` — Full 29×29 WOE correlation heatmap
- `SCR-02` — PAY / delinquency 9×9 heatmap
- `SCR-03` — PAY_AMT1–6 heatmap
- `SCR-04` — LIMIT_BAL / BILL / utilization heatmap
- `SCR-05` — Demographic 4×4 heatmap
- `SCR-06` — Full-candidate VIF diagnostic plot

Keep figures simple.

For VIF plot:

- fixed 29-variable order preferred；
- horizontal presentation acceptable for readability；
- show reference lines at 5 and 10 if visually clean；
- do not use a “winner” color scheme；
- do not label predictors as Keep/Drop。

---

# 45. Heatmap Requirements

Each heatmap must:

- use WOE Pearson correlation；
- show predictor labels clearly；
- use a symmetric correlation scale；
- make magnitude/direction interpretable；
- avoid decorative complexity。

The family heatmaps are not new computations with different definitions.

They are readable subsets of the same WOE correlation logic.

---

# 46. Diagnostic Tables Required

At minimum Notebook and Task Report must contain:

1. 29-variable individual diagnostic summary；
2. full 29×29 WOE correlation matrix；
3. full high-correlation pair table；
4. PAY/delinquency family matrix；
5. PAY/delinquency flagged-pair table；
6. PAY_AMT family matrix；
7. PAY_AMT flagged-pair table；
8. LIMIT/BILL/utilization family matrix；
9. corresponding flagged-pair table；
10. demographic matrix；
11. full 29-variable VIF table；
12. concise family-level factual summary。

---

# 47. Family-level Factual Summary Standard

Allowed:

> `PAY_0 and PAY_2 exhibit strong WOE-space correlation.`

Allowed:

> `severe_delinquent_months has no single pairwise correlation above 0.85 but shows elevated VIF, indicating multivariate overlap.`

Allowed:

> `avg_utilization and max_utilization provide highly similar WOE risk representations.`

Allowed:

> `BILL_AMT variables show weak standalone IV and substantial family overlap.`

Not allowed:

> `Therefore drop BILL_AMT1–6.`

Not allowed:

> `PAY_0 should be retained.`

Not allowed:

> `avg_utilization is the final representative.`

---

# 48. Do Not Overstate Correlation

A high WOE correlation means:

> risk representations overlap strongly.

It does not prove:

- causal equivalence；
- identical business meaning；
- one predictor contains no incremental information；
- one predictor must be deleted。

Similarly, low pairwise correlation does not prove no redundancy.

VIF and structural relationships provide complementary evidence.

---

# 49. Human Checkpoint Groups — Mandatory Stop

After Stage 4.4A is complete:

> STOP.

Architect/user will review four groups.

## Group A — PAY / Delinquency

Questions:

- current severity versus historical persistence；
- raw PAY duplication；
- summary duplication；
- whether one recent PAY and one historical summary may represent distinct roles；
- whether more than two/three family members can be justified。

No decision in Stage 4.4A.

## Group B — PAY_AMT

Questions:

- whether PAY_AMT1 captures most recent-payment information；
- whether PAY_AMT2–6 add sufficiently distinct WOE information；
- how many monthly payment predictors remain justified。

No decision yet.

## Group C — LIMIT_BAL / BILL / Utilization

Questions:

- whether weak BILL predictors add unique information；
- current versus average versus maximum utilization redundancy；
- whether LIMIT_BAL retains distinct capacity information。

No decision yet.

## Group D — Demographics

Questions:

- individual IV；
- sparse-bin dependence；
- redundancy；
- governance review considerations。

No decision yet.

Only after explicit Human approval may Architect issue:

> Stage 4.4B — Final Candidate Freeze.

---

# 50. Current Architect Priors Are NOT Frozen Decisions

Stage 4.4A must not encode current hypotheses as conclusions.

Possible Architect priors include:

- PAY_0 may be useful as a recent delinquency measure；
- one delinquency summary may represent history；
- LIMIT_BAL may be useful；
- BILL_AMT variables may prove redundant；
- PAY_AMT1 may represent recent repayment；
- avg_utilization may prove useful；
- demographic predictors may be weak/governance-sensitive。

These are:

> hypotheses to test against diagnostics.

They are NOT:

> Stage 4.4A Keep/Drop instructions.

Do not bias output to confirm them.

---

# 51. Final Candidate Count Is Not Yet Known

Longer-term Stage 4.4 target is approximately:

> 6–10 candidate predictors

with:

> roughly 10–12 still acceptable if justified.

This is NOT a Stage 4.4A selection quota.

Do not force candidate count.

At the end of Stage 4.4A:

> exactly 29 predictors remain candidates.

---

# 52. Notebook Artifact

Create:

`notebooks/07_woe_iv_variable_screening.ipynb`

Recommended structure:

1. Stage 4.4A objective and scope
2. Git / baseline evidence
3. imports
4. raw CSV load
5. frozen split replay
6. Train/Test identity assertions
7. seal Test
8. reproduce frozen Stage 3 derived predictors
9. reproduce Stage 4.2 final bins
10. reproduce Stage 4.3 final bins
11. rebuild and verify 29 final WOE tables
12. construct 24,000×29 WOE matrix
13. integrity assertions
14. individual IV/bin-support/signal-concentration diagnostics
15. governance-review flags
16. SCR-01 full 29×29 WOE correlation
17. high-correlation pair table
18. SCR-02 PAY/delinquency diagnostics
19. SCR-03 PAY_AMT diagnostics
20. SCR-04 LIMIT/BILL/utilization diagnostics
21. SCR-05 demographics diagnostics
22. full-candidate VIF computation
23. SCR-06 VIF diagnostics
24. correlation vs VIF explanation
25. family-level factual summary
26. explicit statement 29/29 candidates remain
27. Human Checkpoint questions
28. mandatory Stage 4.4A stop

Notebook must:

- clean-run from raw CSV；
- not execute prior notebooks；
- not import outputs from executed prior notebooks；
- independently reproduce frozen transformations；
- save outputs；
- contain no cell errors。

---

# 53. Task Report Artifact

Create:

`task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

Record:

- Stage 4.4A status；
- Git pre-flight；
- Stage 4.3 milestone baseline；
- Stage 4.3 final Reviewer PASS；
- split/hash replay；
- Test sealing；
- Train totals；
- exactly 29 predictors；
- Stage 3 derived-feature reproduction；
- Stage 4.2 final-bin reproduction；
- Stage 4.3 final-bin reproduction；
- final WOE/IV verification；
- 29-variable WOE matrix integrity；
- individual diagnostic summary；
- sparse-bin evidence；
- signal-concentration evidence；
- governance-review flags；
- full WOE correlation matrix；
- high-correlation pairs；
- family correlation diagnostics；
- VIF methodology；
- full VIF results；
- pairwise-correlation versus VIF explanation；
- family-level factual findings；
- confirmation no variable selection；
- confirmation all 29 remain candidates；
- no Logistic；
- no performance metrics；
- no CV；
- Test remains sealed；
- Notebook clean-run；
- protected-input integrity；
- Git status；
- Human Checkpoint pending。

Status:

`Stage 4.4A complete — final candidate selection pending Architect/user Human Checkpoint.`

---

# 54. Immutable Stage 4.4A Architect Specification

Task Report must include this entire Specification verbatim between:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.4A ================`

and

`================ END ARCHITECT SPECIFICATION — STAGE 4.4A ================`

Do not paraphrase.

---

# 55. Protected Project Scope

Do not modify:

- raw CSV；
- Stage 0 artifacts；
- Stage 1 artifacts；
- Stage 2 artifacts；
- Stage 3 artifacts；
- Stage 4.1 artifacts；
- Stage 4.2 artifacts；
- Stage 4.3 artifacts；
- README；
- requirements；
- Luca reference；
- unrelated files。

Only two new Stage 4.4 files authorized.

---

# 56. No New Feature Engineering

Do not create:

- payment ratios；
- BILL/PAY ratios；
- additional delinquency summaries；
- new utilization metrics；
- interaction terms；
- trends；
- slopes；
- averages not already frozen；
- combined WOE scores；
- principal components；
- latent factors。

Stage 4.4A only screens existing frozen predictors.

---

# 57. No External Screening Artifact

Do not create:

- CSV candidate ranking；
- WOE mapping CSV；
- correlation CSV；
- VIF CSV；
- processed dataset；
- pickle；
- joblib；
- model；
- HTML；
- PDF；
- external figure files；
- `src/`；
- `tests/`。

Tables/figures remain inside Notebook and Task Report.

---

# 58. Git Rules

Codex:

- no git add；
- no commit；
- no push。

Expected post-Codex state:

- HEAD remains `5b36512f317b532378977e66b90f1d379693bc4a`
- tracked files unchanged
- staged diff empty
- exactly two authorized Stage 4.4A artifacts untracked:
  - `notebooks/07_woe_iv_variable_screening.ipynb`
  - `task_records/Stage_4.4_WOE_IV_Variable_Screening.md`
- remote remains none

Stage 4.4 milestone commit is NOT allowed after Stage 4.4A.

Commit is reserved for:

> Stage 4.4B completion + final Stage 4.4 Reviewer PASS.

---

# 59. Stage 4.4A Reviewer Workflow

After Codex completion:

> send to Reviewer.

Reviewer must independently validate Stage 4.4A.

## If FAIL

- append/return required fixes as appropriate；
- no commit；
- no Stage 4.4B；
- return to Architect/Codex workflow。

## If PASS

Reviewer verdict must explicitly state:

> `PASS — applies to Stage 4.4A Screening Diagnostics only.`

Reviewer should append Stage 4.4A Reviewer Result to the Task Report.

But:

- no git add；
- no commit；
- no push。

Then mandatory Architect/user Human Checkpoint.

---

# 60. Reviewer Independent Validation Expectations

Reviewer should independently verify, at minimum:

- HEAD / branch / Git baseline；
- Stage 4.3 PASS / commit baseline；
- split and Train/Test hashes；
- Test sealing；
- 29-variable scope；
- frozen feature reproduction；
- frozen Stage 4.2 mappings；
- frozen Stage 4.3 mappings；
- exact final WOE/IV recomputation；
- WOE matrix row/column integrity；
- individual diagnostic metrics；
- sparse-bin calculation；
- signal-concentration calculation；
- governance flags；
- correlation matrix；
- flagged pair thresholds；
- family matrices；
- VIF formula/results；
- no automated deletion；
- no Logistic；
- no AUC/KS；
- no CV；
- all 29 variables remain candidates；
- Notebook clean-run；
- protected-input integrity；
- Git state。

No final candidate selection.

---

# 61. Stage 4.4A Completion Criteria

All must pass:

1. correct Stage 4.3 HEAD；
2. branch `main`；
3. clean initial working tree；
4. no remote；
5. Stage 4.3 final PASS verified；
6. split replay exact；
7. Train hash exact；
8. Test hash exact；
9. Test sealed immediately；
10. Train Count = 24,000；
11. Good = 18,691；
12. Bad = 5,309；
13. exactly 29 predictors；
14. frozen Stage 3 derived features reproduced；
15. Stage 4.2 final bins reproduced exactly；
16. Stage 4.3 final bins reproduced exactly；
17. no re-binning；
18. no smoothing；
19. 29 final WOE tables verified；
20. 24,000×29 WOE matrix constructed；
21. no missing WOE；
22. no infinite WOE；
23. IDs aligned；
24. target unchanged；
25. 29-variable individual summary created；
26. final IV used；
27. min-bin share calculated；
28. max |WOE| calculated；
29. largest-bin IV contribution calculated；
30. largest-bin IV share calculated；
31. sparse-bin flags calculated；
32. governance-review flags recorded；
33. full 29×29 WOE Pearson correlation calculated；
34. SCR-01 created；
35. \(|r|\ge0.70\) pair table created；
36. \(|r|\ge0.85\) strong flags correct；
37. negative correlations handled using absolute magnitude；
38. PAY/delinquency matrix created；
39. SCR-02 created；
40. PAY_AMT matrix created；
41. SCR-03 created；
42. LIMIT/BILL/utilization matrix created；
43. SCR-04 created；
44. demographics matrix created；
45. SCR-05 created；
46. full 29-variable VIF calculated；
47. intercept used in VIF auxiliary regressions；
48. VIF thresholds correctly flagged；
49. SCR-06 created；
50. no iterative VIF elimination；
51. pairwise-correlation vs VIF distinction documented；
52. deterministic PAY-summary relationships documented；
53. family-level factual summary produced；
54. no Keep decisions；
55. no Drop decisions；
56. no shortlist；
57. no IV cutoff deletion；
58. no correlation deletion；
59. no VIF deletion；
60. no stepwise；
61. no LASSO；
62. no model-based feature importance；
63. no predictive Logistic；
64. no coefficients；
65. no p-values；
66. no Odds Ratios；
67. no AUC；
68. no KS；
69. no CV；
70. no Test predictor/risk analysis；
71. exactly 29/29 candidates remain；
72. Human Checkpoint questions recorded；
73. Notebook clean-runs；
74. Task Report complete；
75. immutable Stage 4.4A spec preserved；
76. protected files unchanged；
77. exactly two authorized Stage 4.4A artifacts created；
78. staged diff empty；
79. no commit；
80. no push；
81. mandatory stop reached。

---

# 62. Codex Completion Report

Return a concise report containing:

- pre-flight result；
- branch / HEAD；
- Stage 4.3 PASS baseline；
- Train/Test hash verification；
- Holdout sealed；
- 29-variable scope；
- frozen Stage 3 feature reproduction；
- Stage 4.2 mapping verification；
- Stage 4.3 mapping verification；
- WOE/IV table verification；
- WOE matrix shape；
- individual IV/support/concentration summary completed；
- sparse-bin flags；
- governance-review flags；
- full 29×29 WOE correlation；
- count of \(|r|\ge0.70\) pairs；
- count of \(|r|\ge0.85\) pairs；
- PAY/delinquency family diagnostics；
- PAY_AMT family diagnostics；
- LIMIT/BILL/utilization diagnostics；
- demographic diagnostics；
- VIF methodology；
- VIF flag counts；
- confirmation no automated elimination；
- confirmation 29/29 remain candidates；
- SCR-01–SCR-06 status；
- Notebook clean-run；
- protected-input integrity；
- final Git status；
- confirmation:
  - no re-binning
  - no smoothing
  - no Keep/Drop
  - no IV threshold deletion
  - no correlation deletion
  - no VIF deletion
  - no Logistic
  - no coefficients/p-values
  - no AUC/KS
  - no CV
  - no Test risk analysis
  - no commit
  - no push

Final line:

`Stage 4.4A complete — awaiting Reviewer validation and Architect/user Human Checkpoint for final candidate selection.`

================ END ARCHITECT SPECIFICATION — STAGE 4.4A ================

## Git Pre-flight and Stage 4.3 Baseline

- Pre-flight result: PASS before any Stage 4.4A write.
- Branch: `main`
- HEAD: `5b36512f317b532378977e66b90f1d379693bc4a`
- Parent: `75c1ac1db6c76a7d80d1be3b13b8c8449ac92fd5`
- HEAD message: `Finalize continuous variable binning`
- Initial tracked diff: empty.
- Initial staged diff: empty.
- Initial untracked files: none.
- Remote: none.
- HEAD scope was exactly `notebooks/06_continuous_binning.ipynb` and `task_records/Stage_4.3_Continuous_Variable_Binning.md`.
- Stage 4.3 Notebook: 1,901,709 bytes; SHA-256 `97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b`.
- Stage 4.3 Task Report: 154,540 bytes; SHA-256 `a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5`.
- The final `# REVIEWER RESULT` in the Stage 4.3 Task Report states `PASS`; this is a historical Stage 4.3 baseline only.

## Protected-input Baseline

The pre-flight manifest covered 22 tracked files. Manifest SHA-256: `e0692d1ddd5a266a16306fdd3ad866f4e558f60916bc82a665b8ce098565e4db`.

| Protected path | Bytes at final check | SHA-256 before | SHA-256 after | Result |
|---|---:|---|---|---|
| `.gitignore` | 534 | `300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1` | `300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1` | PASS |
| `README.md` | 2,245 | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` | PASS |
| `data/README.md` | 1,004 | `c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0` | `c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0` | PASS |
| `data/raw/UCI_Credit_Card.csv` | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` | PASS |
| `notebooks/01_baseline_replication.ipynb` | 1,057 | `ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20` | `ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20` | PASS |
| `notebooks/01_luca_audit_and_data_understanding.ipynb` | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` | PASS |
| `notebooks/02_train_test_split_and_eda.ipynb` | 937,648 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` | PASS |
| `notebooks/03_feature_engineering_and_sanity_checks.ipynb` | 142,371 | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` | PASS |
| `notebooks/04_woe_iv_binning_method_design.ipynb` | 52,998 | `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9` | `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9` | PASS |
| `notebooks/05_pay_categorical_binning.ipynb` | 971,553 | `79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796` | `79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796` | PASS |
| `notebooks/06_continuous_binning.ipynb` | 1,901,709 | `97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b` | `97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b` | PASS |
| `references/README.md` | 1,109 | `a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c` | `a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c` | PASS |
| `reports/figures/.gitkeep` | 60 | `73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63` | `73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63` | PASS |
| `requirements.txt` | 95 | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` | PASS |
| `task_records/Stage_1_Project_Foundation.md` | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` | PASS |
| `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md` | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` | PASS |
| `task_records/Stage_3.1_Train_Test_Split_and_EDA.md` | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` | PASS |
| `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md` | 77,812 | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` | PASS |
| `task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md` | 50,459 | `2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851` | `2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851` | PASS |
| `task_records/Stage_4.2_PAY_Categorical_Binning.md` | 102,407 | `9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60` | `9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60` | PASS |
| `task_records/Stage_4.3_Continuous_Variable_Binning.md` | 154,540 | `a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5` | `a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5` | PASS |
| `task_records/TASK_REPORT_Task_0.1_Current-State_Audit.md` | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` | PASS |

All 22 protected inputs matched their pre-flight hashes when this report was constructed.

## Work Performed

1. Replayed the frozen 80/20 stratified split from the raw CSV and checked row counts, disjoint IDs, full ID coverage, and frozen ID hashes.
2. Deleted the in-memory Test frame immediately after those technical identity checks; no Test predictor, target-risk, WOE/IV, stability, or performance analysis followed.
3. Independently reproduced the frozen Stage 3 grouped categorical, delinquency-summary, and utilization features without executing prior Notebooks.
4. Independently reproduced all frozen Stage 4.2 and Stage 4.3 final bins without re-binning or smoothing.
5. Rebuilt 29 final WOE/IV tables and a finite aligned 24,000×29 WOE matrix.
6. Computed individual IV/support/concentration/governance diagnostics, full and family WOE correlations, threshold pair tables, and full-candidate VIF with an intercept.
7. Created SCR-01 through SCR-06 inside the Notebook only.
8. Stopped with all 29 predictors still candidates pending the required Human Checkpoint.

## Files Created and Modified

Created:

- `notebooks/07_woe_iv_variable_screening.ipynb`
- `task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

Modified protected/tracked files: none.

External screening artifacts, model files, processed data, CSV exports, HTML/PDF files, external figures, checkpoints, `src/`, and `tests/`: none created.

## Split Replay, Holdout Protection, and Train Totals

- Raw rows: 30,000.
- Train rows: 24,000.
- Test rows: 6,000.
- Train/Test ID overlap: 0.
- Full-ID coverage: true.
- Train ID SHA-256: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`.
- Test ID SHA-256: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Test handling: `SEALED — Test deleted after technical row/ID/hash assertions`.
- Train target convention: Bad = 1; Good = 0.
- Train Count: 24,000; Good: 18,691; Bad: 5,309.

## Fixed 29-variable Registry

The fixed order was:

`PAY_0, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6, delinquent_months, max_delinquency, severe_delinquent_months, SEX, EDUCATION_GROUP, MARRIAGE_GROUP, AGE, LIMIT_BAL, BILL_AMT1, BILL_AMT2, BILL_AMT3, BILL_AMT4, BILL_AMT5, BILL_AMT6, PAY_AMT1, PAY_AMT2, PAY_AMT3, PAY_AMT4, PAY_AMT5, PAY_AMT6, current_utilization, avg_utilization, max_utilization`

Family counts: PAY 6; delinquency summaries 3; demographic categoricals 3; AGE/LIMIT 2; BILL 6; PAY_AMT 6; utilization 3. Exactly 29/29 remain candidates.

## Frozen Transformation and Bin Verification

- Stage 3 features: PASS — 24,000 rows, ID/order/target unchanged; raw PAY/BILL/PAY_AMT/AGE/LIMIT sources unchanged; frozen groups, summaries, and utilization reproduced exactly; utilization values above 1 retained; no clipping, capping, or winsorization.
- Stage 4.2 mappings: PASS — PAY_0, PAY_2–PAY_6, three delinquency summaries, SEX, EDUCATION_GROUP, and MARRIAGE_GROUP reproduced exactly.
- Stage 4.3 mappings: PASS — AGE, LIMIT_BAL, six BILL, six PAY_AMT, and three utilization mappings reproduced exactly.
- Exactly 29 frozen final-bin columns were created in memory; mappings were deterministic and source integrity was retained.
- No prior Notebook was executed or used as an executed-output dependency.

## Final WOE / IV Verification — Complete 29-table Record

Formula: `WOE = ln(Dist Good / Dist Bad)` and `IV contribution = (Dist Good - Dist Bad) × WOE`. No smoothing was used. Every table reconciled to Count 24,000, Good 18,691, Bad 5,309; every final bin had Good > 0 and Bad > 0; all WOE, contributions, and final IV values were finite.

#### PAY_0

Final IV: `0.872145621429`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| [-2,-1,0] | 18506 | 2540 | 15966 | 0.771083333 | 0.137252783 | 0.854207908 | 0.478432850 | 0.579658749 | 0.217821300 |
| [1] | 3000 | 1031 | 1969 | 0.125000000 | 0.343666667 | 0.105344818 | 0.194198531 | -0.611642036 | 0.054346666 |
| [2+] | 2494 | 1738 | 756 | 0.103916667 | 0.696872494 | 0.040447274 | 0.327368619 | -2.091087561 | 0.599977656 |

#### PAY_2

Final IV: `0.548098696196`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| [-2,-1,0,1] | 20473 | 3329 | 17144 | 0.853041667 | 0.162604406 | 0.917232893 | 0.627048408 | 0.380337669 | 0.110368090 |
| [2] | 3145 | 1749 | 1396 | 0.131041667 | 0.556120827 | 0.074688353 | 0.329440573 | -1.484071823 | 0.378070591 |
| [3+] | 382 | 231 | 151 | 0.015916667 | 0.604712042 | 0.008078754 | 0.043511019 | -1.683776505 | 0.059660015 |

#### PAY_3

Final IV: `0.414433849225`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| [-2,-1,0,1] | 20642 | 3546 | 17096 | 0.860083333 | 0.171785680 | 0.914664812 | 0.667922396 | 0.314385679 | 0.077572282 |
| [2] | 3049 | 1580 | 1469 | 0.127041667 | 0.518202689 | 0.078593976 | 0.297607836 | -1.331481581 | 0.291612921 |
| [3+] | 309 | 183 | 126 | 0.012875000 | 0.592233010 | 0.006741212 | 0.034469768 | -1.631842877 | 0.045248647 |

#### PAY_4

Final IV: `0.359543102661`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| [-2,-1,0,1] | 21190 | 3802 | 17388 | 0.882916667 | 0.179424257 | 0.930287304 | 0.716142400 | 0.261614438 | 0.056023399 |
| [2] | 2542 | 1338 | 1204 | 0.105916667 | 0.526357199 | 0.064416029 | 0.252024863 | -1.364165246 | 0.255929452 |
| [3+] | 268 | 169 | 99 | 0.011166667 | 0.630597015 | 0.005296667 | 0.031832737 | -1.793417496 | 0.047590252 |

#### PAY_5

Final IV: `0.327106490815`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| [-2,-1,0,1] | 21636 | 3997 | 17639 | 0.901500000 | 0.184738399 | 0.943716227 | 0.752872481 | 0.225929649 | 0.043117261 |
| [2] | 2102 | 1148 | 954 | 0.087583333 | 0.546146527 | 0.051040608 | 0.216236579 | -1.443751536 | 0.238501938 |
| [3+] | 262 | 164 | 98 | 0.010916667 | 0.625954198 | 0.005243165 | 0.030890940 | -1.773537580 | 0.045487292 |

#### PAY_6

Final IV: `0.288165668499`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| [-2,-1,0,1] | 21526 | 4016 | 17510 | 0.896916667 | 0.186565084 | 0.936814510 | 0.756451309 | 0.213847133 | 0.038570153 |
| [2] | 2230 | 1130 | 1100 | 0.092916667 | 0.506726457 | 0.058851854 | 0.212846110 | -1.285546084 | 0.197966713 |
| [3+] | 244 | 163 | 81 | 0.010166667 | 0.668032787 | 0.004333637 | 0.030702581 | -1.957939677 | 0.051628802 |

#### delinquent_months

Final IV: `0.871421079861`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 15896 | 1830 | 14066 | 0.662333333 | 0.115123301 | 0.752554705 | 0.344697683 | 0.780805940 | 0.318457186 |
| 1 | 3572 | 1070 | 2502 | 0.148833333 | 0.299552072 | 0.133861217 | 0.201544547 | -0.409206867 | 0.027696484 |
| 2 | 1532 | 612 | 920 | 0.063833333 | 0.399477807 | 0.049221550 | 0.115275947 | -0.850997243 | 0.056212109 |
| 3 | 936 | 471 | 465 | 0.039000000 | 0.503205128 | 0.024878284 | 0.088717273 | -1.271459319 | 0.081168677 |
| 4+ | 2064 | 1326 | 738 | 0.086000000 | 0.642441860 | 0.039484244 | 0.249764551 | -1.844616977 | 0.387886624 |

#### max_delinquency

Final IV: `0.794582506999`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 15896 | 1830 | 14066 | 0.662333333 | 0.115123301 | 0.752554705 | 0.344697683 | 0.780805940 | 0.318457186 |
| 1 | 1380 | 346 | 1034 | 0.057500000 | 0.250724638 | 0.055320743 | 0.065172349 | -0.163887351 | 0.001614554 |
| 2 | 5785 | 2545 | 3240 | 0.241041667 | 0.439930856 | 0.173345460 | 0.479374647 | -1.017195951 | 0.311291649 |
| 3+ | 939 | 588 | 351 | 0.039125000 | 0.626198083 | 0.018779092 | 0.110755321 | -1.774579355 | 0.163219118 |

#### severe_delinquent_months

Final IV: `0.846474512620`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 17276 | 2176 | 15100 | 0.719833333 | 0.125955082 | 0.807875448 | 0.409870032 | 0.678567784 | 0.270073653 |
| 1 | 2799 | 912 | 1887 | 0.116625000 | 0.325830654 | 0.100957680 | 0.171783763 | -0.531535076 | 0.037646548 |
| 2 | 1279 | 571 | 708 | 0.053291667 | 0.446442533 | 0.037879193 | 0.107553212 | -1.043583747 | 0.072710673 |
| 3 | 861 | 461 | 400 | 0.035875000 | 0.535423926 | 0.021400674 | 0.086833679 | -1.400572127 | 0.091643642 |
| 4+ | 1785 | 1189 | 596 | 0.074375000 | 0.666106443 | 0.031887004 | 0.223959314 | -1.949265861 | 0.374399997 |

#### SEX

Final IV: `0.011049354693`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 9486 | 2312 | 7174 | 0.395250000 | 0.243727599 | 0.383821090 | 0.435486909 | -0.126288203 | 0.006524783 |
| 2 | 14514 | 2997 | 11517 | 0.604750000 | 0.206490285 | 0.616178910 | 0.564513091 | 0.087573785 | 0.004524571 |

#### EDUCATION_GROUP

Final IV: `0.036380079009`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Graduate School | 8455 | 1631 | 6824 | 0.352291667 | 0.192903607 | 0.365095501 | 0.307214165 | 0.172613856 | 0.009991121 |
| University | 11256 | 2674 | 8582 | 0.469000000 | 0.237562189 | 0.459151463 | 0.503673008 | -0.092547123 | 0.004120341 |
| High School | 3903 | 976 | 2927 | 0.162625000 | 0.250064053 | 0.156599433 | 0.183838764 | -0.160367931 | 0.004368315 |
| Other / Unknown | 386 | 28 | 358 | 0.016083333 | 0.072538860 | 0.019153603 | 0.005274063 | 1.289689845 | 0.017900302 |

#### MARRIAGE_GROUP

Final IV: `0.004702252234`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Married | 10892 | 2538 | 8354 | 0.453833333 | 0.233015057 | 0.446953079 | 0.478056131 | -0.067274534 | 0.002092443 |
| Single | 12806 | 2694 | 10112 | 0.533583333 | 0.210370139 | 0.541009042 | 0.507440196 | 0.064057128 | 0.002150324 |
| Other / Unknown | 302 | 77 | 225 | 0.012583333 | 0.254966887 | 0.012037879 | 0.014503673 | -0.186343651 | 0.000459485 |

#### AGE

Final IV: `0.020428857210`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| <=25 | 3104 | 836 | 2268 | 0.129333333 | 0.269329897 | 0.121341822 | 0.157468450 | -0.260613579 | 0.009415090 |
| 26–35 | 10446 | 2075 | 8371 | 0.435250000 | 0.198640628 | 0.447862608 | 0.390845734 | 0.136173567 | 0.007764191 |
| 36–49 | 8319 | 1855 | 6464 | 0.346625000 | 0.222983532 | 0.345834894 | 0.349406668 | -0.010275006 | 0.000036700 |
| 50+ | 2131 | 543 | 1588 | 0.088791667 | 0.254809948 | 0.084960676 | 0.102279149 | -0.185517309 | 0.003212876 |

#### LIMIT_BAL

Final IV: `0.190444892886`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| <=30k | 3256 | 1189 | 2067 | 0.135666667 | 0.365171990 | 0.110587984 | 0.223959314 | -0.705652968 | 0.080000816 |
| (30k,140k] | 8957 | 2297 | 6660 | 0.373208333 | 0.256447471 | 0.356321224 | 0.432661518 | -0.194123070 | 0.014819412 |
| (140k,240k] | 6178 | 1083 | 5095 | 0.257416667 | 0.175299450 | 0.272591087 | 0.203993219 | 0.289886068 | 0.019885566 |
| >240k | 5609 | 740 | 4869 | 0.233708333 | 0.131930825 | 0.260499706 | 0.139385948 | 0.625355039 | 0.075739098 |

#### BILL_AMT1

Final IV: `0.015030576031`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Negative | 468 | 82 | 386 | 0.019500000 | 0.175213675 | 0.020651651 | 0.015445470 | 0.290479491 | 0.001512289 |
| Zero | 1612 | 434 | 1178 | 0.067166667 | 0.269230769 | 0.063024985 | 0.081747975 | -0.260109801 | 0.004870033 |
| (0,10k] | 6599 | 1397 | 5202 | 0.274958333 | 0.211698742 | 0.278315767 | 0.263138067 | 0.056077456 | 0.000851127 |
| (10k,40k] | 6061 | 1487 | 4574 | 0.252541667 | 0.245339053 | 0.244716709 | 0.280090413 | -0.135011203 | 0.004775846 |
| >40k | 9260 | 1909 | 7351 | 0.385833333 | 0.206155508 | 0.393290889 | 0.359578075 | 0.089618183 | 0.003021281 |

#### BILL_AMT2

Final IV: `0.013563868019`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Negative | 522 | 90 | 432 | 0.021750000 | 0.172413793 | 0.023112728 | 0.016952345 | 0.309977287 | 0.001909579 |
| Zero | 1986 | 485 | 1501 | 0.082750000 | 0.244209466 | 0.080306030 | 0.091354304 | -0.128900690 | 0.001424130 |
| (0,10k] | 6339 | 1319 | 5020 | 0.264125000 | 0.208076984 | 0.268578460 | 0.248446035 | 0.077917429 | 0.001568667 |
| (10k,40k] | 6180 | 1542 | 4638 | 0.257500000 | 0.249514563 | 0.248140816 | 0.290450179 | -0.157435667 | 0.006661003 |
| >40k | 8973 | 1873 | 7100 | 0.373875000 | 0.208737323 | 0.379861966 | 0.352797137 | 0.073914730 | 0.002000489 |

#### BILL_AMT3

Final IV: `0.011575527611`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Negative | 515 | 96 | 419 | 0.021458333 | 0.186407767 | 0.022417206 | 0.018082501 | 0.214884097 | 0.000931459 |
| Zero | 2308 | 549 | 1759 | 0.096166667 | 0.237868284 | 0.094109464 | 0.103409305 | -0.094236328 | 0.000876383 |
| (0,10k] | 6160 | 1279 | 4881 | 0.256666667 | 0.207629870 | 0.261141726 | 0.240911659 | 0.080632963 | 0.001631210 |
| (10k,40k] | 6483 | 1606 | 4877 | 0.270125000 | 0.247724819 | 0.260927719 | 0.302505180 | -0.147854970 | 0.006147434 |
| >40k | 8534 | 1779 | 6755 | 0.355583333 | 0.208460277 | 0.361403884 | 0.335091354 | 0.075592932 | 0.001989041 |

#### BILL_AMT4

Final IV: `0.013016361921`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Negative | 530 | 91 | 439 | 0.022083333 | 0.171698113 | 0.023487240 | 0.017140704 | 0.315001276 | 0.001999167 |
| Zero | 2555 | 614 | 1941 | 0.106458333 | 0.240313112 | 0.103846771 | 0.115652665 | -0.107674976 | 0.001271199 |
| (0,10k] | 6141 | 1266 | 4875 | 0.255875000 | 0.206155349 | 0.260820716 | 0.238462987 | 0.089619150 | 0.002003681 |
| (10k,40k] | 6976 | 1716 | 5260 | 0.290666667 | 0.245986239 | 0.281418865 | 0.323224713 | -0.138503605 | 0.005790261 |
| >40k | 7798 | 1622 | 6176 | 0.324916667 | 0.208002052 | 0.330426408 | 0.305518930 | 0.078372226 | 0.001952055 |

#### BILL_AMT5

Final IV: `0.016778116821`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Negative | 524 | 103 | 421 | 0.021833333 | 0.196564885 | 0.022524210 | 0.019401017 | 0.149265214 | 0.000466184 |
| Zero | 2828 | 690 | 2138 | 0.117833333 | 0.243988685 | 0.114386603 | 0.129967979 | -0.127704137 | 0.001989806 |
| (0,10k] | 6336 | 1253 | 5083 | 0.264000000 | 0.197758838 | 0.271949066 | 0.236014315 | 0.141722332 | 0.005092757 |
| (10k,40k] | 7023 | 1749 | 5274 | 0.292625000 | 0.249038872 | 0.282167888 | 0.329440573 | -0.154893739 | 0.007322243 |
| >40k | 7289 | 1514 | 5775 | 0.303708333 | 0.207710248 | 0.308972233 | 0.285176116 | 0.080144470 | 0.001907127 |

#### BILL_AMT6

Final IV: `0.015700367798`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| Negative | 556 | 103 | 453 | 0.023166667 | 0.185251799 | 0.024236263 | 0.019401017 | 0.222524506 | 0.001075961 |
| Zero | 3223 | 749 | 2474 | 0.134291667 | 0.232392181 | 0.132363169 | 0.141081183 | -0.063786062 | 0.000556088 |
| (0,10k] | 6369 | 1264 | 5105 | 0.265375000 | 0.198461297 | 0.273126103 | 0.238086269 | 0.137300525 | 0.004810988 |
| (10k,40k] | 6764 | 1699 | 5065 | 0.281833333 | 0.251182732 | 0.270986036 | 0.320022603 | -0.166324336 | 0.008155974 |
| >40k | 7088 | 1494 | 5594 | 0.295333333 | 0.210778781 | 0.299288428 | 0.281408928 | 0.061598877 | 0.001101357 |

#### PAY_AMT1

Final IV: `0.175537864049`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 4180 | 1497 | 2683 | 0.174166667 | 0.358133971 | 0.143545022 | 0.281974006 | -0.675166165 | 0.093462567 |
| (0,5k] | 13723 | 2984 | 10739 | 0.571791667 | 0.217445165 | 0.574554598 | 0.562064419 | 0.021978662 | 0.000274517 |
| (5k,20k] | 5055 | 732 | 4323 | 0.210625000 | 0.144807122 | 0.231287786 | 0.137879073 | 0.517285740 | 0.048318995 |
| >20k | 1042 | 96 | 946 | 0.043416667 | 0.092130518 | 0.050612594 | 0.018082501 | 1.029255747 | 0.033481785 |

#### PAY_AMT2

Final IV: `0.151113586876`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 4324 | 1437 | 2887 | 0.180166667 | 0.332331175 | 0.154459365 | 0.270672443 | -0.560978337 | 0.065193019 |
| (0,5k] | 13854 | 3088 | 10766 | 0.577250000 | 0.222895915 | 0.575999144 | 0.581653795 | -0.009769243 | 0.000055242 |
| (5k,20k] | 4740 | 697 | 4043 | 0.197500000 | 0.147046414 | 0.216307314 | 0.131286495 | 0.499318228 | 0.042452445 |
| >20k | 1082 | 87 | 995 | 0.045083333 | 0.080406654 | 0.053234177 | 0.016387267 | 1.178195988 | 0.043412881 |

#### PAY_AMT3

Final IV: `0.121957912222`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 4780 | 1544 | 3236 | 0.199166667 | 0.323012552 | 0.173131454 | 0.290826898 | -0.518677083 | 0.061045930 |
| (0,5k] | 14043 | 3034 | 11009 | 0.585125000 | 0.216050701 | 0.589000054 | 0.571482388 | 0.030192608 | 0.000528904 |
| (5k,20k] | 4168 | 629 | 3539 | 0.173666667 | 0.150911708 | 0.189342464 | 0.118478056 | 0.468829593 | 0.033223332 |
| >20k | 1009 | 102 | 907 | 0.042041667 | 0.101090188 | 0.048526029 | 0.019212658 | 0.926531006 | 0.027159747 |

#### PAY_AMT4

Final IV: `0.088405981704`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 5141 | 1578 | 3563 | 0.214208333 | 0.306944174 | 0.190626505 | 0.297231117 | -0.444193967 | 0.047353126 |
| (0,5k] | 14201 | 3025 | 11176 | 0.591708333 | 0.213013168 | 0.597934835 | 0.569787154 | 0.048218899 | 0.001357250 |
| (5k,20k] | 3693 | 592 | 3101 | 0.153875000 | 0.160303276 | 0.165908726 | 0.111508759 | 0.397334653 | 0.021614992 |
| >20k | 965 | 114 | 851 | 0.040208333 | 0.118134715 | 0.045529934 | 0.021472970 | 0.751575049 | 0.018080614 |

#### PAY_AMT5

Final IV: `0.075093140327`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 5387 | 1575 | 3812 | 0.224458333 | 0.292370522 | 0.203948424 | 0.296666039 | -0.374739917 | 0.034744991 |
| (0,5k] | 13941 | 3018 | 10923 | 0.580875000 | 0.216483753 | 0.584398909 | 0.568468638 | 0.027637667 | 0.000440276 |
| (5k,20k] | 3746 | 614 | 3132 | 0.156083333 | 0.163908169 | 0.167567278 | 0.115652665 | 0.370793498 | 0.019249601 |
| >20k | 926 | 102 | 824 | 0.038583333 | 0.110151188 | 0.044085389 | 0.019212658 | 0.830559086 | 0.020658273 |

#### PAY_AMT6

Final IV: `0.079904481549`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 5787 | 1675 | 4112 | 0.241125000 | 0.289441852 | 0.219998930 | 0.315501978 | -0.360542268 | 0.034432885 |
| (0,5k] | 13662 | 2973 | 10689 | 0.569250000 | 0.217610892 | 0.571879514 | 0.559992466 | 0.021005000 | 0.000249687 |
| (5k,20k] | 3513 | 537 | 2976 | 0.146375000 | 0.152860803 | 0.159221015 | 0.101148992 | 0.453698670 | 0.026347200 |
| >20k | 1038 | 124 | 914 | 0.043250000 | 0.119460501 | 0.048900540 | 0.023356564 | 0.738910375 | 0.018874709 |

#### current_utilization

Final IV: `0.075545965143`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 2080 | 516 | 1564 | 0.086666667 | 0.248076923 | 0.083676636 | 0.097193445 | -0.149743475 | 0.002024054 |
| (0,0.2] | 8507 | 1399 | 7108 | 0.354458333 | 0.164452804 | 0.380289979 | 0.263514786 | 0.366824584 | 0.042836012 |
| (0.2,0.6] | 4716 | 1045 | 3671 | 0.196500000 | 0.221586090 | 0.196404687 | 0.196835562 | -0.002191412 | 0.000000944 |
| (0.6,1] | 7012 | 1829 | 5183 | 0.292166667 | 0.260838562 | 0.277299235 | 0.344509324 | -0.217023961 | 0.014586200 |
| >1 | 1685 | 520 | 1165 | 0.070208333 | 0.308605341 | 0.062329463 | 0.097946883 | -0.451991077 | 0.016098756 |

#### avg_utilization

Final IV: `0.121261375406`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 771 | 276 | 495 | 0.032125000 | 0.357976654 | 0.026483334 | 0.051987192 | -0.674481734 | 0.017201886 |
| (0,0.2] | 10021 | 1649 | 8372 | 0.417541667 | 0.164554436 | 0.447916109 | 0.310604634 | 0.366085130 | 0.050267689 |
| (0.2,0.6] | 5820 | 1208 | 4612 | 0.242500000 | 0.207560137 | 0.246749773 | 0.227538143 | 0.081056872 | 0.001557235 |
| (0.6,1] | 6880 | 1998 | 4882 | 0.286666667 | 0.290406977 | 0.261195228 | 0.376342061 | -0.365230339 | 0.042055117 |
| >1 | 508 | 178 | 330 | 0.021166667 | 0.350393701 | 0.017655556 | 0.033527971 | -0.641329527 | 0.010179449 |

#### max_utilization

Final IV: `0.089296728682`

| Final Bin | Count | Bad | Good | % Train | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 771 | 276 | 495 | 0.032125000 | 0.357976654 | 0.026483334 | 0.051987192 | -0.674481734 | 0.017201886 |
| (0,0.2] | 8269 | 1421 | 6848 | 0.344541667 | 0.171846656 | 0.366379541 | 0.267658693 | 0.313957159 | 0.030994117 |
| (0.2,0.6] | 5018 | 960 | 4058 | 0.209083333 | 0.191311279 | 0.217109839 | 0.180825014 | 0.182873605 | 0.006635537 |
| (0.6,1] | 6813 | 1700 | 5113 | 0.283875000 | 0.249522971 | 0.273554117 | 0.320210963 | -0.157480566 | 0.007347546 |
| >1 | 3129 | 952 | 2177 | 0.130375000 | 0.304250559 | 0.116473169 | 0.179318139 | -0.431500605 | 0.027117643 |

## WOE Matrix Integrity

- Shape: `(24000, 29)`.
- Column order: exact fixed registry order.
- Duplicate columns: none.
- Missing values: 0.
- Non-finite values: 0.
- Train IDs: unique and aligned with source row order.
- Target: unchanged.
- Mapping audit: every value belongs to its variable's final WOE lookup.
- Determinism: exact frame equality on a second in-memory reconstruction.

## Individual Predictor Diagnostic Summary

This table is diagnostic only. Sparse-bin flag means minimum final-bin Train share < 3%. Governance flags are separate policy/model-governance review prompts and are not deletion rules.

| Predictor | Family | Final Bin Count | Final IV | Minimum Bin Count | Minimum Bin Share | Maximum Absolute WOE | Largest Single-bin IV Contribution | Largest Single-bin IV Share of Total IV | Largest-contribution Bin | Largest-bin Train Share | Sparse-bin Flag | Governance Review Flag | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| PAY_0 | PAY | 3 | 0.872145621 | 2494 | 0.103916667 | 2.091087561 | 0.599977656 | 0.687932888 | [2+] | 0.103916667 | No | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_2 | PAY | 3 | 0.548098696 | 382 | 0.015916667 | 1.683776505 | 0.378070591 | 0.689785606 | [2] | 0.131041667 | Yes | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_3 | PAY | 3 | 0.414433849 | 309 | 0.012875000 | 1.631842877 | 0.291612921 | 0.703641657 | [2] | 0.127041667 | Yes | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_4 | PAY | 3 | 0.359543103 | 268 | 0.011166667 | 1.793417496 | 0.255929452 | 0.711818554 | [2] | 0.105916667 | Yes | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_5 | PAY | 3 | 0.327106491 | 262 | 0.010916667 | 1.773537580 | 0.238501938 | 0.729126277 | [2] | 0.087583333 | Yes | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_6 | PAY | 3 | 0.288165668 | 244 | 0.010166667 | 1.957939677 | 0.197966713 | 0.686989239 | [2] | 0.092916667 | Yes | No | moderate; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| delinquent_months | Delinquency Summary | 5 | 0.871421080 | 936 | 0.039000000 | 1.844616977 | 0.387886624 | 0.445119625 | 4+ | 0.086000000 | No | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| max_delinquency | Delinquency Summary | 4 | 0.794582507 | 939 | 0.039125000 | 1.774579355 | 0.318457186 | 0.400785548 | 0 | 0.662333333 | No | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| severe_delinquent_months | Delinquency Summary | 5 | 0.846474513 | 861 | 0.035875000 | 1.949265861 | 0.374399997 | 0.442305103 | 4+ | 0.074375000 | No | No | strong diagnostic signal; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| SEX | Demographic Categorical | 2 | 0.011049355 | 9486 | 0.395250000 | 0.126288203 | 0.006524783 | 0.590512619 | 1 | 0.395250000 | No | Governance Review Flag — Human policy/model-governance consideration required before final candidate freeze. | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| EDUCATION_GROUP | Demographic Categorical | 4 | 0.036380079 | 386 | 0.016083333 | 1.289689845 | 0.017900302 | 0.492035829 | Other / Unknown | 0.016083333 | Yes | Governance Review Flag — Human policy/model-governance consideration required before final candidate freeze. | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| MARRIAGE_GROUP | Demographic Categorical | 3 | 0.004702252 | 302 | 0.012583333 | 0.186343651 | 0.002150324 | 0.457296582 | Single | 0.533583333 | Yes | Governance Review Flag — Human policy/model-governance consideration required before final candidate freeze. | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| AGE | Demographic / Capacity | 4 | 0.020428857 | 2131 | 0.088791667 | 0.260613579 | 0.009415090 | 0.460872070 | <=25 | 0.129333333 | No | Governance Review Flag — Human policy/model-governance consideration required before final candidate freeze. | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| LIMIT_BAL | Demographic / Capacity | 4 | 0.190444893 | 3256 | 0.135666667 | 0.705652968 | 0.080000816 | 0.420073308 | <=30k | 0.135666667 | No | No | moderate; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| BILL_AMT1 | BILL | 5 | 0.015030576 | 468 | 0.019500000 | 0.290479491 | 0.004870033 | 0.324008418 | Zero | 0.067166667 | Yes | No | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| BILL_AMT2 | BILL | 5 | 0.013563868 | 522 | 0.021750000 | 0.309977287 | 0.006661003 | 0.491084306 | (10k,40k] | 0.257500000 | Yes | No | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| BILL_AMT3 | BILL | 5 | 0.011575528 | 515 | 0.021458333 | 0.214884097 | 0.006147434 | 0.531071619 | (10k,40k] | 0.270125000 | Yes | No | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| BILL_AMT4 | BILL | 5 | 0.013016362 | 530 | 0.022083333 | 0.315001276 | 0.005790261 | 0.444844782 | (10k,40k] | 0.290666667 | Yes | No | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| BILL_AMT5 | BILL | 5 | 0.016778117 | 524 | 0.021833333 | 0.154893739 | 0.007322243 | 0.436416250 | (10k,40k] | 0.292625000 | Yes | No | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| BILL_AMT6 | BILL | 5 | 0.015700368 | 556 | 0.023166667 | 0.222524506 | 0.008155974 | 0.519476649 | (10k,40k] | 0.281833333 | Yes | No | very weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_AMT1 | PAY_AMT | 4 | 0.175537864 | 1042 | 0.043416667 | 1.029255747 | 0.093462567 | 0.532435365 | 0 | 0.174166667 | No | No | moderate; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_AMT2 | PAY_AMT | 4 | 0.151113587 | 1082 | 0.045083333 | 1.178195988 | 0.065193019 | 0.431417322 | 0 | 0.180166667 | No | No | moderate; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_AMT3 | PAY_AMT | 4 | 0.121957912 | 1009 | 0.042041667 | 0.926531006 | 0.061045930 | 0.500549153 | 0 | 0.199166667 | No | No | moderate; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_AMT4 | PAY_AMT | 4 | 0.088405982 | 965 | 0.040208333 | 0.751575049 | 0.047353126 | 0.535632597 | 0 | 0.214208333 | No | No | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_AMT5 | PAY_AMT | 4 | 0.075093140 | 926 | 0.038583333 | 0.830559086 | 0.034744991 | 0.462691945 | 0 | 0.224458333 | No | No | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| PAY_AMT6 | PAY_AMT | 4 | 0.079904482 | 1038 | 0.043250000 | 0.738910375 | 0.034432885 | 0.430925585 | 0 | 0.241125000 | No | No | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| current_utilization | Utilization | 5 | 0.075545965 | 1685 | 0.070208333 | 0.451991077 | 0.042836012 | 0.567019184 | (0,0.2] | 0.354458333 | No | No | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| avg_utilization | Utilization | 5 | 0.121261375 | 508 | 0.021166667 | 0.674481734 | 0.050267689 | 0.414539991 | (0,0.2] | 0.417541667 | Yes | No | moderate; diagnostic evidence only; Candidate status: Pending Human Checkpoint |
| max_utilization | Utilization | 5 | 0.089296729 | 771 | 0.032125000 | 0.674481734 | 0.030994117 | 0.347091293 | (0,0.2] | 0.344541667 | No | No | weak; diagnostic evidence only; Candidate status: Pending Human Checkpoint |

- Sparse-bin flags: 14 of 29.
- Governance-review flags: `AGE`, `EDUCATION_GROUP`, `MARRIAGE_GROUP`, `SEX`.
- Signal concentration is reported using largest-bin IV contribution and its share of total final IV; it is not a composite score or ranking rule.

## Full 29×29 WOE Pearson Correlation Matrix

The full numeric matrix follows. Correlation is computed on the WOE representation expected to enter Stage 4.5; it measures similarity in risk representation, not equality of raw values. Thresholding uses absolute magnitude, so negative correlation is handled symmetrically.

| index | PAY_0 | PAY_2 | PAY_3 | PAY_4 | PAY_5 | PAY_6 | delinquent_months | max_delinquency | severe_delinquent_months | SEX | EDUCATION_GROUP | MARRIAGE_GROUP | AGE | LIMIT_BAL | BILL_AMT1 | BILL_AMT2 | BILL_AMT3 | BILL_AMT4 | BILL_AMT5 | BILL_AMT6 | PAY_AMT1 | PAY_AMT2 | PAY_AMT3 | PAY_AMT4 | PAY_AMT5 | PAY_AMT6 | current_utilization | avg_utilization | max_utilization |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| PAY_0 | 1.000000 | 0.654062 | 0.463843 | 0.416041 | 0.394571 | 0.361009 | 0.755778 | 0.694909 | 0.692029 | 0.033992 | 0.065777 | 0.018927 | 0.048806 | 0.195167 | 0.090995 | 0.066773 | 0.061357 | 0.059504 | 0.072605 | 0.079872 | 0.259709 | 0.190637 | 0.161785 | 0.140970 | 0.118388 | 0.125074 | 0.211420 | 0.198552 | 0.166190 |
| PAY_2 | 0.654062 | 1.000000 | 0.628891 | 0.483011 | 0.450784 | 0.405396 | 0.756965 | 0.669685 | 0.767043 | 0.043513 | 0.085935 | 0.010436 | 0.043513 | 0.231437 | 0.012620 | 0.050575 | 0.056644 | 0.051296 | 0.054007 | 0.081702 | 0.334053 | 0.156910 | 0.118090 | 0.105592 | 0.087784 | 0.094695 | 0.122033 | 0.148559 | 0.104838 |
| PAY_3 | 0.463843 | 0.628891 | 1.000000 | 0.633541 | 0.490013 | 0.442766 | 0.733147 | 0.656158 | 0.774484 | 0.043803 | 0.083339 | 0.003558 | 0.044613 | 0.228149 | 0.035204 | 0.063043 | 0.052146 | 0.038161 | 0.044868 | 0.070032 | 0.093053 | 0.293519 | 0.151879 | 0.112876 | 0.093350 | 0.105817 | 0.091217 | 0.128091 | 0.076765 |
| PAY_4 | 0.416041 | 0.483011 | 0.633541 | 1.000000 | 0.674615 | 0.509754 | 0.692928 | 0.596383 | 0.739088 | 0.037964 | 0.079486 | 0.007388 | 0.035224 | 0.210442 | 0.038715 | 0.063457 | 0.053657 | 0.034042 | 0.035970 | 0.064981 | 0.113374 | 0.090175 | 0.266856 | 0.129305 | 0.090048 | 0.098208 | 0.086385 | 0.128843 | 0.073075 |
| PAY_5 | 0.394571 | 0.450784 | 0.490013 | 0.674615 | 1.000000 | 0.671160 | 0.645914 | 0.543918 | 0.693865 | 0.037595 | 0.067538 | 0.000466 | 0.034044 | 0.198494 | 0.037456 | 0.064318 | 0.052563 | 0.035964 | 0.034851 | 0.061035 | 0.127817 | 0.101075 | 0.078690 | 0.235053 | 0.097502 | 0.088591 | 0.090359 | 0.134338 | 0.070821 |
| PAY_6 | 0.361009 | 0.405396 | 0.442766 | 0.509754 | 0.671160 | 1.000000 | 0.612797 | 0.551825 | 0.658541 | 0.033036 | 0.060451 | 0.002551 | 0.037952 | 0.195784 | 0.032833 | 0.060970 | 0.053594 | 0.027045 | 0.022877 | 0.049453 | 0.108563 | 0.109569 | 0.088512 | 0.049325 | 0.216128 | 0.102942 | 0.079491 | 0.125478 | 0.066461 |
| delinquent_months | 0.755778 | 0.756965 | 0.733147 | 0.692928 | 0.645914 | 0.612797 | 1.000000 | 0.943585 | 0.944882 | 0.043054 | 0.089264 | 0.007464 | 0.053274 | 0.275158 | 0.118440 | 0.095738 | 0.085569 | 0.067500 | 0.077331 | 0.095553 | 0.295018 | 0.254744 | 0.230480 | 0.195949 | 0.185477 | 0.164676 | 0.187172 | 0.197868 | 0.148009 |
| max_delinquency | 0.694909 | 0.669685 | 0.656158 | 0.596383 | 0.543918 | 0.551825 | 0.943585 | 1.000000 | 0.914702 | 0.045913 | 0.090017 | 0.000826 | 0.067651 | 0.282497 | 0.096433 | 0.085880 | 0.077000 | 0.060047 | 0.065656 | 0.088939 | 0.274099 | 0.243778 | 0.210430 | 0.172933 | 0.176702 | 0.149566 | 0.168724 | 0.181498 | 0.138625 |
| severe_delinquent_months | 0.692029 | 0.767043 | 0.774484 | 0.739088 | 0.693865 | 0.658541 | 0.944882 | 0.914702 | 1.000000 | 0.050056 | 0.105309 | 0.003534 | 0.063203 | 0.299524 | 0.031654 | 0.079028 | 0.072763 | 0.056577 | 0.060340 | 0.100770 | 0.208289 | 0.198601 | 0.172371 | 0.139362 | 0.136038 | 0.114088 | 0.146013 | 0.187271 | 0.123566 |
| SEX | 0.033992 | 0.043513 | 0.043803 | 0.037964 | 0.037595 | 0.033036 | 0.043054 | 0.045913 | 0.050056 | 1.000000 | -0.006734 | -0.026393 | -0.015333 | 0.053793 | -0.019447 | -0.002339 | -0.002847 | -0.002315 | 0.007994 | 0.010977 | -0.007069 | 0.002884 | 0.003896 | 0.001657 | 0.007208 | 0.020555 | 0.059623 | 0.055755 | 0.069811 |
| EDUCATION_GROUP | 0.065777 | 0.085935 | 0.083339 | 0.079486 | 0.067538 | 0.060451 | 0.089264 | 0.090017 | 0.105309 | -0.006734 | 1.000000 | 0.110869 | 0.087162 | 0.190164 | 0.008635 | 0.033409 | 0.038930 | 0.044150 | 0.053587 | 0.073193 | 0.040436 | 0.054347 | 0.035392 | 0.012189 | 0.025040 | 0.024130 | 0.100321 | 0.100405 | 0.092000 |
| MARRIAGE_GROUP | 0.018927 | 0.010436 | 0.003558 | 0.007388 | 0.000466 | 0.002551 | 0.007464 | 0.000826 | 0.003534 | -0.026393 | 0.110869 | 1.000000 | 0.048824 | -0.080516 | -0.019995 | -0.016163 | -0.017731 | -0.014999 | -0.018394 | -0.016616 | 0.001461 | -0.008435 | 0.002677 | -0.006206 | -0.003248 | -0.006721 | -0.024404 | -0.023200 | -0.028205 |
| AGE | 0.048806 | 0.043513 | 0.044613 | 0.035224 | 0.034044 | 0.037952 | 0.053274 | 0.067651 | 0.063203 | -0.015333 | 0.087162 | 0.048824 | 1.000000 | 0.249952 | 0.050279 | 0.075765 | 0.086152 | 0.081277 | 0.093562 | 0.097157 | 0.052591 | 0.064708 | 0.051126 | 0.038910 | 0.055088 | 0.068606 | 0.105465 | 0.096564 | 0.095704 |
| LIMIT_BAL | 0.195167 | 0.231437 | 0.228149 | 0.210442 | 0.198494 | 0.195784 | 0.275158 | 0.282497 | 0.299524 | 0.053793 | 0.190164 | -0.080516 | 0.249952 | 1.000000 | 0.173113 | 0.238402 | 0.250681 | 0.240383 | 0.253245 | 0.265877 | 0.227599 | 0.255705 | 0.222042 | 0.201073 | 0.230981 | 0.273361 | 0.344849 | 0.337780 | 0.331800 |
| BILL_AMT1 | 0.090995 | 0.012620 | 0.035204 | 0.038715 | 0.037456 | 0.032833 | 0.118440 | 0.096433 | 0.031654 | -0.019447 | 0.008635 | -0.019995 | 0.050279 | 0.173113 | 1.000000 | 0.642842 | 0.527290 | 0.447425 | 0.364027 | 0.278518 | 0.233122 | 0.208747 | 0.196423 | 0.194801 | 0.179451 | 0.191642 | -0.013845 | 0.019946 | 0.028229 |
| BILL_AMT2 | 0.066773 | 0.050575 | 0.063043 | 0.063457 | 0.064318 | 0.060970 | 0.095738 | 0.085880 | 0.079028 | -0.002339 | 0.033409 | -0.016163 | 0.075765 | 0.238402 | 0.642842 | 1.000000 | 0.677193 | 0.554734 | 0.454149 | 0.394094 | 0.127736 | 0.146308 | 0.129083 | 0.124429 | 0.120618 | 0.133085 | 0.008258 | 0.046004 | 0.054184 |
| BILL_AMT3 | 0.061357 | 0.056644 | 0.052146 | 0.053657 | 0.052563 | 0.053594 | 0.085569 | 0.077000 | 0.072763 | -0.002847 | 0.038930 | -0.017731 | 0.086152 | 0.250681 | 0.527290 | 0.677193 | 1.000000 | 0.668258 | 0.538860 | 0.460258 | 0.137802 | 0.123718 | 0.142873 | 0.135857 | 0.121682 | 0.137707 | 0.019609 | 0.032451 | 0.051478 |
| BILL_AMT4 | 0.059504 | 0.051296 | 0.038161 | 0.034042 | 0.035964 | 0.027045 | 0.067500 | 0.060047 | 0.056577 | -0.002315 | 0.044150 | -0.014999 | 0.081277 | 0.240383 | 0.447425 | 0.554734 | 0.668258 | 1.000000 | 0.646148 | 0.554329 | 0.136255 | 0.137486 | 0.120937 | 0.139688 | 0.115978 | 0.130581 | 0.064989 | 0.078024 | 0.098228 |
| BILL_AMT5 | 0.072605 | 0.054007 | 0.044868 | 0.035970 | 0.034851 | 0.022877 | 0.077331 | 0.065656 | 0.060340 | 0.007994 | 0.053587 | -0.018394 | 0.093562 | 0.253245 | 0.364027 | 0.454149 | 0.538860 | 0.646148 | 1.000000 | 0.673723 | 0.145116 | 0.152311 | 0.149995 | 0.174625 | 0.151557 | 0.144989 | 0.157342 | 0.173567 | 0.192825 |
| BILL_AMT6 | 0.079872 | 0.081702 | 0.070032 | 0.064981 | 0.061035 | 0.049453 | 0.095553 | 0.088939 | 0.100770 | 0.010977 | 0.073193 | -0.016616 | 0.097157 | 0.265877 | 0.278518 | 0.394094 | 0.460258 | 0.554329 | 0.673723 | 1.000000 | 0.074936 | 0.077782 | 0.060947 | 0.044123 | 0.049654 | 0.094977 | 0.188585 | 0.206797 | 0.206268 |
| PAY_AMT1 | 0.259709 | 0.334053 | 0.093053 | 0.113374 | 0.127817 | 0.108563 | 0.295018 | 0.274099 | 0.208289 | -0.007069 | 0.040436 | 0.001461 | 0.052591 | 0.227599 | 0.233122 | 0.127736 | 0.137802 | 0.136255 | 0.145116 | 0.074936 | 1.000000 | 0.448809 | 0.442652 | 0.398748 | 0.372389 | 0.374639 | 0.030784 | 0.025080 | 0.041419 |
| PAY_AMT2 | 0.190637 | 0.156910 | 0.293519 | 0.090175 | 0.101075 | 0.109569 | 0.254744 | 0.243778 | 0.198601 | 0.002884 | 0.054347 | -0.008435 | 0.064708 | 0.255705 | 0.208747 | 0.146308 | 0.123718 | 0.137486 | 0.152311 | 0.077782 | 0.448809 | 1.000000 | 0.452297 | 0.436578 | 0.404541 | 0.408107 | 0.027902 | 0.015174 | 0.038662 |
| PAY_AMT3 | 0.161785 | 0.118090 | 0.151879 | 0.266856 | 0.078690 | 0.088512 | 0.230480 | 0.210430 | 0.172371 | 0.003896 | 0.035392 | 0.002677 | 0.051126 | 0.222042 | 0.196423 | 0.129083 | 0.142873 | 0.120937 | 0.149995 | 0.060947 | 0.442652 | 0.452297 | 1.000000 | 0.441861 | 0.445683 | 0.431467 | 0.009235 | 0.006916 | 0.040200 |
| PAY_AMT4 | 0.140970 | 0.105592 | 0.112876 | 0.129305 | 0.235053 | 0.049325 | 0.195949 | 0.172933 | 0.139362 | 0.001657 | 0.012189 | -0.006206 | 0.038910 | 0.201073 | 0.194801 | 0.124429 | 0.135857 | 0.139688 | 0.174625 | 0.044123 | 0.398748 | 0.436578 | 0.441861 | 1.000000 | 0.442683 | 0.458867 | -0.017880 | -0.016322 | 0.021817 |
| PAY_AMT5 | 0.118388 | 0.087784 | 0.093350 | 0.090048 | 0.097502 | 0.216128 | 0.185477 | 0.176702 | 0.136038 | 0.007208 | 0.025040 | -0.003248 | 0.055088 | 0.230981 | 0.179451 | 0.120618 | 0.121682 | 0.115978 | 0.151557 | 0.049654 | 0.372389 | 0.404541 | 0.445683 | 0.442683 | 1.000000 | 0.471263 | 0.001472 | -0.001055 | 0.029258 |
| PAY_AMT6 | 0.125074 | 0.094695 | 0.105817 | 0.098208 | 0.088591 | 0.102942 | 0.164676 | 0.149566 | 0.114088 | 0.020555 | 0.024130 | -0.006721 | 0.068606 | 0.273361 | 0.191642 | 0.133085 | 0.137707 | 0.130581 | 0.144989 | 0.094977 | 0.374639 | 0.408107 | 0.431467 | 0.458867 | 0.471263 | 1.000000 | 0.014461 | 0.006592 | 0.043272 |
| current_utilization | 0.211420 | 0.122033 | 0.091217 | 0.086385 | 0.090359 | 0.079491 | 0.187172 | 0.168724 | 0.146013 | 0.059623 | 0.100321 | -0.024404 | 0.105465 | 0.344849 | -0.013845 | 0.008258 | 0.019609 | 0.064989 | 0.157342 | 0.188585 | 0.030784 | 0.027902 | 0.009235 | -0.017880 | 0.001472 | 0.014461 | 1.000000 | 0.770533 | 0.783112 |
| avg_utilization | 0.198552 | 0.148559 | 0.128091 | 0.128843 | 0.134338 | 0.125478 | 0.197868 | 0.181498 | 0.187271 | 0.055755 | 0.100405 | -0.023200 | 0.096564 | 0.337780 | 0.019946 | 0.046004 | 0.032451 | 0.078024 | 0.173567 | 0.206797 | 0.025080 | 0.015174 | 0.006916 | -0.016322 | -0.001055 | 0.006592 | 0.770533 | 1.000000 | 0.890213 |
| max_utilization | 0.166190 | 0.104838 | 0.076765 | 0.073075 | 0.070821 | 0.066461 | 0.148009 | 0.138625 | 0.123566 | 0.069811 | 0.092000 | -0.028205 | 0.095704 | 0.331800 | 0.028229 | 0.054184 | 0.051478 | 0.098228 | 0.192825 | 0.206268 | 0.041419 | 0.038662 | 0.040200 | 0.021817 | 0.029258 | 0.043272 | 0.783112 | 0.890213 | 1.000000 |

### Full flagged-pair table

| Predictor A | Family A | Predictor B | Family B | Correlation | Absolute Correlation | Flag Level | Structural Relationship Note |
|---|---|---|---|---|---|---|---|
| delinquent_months | Delinquency Summary | severe_delinquent_months | Delinquency Summary | 0.944882 | 0.944882 | Strong | PAY-derived delinquency summary relationship |
| delinquent_months | Delinquency Summary | max_delinquency | Delinquency Summary | 0.943585 | 0.943585 | Strong | PAY-derived delinquency summary relationship |
| max_delinquency | Delinquency Summary | severe_delinquent_months | Delinquency Summary | 0.914702 | 0.914702 | Strong | PAY-derived delinquency summary relationship |
| avg_utilization | Utilization | max_utilization | Utilization | 0.890213 | 0.890213 | Strong | Current/average/maximum utilization relationship |
| current_utilization | Utilization | max_utilization | Utilization | 0.783112 | 0.783112 | Review | Current/average/maximum utilization relationship |
| PAY_3 | PAY | severe_delinquent_months | Delinquency Summary | 0.774484 | 0.774484 | Review | PAY-derived delinquency summary relationship |
| current_utilization | Utilization | avg_utilization | Utilization | 0.770533 | 0.770533 | Review | Current/average/maximum utilization relationship |
| PAY_2 | PAY | severe_delinquent_months | Delinquency Summary | 0.767043 | 0.767043 | Review | PAY-derived delinquency summary relationship |
| PAY_2 | PAY | delinquent_months | Delinquency Summary | 0.756965 | 0.756965 | Review | PAY-derived delinquency summary relationship |
| PAY_0 | PAY | delinquent_months | Delinquency Summary | 0.755778 | 0.755778 | Review | PAY-derived delinquency summary relationship |
| PAY_4 | PAY | severe_delinquent_months | Delinquency Summary | 0.739088 | 0.739088 | Review | PAY-derived delinquency summary relationship |
| PAY_3 | PAY | delinquent_months | Delinquency Summary | 0.733147 | 0.733147 | Review | PAY-derived delinquency summary relationship |

- Pairs with `|r| >= 0.70`: 12.
- Strong-redundancy flags with `|r| >= 0.85`: 4.
- Review-only pairs with `0.70 <= |r| < 0.85`: 8.
- Flagged negative-correlation pairs: 0; the absolute-magnitude rule was nevertheless used for every pair.
- Strongest off-diagonal absolute correlation: 0.944882.

## Family WOE Correlation Diagnostics

#### PAY / Delinquency

Numeric WOE correlation matrix:

| index | PAY_0 | PAY_2 | PAY_3 | PAY_4 | PAY_5 | PAY_6 | delinquent_months | max_delinquency | severe_delinquent_months |
|---|---|---|---|---|---|---|---|---|---|
| PAY_0 | 1.000000 | 0.654062 | 0.463843 | 0.416041 | 0.394571 | 0.361009 | 0.755778 | 0.694909 | 0.692029 |
| PAY_2 | 0.654062 | 1.000000 | 0.628891 | 0.483011 | 0.450784 | 0.405396 | 0.756965 | 0.669685 | 0.767043 |
| PAY_3 | 0.463843 | 0.628891 | 1.000000 | 0.633541 | 0.490013 | 0.442766 | 0.733147 | 0.656158 | 0.774484 |
| PAY_4 | 0.416041 | 0.483011 | 0.633541 | 1.000000 | 0.674615 | 0.509754 | 0.692928 | 0.596383 | 0.739088 |
| PAY_5 | 0.394571 | 0.450784 | 0.490013 | 0.674615 | 1.000000 | 0.671160 | 0.645914 | 0.543918 | 0.693865 |
| PAY_6 | 0.361009 | 0.405396 | 0.442766 | 0.509754 | 0.671160 | 1.000000 | 0.612797 | 0.551825 | 0.658541 |
| delinquent_months | 0.755778 | 0.756965 | 0.733147 | 0.692928 | 0.645914 | 0.612797 | 1.000000 | 0.943585 | 0.944882 |
| max_delinquency | 0.694909 | 0.669685 | 0.656158 | 0.596383 | 0.543918 | 0.551825 | 0.943585 | 1.000000 | 0.914702 |
| severe_delinquent_months | 0.692029 | 0.767043 | 0.774484 | 0.739088 | 0.693865 | 0.658541 | 0.944882 | 0.914702 | 1.000000 |

Flagged pairs (`|r| >= 0.70`):

| Predictor A | Family A | Predictor B | Family B | Correlation | Absolute Correlation | Flag Level | Structural Relationship Note |
|---|---|---|---|---|---|---|---|
| delinquent_months | Delinquency Summary | severe_delinquent_months | Delinquency Summary | 0.944882 | 0.944882 | Strong | PAY-derived delinquency summary relationship |
| delinquent_months | Delinquency Summary | max_delinquency | Delinquency Summary | 0.943585 | 0.943585 | Strong | PAY-derived delinquency summary relationship |
| max_delinquency | Delinquency Summary | severe_delinquent_months | Delinquency Summary | 0.914702 | 0.914702 | Strong | PAY-derived delinquency summary relationship |
| PAY_3 | PAY | severe_delinquent_months | Delinquency Summary | 0.774484 | 0.774484 | Review | PAY-derived delinquency summary relationship |
| PAY_2 | PAY | severe_delinquent_months | Delinquency Summary | 0.767043 | 0.767043 | Review | PAY-derived delinquency summary relationship |
| PAY_2 | PAY | delinquent_months | Delinquency Summary | 0.756965 | 0.756965 | Review | PAY-derived delinquency summary relationship |
| PAY_0 | PAY | delinquent_months | Delinquency Summary | 0.755778 | 0.755778 | Review | PAY-derived delinquency summary relationship |
| PAY_4 | PAY | severe_delinquent_months | Delinquency Summary | 0.739088 | 0.739088 | Review | PAY-derived delinquency summary relationship |
| PAY_3 | PAY | delinquent_months | Delinquency Summary | 0.733147 | 0.733147 | Review | PAY-derived delinquency summary relationship |

#### PAY_AMT

Numeric WOE correlation matrix:

| index | PAY_AMT1 | PAY_AMT2 | PAY_AMT3 | PAY_AMT4 | PAY_AMT5 | PAY_AMT6 |
|---|---|---|---|---|---|---|
| PAY_AMT1 | 1.000000 | 0.448809 | 0.442652 | 0.398748 | 0.372389 | 0.374639 |
| PAY_AMT2 | 0.448809 | 1.000000 | 0.452297 | 0.436578 | 0.404541 | 0.408107 |
| PAY_AMT3 | 0.442652 | 0.452297 | 1.000000 | 0.441861 | 0.445683 | 0.431467 |
| PAY_AMT4 | 0.398748 | 0.436578 | 0.441861 | 1.000000 | 0.442683 | 0.458867 |
| PAY_AMT5 | 0.372389 | 0.404541 | 0.445683 | 0.442683 | 1.000000 | 0.471263 |
| PAY_AMT6 | 0.374639 | 0.408107 | 0.431467 | 0.458867 | 0.471263 | 1.000000 |

Flagged pairs (`|r| >= 0.70`):

None.

#### LIMIT_BAL / BILL / Utilization

Numeric WOE correlation matrix:

| index | LIMIT_BAL | BILL_AMT1 | BILL_AMT2 | BILL_AMT3 | BILL_AMT4 | BILL_AMT5 | BILL_AMT6 | current_utilization | avg_utilization | max_utilization |
|---|---|---|---|---|---|---|---|---|---|---|
| LIMIT_BAL | 1.000000 | 0.173113 | 0.238402 | 0.250681 | 0.240383 | 0.253245 | 0.265877 | 0.344849 | 0.337780 | 0.331800 |
| BILL_AMT1 | 0.173113 | 1.000000 | 0.642842 | 0.527290 | 0.447425 | 0.364027 | 0.278518 | -0.013845 | 0.019946 | 0.028229 |
| BILL_AMT2 | 0.238402 | 0.642842 | 1.000000 | 0.677193 | 0.554734 | 0.454149 | 0.394094 | 0.008258 | 0.046004 | 0.054184 |
| BILL_AMT3 | 0.250681 | 0.527290 | 0.677193 | 1.000000 | 0.668258 | 0.538860 | 0.460258 | 0.019609 | 0.032451 | 0.051478 |
| BILL_AMT4 | 0.240383 | 0.447425 | 0.554734 | 0.668258 | 1.000000 | 0.646148 | 0.554329 | 0.064989 | 0.078024 | 0.098228 |
| BILL_AMT5 | 0.253245 | 0.364027 | 0.454149 | 0.538860 | 0.646148 | 1.000000 | 0.673723 | 0.157342 | 0.173567 | 0.192825 |
| BILL_AMT6 | 0.265877 | 0.278518 | 0.394094 | 0.460258 | 0.554329 | 0.673723 | 1.000000 | 0.188585 | 0.206797 | 0.206268 |
| current_utilization | 0.344849 | -0.013845 | 0.008258 | 0.019609 | 0.064989 | 0.157342 | 0.188585 | 1.000000 | 0.770533 | 0.783112 |
| avg_utilization | 0.337780 | 0.019946 | 0.046004 | 0.032451 | 0.078024 | 0.173567 | 0.206797 | 0.770533 | 1.000000 | 0.890213 |
| max_utilization | 0.331800 | 0.028229 | 0.054184 | 0.051478 | 0.098228 | 0.192825 | 0.206268 | 0.783112 | 0.890213 | 1.000000 |

Flagged pairs (`|r| >= 0.70`):

| Predictor A | Family A | Predictor B | Family B | Correlation | Absolute Correlation | Flag Level | Structural Relationship Note |
|---|---|---|---|---|---|---|---|
| avg_utilization | Utilization | max_utilization | Utilization | 0.890213 | 0.890213 | Strong | Current/average/maximum utilization relationship |
| current_utilization | Utilization | max_utilization | Utilization | 0.783112 | 0.783112 | Review | Current/average/maximum utilization relationship |
| current_utilization | Utilization | avg_utilization | Utilization | 0.770533 | 0.770533 | Review | Current/average/maximum utilization relationship |

#### Demographics

Numeric WOE correlation matrix:

| index | AGE | SEX | EDUCATION_GROUP | MARRIAGE_GROUP |
|---|---|---|---|---|
| AGE | 1.000000 | -0.015333 | 0.087162 | 0.048824 |
| SEX | -0.015333 | 1.000000 | -0.006734 | -0.026393 |
| EDUCATION_GROUP | 0.087162 | -0.006734 | 1.000000 | 0.110869 |
| MARRIAGE_GROUP | 0.048824 | -0.026393 | 0.110869 | 1.000000 |

Flagged pairs (`|r| >= 0.70`):

None.

## Full-candidate VIF

Method: for each WOE predictor, ordinary least squares regressed that predictor on an intercept plus all other 28 WOE predictors using NumPy least squares. `R² = 1 - SSE/SST`, tolerance `1e-10`, and `VIF = 1/(1-R²)`. No iterative elimination was performed.

| Predictor | Family | VIF | Tolerance | Flag Level | Structural Note | Auxiliary R-squared |
|---|---|---|---|---|---|---|
| PAY_0 | PAY | 2.897468 | 0.345129 | Low | Monthly PAY member with raw/summary family overlap | 0.654871 |
| PAY_2 | PAY | 3.825291 | 0.261418 | Low | Monthly PAY member with raw/summary family overlap | 0.738582 |
| PAY_3 | PAY | 3.573152 | 0.279865 | Low | Monthly PAY member with raw/summary family overlap | 0.720135 |
| PAY_4 | PAY | 3.470961 | 0.288105 | Low | Monthly PAY member with raw/summary family overlap | 0.711895 |
| PAY_5 | PAY | 3.394307 | 0.294611 | Low | Monthly PAY member with raw/summary family overlap | 0.705389 |
| PAY_6 | PAY | 2.575552 | 0.388266 | Low | Monthly PAY member with raw/summary family overlap | 0.611734 |
| delinquent_months | Delinquency Summary | 25.928051 | 0.038568 | Severe | Structurally derived from six PAY fields; assess multivariate overlap | 0.961432 |
| max_delinquency | Delinquency Summary | 17.002214 | 0.058816 | Severe | Structurally derived from six PAY fields; assess multivariate overlap | 0.941184 |
| severe_delinquent_months | Delinquency Summary | 25.142122 | 0.039774 | Severe | Structurally derived from six PAY fields; assess multivariate overlap | 0.960226 |
| SEX | Demographic Categorical | 1.012062 | 0.988082 | Low | Governance review flag is separate from VIF mathematics | 0.011918 |
| EDUCATION_GROUP | Demographic Categorical | 1.064115 | 0.939748 | Low | Governance review flag is separate from VIF mathematics | 0.060252 |
| MARRIAGE_GROUP | Demographic Categorical | 1.030746 | 0.970171 | Low | Governance review flag is separate from VIF mathematics | 0.029829 |
| AGE | Demographic / Capacity | 1.078982 | 0.926800 | Low | Governance review flag is separate from VIF mathematics | 0.073200 |
| LIMIT_BAL | Demographic / Capacity | 1.611420 | 0.620571 | Low | Capacity/demographic representation in full WOE candidate set | 0.379429 |
| BILL_AMT1 | BILL | 2.015117 | 0.496249 | Low | Monthly BILL family and utilization relationship | 0.503751 |
| BILL_AMT2 | BILL | 2.437619 | 0.410236 | Low | Monthly BILL family and utilization relationship | 0.589764 |
| BILL_AMT3 | BILL | 2.501472 | 0.399765 | Low | Monthly BILL family and utilization relationship | 0.600235 |
| BILL_AMT4 | BILL | 2.400651 | 0.416554 | Low | Monthly BILL family and utilization relationship | 0.583446 |
| BILL_AMT5 | BILL | 2.425373 | 0.412308 | Low | Monthly BILL family and utilization relationship | 0.587692 |
| BILL_AMT6 | BILL | 2.027305 | 0.493266 | Low | Monthly BILL family and utilization relationship | 0.506734 |
| PAY_AMT1 | PAY_AMT | 1.847939 | 0.541143 | Low | Monthly payment-amount family | 0.458857 |
| PAY_AMT2 | PAY_AMT | 1.821043 | 0.549136 | Low | Monthly payment-amount family | 0.450864 |
| PAY_AMT3 | PAY_AMT | 1.830652 | 0.546254 | Low | Monthly payment-amount family | 0.453746 |
| PAY_AMT4 | PAY_AMT | 1.808453 | 0.552959 | Low | Monthly payment-amount family | 0.447041 |
| PAY_AMT5 | PAY_AMT | 1.654752 | 0.604320 | Low | Monthly payment-amount family | 0.395680 |
| PAY_AMT6 | PAY_AMT | 1.577494 | 0.633917 | Low | Monthly payment-amount family | 0.366083 |
| current_utilization | Utilization | 2.990464 | 0.334396 | Low | Shared BILL/LIMIT construction across utilization measures | 0.665604 |
| avg_utilization | Utilization | 5.456704 | 0.183261 | Review | Shared BILL/LIMIT construction across utilization measures | 0.816739 |
| max_utilization | Utilization | 5.636386 | 0.177419 | Review | Shared BILL/LIMIT construction across utilization measures | 0.822581 |

- VIF flag counts: Low 24; Review 2; Severe 3.
- Infinite or effectively infinite VIF values: 0.
- Maximum VIF: 25.928051.

Pairwise correlation and VIF answer different questions. Correlation is a two-variable WOE relationship, while VIF assesses how well one WOE predictor is explained jointly by all remaining predictors. Therefore one may be elevated while the other is not; neither diagnostic is a deletion instruction in Stage 4.4A.

## SCR-01–SCR-06 Inventory

| Figure | Content | Saved inline | Result |
|---|---|---|---|
| SCR-01 | Full 29×29 WOE correlation heatmap | Yes | PASS |
| SCR-02 | PAY / delinquency WOE correlation heatmap | Yes | PASS |
| SCR-03 | PAY_AMT WOE correlation heatmap | Yes | PASS |
| SCR-04 | LIMIT_BAL / BILL / utilization WOE correlation heatmap | Yes | PASS |
| SCR-05 | Demographics WOE correlation heatmap | Yes | PASS |
| SCR-06 | Fixed-order full-candidate VIF diagnostic | Yes | PASS |

The clean-run Notebook contains exactly 6 inline PNG outputs. No external figure file was created.

## Family-level Factual Summary

| Family Review Group | Members | Final IV Range | Sparse-bin Flags | Largest Concentration Share | Strongest Pairwise |r| | Flagged Pairs | Max VIF | Structural Fact | Candidate status |
|---|---|---|---|---|---|---|---|---|---|
| PAY / Delinquency | PAY_0, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6, delinquent_months, max_delinquency, severe_delinquent_months | 0.288166–0.872146 | 5 | 0.729126 | 0.944882 | 9 | 25.928051 | Three summaries are deterministic PAY derivatives | Pending Human Checkpoint |
| PAY_AMT | PAY_AMT1, PAY_AMT2, PAY_AMT3, PAY_AMT4, PAY_AMT5, PAY_AMT6 | 0.075093–0.175538 | 0 | 0.535633 | 0.471263 | 0 | 1.847939 | Repeated monthly measurements | Pending Human Checkpoint |
| LIMIT / BILL / Utilization | LIMIT_BAL, BILL_AMT1, BILL_AMT2, BILL_AMT3, BILL_AMT4, BILL_AMT5, BILL_AMT6, current_utilization, avg_utilization, max_utilization | 0.011576–0.190445 | 7 | 0.567019 | 0.890213 | 3 | 5.636386 | Repeated monthly measurements | Pending Human Checkpoint |
| Demographics | AGE, SEX, EDUCATION_GROUP, MARRIAGE_GROUP | 0.004702–0.036380 | 2 | 0.590513 | 0.110869 | 0 | 1.078982 | Four predictors require explicit governance review | Pending Human Checkpoint |

- PAY / delinquency: the three summaries are deterministic functions of the six monthly PAY fields. High correlation and severe summary-variable VIF are factual redundancy diagnostics; they are not Keep/Drop decisions.
- PAY_AMT: monthly WOE correlations are below 0.70 and VIF values are low, while the variables remain repeated monthly measurements.
- LIMIT/BILL/utilization: average and maximum utilization have strong WOE correlation; current utilization has review-level correlations with both. BILL and utilization relationships are diagnostic only.
- Demographics: correlations and VIF are low, while AGE, SEX, EDUCATION_GROUP, and MARRIAGE_GROUP remain explicitly governance-flagged regardless of statistical diagnostics.

## Human Checkpoint — Mandatory Pending Questions

- Group A — PAY / Delinquency: how should recent severity, historical persistence, and deterministic raw/summary overlap be represented as distinct roles?
- Group B — PAY_AMT: do repeated months provide distinct older-month information or unnecessary repetition?
- Group C — LIMIT_BAL / BILL / Utilization: how should weak BILL uniqueness, utilization overlap, and LIMIT_BAL's capacity role be balanced?
- Group D — Demographics: how should IV, sparse/special-category concentration, overlap, and governance considerations affect final inclusion?

No answer or candidate decision is made here. Exactly 29/29 variables remain `Pending Human Checkpoint`; final candidate count is not forced.

## Notebook Clean-run Verification

- Notebook JSON: valid.
- Cells: 32 total; 16 code; 16 Markdown.
- Execution counts: sequential 1–16.
- Error outputs: 0.
- Inline PNG outputs: 6.
- Saved Notebook bytes: 687,837.
- Saved Notebook SHA-256: `b3de384b6e3831f1365a178208f4f8d0aed23bf1609df8abc510701ba991dc88`.
- Execution route: current project `.venv` Python module entry point; the `.venv/bin/jupyter` wrapper itself was stale and pointed to an old path, so it exited before executing any cell. This did not require or cause an environment modification.
- Runtime interpreter reported by the clean run: `/opt/anaconda3/bin/python`, Python 3.11.7, reached through the existing project `.venv` Python entry point.

## Acceptance Criteria Check

- [x] 1. Correct Stage 4.3 HEAD.
- [x] 2. Branch is `main`.
- [x] 3. Initial working tree was clean.
- [x] 4. No remote configured.
- [x] 5. Stage 4.3 final Reviewer PASS verified.
- [x] 6. Split replay exact.
- [x] 7. Train hash exact.
- [x] 8. Test hash exact.
- [x] 9. Test sealed immediately after technical identity checks.
- [x] 10. Train Count = 24,000.
- [x] 11. Good = 18,691.
- [x] 12. Bad = 5,309.
- [x] 13. Exactly 29 predictors.
- [x] 14. Frozen Stage 3 derived features reproduced.
- [x] 15. Stage 4.2 final bins reproduced exactly.
- [x] 16. Stage 4.3 final bins reproduced exactly.
- [x] 17. No re-binning.
- [x] 18. No smoothing.
- [x] 19. 29 final WOE tables verified.
- [x] 20. 24,000×29 WOE matrix constructed.
- [x] 21. No missing WOE.
- [x] 22. No infinite WOE.
- [x] 23. IDs aligned.
- [x] 24. Target unchanged.
- [x] 25. 29-variable individual summary created.
- [x] 26. Final IV used.
- [x] 27. Minimum-bin share calculated.
- [x] 28. Maximum absolute WOE calculated.
- [x] 29. Largest-bin IV contribution calculated.
- [x] 30. Largest-bin IV share calculated.
- [x] 31. Sparse-bin flags calculated.
- [x] 32. Governance-review flags recorded.
- [x] 33. Full 29×29 WOE Pearson correlation calculated.
- [x] 34. SCR-01 created.
- [x] 35. `|r| >= 0.70` pair table created.
- [x] 36. `|r| >= 0.85` strong flags correct.
- [x] 37. Negative correlations handled using absolute magnitude.
- [x] 38. PAY/delinquency matrix created.
- [x] 39. SCR-02 created.
- [x] 40. PAY_AMT matrix created.
- [x] 41. SCR-03 created.
- [x] 42. LIMIT/BILL/utilization matrix created.
- [x] 43. SCR-04 created.
- [x] 44. Demographics matrix created.
- [x] 45. SCR-05 created.
- [x] 46. Full 29-variable VIF calculated.
- [x] 47. Intercept used in VIF auxiliary regressions.
- [x] 48. VIF thresholds correctly flagged.
- [x] 49. SCR-06 created.
- [x] 50. No iterative VIF elimination.
- [x] 51. Pairwise-correlation versus VIF distinction documented.
- [x] 52. Deterministic PAY-summary relationships documented.
- [x] 53. Family-level factual summary produced.
- [x] 54. No Keep decisions.
- [x] 55. No Drop decisions.
- [x] 56. No shortlist.
- [x] 57. No IV cutoff deletion.
- [x] 58. No correlation deletion.
- [x] 59. No VIF deletion.
- [x] 60. No stepwise.
- [x] 61. No LASSO.
- [x] 62. No model-based feature importance.
- [x] 63. No predictive Logistic.
- [x] 64. No coefficients.
- [x] 65. No p-values.
- [x] 66. No Odds Ratios.
- [x] 67. No AUC.
- [x] 68. No KS.
- [x] 69. No CV.
- [x] 70. No Test predictor/risk analysis.
- [x] 71. Exactly 29/29 candidates remain.
- [x] 72. Human Checkpoint questions recorded.
- [x] 73. Notebook clean-runs.
- [x] 74. Task Report complete.
- [x] 75. Immutable Stage 4.4A specification preserved.
- [x] 76. Protected files unchanged.
- [x] 77. Exactly two authorized Stage 4.4A artifacts created.
- [x] 78. Staged diff empty.
- [x] 79. No commit.
- [x] 80. No push.
- [x] 81. Mandatory stop reached.

## Scope, Prohibition, and Test-sealing Confirmations

- No re-binning; no smoothing; no new feature engineering.
- No Keep/Drop, shortlist, IV-threshold deletion, correlation deletion, VIF deletion, stepwise, LASSO, or model-based feature importance.
- No predictive Logistic model; no coefficients, p-values, or Odds Ratios.
- No AUC, KS, performance metric, KFold, or cross-validation.
- No Test predictor/risk/target analysis beyond the authorized identity assertions before immediate deletion.
- No `git add`, commit, push, fetch, pull, reset, rebase, or remote modification.
- Stage 4.4B was not started.

## Git Diff and Final Working-tree Summary

At final read-only verification:

- Branch: `main`.
- HEAD: `5b36512f317b532378977e66b90f1d379693bc4a` (unchanged).
- Tracked diff: empty.
- Staged diff: empty.
- Remote: none.
- `git status --short --untracked-files=all` showed exactly the two authorized untracked artifacts: `notebooks/07_woe_iv_variable_screening.ipynb` and `task_records/Stage_4.4_WOE_IV_Variable_Screening.md`.
- `git diff --name-status`, `git diff --stat`, and `git diff --cached --name-status` were empty.
- Index SHA-256 before Stage 4.4A: `185c93ffab373f5894a4787a15122e319409f94a16abf1b60fee8665d9a35543`.
- Index SHA-256 after Stage 4.4A: `185c93ffab373f5894a4787a15122e319409f94a16abf1b60fee8665d9a35543` — exact equality PASS.

## Deviations, Limitations, Incomplete Items, and Unexpected Artifacts

- Deviation: none from the Architect's Stage 4.4A analytical or file scope.
- Operational limitation: `.venv/bin/jupyter` contains a stale absolute interpreter path and could not start. The clean-run succeeded using `.venv/bin/python -m jupyter`; no package or environment change was made.
- Incomplete by design: Reviewer validation and the Architect/user Human Checkpoint are pending. Stage 4.4B and final selection are not begun.
- Unexpected project artifacts: none observed.

## Reviewer Result

Pending independent Stage 4.4A Reviewer validation. The Reviewer must not perform final candidate selection in this section.

---

`Stage 4.4A complete — final candidate selection pending Architect/user Human Checkpoint.`

# REVIEWER RESULT

## Verdict

PASS WITH FIXES — applies to Stage 4.4A Screening Diagnostics only.

## Requirement Check

- PASS — The immutable Stage 4.4A Architect Specification is present and defines the current acceptance contract.
- PASS — The frozen stratified split was replayed as 24,000 Train / 6,000 sealed Test with disjoint, complete ID coverage and the required Train/Test ID hashes; Test was deleted after the authorized identity checks.
- PASS — All 29 frozen predictors were reproduced in the required order, all 29 final WOE/IV tables were rebuilt without smoothing, and the resulting WOE matrix is finite, aligned, and 24,000×29.
- PASS — Individual diagnostics, the full 29×29 WOE correlation matrix, the four required family matrices, thresholded pair flags, intercept-based full-candidate VIF, governance flags, and SCR-01 through SCR-06 are present.
- PASS — The reported analytical results are supported: 14 sparse-bin flags under the required strict `<3%` rule; 12 correlation pairs at `|r| >= 0.70`, including 4 Strong pairs at `|r| >= 0.85`; and VIF flags of 24 Low, 2 Review, and 3 Severe.
- PASS — Exactly 29/29 predictors remain candidates pending the mandatory Human Checkpoint. No Keep/Drop, shortlist, modeling, performance evaluation, CV, Test-risk analysis, or Stage 4.4B work was performed.
- PASS — The 22 protected tracked inputs remain unchanged. The working tree contains exactly the two authorized untracked Stage 4.4A artifacts, with no staged changes, commit, remote, or push.
- FIX REQUIRED — The Task Report narrative incorrectly states that the sparse-bin flag means minimum Train share `<5%`; the Architect Specification and Notebook implementation require and use the strict `<3%` rule.

## Independent Verification

- Read the complete immutable Stage 4.4A specification and the Codex execution record in this Task Report.
- Inspected the Notebook source, saved execution state, outputs, candidate registry, frozen mappings, WOE construction, sparse-bin condition, correlation flag logic, VIF implementation, scope assertions, and final stop state.
- Independently replayed the raw-data split and confirmed 24,000/6,000 rows, Train class totals of 18,691 Good and 5,309 Bad, zero ID overlap, complete 30,000-ID coverage, and the required Train/Test ID hashes.
- Clean-ran all 16 Notebook code cells in an isolated local kernel: execution counts were 1–16, 103 outputs were produced, and no error output occurred.
- Confirmed the 29-row individual table, 14 sparse-bin flags under `<3%`, the four governance flags, all 12 thresholded correlation pairs and their levels, and all 29 VIF values/levels. Key values included strongest `|r| = 0.944882` and maximum VIF `25.928051`.
- Visually inspected all six embedded diagnostic figures and confirmed SCR-01 through SCR-06 match the required full/family correlation and VIF scopes.
- Recomputed current SHA-256 values for all 22 protected paths and found zero mismatches against the report manifest. Confirmed Notebook SHA-256 `b3de384b6e3831f1365a178208f4f8d0aed23bf1609df8abc510701ba991dc88`.
- Rechecked Git state: branch `main`, HEAD `5b36512f317b532378977e66b90f1d379693bc4a`, empty tracked and staged diffs, no remote, and exactly the two authorized untracked artifacts.

## Issues Found

1. The explanatory sentence in the Task Report under the individual diagnostic table says: `Sparse-bin flag means minimum final-bin Train share < 5%.` This contradicts the immutable requirement (`<3%`) and the Notebook's actual implementation (`Minimum Bin Share < 0.03`). The computed flags and the reported count of 14 are correct for `<3%`; the defect is limited to the Task Report wording but leaves the formal audit record internally inconsistent.

## Required Fixes

1. Correct only that Task Report sentence so it states the strict `<3%` threshold. Preserve the original Architect Specification, Codex evidence, and this Reviewer Result. The correction must be made by Codex or another explicitly authorized implementation step; the Reviewer has not repaired Codex's record.
2. After correction, recheck that the numerical tables and 14 flags remain unchanged. No analytical rerun or re-binning is required unless the correction introduces an unexpected change.

No local commit is permitted at this checkpoint. The Architect Specification reserves the Stage 4.4 milestone commit for completion of Stage 4.4B and final Stage 4.4 Reviewer PASS.

## Architect Decision Needed

After the limited documentation correction is confirmed, conduct the mandatory Human Checkpoint and decide the final 29-variable Keep/Drop disposition for Stage 4.4B. No candidate-selection decision is made by this review.


# Stage 4.4B — Human-Approved Final Candidate Freeze

## Status and Stage 4.4A Reviewer Handoff

- Status: `Stage 4.4B implementation complete — awaiting final Stage 4.4 Reviewer validation.`
- Stage 4.4A Reviewer verdict preserved verbatim: `PASS WITH FIXES — applies to Stage 4.4A Screening Diagnostics only.`
- This section records implementation evidence only. It does not claim final Stage 4.4 Reviewer PASS, milestone commit, production-model status, or Stage 4.5 completion.
- Human Checkpoint: completed by Architect/user. All Keep/Drop decisions below are Human-approved and frozen, not automated outputs of IV, correlation, VIF, or model performance.

## Stage 4.4A Documentation Fix Closure

- Defect type: wording-only.
- The operative Stage 4.4A narrative sentence was changed only from `Sparse-bin flag means minimum final-bin Train share < 5%.` to `Sparse-bin flag means minimum final-bin Train share < 3%.`
- Frozen rule: strict minimum final-bin Train share `<3%`.
- Stage 4.4A sparse-bin flags remain exactly 14/29.
- The 29-row diagnostic table, flags, IV values, correlation matrices, VIF results, Stage 4.4A Notebook code, immutable Stage 4.4A specification, and Reviewer Result were not rewritten.
- No re-binning, smoothing, or analytical recomputation was required solely for this documentation fix.
- Sparse-bin Flag means only to inspect whether apparent signal depends materially on a small final bin. It does not automatically Drop, re-bin, smooth, or invalidate a predictor.

## Pre-flight and Protected Baseline

- Root: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD: `5b36512f317b532378977e66b90f1d379693bc4a`
- Parent: `75c1ac1db6c76a7d80d1be3b13b8c8449ac92fd5`
- Message: `Finalize continuous variable binning`
- Initial tracked and staged diffs: empty; remote: none; exactly the two Stage 4.4 artifacts were untracked.
- Approved Stage 4.4A Notebook baseline: 687,837 bytes; SHA-256 `b3de384b6e3831f1365a178208f4f8d0aed23bf1609df8abc510701ba991dc88`.
- Stage 4.4 Task Report baseline after Stage 4.4A Reviewer Result: 111,136 bytes; SHA-256 `cc2a4242310bc19bfefff22fa7331ba5dd1467fca18904c0f0819a1904b14852`.
- Git index SHA-256 before Stage 4.4B: `185c93ffab373f5894a4787a15122e319409f94a16abf1b60fee8665d9a35543`.

### Protected-input fingerprint comparison

| Protected path | Bytes | SHA-256 before | SHA-256 after | Result |
|---|---|---|---|---|
| .gitignore | 534 | 300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1 | 300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1 | PASS |
| README.md | 2245 | c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34 | c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34 | PASS |
| data/README.md | 1004 | c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0 | c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0 | PASS |
| data/raw/UCI_Credit_Card.csv | 2862995 | a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1 | a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1 | PASS |
| notebooks/01_baseline_replication.ipynb | 1057 | ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20 | ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20 | PASS |
| notebooks/01_luca_audit_and_data_understanding.ipynb | 78438 | 1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c | 1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c | PASS |
| notebooks/02_train_test_split_and_eda.ipynb | 937648 | b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d | b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d | PASS |
| notebooks/03_feature_engineering_and_sanity_checks.ipynb | 142371 | 68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256 | 68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256 | PASS |
| notebooks/04_woe_iv_binning_method_design.ipynb | 52998 | 97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9 | 97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9 | PASS |
| notebooks/05_pay_categorical_binning.ipynb | 971553 | 79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796 | 79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796 | PASS |
| notebooks/06_continuous_binning.ipynb | 1901709 | 97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b | 97bffcf53584ce40635fe0ff33fb6c1a38feb669be61e7914922f6029047497b | PASS |
| references/README.md | 1109 | a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c | a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c | PASS |
| references/credit-card-default-a-very-pedagogical-notebook.ipynb | 805686 | 6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd | 6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd | PASS |
| reports/figures/.gitkeep | 60 | 73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63 | 73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63 | PASS |
| requirements.txt | 95 | 5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b | 5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b | PASS |
| task_records/Stage_1_Project_Foundation.md | 32398 | 0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d | 0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d | PASS |
| task_records/Stage_2_Luca_Audit_and_Data_Understanding.md | 62868 | 3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7 | 3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7 | PASS |
| task_records/Stage_3.1_Train_Test_Split_and_EDA.md | 43454 | 62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0 | 62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0 | PASS |
| task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md | 77812 | b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b | b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b | PASS |
| task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md | 50459 | 2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851 | 2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851 | PASS |
| task_records/Stage_4.2_PAY_Categorical_Binning.md | 102407 | 9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60 | 9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60 | PASS |
| task_records/Stage_4.3_Continuous_Variable_Binning.md | 154540 | a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5 | a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5 | PASS |
| task_records/TASK_REPORT_Task_0.1_Current-State_Audit.md | 25094 | 615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8 | 615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8 | PASS |

All 23 protected inputs match their within-run pre-flight bytes and SHA-256 values.

## Split Replay, Holdout Sealing, and Full 29-variable Baseline

- Raw / Train / Test rows: 30,000 / 24,000 / 6,000.
- Train/Test ID overlap: 0; ID union covers all raw IDs.
- Train ID SHA-256: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`.
- Test ID SHA-256: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Test was used only for row count, ID overlap, union coverage, and frozen ID hash, then immediately deleted. No Test predictor, target-distribution, bin, WOE/IV, correlation, VIF, risk, or performance analysis occurred.
- Train totals: Count 24,000; Good 18,691; Bad 5,309.
- Stage 3 deterministic features, Stage 4.2 final bins, Stage 4.3 final bins, and final Train WOE mappings were independently reproduced from the raw CSV.
- Full WOE matrix: exact 24,000×29 fixed registry; zero missing/non-finite WOE; unique aligned IDs; row order and target unchanged; no smoothing, re-binning, clipping, capping, or winsorization.
- Historical Phase A sparse flags: 14/29 under strict `<3%`.
- Historical full WOE correlation: 12 unique `|r| >= 0.70` pairs, 4 Strong `|r| >= 0.85` pairs, maximum off-diagonal `|r| = 0.944882`.
- Historical full-candidate VIF: Low 24, Review 2, Severe 3, maximum `25.928051`.

## Human-approved Frozen Decisions

### Exact Keep list — canonical Stage 4.5 candidate order

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

### Exact Drop list — 19

`PAY_2, PAY_3, PAY_4, PAY_5, PAY_6, delinquent_months, max_delinquency, SEX, EDUCATION_GROUP, MARRIAGE_GROUP, AGE, BILL_AMT1, BILL_AMT2, BILL_AMT3, BILL_AMT4, BILL_AMT5, BILL_AMT6, current_utilization, max_utilization`

Reconciliation: 29 original candidates − 19 Drop = 10 Keep.

### Family rationales and wording boundaries

- **PAY / delinquency:** `PAY_0` represents recent/current delinquency status; its retention is not “highest IV wins.” `severe_delinquent_months` represents serious-delinquency persistence/frequency across six months. `PAY_2`–`PAY_6` are excluded for parsimonious current + historical role representation, not because they are weak. `delinquent_months` and `max_delinquency` are individually strong but structurally redundant historical summaries; `max_delinquency` captures worst severity rather than repeated persistence. The approximately 0.692 `PAY_0`/severe-summary correlation is not proof of mathematical independence.
- **PAY_AMT:** all six remain separate candidates because no Stage 4.4A pair reached `|r| >= 0.70`, maximum within-family WOE correlation was approximately 0.471263, all six full-set VIF values were below 2, and no defensible natural month cutoff existed. This does not guarantee eventual production-model inclusion.
- **LIMIT_BAL / BILL / utilization:** `LIMIT_BAL` represents assigned credit capacity. BILL variables are excluded for consistently very weak standalone discrimination and no compelling representative—not because they are highly correlated with utilization. `avg_utilization` is the frozen sustained six-month family representative given its signal, interpretation, and overlap; the 0.20/0.60 project bins are not industry or regulatory thresholds.
- **Demographics:** AGE, SEX, EDUCATION_GROUP, and MARRIAGE_GROUP are excluded to prefer behavioral and credit-exposure candidates on project parsimony/model-governance grounds. This is a project modeling decision, not a legal determination or universal prohibition.
- **EDUCATION sparse-signal note:** total IV approximately 0.036380; Other / Unknown Train count 386, share approximately 1.61%, IV contribution approximately 0.017900. Roughly half the total IV came from this small special category; that is a caution and does not alone prove instability.

## Complete 29-row Human Decision Record

| Predictor | Family | Final IV | Stage 4.4 Human Decision | Final Role / Exclusion Rationale | Stage 4.5 Candidate? |
|---|---|---|---|---|---|
| PAY_0 | PAY | 0.872145621 | KEEP — Human-approved | Human-approved Keep: recent / most-current delinquency-status representation; not an automatic highest-IV choice. | Yes |
| PAY_2 | PAY | 0.548098696 | DROP — Human-approved | Human-approved Drop: parsimonious current + historical PAY representation; this monthly PAY predictor is not weak. | No |
| PAY_3 | PAY | 0.414433849 | DROP — Human-approved | Human-approved Drop: parsimonious current + historical PAY representation; this monthly PAY predictor is not weak. | No |
| PAY_4 | PAY | 0.359543103 | DROP — Human-approved | Human-approved Drop: parsimonious current + historical PAY representation; this monthly PAY predictor is not weak. | No |
| PAY_5 | PAY | 0.327106491 | DROP — Human-approved | Human-approved Drop: parsimonious current + historical PAY representation; this monthly PAY predictor is not weak. | No |
| PAY_6 | PAY | 0.288165668 | DROP — Human-approved | Human-approved Drop: parsimonious current + historical PAY representation; this monthly PAY predictor is not weak. | No |
| delinquent_months | Delinquency Summary | 0.871421080 | DROP — Human-approved | Human-approved Drop: individually strong but structurally redundant within the chosen current + historical representation. | No |
| max_delinquency | Delinquency Summary | 0.794582507 | DROP — Human-approved | Human-approved Drop: individually strong historical summary with high overlap; worst severity does not capture repeated persistence. | No |
| severe_delinquent_months | Delinquency Summary | 0.846474513 | KEEP — Human-approved | Human-approved Keep: historical serious-delinquency persistence/frequency across the six-month PAY history. | Yes |
| SEX | Demographic Categorical | 0.011049355 | DROP — Human-approved | Human-approved Drop: project parsimony/model-governance decision for the weak demographic family; not a legal conclusion. | No |
| EDUCATION_GROUP | Demographic Categorical | 0.036380079 | DROP — Human-approved | Human-approved Drop: project parsimony/model-governance decision for the weak demographic family; not a legal conclusion. | No |
| MARRIAGE_GROUP | Demographic Categorical | 0.004702252 | DROP — Human-approved | Human-approved Drop: project parsimony/model-governance decision for the weak demographic family; not a legal conclusion. | No |
| AGE | Demographic / Capacity | 0.020428857 | DROP — Human-approved | Human-approved Drop: project parsimony/model-governance decision for the weak demographic family; not a legal conclusion. | No |
| LIMIT_BAL | Demographic / Capacity | 0.190444893 | KEEP — Human-approved | Human-approved Keep: credit capacity / assigned credit-limit information with meaningful signal and low overlap. | Yes |
| BILL_AMT1 | BILL | 0.015030576 | DROP — Human-approved | Human-approved Drop: consistently very weak standalone discrimination and no compelling single BILL representative; not a utilization-correlation claim. | No |
| BILL_AMT2 | BILL | 0.013563868 | DROP — Human-approved | Human-approved Drop: consistently very weak standalone discrimination and no compelling single BILL representative; not a utilization-correlation claim. | No |
| BILL_AMT3 | BILL | 0.011575528 | DROP — Human-approved | Human-approved Drop: consistently very weak standalone discrimination and no compelling single BILL representative; not a utilization-correlation claim. | No |
| BILL_AMT4 | BILL | 0.013016362 | DROP — Human-approved | Human-approved Drop: consistently very weak standalone discrimination and no compelling single BILL representative; not a utilization-correlation claim. | No |
| BILL_AMT5 | BILL | 0.016778117 | DROP — Human-approved | Human-approved Drop: consistently very weak standalone discrimination and no compelling single BILL representative; not a utilization-correlation claim. | No |
| BILL_AMT6 | BILL | 0.015700368 | DROP — Human-approved | Human-approved Drop: consistently very weak standalone discrimination and no compelling single BILL representative; not a utilization-correlation claim. | No |
| PAY_AMT1 | PAY_AMT | 0.175537864 | KEEP — Human-approved | Human-approved Keep: retain all six separate months for Stage 4.5 rather than impose an arbitrary month cutoff. | Yes |
| PAY_AMT2 | PAY_AMT | 0.151113587 | KEEP — Human-approved | Human-approved Keep: retain all six separate months for Stage 4.5 rather than impose an arbitrary month cutoff. | Yes |
| PAY_AMT3 | PAY_AMT | 0.121957912 | KEEP — Human-approved | Human-approved Keep: retain all six separate months for Stage 4.5 rather than impose an arbitrary month cutoff. | Yes |
| PAY_AMT4 | PAY_AMT | 0.088405982 | KEEP — Human-approved | Human-approved Keep: retain all six separate months for Stage 4.5 rather than impose an arbitrary month cutoff. | Yes |
| PAY_AMT5 | PAY_AMT | 0.075093140 | KEEP — Human-approved | Human-approved Keep: retain all six separate months for Stage 4.5 rather than impose an arbitrary month cutoff. | Yes |
| PAY_AMT6 | PAY_AMT | 0.079904482 | KEEP — Human-approved | Human-approved Keep: retain all six separate months for Stage 4.5 rather than impose an arbitrary month cutoff. | Yes |
| current_utilization | Utilization | 0.075545965 | DROP — Human-approved | Human-approved Drop: overlapping utilization representation; avg_utilization is the frozen sustained family representative. | No |
| avg_utilization | Utilization | 0.121261375 | KEEP — Human-approved | Human-approved Keep: sustained six-month utilization and representative of the overlapping utilization family. | Yes |
| max_utilization | Utilization | 0.089296729 | DROP — Human-approved | Human-approved Drop: overlapping utilization representation; avg_utilization is the frozen sustained family representative. | No |

Validation: 29 rows, 10 Yes, 19 No; no `Pending Human Checkpoint` remains in the final disposition record.

## Reduced 24,000×10 WOE Matrix Integrity

- Shape: `(24000, 10)`.
- Columns: exact frozen canonical order.
- Construction: direct subset of the verified full 24,000×29 WOE matrix; no WOE was re-estimated.
- Every reduced column equals its corresponding full-matrix column exactly.
- Missing: 0; non-finite: 0; duplicate columns: 0.
- Row order, ID alignment, and target remain unchanged; ID and target are not predictor columns.

## Complete Reduced 10×10 WOE Pearson Correlation Matrix

| index | PAY_0 | severe_delinquent_months | PAY_AMT1 | PAY_AMT2 | PAY_AMT3 | PAY_AMT4 | PAY_AMT5 | PAY_AMT6 | LIMIT_BAL | avg_utilization |
|---|---|---|---|---|---|---|---|---|---|---|
| PAY_0 | 1.000000 | 0.692029 | 0.259709 | 0.190637 | 0.161785 | 0.140970 | 0.118388 | 0.125074 | 0.195167 | 0.198552 |
| severe_delinquent_months | 0.692029 | 1.000000 | 0.208289 | 0.198601 | 0.172371 | 0.139362 | 0.136038 | 0.114088 | 0.299524 | 0.187271 |
| PAY_AMT1 | 0.259709 | 0.208289 | 1.000000 | 0.448809 | 0.442652 | 0.398748 | 0.372389 | 0.374639 | 0.227599 | 0.025080 |
| PAY_AMT2 | 0.190637 | 0.198601 | 0.448809 | 1.000000 | 0.452297 | 0.436578 | 0.404541 | 0.408107 | 0.255705 | 0.015174 |
| PAY_AMT3 | 0.161785 | 0.172371 | 0.442652 | 0.452297 | 1.000000 | 0.441861 | 0.445683 | 0.431467 | 0.222042 | 0.006916 |
| PAY_AMT4 | 0.140970 | 0.139362 | 0.398748 | 0.436578 | 0.441861 | 1.000000 | 0.442683 | 0.458867 | 0.201073 | -0.016322 |
| PAY_AMT5 | 0.118388 | 0.136038 | 0.372389 | 0.404541 | 0.445683 | 0.442683 | 1.000000 | 0.471263 | 0.230981 | -0.001055 |
| PAY_AMT6 | 0.125074 | 0.114088 | 0.374639 | 0.408107 | 0.431467 | 0.458867 | 0.471263 | 1.000000 | 0.273361 | 0.006592 |
| LIMIT_BAL | 0.195167 | 0.299524 | 0.227599 | 0.255705 | 0.222042 | 0.201073 | 0.230981 | 0.273361 | 1.000000 | 0.337780 |
| avg_utilization | 0.198552 | 0.187271 | 0.025080 | 0.015174 | 0.006916 | -0.016322 | -0.001055 | 0.006592 | 0.337780 | 1.000000 |

### Reduced flagged-pair table

No pairs met `|r| >= 0.70`; the complete threshold table is empty. Thresholds remained Review for `0.70 <= |r| < 0.85` and Strong for `|r| >= 0.85`, using absolute magnitude and signed WOE Pearson correlation.

- Maximum absolute off-diagonal correlation: `0.692029` (`PAY_0` versus `severe_delinquent_months`).
- `|r| >= 0.70` pair count: 0.
- `|r| >= 0.85` pair count: 0.

## Reduced VIF Method and Results

For each candidate, deterministic NumPy least squares regressed that WOE predictor on an intercept plus the other nine reduced WOE predictors. `VIF = 1/(1-R²)`, numerical tolerance `1e-10`; no jitter, perturbation, or automatic removal was used.

| Predictor | VIF | Tolerance | Flag Level | Final Role |
|---|---|---|---|---|
| PAY_0 | 2.009437 | 0.497652 | Low | Current delinquency status |
| severe_delinquent_months | 2.040957 | 0.489966 | Low | Historical serious-delinquency persistence |
| PAY_AMT1 | 1.502995 | 0.665338 | Low | Monthly payment amount 1 |
| PAY_AMT2 | 1.552006 | 0.644328 | Low | Monthly payment amount 2 |
| PAY_AMT3 | 1.582135 | 0.632057 | Low | Monthly payment amount 3 |
| PAY_AMT4 | 1.549837 | 0.645229 | Low | Monthly payment amount 4 |
| PAY_AMT5 | 1.533791 | 0.651979 | Low | Monthly payment amount 5 |
| PAY_AMT6 | 1.565398 | 0.638815 | Low | Monthly payment amount 6 |
| LIMIT_BAL | 1.350414 | 0.740514 | Low | Assigned credit capacity |
| avg_utilization | 1.177133 | 0.849522 | Low | Sustained six-month utilization |

- Flag counts: Low 10; Review 0; Severe 0; Infinite 0.
- Maximum VIF: `2.040957` (`severe_delinquent_months`).
- Infinite/effectively infinite findings: 0.

## SCR-07 and SCR-08

- SCR-07 — Final 10-variable WOE correlation heatmap: PASS, saved inline in the Notebook using the canonical order and symmetric `[-1,1]` scale.
- SCR-08 — Final 10-variable VIF diagnostic: PASS, saved inline in the Notebook with all ten candidates and reference lines at 5 and 10.
- No external PNG or figure artifact was created.

## Conditional Validation and Candidate Freeze

- Result: `CLEAN — no Stage 4.4 redundancy flags remain in the reduced candidate set.`
- Architect review required: No.
- Every reduced `|r| < 0.70` and every reduced VIF `<5`.
- `no Stage 4.4 redundancy flags remain in the reduced candidate set.`
- `Stage 4.4 Candidate Logistic Predictor List — FROZEN`
- This is the fixed candidate list for Stage 4.5, not a predictive model or final production model.

## Notebook Clean-run

- Valid Notebook JSON with 48 cells: 24 Markdown and 24 code.
- Execution counts: sequential 1–24.
- Error outputs / tracebacks: 0.
- Inline PNG outputs: 8, SCR-01 through SCR-08.
- Final Notebook bytes at report construction: 850,894.
- Final Notebook SHA-256 at report construction: `1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce`.
- Stage 4.4A source preservation: all original 32 cell sources match the pre-flight per-cell hashes exactly.
- The Notebook executed from raw CSV in a fresh kernel; prior Notebooks were not executed and no prior runtime state was used.

## Scope and Modeling-boundary Confirmation

- No re-binning, smoothing, clipping, capping, winsorization, new feature engineering, automated selection, or alternate subset experiment.
- No predictive Logistic, coefficients, signs, standard errors, p-values, Odds Ratios, AIC/BIC, or likelihood tests.
- No ROC/AUC, KS, PR-AUC, accuracy, precision, recall, F1, calibration, confusion matrix, or threshold analysis.
- No KFold, StratifiedKFold, CV, bootstrap, fold-specific WOE/selection/VIF, or Test risk analysis.
- No external CSV, reduced dataset, mapping, model, pickle/joblib, HTML/PDF, PNG, `src/`, or `tests/` artifact.
- No `git add`, commit, push, remote change, restore, reset, stash, clean, checkout, amend, or rebase.
- Stage 4.5 was not started.

## Acceptance Criteria Check

- [x] 1. Correct Stage 4.3 HEAD.
- [x] 2. Branch `main`.
- [x] 3. Exactly two expected Stage 4.4A artifacts.
- [x] 4. Stage 4.4A Notebook baseline verified.
- [x] 5. Stage 4.4A report baseline verified.
- [x] 6. Stage 4.4A Reviewer `PASS WITH FIXES` preserved.
- [x] 7. Only the authorized documentation fix was performed in prior Phase A narrative.
- [x] 8. `<5%` corrected to `<3%`.
- [x] 9. 14/29 sparse flags unchanged.
- [x] 10. Stage 4.4A diagnostics preserved.
- [x] 11. Split replay exact.
- [x] 12. Train hash exact.
- [x] 13. Test hash exact.
- [x] 14. Test sealed.
- [x] 15. Count = 24,000.
- [x] 16. Good = 18,691.
- [x] 17. Bad = 5,309.
- [x] 18. Full 29-variable WOE baseline reproduced.
- [x] 19. No re-binning.
- [x] 20. No smoothing.
- [x] 21. Human Checkpoint recorded as completed.
- [x] 22. Exact 10 Keep variables.
- [x] 23. Exact 19 Drop variables.
- [x] 24. PAY_0 retained.
- [x] 25. severe_delinquent_months retained.
- [x] 26. PAY_2–PAY_6 excluded.
- [x] 27. delinquent_months excluded.
- [x] 28. max_delinquency excluded.
- [x] 29. All PAY_AMT1–6 retained.
- [x] 30. LIMIT_BAL retained.
- [x] 31. All BILL_AMT1–6 excluded.
- [x] 32. avg_utilization retained.
- [x] 33. current_utilization excluded.
- [x] 34. max_utilization excluded.
- [x] 35. AGE excluded.
- [x] 36. SEX excluded.
- [x] 37. EDUCATION_GROUP excluded.
- [x] 38. MARRIAGE_GROUP excluded.
- [x] 39. Rationales accurately documented.
- [x] 40. 29-row final disposition table created.
- [x] 41. Reduced WOE matrix shape = 24,000×10.
- [x] 42. Reduced matrix is an exact subset of the full WOE matrix.
- [x] 43. No missing/non-finite reduced values.
- [x] 44. Reduced 10×10 WOE correlation created.
- [x] 45. SCR-07 created.
- [x] 46. Reduced correlation pair table created.
- [x] 47. Reduced VIF for all 10 created.
- [x] 48. SCR-08 created.
- [x] 49. Same correlation thresholds used.
- [x] 50. Same VIF thresholds used.
- [x] 51. No automatic variable removal.
- [x] 52. Conditional-stop rules applied correctly.
- [x] 53. No predictive Logistic.
- [x] 54. No coefficients.
- [x] 55. No p-values.
- [x] 56. No Odds Ratios.
- [x] 57. No AUC.
- [x] 58. No KS.
- [x] 59. No CV.
- [x] 60. No Test risk analysis.
- [x] 61. No new feature engineering.
- [x] 62. No alternate subset experiments.
- [x] 63. Final Candidate Logistic Predictor List frozen because validation is clean.
- [x] 64. Notebook clean-runs.
- [x] 65. Task Report complete.
- [x] 66. Stage 4.4B immutable specification preserved.
- [x] 67. Protected files unchanged.
- [x] 68. Only two Stage 4.4 artifacts changed.
- [x] 69. Staged diff empty.
- [x] 70. No commit.
- [x] 71. No push.
- [x] 72. Final Reviewer handoff reached.

## Immutable Architect Specification — Stage 4.4B

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.4B ================

# Stage 4.4B — Final Candidate Freeze

## 1. Stage Context

Stage 4 固定结构：

- Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
- Stage 4.2 — PAY + Categorical Binning ✅ CLOSED
- Stage 4.3 — Continuous Variable Binning ✅ CLOSED
- Stage 4.4A — Screening Diagnostics ✅ IMPLEMENTED / REVIEWED
- Stage 4.4B — Final Candidate Freeze ← CURRENT
- Stage 4.5 — Candidate WOE Logistic Regression
- Stage 4.6 — Stratified 5-Fold Model Stability Test
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit

Stage 4.4A Reviewer verdict:

`PASS WITH FIXES — applies to Stage 4.4A Screening Diagnostics only.`

The analytical work passed.

Exactly one documentation defect remains:

`Sparse-bin flag means minimum final-bin Train share < 5%.`

The correct frozen rule is:

`Sparse-bin flag means minimum final-bin Train share < 3%.`

Reviewer independently confirmed:

- actual Notebook implementation uses strict `<3%`;
- actual sparse-bin count = 14 / 29;
- numerical tables are correct;
- correlation results are correct;
- VIF results are correct;
- 29/29 predictors remained candidates;
- no Stage 4.4B work had been performed.

Architect/user explicitly decided:

> Do not run a separate repair round. Correct this documentation defect as the first limited task inside Stage 4.4B.

---

# 2. Git Baseline

Project root:

`/Users/yu/2.工作/Risk Analysis Project`

Expected branch:

`main`

Expected HEAD remains Stage 4.3 milestone:

`5b36512f317b532378977e66b90f1d379693bc4a`

Short:

`5b36512`

Message:

`Finalize continuous variable binning`

Parent:

`75c1ac1db6c76a7d80d1be3b13b8c8449ac92fd5`

Expected:

- tracked diff empty;
- staged diff empty;
- remote none;
- no Stage 4.4 commit yet;
- exactly two authorized Stage 4.4 artifacts exist untracked.

Expected Stage 4.4A artifacts:

`notebooks/07_woe_iv_variable_screening.ipynb`

and:

`task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

Approved Stage 4.4A Notebook baseline:

- bytes: `687837`
- SHA-256:
  `b3de384b6e3831f1365a178208f4f8d0aed23bf1609df8abc510701ba991dc88`

Stage 4.4 Task Report after Stage 4.4A Reviewer Result:

- bytes: `111136`
- SHA-256:
  `cc2a4242310bc19bfefff22fa7331ba5dd1467fca18904c0f0819a1904b14852`

The Task Report must contain the Stage 4.4A Reviewer Result:

> `PASS WITH FIXES`

and the known `<5%` → `<3%` documentation correction requirement.

Any unrelated baseline mismatch:

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

# 3. Stage 4.4B Objective

Stage 4.4B has exactly five purposes:

1. close the single Stage 4.4A documentation fix;
2. record the completed Human Checkpoint Keep/Drop decisions;
3. implement the exact Human-approved 10-variable reduced WOE candidate matrix;
4. recompute reduced-set WOE correlation and VIF;
5. freeze the final Candidate Logistic Predictor List for Stage 4.5.

Stage 4.4B is NOT a second feature-selection search.

Human decisions are already frozen.

---

# 4. Preserve Stage 4.4A History

Continue using the same two artifacts:

`notebooks/07_woe_iv_variable_screening.ipynb`

`task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

Do not create a new Stage 4.4B Notebook.

Do not create a new Stage 4.4B report.

Notebook requirements:

- preserve all existing Stage 4.4A cell source content;
- preserve Stage 4.4A methodology and diagnostic history;
- preserve SCR-01 through SCR-06;
- append Stage 4.4B after the existing Phase A material;
- do not rewrite Stage 4.4A as though the final candidate set had been known in advance.

Task Report requirements:

- preserve the immutable Stage 4.4A Architect Specification;
- preserve the Stage 4.4A Reviewer Result verbatim;
- preserve the complete diagnostic evidence;
- append a distinct Stage 4.4B section.

One exception is explicitly authorized:

> the single incorrect `<5%` narrative sentence must be corrected to `<3%`.

No other Stage 4.4A narrative rewriting is authorized.

---

# 5. Stage 4.4A Documentation Fix — REQUIRED FIRST

Locate the Task Report sentence:

`Sparse-bin flag means minimum final-bin Train share < 5%.`

Replace only that threshold wording with:

`Sparse-bin flag means minimum final-bin Train share < 3%.`

Do not alter:

- the 29-row diagnostic table;
- sparse-bin flags;
- 14/29 count;
- IV values;
- correlation matrices;
- VIF values;
- Notebook Stage 4.4A code;
- Stage 4.4A Reviewer Result.

After correction assert:

- sparse flags still = 14 / 29;
- all existing numeric values remain unchanged;
- no analytical selection decision was retroactively changed.

Append a short section:

`Stage 4.4A Documentation Fix Closure`

recording:

- defect was wording-only;
- `<3%` is the frozen rule;
- 14/29 result unchanged;
- no re-binning;
- no analytical recomputation was required solely for the fix.

A later full Stage 4.4 Notebook clean-run is still required as normal Stage 4.4B validation.

---

# 6. Holdout Test Protection

Replay the frozen split exactly from:

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

Test is permitted only for:

- row count;
- ID overlap;
- ID union coverage;
- frozen ID hash.

Then delete/seal Test.

Do not inspect:

- Test predictor values;
- Test ranges;
- Test bin occupancy;
- Test WOE;
- Test IV;
- Test correlation;
- Test VIF;
- Test target distribution;
- Test risk;
- Test performance.

Holdout remains sealed until Stage 5.

---

# 7. Reproduce the Exact Stage 4.4A 29-variable WOE Baseline

Stage 4.4B must still clean-run the complete Notebook from raw CSV.

Therefore reproduce the frozen:

- Stage 3 deterministic features;
- Stage 4.2 final bins;
- Stage 4.3 final bins;
- final Train WOE mappings.

Recreate the exact:

> 24,000 × 29 WOE predictor matrix.

Requirements:

- 24,000 rows;
- 29 predictors;
- all finite;
- no NaN;
- no smoothing;
- ID/order alignment preserved;
- target unchanged.

Do not change any frozen bin definition.

---

# 8. Stage 4.4A Analytical Baseline — Preserve

The following verified results are historical Phase A evidence and must remain reproducible:

## Sparse bins

- 14 / 29 predictors flagged under strict `<3%`.

## Full WOE correlation

- 12 unique pairs with `|r| >= 0.70`;
- 4 Strong pairs with `|r| >= 0.85`;
- maximum absolute off-diagonal correlation = `0.944882`.

## Full-candidate VIF

- Low = 24;
- Review = 2;
- Severe = 3;
- maximum VIF = `25.928051`.

Do not optimize or alter predictors to reproduce these numbers.

They are diagnostic baseline evidence only.

---

# 9. Mandatory Human Checkpoint — COMPLETED

Architect/user reviewed the four required groups:

- Group A — PAY / Delinquency
- Group B — PAY\_AMT
- Group C — LIMIT\_BAL / BILL / Utilization
- Group D — Demographics

The final decisions below are now:

> FROZEN.

Codex does not re-decide them.

---

# 10. FINAL KEEP LIST — EXACTLY 10 PREDICTORS

The Stage 4.5 Candidate Logistic Predictor List is:

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

Use this fixed order in all Stage 4.4B reduced-set tables and figures.

Exactly 10 predictors.

No additions.

No substitutions.

No reordering unless technically unavoidable; if display ordering differs, preserve the above list as the canonical frozen order.

---

# 11. FINAL DROP LIST — EXACTLY 19 PREDICTORS

Drop from the Stage 4.5 candidate set:

## PAY monthly variables

- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`

## Other delinquency summaries

- `delinquent_months`
- `max_delinquency`

## Demographics

- `SEX`
- `EDUCATION_GROUP`
- `MARRIAGE_GROUP`
- `AGE`

## BILL

- `BILL_AMT1`
- `BILL_AMT2`
- `BILL_AMT3`
- `BILL_AMT4`
- `BILL_AMT5`
- `BILL_AMT6`

## Other utilization representations

- `current_utilization`
- `max_utilization`

Exactly:

> 19 dropped.

29 original candidates − 19 = 10 final Stage 4.5 candidates.

---

# 12. Group A Decision — PAY / Delinquency

KEEP:

- `PAY_0`
- `severe_delinquent_months`

DROP:

- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`
- `delinquent_months`
- `max_delinquency`

This is an explicit Human decision.

Do not reopen it.

---

# 13. PAY\_0 Rationale

`PAY_0` is retained as:

> the recent / most-current delinquency-status representation.

Stage 4.4A final IV:

approximately:

`0.872146`

It is a very strong standalone predictor.

Its role is intentionally different from the historical summary.

Do not describe its retention as:

> highest IV wins.

Its retained role is:

> current delinquency severity/status.

---

# 14. severe\_delinquent\_months Rationale

`severe_delinquent_months` is retained as:

> historical serious-delinquency persistence/frequency across the six-month PAY history.

Definition:

[
severe\_delinquent\_months
==========================

\sum\_i 1(PAY\_i\ge2)
]

It combines:

- severity threshold;
- persistence/frequency across months.

Stage 4.4A final IV:

approximately:

`0.846475`

Its WOE correlation with `PAY_0` was approximately:

`0.692029`

which is materially below the approximately 0.915–0.945 correlations observed among the three historical summary variables themselves.

Do not treat the 0.70 review threshold as a mathematical proof of independence.

This was a Human role-based decision.

---

# 15. Why delinquent\_months Is Dropped

`delinquent_months` is individually very strong.

Stage 4.4A final IV:

approximately:

`0.871421`

It is NOT dropped because it lacks predictive signal.

It is dropped because:

- it is a deterministic PAY-derived historical summary;
- it is extremely correlated with the other historical summaries;
- it overlaps more strongly with `PAY_0` than the chosen severe summary;
- the chosen design seeks one current PAY representation plus one historical summary.

Important verified correlations:

approximately:

- delinquent\_months vs severe\_delinquent\_months = `0.944882`
- delinquent\_months vs max\_delinquency = `0.943585`

Verified full-candidate VIF:

approximately:

`25.928051`

Correct rationale:

> strong but structurally redundant within the chosen current + historical representation.

Not:

> weak variable.

---

# 16. Why max\_delinquency Is Dropped

`max_delinquency` is also individually very strong.

Stage 4.4A final IV:

approximately:

`0.794583`

It is dropped because:

- it is highly redundant with the two other historical summary variables;
- it describes the worst observed delinquency severity but not repeated persistence;
- the Human checkpoint preferred `severe_delinquent_months` as the historical representative because it captures repeated serious delinquency.

Verified correlations approximately:

- max vs delinquent\_months = `0.943585`
- max vs severe\_delinquent\_months = `0.914702`

Verified VIF approximately:

`17.002214`

Do not claim max\_delinquency has no information.

---

# 17. Why PAY\_2–PAY\_6 Are Dropped

PAY\_2 through PAY\_6 remain individually strong predictors.

They are NOT dropped because of low IV.

Stage 4.4A IVs approximately:

- PAY\_2 = 0.5481
- PAY\_3 = 0.4144
- PAY\_4 = 0.3595
- PAY\_5 = 0.3271
- PAY\_6 = 0.2882

The Human decision is to avoid carrying multiple monthly PAY status representations once the candidate set already contains:

- `PAY_0` for current delinquency;
- `severe_delinquent_months` for six-month serious delinquency history.

Correct rationale:

> parsimonious role-based representation of current + historical delinquency behavior.

Do NOT write:

> all PAY\_2–PAY\_6 are highly correlated above 0.70 with PAY\_0.

That is false.

Do NOT write:

> they are weak.

That is also false.

---

# 18. Group B Decision — PAY\_AMT

KEEP ALL SIX:

- PAY\_AMT1
- PAY\_AMT2
- PAY\_AMT3
- PAY\_AMT4
- PAY\_AMT5
- PAY\_AMT6

No PAY\_AMT variable is dropped in Stage 4.4.

---

# 19. PAY\_AMT Rationale

Stage 4.4A showed:

- all six have meaningful standalone IV;
- no PAY\_AMT pair reached the `|r| >= 0.70` redundancy flag;
- maximum within-family WOE correlation was approximately `0.471263`;
- all six VIF values were below 2.

Approximate final IV:

- PAY\_AMT1 = 0.175538
- PAY\_AMT2 = 0.151114
- PAY\_AMT3 = 0.121958
- PAY\_AMT4 = 0.088406
- PAY\_AMT5 = 0.075093
- PAY\_AMT6 = 0.079904

There is a broad recency gradient, but Stage 4.4A did not reveal a defensible natural cutoff such as:

- keep 2 months;
- keep 3 months;
- keep 4 months.

Human decision:

> retain all six for the Stage 4.5 candidate Logistic rather than create an arbitrary month-selection rule.

Do not reinterpret this as proof that all six must remain in the eventual final model.

They are frozen only as Stage 4.5 candidate predictors.

---

# 20. Group C Decision — LIMIT\_BAL / BILL / Utilization

KEEP:

- `LIMIT_BAL`
- `avg_utilization`

DROP:

- BILL\_AMT1–6
- current\_utilization
- max\_utilization

---

# 21. LIMIT\_BAL Rationale

KEEP `LIMIT_BAL`.

Stage 4.4A final IV:

approximately:

`0.190445`

It has:

- meaningful moderate standalone signal;
- low full-candidate VIF, approximately `1.611`;
- only moderate/low WOE correlation with utilization.

Approximate correlations:

- LIMIT\_BAL vs current\_utilization = 0.344849
- LIMIT\_BAL vs avg\_utilization = 0.337780
- LIMIT\_BAL vs max\_utilization = 0.331800

Therefore utilization does not simply duplicate the WOE risk representation of LIMIT\_BAL.

Role:

> credit capacity / assigned credit-limit information.

---

# 22. BILL\_AMT1–6 Rationale

DROP all six raw BILL variables.

Stage 4.4A final IV values are consistently:

approximately:

`0.0116–0.0168`

All six fall in the project's very-weak IV reference range:

[
IV<0.02
]

The Human decision is family-level exclusion because:

- standalone discrimination is consistently very weak;
- there is no compelling single BILL representative;
- retaining multiple weak monthly BILL predictors would add complexity without sufficient screening evidence.

Important:

Do NOT state that BILL is dropped because of high correlation with utilization.

Stage 4.4A showed WOE correlations between BILL and utilization were generally low.

Correct rationale:

> consistently weak standalone discrimination and no compelling representative.

---

# 23. avg\_utilization Rationale

KEEP:

`avg_utilization`

DROP:

- `current_utilization`
- `max_utilization`

Stage 4.4A final IV:

- current ≈ 0.075546
- avg ≈ 0.121261
- max ≈ 0.089297

Pairwise WOE correlations:

- current vs avg ≈ 0.770533
- current vs max ≈ 0.783112
- avg vs max ≈ 0.890213

The avg/max pair was a Strong redundancy flag.

Full-candidate VIF:

- current ≈ 2.990
- avg ≈ 5.457
- max ≈ 5.636

Human rationale for choosing avg:

1. strongest standalone IV of the three;
2. represents sustained six-month utilization rather than one current month;
3. less conceptually dependent on a single maximum observation than `max_utilization`;
4. overlaps strongly with the other two and is therefore a reasonable family representative.

Do not describe 0.20 / 0.60 utilization bins as industry or regulatory thresholds.

---

# 24. Group D Decision — Demographics

DROP:

- AGE
- SEX
- EDUCATION\_GROUP
- MARRIAGE\_GROUP

No demographic predictor enters the Stage 4.5 candidate Logistic.

---

# 25. Demographic Rationale

Approximate final IV:

- MARRIAGE\_GROUP = 0.004702
- SEX = 0.011049
- AGE = 0.020429
- EDUCATION\_GROUP = 0.036380

The family showed:

- little multicollinearity;
- generally weak standalone discrimination;
- explicit Stage 4.4A governance-review flags.

Human decision:

> prefer behavioral and credit-exposure predictors and exclude the weak demographic family from the candidate Logistic on parsimony/model-governance grounds.

This is:

- a project modeling decision;
- not a legal determination;
- not an assertion that these variables are universally prohibited.

Do not provide legal conclusions.

---

# 26. EDUCATION Sparse-signal Note

Record explicitly:

`EDUCATION_GROUP` had:

- total IV ≈ 0.036380;
- Other / Unknown Train count = 386;
- Other / Unknown Train share ≈ 1.61%;
- Other / Unknown IV contribution ≈ 0.017900.

Therefore approximately half of its total IV came from a small special category.

This was an additional caution in the Human decision.

Do not claim sparse-bin concentration alone proves instability.

---

# 27. Sparse-bin Flag Interpretation — Preserve

The correct frozen definition is:

[
Minimum\ Final\ Bin\ Train\ Share<3%
]

Sparse-bin Flag means only:

> inspect whether a predictor's apparent signal depends materially on a small final bin.

It does NOT mean:

- drop the variable;
- re-bin the variable;
- smooth the variable;
- the variable is invalid.

This exact interpretation must remain in the Task Report.

---

# 28. Human Decision Record Table

Append a complete 29-row table with:

- Predictor
- Family
- Final IV
- Stage 4.4 Human Decision
- Final Role / Exclusion Rationale
- Stage 4.5 Candidate? Yes/No

Use exact frozen decisions.

No `Pending Human Checkpoint` remains after Stage 4.4B.

Expected:

- Yes = 10
- No = 19

---

# 29. Construct Reduced 10-variable WOE Matrix

From the verified full 29-variable WOE matrix construct:

> 24,000 × 10 reduced WOE matrix.

Exact fixed column order:

1. PAY\_0
2. severe\_delinquent\_months
3. PAY\_AMT1
4. PAY\_AMT2
5. PAY\_AMT3
6. PAY\_AMT4
7. PAY\_AMT5
8. PAY\_AMT6
9. LIMIT\_BAL
10. avg\_utilization

Requirements:

- exact subset of the full WOE matrix;
- no WOE recalculation difference;
- exactly 24,000 rows;
- exactly 10 columns;
- all values finite;
- no NaN;
- row order unchanged;
- ID alignment unchanged;
- target unchanged.

Assert every reduced column equals the corresponding full-matrix column exactly.

---

# 30. Reduced-set Correlation

Compute Pearson correlation on the exact 10-variable WOE matrix.

Output:

- full 10×10 numeric matrix;
- high-correlation pair table using the same Stage 4.4A rules;
- one new heatmap.

Use the same thresholds:

## Review

[
|r|\ge0.70
]

## Strong

[
|r|\ge0.85
]

Do not change thresholds.

Do not use raw correlation as the primary metric.

---

# 31. New Figure SCR-07

Create:

`SCR-07 — Final 10-variable WOE correlation heatmap`

Requirements:

- 10×10;
- WOE Pearson correlation;
- canonical final candidate order;
- symmetric correlation scale;
- readable labels;
- no Keep/Drop color ranking.

---

# 32. Reduced-set VIF

Recompute VIF on exactly the reduced:

> 24,000 × 10 WOE matrix.

Use the same implementation methodology as Stage 4.4A:

[
VIF\_j=\frac{1}{1-R\_j^2}
]

with:

- intercept;
- deterministic auxiliary OLS;
- no jitter;
- no perturbation;
- no automatic elimination.

Output fixed-order table containing:

- Predictor
- VIF
- Tolerance
- Flag Level
- Final Role

Use same flag rules:

- Low: VIF < 5
- Review: 5 <= VIF < 10
- Severe: VIF >= 10
- Infinite if applicable

---

# 33. New Figure SCR-08

Create:

`SCR-08 — Final 10-variable VIF diagnostic`

Show:

- all 10 candidates;
- fixed candidate order;
- VIF;
- reference lines at 5 and 10 if readable.

No automated deletion.

---

# 34. Reduced-set Validation Logic

Expected objective:

> confirm that the Human-approved 10-variable candidate set has no unresolved major multicollinearity problem before Stage 4.5.

If all:

[
|r|<0.70
]

and:

[
VIF<5
]

then record:

> no Stage 4.4 redundancy flags remain in the reduced candidate set.

Proceed to final Reviewer handoff.

---

# 35. Unexpected Review-level Redundancy

If any reduced candidate pair has:

[
0.70\le|r|<0.85
]

or any reduced predictor has:

[
5\le VIF<10
]

do NOT automatically change the frozen 10-variable list.

Instead:

- report the exact flag;
- retain all 10 temporarily;
- mark `Architect Decision Needed`;
- stop before claiming Stage 4.4 final freeze complete.

No automatic removal.

---

# 36. Unexpected Severe Redundancy

If any reduced pair has:

[
|r|\ge0.85
]

or any reduced predictor has:

[
VIF\ge10
]

then:

> BLOCKED for final Stage 4.4 closure.

Do not:

- remove a variable;
- substitute a variable;
- rerun selection;
- optimize a subset.

Return evidence to Architect/user.

This is not expected from the approved 4.4A evidence, but must be handled explicitly.

---

# 37. Final Candidate Freeze Condition

Only if reduced-set validation contains no unresolved Review/Severe flags requiring Architect action may the Task Report state:

> `Stage 4.4 Candidate Logistic Predictor List — FROZEN`

with exactly the 10 approved variables.

This is a candidate list for Stage 4.5.

It is NOT yet the final production model.

---

# 38. No Reopening Dropped Variables

Stage 4.4B must not compare alternative subsets such as:

- severe\_delinquent\_months vs delinquent\_months;
- avg\_utilization vs max\_utilization;
- PAY\_AMT1–3 vs PAY\_AMT1–6;
- models with/without demographics;
- one BILL month versus another.

Human decisions are frozen.

No alternate-screening experiments.

---

# 39. No Logistic Modeling

Do not fit predictive Logistic Regression.

Do not calculate:

- Logistic coefficients;
- coefficient signs;
- standard errors;
- p-values;
- Odds Ratios;
- AIC;
- BIC;
- likelihood-ratio tests.

Stage 4.5 owns modeling.

The auxiliary linear regressions used solely for VIF remain allowed.

---

# 40. No Performance Testing

Do not calculate:

- AUC;
- KS;
- ROC;
- PR-AUC;
- accuracy;
- precision;
- recall;
- F1;
- calibration;
- confusion matrix;
- threshold metrics.

Do not choose the 10 predictors based on performance.

---

# 41. No CV

Do not run:

- KFold;
- StratifiedKFold;
- bootstrap;
- repeated CV;
- fold-specific WOE;
- fold-specific selection;
- fold-specific VIF.

Stage 4.6 owns model stability work.

---

# 42. No New Feature Engineering

Do not create:

- payment averages;
- PAY\_AMT summaries;
- payment ratios;
- bill/payment ratios;
- new delinquency summaries;
- utilization trends;
- interactions;
- slopes;
- principal components;
- combined risk scores.

PAY\_AMT1–6 remain six separate predictors.

No attempt to solve the six-month representation question by engineering a new summary.

---

# 43. Notebook Stage 4.4B Structure

Extend:

`notebooks/07_woe_iv_variable_screening.ipynb`

Append a distinct section:

`Stage 4.4B — Human-Approved Final Candidate Freeze`

Recommended appended structure:

1. Stage 4.4A Reviewer handoff
2. documentation-fix closure note
3. Human Checkpoint decision summary
4. 29-row Keep/Drop decision record
5. exact frozen 10-variable list
6. exact 19-variable exclusion record
7. decision rationales by family
8. construct 24,000×10 WOE matrix
9. reduced-matrix integrity checks
10. SCR-07 final correlation heatmap
11. reduced high-correlation pair table
12. reduced VIF table
13. SCR-08 VIF diagnostic
14. reduced-set validation
15. final Candidate Logistic Predictor List freeze
16. explicit Stage 4.5 handoff boundary

Do not alter Stage 4.4A cell source content.

---

# 44. Task Report Stage 4.4B Structure

Extend:

`task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

Append:

- Stage 4.4A Reviewer handoff;
- documentation fix closure;
- Human Checkpoint decision record;
- exact 10 Keep variables;
- exact 19 Drop variables;
- family-level rationales;
- explicit statement that decisions are Human-approved, not automated;
- 29-row final disposition table;
- reduced WOE matrix integrity;
- reduced 10×10 WOE correlation;
- reduced correlation flags;
- reduced VIF;
- SCR-07;
- SCR-08;
- final validation result;
- final Candidate Logistic Predictor List;
- Test sealing;
- no modeling confirmation;
- no performance-metric confirmation;
- no CV confirmation;
- Notebook clean-run;
- protected-file integrity;
- Git status;
- final Reviewer handoff.

Before final Reviewer:

Status:

`Stage 4.4B implementation complete — awaiting final Stage 4.4 Reviewer validation.`

---

# 45. Immutable Stage 4.4B Architect Specification

Append this entire Specification verbatim to the Task Report between:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.4B ================`

and

`================ END ARCHITECT SPECIFICATION — STAGE 4.4B ================`

Do not alter the existing Stage 4.4A immutable specification.

Do not alter the Stage 4.4A Reviewer Result.

---

# 46. Stage 4.4 Final Reviewer Workflow

After Codex completion:

> send the complete Stage 4.4 artifact pair to Reviewer.

Reviewer validates Stage 4.4A + 4.4B as one final Stage 4.4 milestone.

## FAIL

If requirements fail:

- no commit;
- return Required Fixes;
- no Stage 4.5.

## PASS

If all final Stage 4.4 requirements pass:

In the same Reviewer round:

1. append final Stage 4.4 Reviewer Result;
2. stage exactly the two Stage 4.4 artifacts;
3. commit the Stage 4.4 milestone;
4. verify post-commit Git state.

Recommended commit message:

`Finalize WOE variable screening`

Expected commit scope only:

- `notebooks/07_woe_iv_variable_screening.ipynb`
- `task_records/Stage_4.4_WOE_IV_Variable_Screening.md`

No push.

---

# 47. Reviewer Must Verify Documentation Fix

Final Reviewer must explicitly verify:

- old incorrect `<5%` narrative no longer remains as the operative sparse-bin definition;
- frozen rule is `<3%`;
- 14/29 original Phase A sparse flags remain unchanged;
- Phase A Reviewer Result remains intact;
- no analytical result was altered by the documentation fix.

---

# 48. Reviewer Must Verify Human Decisions

Reviewer must verify exact final candidate list:

1. PAY\_0
2. severe\_delinquent\_months
3. PAY\_AMT1
4. PAY\_AMT2
5. PAY\_AMT3
6. PAY\_AMT4
7. PAY\_AMT5
8. PAY\_AMT6
9. LIMIT\_BAL
10. avg\_utilization

And exact excluded count:

> 19.

No additional variable may enter the Stage 4.5 candidate set.

---

# 49. Reviewer Must Verify Rationales Are Accurate

In particular:

- PAY\_2–PAY\_6 must not be described as weak;
- delinquent\_months/max\_delinquency must not be described as weak;
- BILL must not be described as highly redundant with utilization;
- PAY\_AMT1–6 must not be described as highly collinear;
- avg\_utilization must not be described as selected by automatic IV maximization;
- demographics must not be described as legally prohibited;
- sparse-bin flag must not be treated as automatic deletion.

These wording boundaries are important.

---

# 50. Protected Project Scope

Do not modify any tracked Stage 0–4.3 file.

Do not modify:

- raw CSV;
- README;
- requirements;
- references;
- prior notebooks;
- prior Task Reports.

Only:

- Stage 4.4 Notebook;
- Stage 4.4 Task Report

may change.

---

# 51. No External Artifacts

Do not create:

- reduced dataset CSV;
- WOE CSV;
- final feature-list CSV;
- correlation CSV;
- VIF CSV;
- pickle/joblib;
- model;
- scorecard;
- HTML;
- PDF;
- external PNG;
- `src/`;
- `tests/`.

All results remain inside Notebook/Task Report.

---

# 52. Codex Git Rules

Codex:

- no git add;
- no commit;
- no push.

At Codex completion expected:

- HEAD remains `5b36512f317b532378977e66b90f1d379693bc4a`;
- tracked diff empty;
- staged diff empty;
- exactly the two Stage 4.4 artifacts remain untracked;
- remote none.

Commit occurs only after final Stage 4.4 Reviewer PASS.

---

# 53. Stage 4.4B Completion Criteria

All must pass:

1. correct Stage 4.3 HEAD;
2. main branch;
3. exactly two expected Stage 4.4A artifacts;
4. Stage 4.4A Notebook baseline verified;
5. Stage 4.4A report baseline verified;
6. Stage 4.4A Reviewer `PASS WITH FIXES` preserved;
7. only authorized documentation fix performed in prior Phase A narrative;
8. `<5%` corrected to `<3%`;
9. 14/29 sparse flags unchanged;
10. Stage 4.4A diagnostics preserved;
11. split replay exact;
12. Train hash exact;
13. Test hash exact;
14. Test sealed;
15. Count = 24,000;
16. Good = 18,691;
17. Bad = 5,309;
18. full 29-variable WOE baseline reproduced;
19. no re-binning;
20. no smoothing;
21. Human Checkpoint recorded as completed;
22. exact 10 Keep variables;
23. exact 19 Drop variables;
24. PAY\_0 retained;
25. severe\_delinquent\_months retained;
26. PAY\_2–PAY\_6 excluded;
27. delinquent\_months excluded;
28. max\_delinquency excluded;
29. all PAY\_AMT1–6 retained;
30. LIMIT\_BAL retained;
31. all BILL\_AMT1–6 excluded;
32. avg\_utilization retained;
33. current\_utilization excluded;
34. max\_utilization excluded;
35. AGE excluded;
36. SEX excluded;
37. EDUCATION\_GROUP excluded;
38. MARRIAGE\_GROUP excluded;
39. rationales accurately documented;
40. 29-row final disposition table created;
41. reduced WOE matrix shape = 24,000×10;
42. reduced matrix exact subset of full WOE matrix;
43. no missing/non-finite reduced values;
44. reduced 10×10 WOE correlation created;
45. SCR-07 created;
46. reduced correlation pair table created;
47. reduced VIF for all 10 created;
48. SCR-08 created;
49. same correlation thresholds used;
50. same VIF thresholds used;
51. no automatic variable removal;
52. unexpected redundancy handled according to conditional-stop rules;
53. no predictive Logistic;
54. no coefficients;
55. no p-values;
56. no Odds Ratios;
57. no AUC;
58. no KS;
59. no CV;
60. no Test risk analysis;
61. no new feature engineering;
62. no alternate subset experiments;
63. final Candidate Logistic Predictor List frozen only if validation clean;
64. Notebook clean-runs;
65. Task Report complete;
66. Stage 4.4B immutable specification preserved;
67. protected tracked files unchanged;
68. only two Stage 4.4 artifacts changed;
69. staged diff empty;
70. no commit;
71. no push;
72. final Reviewer handoff reached.

---

# 54. Codex Completion Report

Return a concise Completion Report containing:

- pre-flight result;
- branch / HEAD;
- Stage 4.4A Notebook hash verification;
- Stage 4.4A report hash verification;
- Stage 4.4A Reviewer verdict verification;
- `<5%` → `<3%` documentation fix confirmation;
- confirmation 14/29 sparse flags unchanged;
- Train/Test identity verification;
- Test sealed;
- 29-variable WOE baseline verification;
- Human Checkpoint completion recorded;
- exact 10 Keep list;
- exact 19 Drop count/list;
- reduced WOE matrix shape;
- reduced correlation maximum absolute off-diagonal value;
- count of reduced pairs with `|r| >= 0.70`;
- count with `|r| >= 0.85`;
- reduced VIF results;
- reduced VIF flag counts;
- maximum reduced VIF;
- SCR-07 status;
- SCR-08 status;
- whether any additional Architect review is required;
- final Candidate Logistic Predictor List freeze status;
- Notebook clean-run;
- protected-input integrity;
- final Git status;
- confirmation:
  - no re-binning
  - no smoothing
  - no automated selection
  - no alternate subset search
  - no predictive Logistic
  - no coefficients/p-values
  - no AUC/KS
  - no CV
  - no Test risk analysis
  - no commit
  - no push

If reduced-set validation is clean, final line:

`Stage 4.4B implementation complete — final 10-variable Candidate Logistic Predictor List frozen; awaiting final Stage 4.4 Reviewer validation and milestone commit.`

If reduced-set Review/Severe flags require Human action, final line instead:

`Stage 4.4B reduced-set validation requires Architect review — no automatic candidate-list change performed and Stage 4.4 is not closed.`

================ END ARCHITECT SPECIFICATION — STAGE 4.4B ================

## Protected-scope and Git Verification

- All 23 protected inputs matched their pre-flight size/SHA-256 values both at report construction and in the post-report final read-only verification.
- Final saved Notebook: 850,894 bytes; SHA-256 `1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce`.
- Final Notebook validation: 48 cells, 24 sequential code executions, 0 error outputs, and 8 inline PNG outputs.
- Stage 4.4A preservation checks: original 32 cell sources exact; Reviewer Result exact; immutable Stage 4.4A block exact.
- Superseded non-authoritative provenance claim: the previously recorded 31,400-byte / `590de430376d765237cdd91882fdae5af5d63b8bbe83830a14f2a19ea3043c88` fingerprint was derived from a different source representation and is not Architect-authoritative. The authoritative post-FAIL repair record appears below.
- Branch remains `main`; HEAD remains `5b36512f317b532378977e66b90f1d379693bc4a`; parent and message are unchanged.
- `git diff --name-status`, `git diff --stat`, and `git diff --cached --name-status` are empty; remote remains absent.
- `git status --short --untracked-files=all` and `git ls-files --others --exclude-standard` show exactly the same two Stage 4.4 artifacts; no third artifact exists.
- Git index SHA-256 remains `185c93ffab373f5894a4787a15122e319409f94a16abf1b60fee8665d9a35543`, exactly matching pre-flight.

## Deviations, Limitations, Unexpected Artifacts, and Incomplete Items

- Deviation from frozen analytical decisions: none.
- Two initial clean-run attempts stopped before saving because new Stage 4.4B audit assertions contained (1) an invalid assumption that canonical Keep order must equal the full-registry filtered order and (2) a misspelled Stage 4.4A ID snapshot variable name. Only the new Stage 4.4B cells were corrected; no predictor, result, Stage 4.4A source, protected file, environment, or Git state was changed. The final clean-run completed without errors.
- Unexpected project artifacts: none.
- Incomplete by design: final Stage 4.4 Reviewer validation and the milestone commit are pending. Codex does not perform either action.

## Final Reviewer Handoff

The complete Stage 4.4A + Stage 4.4B artifact pair is ready for independent final Stage 4.4 Reviewer validation. No final Reviewer verdict is prewritten here.

`Stage 4.4B implementation complete — awaiting final Stage 4.4 Reviewer validation.`

# REVIEWER RESULT

## Verdict

FAIL — final Stage 4.4 milestone closure is blocked by an unresolved immutable-specification integrity contradiction. No commit was created.

## Requirement Check

- PASS — The Stage 4.4A wording defect was limited to the authorized operative sentence and was correctly changed from `<5%` to `<3%`. Reconstructing the pre-Stage-4.4B Task Report prefix by reversing only this correction reproduced the approved 111,136-byte baseline SHA-256 `cc2a4242310bc19bfefff22fa7331ba5dd1467fca18904c0f0819a1904b14852` exactly. The Stage 4.4A Reviewer Result and analytical evidence remain intact, and the 14/29 sparse flags remain unchanged.
- PASS — The exact Human-approved candidate decisions are recorded: 10 Keep predictors in the required canonical order and 19 Drop predictors, with complete reconciliation and accurate rationale boundaries.
- PASS — The reduced Train WOE matrix is an exact fixed-order subset of the full matrix, with shape 24,000×10, unchanged alignment/target, and no missing or non-finite values.
- PASS — Reduced-set validation is clean: maximum off-diagonal `|r| = 0.692029`, zero Review/Strong correlation pairs, all 10 VIF values are Low, and maximum VIF is `2.040957`. The frozen Stage 4.5 candidate list is therefore analytically supported under the Architect's conditional logic.
- PASS — SCR-07 and SCR-08 are present, correctly scoped, readable, and saved inline. No predictive Logistic model, performance metric, CV, Test-risk analysis, alternate subset search, new feature engineering, or external analytical artifact was created.
- PASS — The full Notebook clean-runs from raw CSV in a fresh isolated kernel: 48 cells, 24 code cells with sequential execution counts 1–24, 117 outputs, 8 inline PNGs, and zero errors.
- PASS — All 23 protected tracked inputs match the Task Report manifest. Git remains on `main` at HEAD `5b36512f317b532378977e66b90f1d379693bc4a`, with no tracked or staged diff, no remote, and exactly the two authorized untracked Stage 4.4 artifacts.
- FAIL — The Task Report's post-specification verification states that the embedded Stage 4.4B immutable copy is 31,400 bytes with SHA-256 `590de430376d765237cdd91882fdae5af5d63b8bbe83830a14f2a19ea3043c88`. Direct extraction of the current block from the `BEGIN ARCHITECT SPECIFICATION — STAGE 4.4B` marker through the `END` marker yields 24,357 UTF-8 bytes with SHA-256 `3b27b716e6153865b2c5646aa6b3b669fedaa443d42eb43ed7362aaba5a266e6`. The claimed exact-match evidence is therefore false under the stated embedded-copy scope, so completion criterion 66 and the immutable acceptance contract cannot be reliably verified.

## Independent Verification

- Read the complete embedded Stage 4.4A and Stage 4.4B specifications, all Stage 4.4A/4.4B execution records, the previous Reviewer Result, and the final Codex handoff.
- Verified the authorized Stage 4.4A documentation correction by byte-level reconstruction of the approved Task Report baseline.
- Inspected all 16 appended Stage 4.4B Notebook cells, including fixed candidate registries, 29-row disposition logic, exact-subset assertions, correlation thresholds, VIF implementation, conditional-stop logic, and Stage 4.5 boundary.
- Clean-ran the complete Notebook and independently cross-checked the reduced VIF using the diagonal of the inverse reduced correlation matrix; all ten values matched the saved NumPy auxiliary-regression VIF results to six decimals.
- Inspected the saved 29-row decision table, reduced 10×10 correlation table, empty flagged-pair table, 10-row VIF table, final freeze outputs, and all eight embedded PNG outputs; SCR-07 and SCR-08 were visually reviewed directly.
- Recomputed the 23 protected-file SHA-256 values and found zero mismatches. Rechecked the project tree, branch, HEAD, tracked/staged status, untracked scope, index state, and absence of remotes.
- Independently extracted and hashed the embedded Stage 4.4B immutable block, producing the contradictory byte count and hash stated above.

## Issues Found

1. **Immutable Stage 4.4B specification integrity is not auditable from the current record.** The Task Report claims an exact 31,400-byte immutable copy with SHA-256 `590de430…`, but the actual marked block is 24,357 bytes with SHA-256 `3b27b716…`. This is not a rounding or newline-only difference. Either the byte/hash claim references a different source than stated, or the embedded specification is not the exact copy that the report claims. Because the immutable Architect Specification is the acceptance contract, this contradiction blocks reliable final acceptance even though the analytical implementation passes.

## Required Fixes

1. Reconcile the immutable-specification evidence against the authoritative Stage 4.4B Architect Specification:
   - if `31,400 bytes / 590de430…` is the authoritative specification fingerprint, restore the exact authoritative block between the required BEGIN/END markers; or
   - if the currently embedded 24,357-byte block is authoritative and complete, correct the verification statement and document the precise hashing boundaries/encoding that produce the authoritative fingerprint.
2. Preserve the existing Stage 4.4A/4.4B analytical results, Human decisions, Notebook, prior Reviewer Result, and this final Reviewer Result. Do not redesign or rerun selection.
3. After the specification record is reconciled, re-verify the Task Report block, Git scope, and two-artifact milestone contents before requesting final Stage 4.4 review again.

No local commit is allowed while Verdict is FAIL. No push or remote operation was performed.

## Architect Decision Needed

Confirm which Stage 4.4B specification representation/fingerprint is authoritative, or provide the authoritative original specification for exact comparison. No analytical or candidate-list decision is required; the 10-variable reduced-set validation itself is clean.


# Stage 4.4B Immutable Specification Provenance Repair

## Repair Status and Authority Decision

- Status: `READY FOR FINAL STAGE 4.4 REVIEWER`.
- Reason for repair: the latest Stage 4.4 Reviewer correctly blocked milestone closure because the Task Report contained an internally inconsistent immutable-specification fingerprint/provenance claim.
- Architect authority decision: the complete Stage 4.4B block embedded in `/Users/yu/.codex/attachments/461bd74a-c701-430f-a032-f8f57297c89d/pasted-text.txt` is the sole authoritative source for this repair.
- The previously recorded `31,400 bytes / 590de430376d765237cdd91882fdae5af5d63b8bbe83830a14f2a19ea3043c88` fingerprint is explicitly non-authoritative and superseded.
- Repair branch used: Branch B — the pre-repair embedded block did not exactly match the authoritative source.
- Immutable block rewritten: Yes; only the Task Report Stage 4.4B immutable block was replaced with the authoritative source block.

## Deterministic Comparison Policy

- Encoding: UTF-8.
- Newline policy: raw file bytes with LF line endings; no newline translation, Unicode normalization, whitespace normalization, trimming, or semantic comparison.
- Hash boundary: exact standalone BEGIN marker through exact standalone END marker, inclusive.
- The boundary begins at the first byte of the unique standalone BEGIN marker and ends immediately after the final byte of the unique standalone END marker. It excludes the line terminator or any Task Report text following the END marker.
- Marker discovery required a whole standalone line. Backticked or inline textual mentions of marker text were not treated as boundaries.

## Pre-repair Evidence

- Authoritative source markers: one standalone BEGIN and one standalone END, correctly ordered.
- Pre-repair Task Report markers: one standalone BEGIN and one standalone END, correctly ordered.
- Authoritative source block: 31,518 UTF-8 bytes; SHA-256 `50c879b875631213e1644cdf9ffb7692bcc01275543fedb1885e74b4d3808ed7`.
- Pre-repair embedded block: 31,400 UTF-8 bytes; SHA-256 `590de430376d765237cdd91882fdae5af5d63b8bbe83830a14f2a19ea3043c88`.
- Exact pre-repair source match: No.
- First deterministic mismatch: 0-based byte offset 167. The authoritative source contains byte `0x2d` (`-`) and the pre-repair report contains byte `0x2a` (`*`) at the first list marker. Additional differences include Markdown escaping and formula/list representation; source length exceeded the pre-repair block by 118 bytes.
- Reviewer cross-check reproduction: searching for the first textual END-marker byte sequence without requiring a standalone line prematurely stops at the backticked marker mention inside specification Section 45. Applied to the pre-repair report, that non-authoritative boundary reproduces 24,357 bytes and SHA-256 `3b27b716e6153865b2c5646aa6b3b669fedaa443d42eb43ed7362aaba5a266e6`.

## Restoration and Final Provenance

- Restoration action: the complete pre-repair Stage 4.4B embedded block, including its standalone markers, was replaced with the authoritative source block without modifying the source.
- Final encoding: UTF-8.
- Final hash boundary: exact standalone BEGIN marker through exact standalone END marker, inclusive.
- Final authoritative byte count: 31,518.
- Final authoritative SHA-256: `50c879b875631213e1644cdf9ffb7692bcc01275543fedb1885e74b4d3808ed7`.
- Agreement with Reviewer cross-check values: No under the authoritative standalone-marker policy. The Reviewer values are preserved as useful evidence of the earlier ambiguous non-standalone extraction, but they are not the Architect-authoritative fingerprint.
- Final byte-for-byte equality: PASS — the repaired embedded block is exactly equal to the authoritative source block.
- The earlier operative provenance line was relabeled as a superseded non-authoritative claim; the latest Reviewer FAIL remains unchanged and preserves the original contradiction for auditability.

## Preservation and Scope Verification

- Latest Stage 4.4 Reviewer FAIL: preserved byte-for-byte.
- Stage 4.4A Reviewer Result and immutable Stage 4.4A specification: preserved.
- Notebook preservation baseline: 850,894 bytes; SHA-256 `1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce`.
- Notebook modified: No.
- Analytical rerun: No. The Notebook was not executed.
- WOE, IV, reduced-matrix, correlation, and VIF recomputation: none.
- Analytical results preserved: strict `<3%`, 14/29 sparse flags, 24,000×10 reduced matrix, maximum reduced `|r| = 0.692029`, zero reduced `|r| >= 0.70` and `|r| >= 0.85` pairs, ten Low VIF flags, maximum reduced VIF `2.040957`, and SCR-07/SCR-08 records.
- Human-approved decisions preserved: Keep = 10 and Drop = 19; the frozen candidate list is unchanged.
- Predictive Logistic, AUC/KS, CV/bootstrap, Test analysis, new feature engineering, and alternate subset work: none.
- Files modified in this repair: `task_records/Stage_4.4_WOE_IV_Variable_Screening.md` only.
- Files created: none.
- Git writes: no add, commit, push, reset, restore, stash, clean, checkout, amend, or rebase.

## Final Reviewer Revalidation Handoff

The documentation/provenance repair is complete and ready for final Stage 4.4 Reviewer revalidation. This section does not append or claim Reviewer PASS, and no milestone commit has been created.

`Stage 4.4 documentation/provenance repair complete — authoritative Stage 4.4B immutable block and derived fingerprint are internally consistent; awaiting final Stage 4.4 Reviewer revalidation.`

# REVIEWER RESULT

## Verdict

PASS — final Stage 4.4 milestone accepted after independent revalidation of the documentation/provenance repair.

## Requirement Check

- PASS — The Architect-designated authoritative source contains exactly one standalone Stage 4.4B BEGIN marker and one standalone END marker in valid order. The repaired Task Report also contains exactly one such standalone pair.
- PASS — Using raw LF file bytes, UTF-8 encoding, and the exact standalone BEGIN marker through exact standalone END marker inclusive, both the authoritative source block and embedded Task Report block are 31,518 bytes with SHA-256 `50c879b875631213e1644cdf9ffb7692bcc01275543fedb1885e74b4d3808ed7`; byte-for-byte equality is exact.
- PASS — The earlier `31,400 / 590de430…` fingerprint is clearly marked non-authoritative and superseded. The prior Reviewer `24,357 / 3b27b716…` cross-check is retained for auditability and correctly explained as a premature match to the inline/backticked END-marker mention rather than the unique standalone END-marker line.
- PASS — The Stage 4.4A Reviewer Result and the latest Stage 4.4 Reviewer FAIL remain present and auditable. No fabricated PASS was inserted by Codex; this final PASS is the independent revalidation result.
- PASS — The Stage 4.4 Notebook remains exactly 850,894 bytes with SHA-256 `1c1690e012ac54289f36824b4af4883a5a4ddff9e6745be294094da2c47ffdce`; it was not modified or rerun during the provenance repair.
- PASS — The previously independently validated analytical state is unchanged: strict `<3%`, 14/29 Phase A sparse flags, exact Human-approved Keep 10 / Drop 19 decisions, 24,000×10 reduced WOE matrix, maximum reduced `|r| = 0.692029`, zero reduced Review/Strong pairs, ten Low VIF flags, maximum VIF `2.040957`, and SCR-07/SCR-08.
- PASS — All 23 protected tracked inputs match their recorded bytes and SHA-256 values. No new project artifact, analytical rerun, alternative selection, predictive Logistic model, performance analysis, CV/bootstrap, or Test-risk analysis was introduced.
- PASS — Pre-commit Git state is `main` at `5b36512f317b532378977e66b90f1d379693bc4a`, with empty tracked/staged diffs, no remote, and exactly the two authorized untracked Stage 4.4 artifacts.

## Independent Verification

- Read the complete provenance-repair authority prompt and its embedded authoritative Stage 4.4B specification from `/Users/yu/.codex/attachments/461bd74a-c701-430f-a032-f8f57297c89d/pasted-text.txt`.
- Located markers using whole standalone-line matching rather than unrestricted substring search; independently extracted and hashed the authoritative and Task Report blocks and confirmed exact byte equality.
- Rechecked the repair audit trail, superseded-claim labeling, deterministic comparison policy, first-difference explanation, prior Reviewer history, Notebook preservation statement, analytical preservation statements, and final revalidation handoff.
- Recomputed the current Notebook fingerprint and every protected-file fingerprint; confirmed zero mismatches.
- Rechecked the project tree, branch, HEAD, tracked/staged/untracked scope, and absence of remotes immediately before closeout.
- Relied on the previous final Stage 4.4 independent analytical review for the unchanged Notebook; the unchanged Notebook hash proves that no analytical artifact drift occurred during the documentation-only repair.

## Issues Found

No remaining issue affecting Stage 4.4 acceptance. The prior immutable-specification provenance conflict is resolved with an Architect-authoritative source, deterministic standalone-marker boundaries, and exact byte/hash equality.

## Required Fixes

None.

## Architect Decision Needed

None.
