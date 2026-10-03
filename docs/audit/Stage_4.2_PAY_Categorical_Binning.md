# Stage 4.2 — PAY + Categorical Binning

## 1. Phase A Status and Task Identification

- Phase: `Stage 4.2A — PAY + Categorical Raw Risk Diagnostics`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Status: `Stage 4.2A complete — final grouping pending Architect/user Human Checkpoint.`
- Purpose: Train-only raw-state/category evidence for later Human decisions
- Holdout Test: sealed after technical split assertions
- Stage 4.2B: not started
- Final grouping: not performed

## 2. Immutable Architect Specification — Stage 4.2A

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.2A ================

# Stage 4.2A — PAY + Categorical Raw Risk Diagnostics

## 1. Stage Context

Stage 4 固定结构：

- Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
- Stage 4.2 — PAY + Categorical Binning
- Stage 4.3 — Continuous Variable Supervised Binning
- Stage 4.4 — WOE / IV + Variable Screening
- Stage 4.5 — Candidate WOE Logistic Regression
- Stage 4.6 — Stratified 5-Fold Model Stability Test
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit

Stage 4.2 内部分为：

- **Stage 4.2A — Raw Risk Diagnostics**
- **Stage 4.2B — Final Binning Implementation**

当前只授权：

> **Stage 4.2A**

Stage 4.2A 的目的不是完成分箱，而是：

> 把 PAY、delinquency summaries 和现有 categorical variables 的原始 Train risk structure 完整展示给 Architect/user，用于 Human Checkpoint 决定 Stage 4.2B 的最终 grouping。

---

# 2. Git Baseline

Project root：

`/Users/yu/2.工作/Risk Analysis Project`

Stage 4.1 正式 milestone：

- Branch: `main`
- Commit:
  `9d22109b9db4abfafbb653e0cb7b9068bda6a8d3`
- Message:
  `Define WOE and binning methodology`
- Parent:
  `8fbe2fa4b3715719e652101f694e5673abebba03`

Pre-flight 必须验证：

1. repository root 正确；
2. branch = `main`；
3. HEAD 精确等于上述 Stage 4.1 commit；
4. working tree clean；
5. staged diff empty；
6. no remote；
7. Stage 4.1 Notebook 与 Task Report 存在；
8. Stage 4.1 Task Report Reviewer verdict = PASS；
9. Stage 0–4.1 protected files 无修改。

Stage 4.1 approved artifact hashes：

Notebook:

`97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9`

Task Report:

`2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851`

任何 baseline mismatch：

> BLOCKED。

不得自行 restore/reset/stash/clean。

---

# 3. Data Boundary

正式数据：

`data/raw/UCI_Credit_Card.csv`

必须 deterministic replay Stage 3 split：

- Train = 80%
- Holdout Test = 20%
- `random_state=42`
- `stratify=default.payment.next.month`

必须复现：

Train rows:

`24,000`

Test rows:

`6,000`

Train ID-set SHA-256:

`bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`

Test ID-set SHA-256:

`d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

Test 只能用于：

- row-count assertion；
- ID overlap/coverage；
- frozen ID-set hash verification。

之后立即 seal。

Stage 4.2A 严禁：

- Test default rate；
- Test WOE；
- Test IV；
- Test PAY analysis；
- Test categorical analysis；
- Train/Test risk comparison。

---

# 4. Reproduce Frozen Stage 3 Feature Engineering on Train

Stage 4.2A 需要以下 Stage 3 derived variables：

- `EDUCATION_GROUP`
- `MARRIAGE_GROUP`
- `delinquent_months`
- `max_delinquency`
- `severe_delinquent_months`

必须使用 Stage 3.3 已冻结的 exact deterministic definitions。

不得修改 Feature Engineering。

## EDUCATION_GROUP

- 1 → Graduate School
- 2 → University
- 3 → High School
- 0/4/5/6 → Other / Unknown

## MARRIAGE_GROUP

- 1 → Married
- 2 → Single
- 0/3 → Other / Unknown

## delinquent_months

Six PAY variables:

`PAY_0, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6`

\[
\sum_i 1(PAY_i>0)
\]

## max_delinquency

\[
\max_i\max(PAY_i,0)
\]

## severe_delinquent_months

\[
\sum_i 1(PAY_i\ge2)
\]

Raw PAY must remain unchanged.

---

# 5. Variables in Scope

Exactly 12 variables are in Stage 4.2A scope.

## Raw PAY variables — 6

- `PAY_0`
- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`

## Delinquency summaries — 3

- `delinquent_months`
- `max_delinquency`
- `severe_delinquent_months`

## Existing categorical variables — 3

- `SEX`
- `EDUCATION_GROUP`
- `MARRIAGE_GROUP`

No other predictor receives WOE/IV analysis in this phase.

In particular, do not analyze:

- AGE
- LIMIT_BAL
- BILL_AMT1–6
- PAY_AMT1–6
- utilization variables

Those belong to Stage 4.3.

---

# 6. Good / Bad and WOE Convention

Frozen Stage 4.1 convention:

- `0 = Good / Non-default`
- `1 = Bad / Default`

For each raw state/category \(j\):

\[
DistGood_j=\frac{Good_j}{TotalGood}
\]

\[
DistBad_j=\frac{Bad_j}{TotalBad}
\]

\[
WOE_j=
\ln
\left(
\frac{DistGood_j}{DistBad_j}
\right)
\]

Therefore:

- positive WOE = relatively lower risk；
- negative WOE = relatively higher risk。

IV contribution:

\[
IV_j=(DistGood_j-DistBad_j)\times WOE_j
\]

Raw-state IV contributions may be displayed.

Total raw-state IV may also be displayed as **diagnostic evidence only**.

It must not be used for variable selection in Stage 4.2A.

---

# 7. PAY Raw-state Diagnostics

For each of the six PAY variables:

> Do not merge anything.

For every observed raw PAY code, produce a table containing:

- Raw PAY code
- Count
- % of Train
- Good count
- Bad count
- Default Rate
- Dist Good
- Dist Bad
- WOE
- IV Contribution

Sort by numerical PAY code.

Include all observed raw codes, including:

- `-2`
- `-1`
- `0`
- `1`
- positive higher-severity codes

Do not invent semantic definitions for `-2` or `0`.

---

# 8. PAY Diagnostic Figures

For each PAY variable, create a compact diagnostic figure that makes two things visible together:

1. Default Rate by raw PAY code
2. Sample support by raw PAY code

The figure should allow the user to distinguish:

> genuine risk change

from

> unstable rate based on tiny sample size.

Preferred simple design:

- main axis / panel: Default Rate by raw code
- secondary panel or clearly aligned annotation: Count / % Train

Do not produce unnecessary decorative charts.

Each figure must include concise explanatory text:

- Question
- What to inspect
- Observed factual pattern

The “Observed” section may describe facts but **must not make final grouping decisions**.

---

# 9. PAY Candidate Observations — Allowed Boundary

Codex may write evidence-based observations such as:

> `PAY=-2/-1/0 show similar default rates and WOE direction and may be candidate adjacent states for later review.`

or:

> `PAY=2 shows a visible risk jump relative to neighboring lower states.`

or:

> `High PAY codes have small sample support and may require tail consolidation.`

But Codex must not write:

> `Final bin = [-2,-1,0]`

or:

> `PAY_0 should definitely use 4 bins.`

Final grouping belongs exclusively to the Human Checkpoint.

---

# 10. PAY=1 Special Attention

For every PAY variable explicitly report:

- whether code `1` exists；
- count；
- % Train；
- default rate；
- WOE。

This is required because Stage 3 found code `1` frequencies highly asymmetric across months.

Do not force identical treatment across six PAY variables.

---

# 11. PAY=2 Risk Boundary Attention

For every PAY variable explicitly make it easy to compare:

- state 0 / 1
- state 2
- state 3+

because Stage 3 EDA suggested an important risk increase around positive delinquency severity 2.

However:

> Stage 4.2A must independently report actual Train evidence.

Do not assume the Stage 3 observation guarantees the Stage 4.2 final grouping.

---

# 12. High-severity PAY Tail

Explicitly report sample support for:

- 3
- 4
- 5
- 6
- 7
- 8

where observed.

The purpose is to allow later consideration of contiguous tail bins such as `3+` or `4+`.

Do not create those bins yet.

Small high-severity states are allowed to remain visible in raw diagnostics even when below 3% of Train.

---

# 13. delinquent_months Diagnostics

Observed values should theoretically be:

`0–6`

For every observed value show:

- Value
- Count
- % Train
- Good
- Bad
- Default Rate
- Dist Good
- Dist Bad
- WOE
- IV Contribution

Sort ascending.

Create one simple figure:

> Default Rate + sample support by delinquent_months.

Do not merge values.

---

# 14. severe_delinquent_months Diagnostics

Same structure.

Observed values theoretically:

`0–6`

Show full raw-value table and one diagnostic figure.

Do not merge values.

---

# 15. max_delinquency Diagnostics

Show every observed raw value in ascending order.

Expected range based on Stage 3 evidence may include:

`0–8`

but use actual Train values.

Table and diagnostic figure should expose:

- risk progression；
- small high-severity states；
- possible tail sparsity。

Do not merge.

---

# 16. Ordinal Integrity Principle

For PAY and three delinquency summaries:

future Stage 4.2B grouping may only merge contiguous / ordinal-coherent states.

Stage 4.2A must preserve this ordering in tables and figures.

Do not propose noncontiguous groups such as:

- `0 + 4`
- `1 + 6`
- `-2 + 3`

---

# 17. SEX Diagnostics

For each existing SEX category show:

- Category/code
- Count
- % Train
- Good
- Bad
- Default Rate
- Dist Good
- Dist Bad
- WOE
- IV Contribution

No merging.

Because SEX has two categories:

> Stage 4.2A should not propose merging them.

If merged, the variable would cease to contain discrimination.

Whether SEX is retained later belongs to Stage 4.4.

---

# 18. EDUCATION_GROUP Diagnostics

Categories:

- Graduate School
- University
- High School
- Other / Unknown

Show full WOE/IV table.

Explicitly flag:

`Other / Unknown`

as a **sparse special category** if its Train share remains small.

Do not automatically merge it into another education level.

No final grouping decision in 4.2A.

---

# 19. MARRIAGE_GROUP Diagnostics

Categories:

- Married
- Single
- Other / Unknown

Show full WOE/IV table.

Explicitly flag:

`Other / Unknown`

as a sparse special category when appropriate.

Do not automatically merge it into Married or Single.

---

# 20. Categorical Merge Boundary

Stage 4.2A must document:

> Categorical variables are not governed by the same automatic adjacency logic as ordinal PAY variables.

Similar WOE alone is not sufficient reason to merge semantically distinct categories.

No categorical merging occurs in this phase.

---

# 21. Zero Good / Zero Bad Handling

This is a diagnostic phase.

If a raw state/category has:

- Good = 0
  or
- Bad = 0

do not silently smooth immediately.

First:

1. visibly flag the zero-event issue；
2. report raw counts；
3. report raw WOE as infinite/undefined where mathematically appropriate；
4. mark it for Human Checkpoint。

Stage 4.1 policy remains:

> merge-first, 0.5 smoothing only as fallback.

Because no merging occurs in 4.2A:

> do not finalize smoothing decisions in this phase.

If code requires a finite plotting representation, keep plotting treatment separate from formal table values and clearly label it.

---

# 22. No Automatic Binning Algorithm

Stage 4.2A must not:

- run optimal binning；
- run decision-tree binning；
- run ChiMerge to produce final groups；
- run automated monotonic binning；
- search bin combinations to maximize IV；
- optimize cut/grouping based on AUC。

This is an evidence-generation phase for Human Review.

---

# 23. No Variable Selection

Do not:

- rank the 12 variables from best to worst as a selection recommendation；
- apply IV cutoff；
- drop low-IV variables；
- compare predictors to choose winners；
- remove raw PAY because a summary appears stronger；
- remove summaries because raw PAY appears stronger。

Stage 4.4 handles variable screening.

Raw IV totals may be displayed per variable because they naturally arise from WOE tables, but must be labelled:

> diagnostic only — no selection decision.

---

# 24. No Continuous Binning

Do not analyze or bin:

- AGE
- LIMIT_BAL
- BILL_AMT
- PAY_AMT
- utilization

Stage 4.3 has not started.

---

# 25. No Modeling

Forbidden:

- Logistic Regression
- VIF
- coefficient estimation
- Odds Ratio
- KFold
- CV
- ROC
- AUC
- KS
- calibration
- confusion matrix
- threshold optimization
- sampling
- class weights

---

# 26. Notebook Artifact

Create:

`notebooks/05_pay_categorical_binning.ipynb`

This Notebook will later be extended in Stage 4.2B.

Recommended Phase A structure:

1. Stage 4.2A objective / boundaries
2. imports
3. raw data load
4. deterministic Train/Test split replay
5. split identity validation
6. Holdout Test sealing
7. reproduce frozen Stage 3 Train features
8. verify target / row / predictor integrity
9. WOE / IV helper definitions
10. PAY_0 raw diagnostics
11. PAY_2 raw diagnostics
12. PAY_3 raw diagnostics
13. PAY_4 raw diagnostics
14. PAY_5 raw diagnostics
15. PAY_6 raw diagnostics
16. delinquent_months diagnostics
17. max_delinquency diagnostics
18. severe_delinquent_months diagnostics
19. SEX diagnostics
20. EDUCATION_GROUP diagnostics
21. MARRIAGE_GROUP diagnostics
22. cross-variable factual summary
23. candidate observations only
24. explicit Human Checkpoint

Notebook must:

- clean-run from raw CSV；
- contain saved outputs；
- contain no cell errors；
- not execute earlier notebooks；
- not read Test target relationships；
- not implement final bins。

---

# 27. Diagnostic Figure Numbering

Use clear numbering beginning:

- `BIN-01`
- `BIN-02`
- ...

At minimum produce one diagnostic figure for each:

- six PAY variables；
- three delinquency summaries。

Categorical figures are optional if tables communicate the evidence sufficiently.

Each figure should have concise:

- Question
- Why it matters
- Observed
- Human decision needed

Do not write final decisions.

---

# 28. Task Report Artifact

Create:

`task_records/Stage_4.2_PAY_Categorical_Binning.md`

This report will later be extended by Stage 4.2B.

For Phase A it must record:

- status:
  `Stage 4.2A complete — final grouping pending Architect/user Human Checkpoint.`
- Git pre-flight；
- Stage 4.1 baseline；
- split replay；
- Train/Test ID hashes；
- Holdout Test protection；
- Stage 3 frozen feature reproduction；
- 12 variables in scope；
- WOE convention；
- each PAY raw-state diagnostic table；
- PAY=1 evidence by month；
- high-severity tail evidence；
- three delinquency raw-value tables；
- three categorical tables；
- sparse categorical flags；
- any zero-good / zero-bad states；
- candidate observations；
- explicit statement no final grouping performed；
- explicit statement no variable selection；
- explicit statement no continuous binning；
- explicit statement no modeling；
- Notebook clean-run；
- files created/modified；
- protected-input integrity；
- Git status；
- Human Checkpoint status。

---

# 29. Immutable Architect Specification

Task Report must preserve this full Phase A Architect Specification using:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.2A ================`

and

`================ END ARCHITECT SPECIFICATION — STAGE 4.2A ================`

Do not paraphrase.

---

# 30. Human Checkpoint — Mandatory Stop

When Phase A diagnostics are complete:

> STOP.

Do not continue to final binning.

The user will provide the Notebook and Task Report to Architect.

Architect + user will review, variable by variable:

### PAY

- whether `-2/-1/0` should merge；
- how PAY=1 behaves；
- whether PAY=2 deserves a separate boundary；
- where high-severity tail should merge；
- whether different months need different bins。

### Delinquency summaries

- which adjacent integer values should merge。

### Categorical variables

- whether sparse special categories should remain separate。

Only after explicit user approval will Architect generate:

> **Stage 4.2B — Final Binning Implementation**

---

# 31. Git Rules

Phase A Codex:

- no git add
- no commit
- no push
- no remote
- no reset
- no restore
- no stash
- no rebase
- no checkout
- no clean

Expected working tree after Phase A:

only the two authorized Stage 4.2 artifacts should be new/modified:

- `notebooks/05_pay_categorical_binning.ipynb`
- `task_records/Stage_4.2_PAY_Categorical_Binning.md`

No Stage 4.2 milestone commit yet.

Commit occurs only after:

- Stage 4.2B complete；
- final Reviewer PASS。

---

# 32. Protected Project Scope

Do not modify:

- raw CSV
- Stage 0–4.1 notebooks
- Stage 0–4.1 reports
- Luca reference
- README
- requirements
- Stage 3 artifacts
- unrelated files

Do not create:

- processed CSV
- WOE mapping CSV
- final bin definitions file
- model file
- feature-ranking file
- `src/`
- `tests/`
- Stage 4.3 artifact

---

# 33. Completion Criteria

All must pass:

1. correct Stage 4.1 Git baseline；
2. clean authorized pre-flight；
3. Train/Test split reproduced；
4. Train/Test hashes exact；
5. Test sealed；
6. frozen Stage 3 category/summary features reproduced correctly；
7. exactly 12 variables analyzed；
8. all six PAY raw-code tables generated；
9. PAY raw code ordering preserved；
10. PAY=1 explicitly reported by month；
11. PAY high-severity sample support reported；
12. no PAY codes merged；
13. three delinquency raw-value tables generated；
14. three delinquency figures generated；
15. no delinquency values merged；
16. SEX table generated；
17. EDUCATION_GROUP table generated；
18. MARRIAGE_GROUP table generated；
19. sparse special categories flagged；
20. Good/Bad convention correct；
21. WOE sign convention correct；
22. IV contribution correct；
23. any zero-event states explicitly flagged；
24. no silent smoothing；
25. diagnostic figures show risk + sample support；
26. candidate observations remain non-binding；
27. no final bins；
28. no variable selection；
29. no continuous binning；
30. no Logistic；
31. no KFold；
32. no AUC/KS；
33. no Test target analysis；
34. Notebook clean-runs；
35. Task Report complete；
36. immutable specification preserved；
37. protected files unchanged；
38. only two authorized artifacts changed；
39. no Git staging；
40. no commit；
41. no push；
42. stopped at Human Checkpoint。

---

# 34. Codex Completion Report

Return a concise report including:

- pre-flight result
- branch / HEAD
- files created
- split/hash verification
- Holdout Test sealing
- 12 variables analyzed
- six PAY raw-code diagnostics completed
- PAY=1 month-by-month evidence captured
- high-severity tail evidence captured
- three delinquency diagnostics completed
- three categorical diagnostics completed
- zero-event issues, if any
- candidate observations generated
- confirmation:
  - no state/value merging
  - no final binning
  - no variable selection
  - no continuous binning
  - no modeling
  - no KFold
  - no Test target use
- Notebook clean-run status
- protected-input integrity
- Git status
- no commit
- no push

Final line:

`Stage 4.2A complete — awaiting Architect/user Human Checkpoint for final PAY, delinquency, and categorical grouping decisions.`

================ END ARCHITECT SPECIFICATION — STAGE 4.2A ================

## 3. Git Pre-flight

All required gates passed before the first write.

| Check | Actual |
|---|---|
| Repository root | `/Users/yu/2.工作/Risk Analysis Project` |
| Branch | `main` |
| HEAD | `9d22109b9db4abfafbb653e0cb7b9068bda6a8d3` |
| Parent | `8fbe2fa4b3715719e652101f694e5673abebba03` |
| Message | `Define WOE and binning methodology` |
| Working tree | Clean |
| Tracked unstaged diff | Empty |
| Staged diff | Empty |
| Untracked files | None |
| Remote | None |
| Output paths before work | Both absent |

Stage 4.1 milestone scope was exactly the Stage 4.1 Notebook and Task Report. The Stage 4.1 Task Report contains Reviewer verdict `PASS`.

Approved Stage 4.1 hashes matched exactly:

- Notebook: `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9`
- Task Report: `2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851`

No reset, restore, stash, checkout, clean, amend, rebase, Git index/history write, or remote operation was performed.

## 4. Protected-input Baseline

| Protected input | Size (bytes) | SHA-256 |
|:---|---:|:---|
| .gitignore | 534 | `300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1` |
| README.md | 2,245 | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` |
| data/README.md | 1,004 | `c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0` |
| Raw CSV | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |
| Stage 0.1 Notebook placeholder | 1,057 | `ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20` |
| Stage 2 Notebook | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` |
| Stage 3.1 Notebook | 937,648 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` |
| Stage 3.3 Notebook | 142,371 | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` |
| Stage 4.1 Notebook | 52,998 | `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9` |
| references/README.md | 1,109 | `a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c` |
| Luca reference Notebook | 805,686 | `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd` |
| reports/figures/.gitkeep | 60 | `73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63` |
| requirements.txt | 95 | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` |
| Task 0.1 Report | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` |
| Stage 1 Report | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` |
| Stage 2 Report | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` |
| Stage 3.1 Report | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` |
| Stage 3.3 Report | 77,812 | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` |
| Stage 4.1 Report | 50,459 | `2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851` |

Pre-execution Git index SHA-256: `e1c8c3b1b2122ac163d31a522a064d51cb4abfc633968317acb01e7951e72f0a`.

## 5. Split Replay and Holdout Test Sealing

The Notebook independently loaded the formal raw CSV and replayed the locked split:

| Check | Actual |
|---|---|
| Method | `train_test_split` |
| Train/Test | 80% / 20% |
| random_state | 42 |
| Stratification field | `default.payment.next.month` |
| Raw rows | 30,000 |
| Train rows | 24,000 |
| Test rows | 6,000 |
| ID overlap | 0 |
| ID union coverage | 30,000 |
| Train ID SHA-256 | `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` |
| Test ID SHA-256 | `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f` |

After these technical row/ID/hash assertions, the Test frame and full raw frame were deleted from live Notebook state. No Test target count, rate, distribution, WOE, IV, PAY/category diagnostic, predictor-target relationship, or Train/Test risk comparison was computed.

Saved status:

`SEALED — technical row/ID/hash assertions completed; Test frame deleted`

## 6. Frozen Stage 3 Train Feature Reproduction

Only the required frozen fields were replayed on Train:

- EDUCATION_GROUP: exact 1/2/3 and 0/4/5/6 mapping;
- MARRIAGE_GROUP: exact 1/2 and 0/3 mapping;
- delinquent_months = count of six PAY values >0;
- max_delinquency = maximum after calculation-only flooring at zero;
- severe_delinquent_months = count of six PAY values ≥2.

All assertions passed:

- 24,000 rows preserved;
- original index/ID alignment and unique IDs preserved;
- target unchanged;
- six raw PAY columns unchanged exactly;
- mappings contain no missing value and match frozen category sets;
- three delinquency formulas independently recalculated exactly;
- theoretical ranges and cross-feature logic valid;
- no Test transformation;
- no utilization or other Stage 4.3 feature reproduced.

Train totals used in every WOE table: Good = 18,691, Bad = 5,309.

## 7. Fixed Diagnostic Scope and Convention

Exactly these 12 variables were analyzed, in this fixed order:

1. PAY_0
2. PAY_2
3. PAY_3
4. PAY_4
5. PAY_5
6. PAY_6
7. delinquent_months
8. max_delinquency
9. severe_delinquent_months
10. SEX
11. EDUCATION_GROUP
12. MARRIAGE_GROUP

No AGE, LIMIT_BAL, BILL_AMT, PAY_AMT, or utilization diagnostic/binning was performed.

Frozen convention:

- 0 = Good / Non-default
- 1 = Bad / Default
- WOE = ln(Dist Good / Dist Bad)
- positive WOE = relatively lower risk
- negative WOE = relatively higher risk
- IV contribution = (Dist Good − Dist Bad) × WOE

Every table reconciles Count/Good/Bad and Train percentages/distributions to the full Train totals. No merging, replacement, relabeling, or smoothing occurred.

## 8. Six PAY Raw-state Diagnostic Tables

### PAY_0

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| -2 | 2,198 | 9.1583% | 1,919 | 279 | 12.6934% | 0.102670 | 0.052552 | 0.669709 | 0.033564 | None |
| -1 | 4,527 | 18.8625% | 3,774 | 753 | 16.6335% | 0.201915 | 0.141835 | 0.353187 | 0.021220 | None |
| 0 | 11,781 | 49.0875% | 10,273 | 1,508 | 12.8003% | 0.549623 | 0.284046 | 0.660096 | 0.175306 | None |
| 1 | 3,000 | 12.5000% | 1,969 | 1,031 | 34.3667% | 0.105345 | 0.194199 | -0.611642 | 0.054347 | None |
| 2 | 2,138 | 8.9083% | 655 | 1,483 | 69.3639% | 0.035044 | 0.279337 | -2.075826 | 0.507110 | None |
| 3 | 245 | 1.0208% | 57 | 188 | 76.7347% | 0.003050 | 0.035412 | -2.452029 | 0.079352 | None |
| 4 | 58 | 0.2417% | 20 | 38 | 65.5172% | 0.001070 | 0.007158 | -1.900493 | 0.011569 | None |
| 5 | 18 | 0.0750% | 10 | 8 | 44.4444% | 0.000535 | 0.001507 | -1.035495 | 0.001006 | None |
| 6 | 10 | 0.0417% | 4 | 6 | 60.0000% | 0.000214 | 0.001130 | -1.664104 | 0.001525 | None |
| 7 | 9 | 0.0375% | 2 | 7 | 77.7778% | 0.000107 | 0.001319 | -2.511402 | 0.003043 | None |
| 8 | 16 | 0.0667% | 8 | 8 | 50.0000% | 0.000428 | 0.001507 | -1.258639 | 0.001358 | None |

Raw-state total IV: `0.889400645381` — **diagnostic only; no selection decision**.

Observed factual pattern: Default Rate ranges from 12.6934% to 77.7778%. The smallest observed raw state has 9 Train row(s). No code was merged.

### PAY_2

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| -2 | 3,030 | 12.6250% | 2,486 | 544 | 17.9538% | 0.133005 | 0.102468 | 0.260842 | 0.007966 | None |
| -1 | 4,845 | 20.1875% | 4,071 | 774 | 15.9752% | 0.217805 | 0.145790 | 0.401433 | 0.028909 | None |
| 0 | 12,573 | 52.3875% | 10,567 | 2,006 | 15.9548% | 0.565352 | 0.377849 | 0.402955 | 0.075555 | None |
| 1 | 25 | 0.1042% | 20 | 5 | 20.0000% | 0.001070 | 0.000942 | 0.127656 | 0.000016 | None |
| 2 | 3,145 | 13.1042% | 1,396 | 1,749 | 55.6121% | 0.074688 | 0.329441 | -1.484072 | 0.378071 | None |
| 3 | 254 | 1.0583% | 92 | 162 | 63.7795% | 0.004922 | 0.030514 | -1.824446 | 0.046691 | None |
| 4 | 78 | 0.3250% | 39 | 39 | 50.0000% | 0.002087 | 0.007346 | -1.258639 | 0.006620 | None |
| 5 | 21 | 0.0875% | 8 | 13 | 61.9048% | 0.000428 | 0.002449 | -1.744146 | 0.003524 | None |
| 6 | 11 | 0.0458% | 3 | 8 | 72.7273% | 0.000161 | 0.001507 | -2.239468 | 0.003015 | None |
| 7 | 17 | 0.0708% | 8 | 9 | 52.9412% | 0.000428 | 0.001695 | -1.376422 | 0.001744 | None |
| 8 | 1 | 0.0042% | 1 | 0 | 0.0000% | 0.000054 | 0.000000 | inf | inf | Bad = 0 — Human Checkpoint required |

Raw-state total IV: `inf` — **diagnostic only; no selection decision**.

Observed factual pattern: Default Rate ranges from 0.0000% to 72.7273%. The smallest observed raw state has 1 Train row(s). No code was merged.

### PAY_3

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| -2 | 3,270 | 13.6250% | 2,681 | 589 | 18.0122% | 0.143438 | 0.110944 | 0.256880 | 0.008347 | None |
| -1 | 4,733 | 19.7208% | 3,997 | 736 | 15.5504% | 0.213846 | 0.138633 | 0.433431 | 0.032600 | None |
| 0 | 12,636 | 52.6500% | 10,416 | 2,220 | 17.5689% | 0.557274 | 0.418158 | 0.287197 | 0.039954 | None |
| 1 | 3 | 0.0125% | 2 | 1 | 33.3333% | 0.000107 | 0.000188 | -0.565491 | 0.000046 | None |
| 2 | 3,049 | 12.7042% | 1,469 | 1,580 | 51.8203% | 0.078594 | 0.297608 | -1.331482 | 0.291613 | None |
| 3 | 192 | 0.8000% | 81 | 111 | 57.8125% | 0.004334 | 0.020908 | -1.573720 | 0.026083 | None |
| 4 | 60 | 0.2500% | 27 | 33 | 55.0000% | 0.001445 | 0.006216 | -1.459309 | 0.006963 | None |
| 5 | 16 | 0.0667% | 6 | 10 | 62.5000% | 0.000321 | 0.001884 | -1.769464 | 0.002765 | None |
| 6 | 19 | 0.0792% | 8 | 11 | 57.8947% | 0.000428 | 0.002072 | -1.577092 | 0.002593 | None |
| 7 | 20 | 0.0833% | 4 | 16 | 80.0000% | 0.000214 | 0.003014 | -2.644933 | 0.007405 | None |
| 8 | 2 | 0.0083% | 0 | 2 | 100.0000% | 0.000000 | 0.000377 | -inf | inf | Good = 0 — Human Checkpoint required |

Raw-state total IV: `inf` — **diagnostic only; no selection decision**.

Observed factual pattern: Default Rate ranges from 15.5504% to 100.0000%. The smallest observed raw state has 2 Train row(s). No code was merged.

### PAY_4

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| -2 | 3,477 | 14.4875% | 2,831 | 646 | 18.5792% | 0.151463 | 0.121680 | 0.218947 | 0.006521 | None |
| -1 | 4,531 | 18.8792% | 3,809 | 722 | 15.9347% | 0.203788 | 0.135995 | 0.404458 | 0.027419 | None |
| 0 | 13,180 | 54.9167% | 10,747 | 2,433 | 18.4598% | 0.574983 | 0.458278 | 0.226863 | 0.026476 | None |
| 1 | 2 | 0.0083% | 1 | 1 | 50.0000% | 0.000054 | 0.000188 | -1.258639 | 0.000170 | None |
| 2 | 2,542 | 10.5917% | 1,204 | 1,338 | 52.6357% | 0.064416 | 0.252025 | -1.364165 | 0.255929 | None |
| 3 | 143 | 0.5958% | 56 | 87 | 60.8392% | 0.002996 | 0.016387 | -1.699195 | 0.022754 | None |
| 4 | 53 | 0.2208% | 19 | 34 | 64.1509% | 0.001017 | 0.006404 | -1.840560 | 0.009916 | None |
| 5 | 28 | 0.1167% | 16 | 12 | 42.8571% | 0.000856 | 0.002260 | -0.970957 | 0.001364 | None |
| 6 | 3 | 0.0125% | 2 | 1 | 33.3333% | 0.000107 | 0.000188 | -0.565491 | 0.000046 | None |
| 7 | 40 | 0.1667% | 6 | 34 | 85.0000% | 0.000321 | 0.006404 | -2.993240 | 0.018209 | None |
| 8 | 1 | 0.0042% | 0 | 1 | 100.0000% | 0.000000 | 0.000188 | -inf | inf | Good = 0 — Human Checkpoint required |

Raw-state total IV: `inf` — **diagnostic only; no selection decision**.

Observed factual pattern: Default Rate ranges from 15.9347% to 100.0000%. The smallest observed raw state has 1 Train row(s). No code was merged.

### PAY_5

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| -2 | 3,659 | 15.2458% | 2,958 | 701 | 19.1582% | 0.158258 | 0.132040 | 0.181122 | 0.004749 | None |
| -1 | 4,417 | 18.4042% | 3,681 | 736 | 16.6629% | 0.196940 | 0.138633 | 0.351071 | 0.020470 | None |
| 0 | 13,560 | 56.5000% | 11,000 | 2,560 | 18.8791% | 0.588519 | 0.482200 | 0.199249 | 0.021184 | None |
| 2 | 2,102 | 8.7583% | 954 | 1,148 | 54.6147% | 0.051041 | 0.216237 | -1.443752 | 0.238502 | None |
| 3 | 142 | 0.5917% | 58 | 84 | 59.1549% | 0.003103 | 0.015822 | -1.629012 | 0.020720 | None |
| 4 | 65 | 0.2708% | 28 | 37 | 56.9231% | 0.001498 | 0.006969 | -1.537352 | 0.008411 | None |
| 5 | 13 | 0.0542% | 6 | 7 | 53.8462% | 0.000321 | 0.001319 | -1.412789 | 0.001409 | None |
| 6 | 3 | 0.0125% | 0 | 3 | 100.0000% | 0.000000 | 0.000565 | -inf | inf | Good = 0 — Human Checkpoint required |
| 7 | 38 | 0.1583% | 6 | 32 | 84.2105% | 0.000321 | 0.006028 | -2.932615 | 0.016735 | None |
| 8 | 1 | 0.0042% | 0 | 1 | 100.0000% | 0.000000 | 0.000188 | -inf | inf | Good = 0 — Human Checkpoint required |

Raw-state total IV: `inf` — **diagnostic only; no selection decision**.

Observed factual pattern: Default Rate ranges from 16.6629% to 100.0000%. The smallest observed raw state has 1 Train row(s). No code was merged.

### PAY_6

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| -2 | 3,942 | 16.4250% | 3,164 | 778 | 19.7362% | 0.169279 | 0.146544 | 0.144227 | 0.003279 | None |
| -1 | 4,576 | 19.0667% | 3,803 | 773 | 16.8925% | 0.203467 | 0.145602 | 0.334628 | 0.019363 | None |
| 0 | 13,008 | 54.2000% | 10,543 | 2,465 | 18.9499% | 0.564068 | 0.464306 | 0.194632 | 0.019417 | None |
| 2 | 2,230 | 9.2917% | 1,100 | 1,130 | 50.6726% | 0.058852 | 0.212846 | -1.285546 | 0.197967 | None |
| 3 | 149 | 0.6208% | 56 | 93 | 62.4161% | 0.002996 | 0.017517 | -1.765886 | 0.025643 | None |
| 4 | 42 | 0.1750% | 13 | 29 | 69.0476% | 0.000696 | 0.005462 | -2.060985 | 0.009825 | None |
| 5 | 8 | 0.0333% | 3 | 5 | 62.5000% | 0.000161 | 0.000942 | -1.769464 | 0.001382 | None |
| 6 | 11 | 0.0458% | 3 | 8 | 72.7273% | 0.000161 | 0.001507 | -2.239468 | 0.003015 | None |
| 7 | 33 | 0.1375% | 6 | 27 | 81.8182% | 0.000321 | 0.005086 | -2.762716 | 0.013163 | None |
| 8 | 1 | 0.0042% | 0 | 1 | 100.0000% | 0.000000 | 0.000188 | -inf | inf | Good = 0 — Human Checkpoint required |

Raw-state total IV: `inf` — **diagnostic only; no selection decision**.

Observed factual pattern: Default Rate ranges from 16.8925% to 100.0000%. The smallest observed raw state has 1 Train row(s). No code was merged.

## 9. PAY=1 Month-by-month Evidence

| PAY variable | Code 1 exists | Count | Percent of Train | Default Rate | WOE |
|:---|---:|---:|---:|---:|---:|
| PAY_0 | true | 3,000 | 12.5000% | 34.3667% | -0.611642 |
| PAY_2 | true | 25 | 0.1042% | 20.0000% | 0.127656 |
| PAY_3 | true | 3 | 0.0125% | 33.3333% | -0.565491 |
| PAY_4 | true | 2 | 0.0083% | 50.0000% | -1.258639 |
| PAY_5 | false | not observed | not observed | not observed | not observed |
| PAY_6 | false | not observed | not observed | not observed | not observed |

PAY=1 is highly asymmetric: it is present for PAY_0 through PAY_4 with counts 3,000, 25, 3, and 2, and is not observed for PAY_5 or PAY_6. Missing code 1 states were reported as `not observed`; no zero-sample pseudo-state or pseudo-WOE was created.

## 10. PAY=2 and High-severity Tail Evidence

Code 0, code 1 where observed, code 2, and every observed code 3–8 remain individual rows in the complete tables. Code 2 shows a visible Train risk increase relative to lower states in each month, but this evidence does not establish a final boundary.

Observed codes 3–8:

| PAY variable | Raw code | Count | Percent of Train | Default Rate | WOE |
|:---|---:|---:|---:|---:|---:|
| PAY_0 | 3 | 245 | 1.0208% | 76.7347% | -2.452029 |
| PAY_0 | 4 | 58 | 0.2417% | 65.5172% | -1.900493 |
| PAY_0 | 5 | 18 | 0.0750% | 44.4444% | -1.035495 |
| PAY_0 | 6 | 10 | 0.0417% | 60.0000% | -1.664104 |
| PAY_0 | 7 | 9 | 0.0375% | 77.7778% | -2.511402 |
| PAY_0 | 8 | 16 | 0.0667% | 50.0000% | -1.258639 |
| PAY_2 | 3 | 254 | 1.0583% | 63.7795% | -1.824446 |
| PAY_2 | 4 | 78 | 0.3250% | 50.0000% | -1.258639 |
| PAY_2 | 5 | 21 | 0.0875% | 61.9048% | -1.744146 |
| PAY_2 | 6 | 11 | 0.0458% | 72.7273% | -2.239468 |
| PAY_2 | 7 | 17 | 0.0708% | 52.9412% | -1.376422 |
| PAY_2 | 8 | 1 | 0.0042% | 0.0000% | inf |
| PAY_3 | 3 | 192 | 0.8000% | 57.8125% | -1.573720 |
| PAY_3 | 4 | 60 | 0.2500% | 55.0000% | -1.459309 |
| PAY_3 | 5 | 16 | 0.0667% | 62.5000% | -1.769464 |
| PAY_3 | 6 | 19 | 0.0792% | 57.8947% | -1.577092 |
| PAY_3 | 7 | 20 | 0.0833% | 80.0000% | -2.644933 |
| PAY_3 | 8 | 2 | 0.0083% | 100.0000% | -inf |
| PAY_4 | 3 | 143 | 0.5958% | 60.8392% | -1.699195 |
| PAY_4 | 4 | 53 | 0.2208% | 64.1509% | -1.840560 |
| PAY_4 | 5 | 28 | 0.1167% | 42.8571% | -0.970957 |
| PAY_4 | 6 | 3 | 0.0125% | 33.3333% | -0.565491 |
| PAY_4 | 7 | 40 | 0.1667% | 85.0000% | -2.993240 |
| PAY_4 | 8 | 1 | 0.0042% | 100.0000% | -inf |
| PAY_5 | 3 | 142 | 0.5917% | 59.1549% | -1.629012 |
| PAY_5 | 4 | 65 | 0.2708% | 56.9231% | -1.537352 |
| PAY_5 | 5 | 13 | 0.0542% | 53.8462% | -1.412789 |
| PAY_5 | 6 | 3 | 0.0125% | 100.0000% | -inf |
| PAY_5 | 7 | 38 | 0.1583% | 84.2105% | -2.932615 |
| PAY_5 | 8 | 1 | 0.0042% | 100.0000% | -inf |
| PAY_6 | 3 | 149 | 0.6208% | 62.4161% | -1.765886 |
| PAY_6 | 4 | 42 | 0.1750% | 69.0476% | -2.060985 |
| PAY_6 | 5 | 8 | 0.0333% | 62.5000% | -1.769464 |
| PAY_6 | 6 | 11 | 0.0458% | 72.7273% | -2.239468 |
| PAY_6 | 7 | 33 | 0.1375% | 81.8182% | -2.762716 |
| PAY_6 | 8 | 1 | 0.0042% | 100.0000% | -inf |

All tail rows remain raw and individually visible. Small support and zero-event states are Human Checkpoint evidence only; no `3+`, `4+`, or other tail grouping was created.

## 11. Three Delinquency-summary Raw-value Tables

### delinquent_months

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 15,896 | 66.2333% | 14,066 | 1,830 | 11.5123% | 0.752555 | 0.344698 | 0.780806 | 0.318457 | None |
| 1 | 3,572 | 14.8833% | 2,502 | 1,070 | 29.9552% | 0.133861 | 0.201545 | -0.409207 | 0.027696 | None |
| 2 | 1,532 | 6.3833% | 920 | 612 | 39.9478% | 0.049222 | 0.115276 | -0.850997 | 0.056212 | None |
| 3 | 936 | 3.9000% | 465 | 471 | 50.3205% | 0.024878 | 0.088717 | -1.271459 | 0.081169 | None |
| 4 | 767 | 3.1958% | 322 | 445 | 58.0183% | 0.017228 | 0.083820 | -1.582161 | 0.105360 | None |
| 5 | 237 | 0.9875% | 95 | 142 | 59.9156% | 0.005083 | 0.026747 | -1.660589 | 0.035976 | None |
| 6 | 1,060 | 4.4167% | 321 | 739 | 69.7170% | 0.017174 | 0.139198 | -2.092495 | 0.255334 | None |

Raw-value total IV: `0.880203687638` — **diagnostic only; no selection decision**. All observed integer values remain separate.

### max_delinquency

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 15,896 | 66.2333% | 14,066 | 1,830 | 11.5123% | 0.752555 | 0.344698 | 0.780806 | 0.318457 | None |
| 1 | 1,380 | 5.7500% | 1,034 | 346 | 25.0725% | 0.055321 | 0.065172 | -0.163887 | 0.001615 | None |
| 2 | 5,785 | 24.1042% | 3,240 | 2,545 | 43.9931% | 0.173345 | 0.479375 | -1.017196 | 0.311292 | None |
| 3 | 629 | 2.6208% | 234 | 395 | 62.7981% | 0.012519 | 0.074402 | -1.782203 | 0.110287 | None |
| 4 | 173 | 0.7208% | 66 | 107 | 61.8497% | 0.003531 | 0.020154 | -1.741813 | 0.028955 | None |
| 5 | 49 | 0.2042% | 25 | 24 | 48.9796% | 0.001338 | 0.004521 | -1.217817 | 0.003876 | None |
| 6 | 20 | 0.0833% | 9 | 11 | 55.0000% | 0.000482 | 0.002072 | -1.459309 | 0.002321 | None |
| 7 | 49 | 0.2042% | 8 | 41 | 83.6735% | 0.000428 | 0.007723 | -2.892769 | 0.021102 | None |
| 8 | 19 | 0.0792% | 9 | 10 | 52.6316% | 0.000482 | 0.001884 | -1.363999 | 0.001912 | None |

Raw-value total IV: `0.799817181916` — **diagnostic only; no selection decision**. All observed integer values remain separate.

### severe_delinquent_months

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 17,276 | 71.9833% | 15,100 | 2,176 | 12.5955% | 0.807875 | 0.409870 | 0.678568 | 0.270074 | None |
| 1 | 2,799 | 11.6625% | 1,887 | 912 | 32.5831% | 0.100958 | 0.171784 | -0.531535 | 0.037647 | None |
| 2 | 1,279 | 5.3292% | 708 | 571 | 44.6443% | 0.037879 | 0.107553 | -1.043584 | 0.072711 | None |
| 3 | 861 | 3.5875% | 400 | 461 | 53.5424% | 0.021401 | 0.086834 | -1.400572 | 0.091644 | None |
| 4 | 570 | 2.3750% | 224 | 346 | 60.7018% | 0.011984 | 0.065172 | -1.693431 | 0.090070 | None |
| 5 | 483 | 2.0125% | 197 | 286 | 59.2133% | 0.010540 | 0.053871 | -1.631427 | 0.070691 | None |
| 6 | 732 | 3.0500% | 175 | 557 | 76.0929% | 0.009363 | 0.104916 | -2.416418 | 0.230897 | None |

Raw-value total IV: `0.863732879371` — **diagnostic only; no selection decision**. All observed integer values remain separate.

Figures BIN-07 through BIN-09 show Default Rate and sample support without merging any integer.

## 12. Three Existing-categorical Tables

### SEX

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | 9,486 | 39.5250% | 7,174 | 2,312 | 24.3728% | 0.383821 | 0.435487 | -0.126288 | 0.006525 | None |
| 2 | 14,514 | 60.4750% | 11,517 | 2,997 | 20.6490% | 0.616179 | 0.564513 | 0.087574 | 0.004525 | None |

Raw-category total IV: `0.011049354693` — **diagnostic only; no selection decision**. No category was merged.

### EDUCATION_GROUP

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Graduate School | 8,455 | 35.2292% | 6,824 | 1,631 | 19.2904% | 0.365096 | 0.307214 | 0.172614 | 0.009991 | None |
| University | 11,256 | 46.9000% | 8,582 | 2,674 | 23.7562% | 0.459151 | 0.503673 | -0.092547 | 0.004120 | None |
| High School | 3,903 | 16.2625% | 2,927 | 976 | 25.0064% | 0.156599 | 0.183839 | -0.160368 | 0.004368 | None |
| Other / Unknown | 386 | 1.6083% | 358 | 28 | 7.2539% | 0.019154 | 0.005274 | 1.289690 | 0.017900 | None |

Raw-category total IV: `0.036380079009` — **diagnostic only; no selection decision**. No category was merged.

### MARRIAGE_GROUP

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Married | 10,892 | 45.3833% | 8,354 | 2,538 | 23.3015% | 0.446953 | 0.478056 | -0.067275 | 0.002092 | None |
| Single | 12,806 | 53.3583% | 10,112 | 2,694 | 21.0370% | 0.541009 | 0.507440 | 0.064057 | 0.002150 | None |
| Other / Unknown | 302 | 1.2583% | 225 | 77 | 25.4967% | 0.012038 | 0.014504 | -0.186344 | 0.000459 | None |

Raw-category total IV: `0.004702252234` — **diagnostic only; no selection decision**. No category was merged.

Categorical variables are not governed by automatic ordinal adjacency. Similar WOE alone is not sufficient to merge semantically distinct categories. No SEX merge was proposed.

Sparse special-category flags:

- EDUCATION_GROUP Other / Unknown: 386 rows (1.6083%), flagged for Human Checkpoint and left separate.
- MARRIAGE_GROUP Other / Unknown: 302 rows (1.2583%), flagged for Human Checkpoint and left separate.

## 13. Zero-Good / Zero-Bad Findings

Six raw PAY states have a zero Good or zero Bad count:

| State / Category | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution | Zero-event flag |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| PAY_2: 8 | 1 | 0.0042% | 1 | 0 | 0.0000% | 0.000054 | 0.000000 | inf | inf | Bad = 0 — Human Checkpoint required |
| PAY_3: 8 | 2 | 0.0083% | 0 | 2 | 100.0000% | 0.000000 | 0.000377 | -inf | inf | Good = 0 — Human Checkpoint required |
| PAY_4: 8 | 1 | 0.0042% | 0 | 1 | 100.0000% | 0.000000 | 0.000188 | -inf | inf | Good = 0 — Human Checkpoint required |
| PAY_5: 6 | 3 | 0.0125% | 0 | 3 | 100.0000% | 0.000000 | 0.000565 | -inf | inf | Good = 0 — Human Checkpoint required |
| PAY_5: 8 | 1 | 0.0042% | 0 | 1 | 100.0000% | 0.000000 | 0.000188 | -inf | inf | Good = 0 — Human Checkpoint required |
| PAY_6: 8 | 1 | 0.0042% | 0 | 1 | 100.0000% | 0.000000 | 0.000188 | -inf | inf | Good = 0 — Human Checkpoint required |

Formal WOE and IV Contribution remain mathematically `+inf` or `-inf/inf` as applicable. No 0.5 smoothing was applied and no state was merged. These rows are explicitly deferred to the Human Checkpoint under the Stage 4.1 merge-first, smoothing-fallback policy.

## 14. Fixed-order Cross-variable Diagnostic Summary

| Variable | Raw states/categories | Total raw-state IV | Zero-event issue | Smallest raw-state support | Decision boundary |
|:---|---:|---:|---:|---:|---:|
| PAY_0 | 11 | 0.889401 | false | 9 | diagnostic only — no variable-selection decision |
| PAY_2 | 11 | inf | true | 1 | diagnostic only — no variable-selection decision |
| PAY_3 | 11 | inf | true | 2 | diagnostic only — no variable-selection decision |
| PAY_4 | 11 | inf | true | 1 | diagnostic only — no variable-selection decision |
| PAY_5 | 10 | inf | true | 1 | diagnostic only — no variable-selection decision |
| PAY_6 | 10 | inf | true | 1 | diagnostic only — no variable-selection decision |
| delinquent_months | 7 | 0.880204 | false | 237 | diagnostic only — no variable-selection decision |
| max_delinquency | 9 | 0.799817 | false | 19 | diagnostic only — no variable-selection decision |
| severe_delinquent_months | 7 | 0.863733 | false | 483 | diagnostic only — no variable-selection decision |
| SEX | 2 | 0.011049 | false | 9,486 | diagnostic only — no variable-selection decision |
| EDUCATION_GROUP | 4 | 0.036380 | false | 386 | diagnostic only — no variable-selection decision |
| MARRIAGE_GROUP | 3 | 0.004702 | false | 302 | diagnostic only — no variable-selection decision |

The order is pre-specified, not sorted by IV. There is no rank, winner, retain/drop field, cutoff, or selection conclusion. Infinite raw totals arise only from visibly flagged unsmoothed zero-event raw states.

## 15. Non-binding Candidate Observations

| Variable | Lower adjacent-state evidence | PAY=1 evidence | PAY=2 evidence | High-tail evidence | Status |
|:---|---:|---:|---:|---:|---:|
| PAY_0 | -2/-1/0 Default Rates 12.69%–16.63%; supports -2=2198, -1=4527, 0=11781. These adjacent states may be reviewed; no merge is presumed. | 3,000 rows; 34.37% Default Rate; WOE -0.6116 | 2,138 rows; 69.36% Default Rate; WOE -2.0758; boundary relative to code 0/1 may be reviewed | codes/supports 3=245, 4=58, 5=18, 6=10, 7=9, 8=16; contiguous tail may be reviewed only with risk/support evidence | candidate for later Human review — no final grouping |
| PAY_2 | -2/-1/0 Default Rates 15.95%–17.95%; supports -2=3030, -1=4845, 0=12573. These adjacent states may be reviewed; no merge is presumed. | 25 rows; 20.00% Default Rate; WOE 0.1277 | 3,145 rows; 55.61% Default Rate; WOE -1.4841; boundary relative to code 0/1 may be reviewed | codes/supports 3=254, 4=78, 5=21, 6=11, 7=17, 8=1; contiguous tail may be reviewed only with risk/support evidence | candidate for later Human review — no final grouping |
| PAY_3 | -2/-1/0 Default Rates 15.55%–18.01%; supports -2=3270, -1=4733, 0=12636. These adjacent states may be reviewed; no merge is presumed. | 3 rows; 33.33% Default Rate; WOE -0.5655 | 3,049 rows; 51.82% Default Rate; WOE -1.3315; boundary relative to code 0/1 may be reviewed | codes/supports 3=192, 4=60, 5=16, 6=19, 7=20, 8=2; contiguous tail may be reviewed only with risk/support evidence | candidate for later Human review — no final grouping |
| PAY_4 | -2/-1/0 Default Rates 15.93%–18.58%; supports -2=3477, -1=4531, 0=13180. These adjacent states may be reviewed; no merge is presumed. | 2 rows; 50.00% Default Rate; WOE -1.2586 | 2,542 rows; 52.64% Default Rate; WOE -1.3642; boundary relative to code 0/1 may be reviewed | codes/supports 3=143, 4=53, 5=28, 6=3, 7=40, 8=1; contiguous tail may be reviewed only with risk/support evidence | candidate for later Human review — no final grouping |
| PAY_5 | -2/-1/0 Default Rates 16.66%–19.16%; supports -2=3659, -1=4417, 0=13560. These adjacent states may be reviewed; no merge is presumed. | not observed | 2,102 rows; 54.61% Default Rate; WOE -1.4438; boundary relative to code 0/1 may be reviewed | codes/supports 3=142, 4=65, 5=13, 6=3, 7=38, 8=1; contiguous tail may be reviewed only with risk/support evidence | candidate for later Human review — no final grouping |
| PAY_6 | -2/-1/0 Default Rates 16.89%–19.74%; supports -2=3942, -1=4576, 0=13008. These adjacent states may be reviewed; no merge is presumed. | not observed | 2,230 rows; 50.67% Default Rate; WOE -1.2855; boundary relative to code 0/1 may be reviewed | codes/supports 3=149, 4=42, 5=8, 6=11, 7=33, 8=1; contiguous tail may be reviewed only with risk/support evidence | candidate for later Human review — no final grouping |
| delinquent_months | Smallest adjacent Default Rate gap 1.90% for 4 and 5 (supports 767/237); may be reviewed, not merged here | not applicable | not applicable | all raw high values remain separately visible | candidate for later Human review — no final grouping |
| max_delinquency | Smallest adjacent Default Rate gap 0.95% for 3 and 4 (supports 629/173); may be reviewed, not merged here | not applicable | not applicable | all raw high values remain separately visible | candidate for later Human review — no final grouping |
| severe_delinquent_months | Smallest adjacent Default Rate gap 1.49% for 4 and 5 (supports 570/483); may be reviewed, not merged here | not applicable | not applicable | all raw high values remain separately visible | candidate for later Human review — no final grouping |

These observations use `may be reviewed` / `candidate for later Human review` language only. They preserve monthly differences and ordinal adjacency. They do not define a final bin, final bin count, approved grouping, automatic merge, or variable recommendation.

Categorical checkpoint facts are limited to the two sparse Other / Unknown categories; neither is assigned a final treatment.

## 16. Diagnostic Figures

Nine uniquely numbered inline figures were generated and saved inside the Notebook:

- BIN-01 — PAY_0
- BIN-02 — PAY_2
- BIN-03 — PAY_3
- BIN-04 — PAY_4
- BIN-05 — PAY_5
- BIN-06 — PAY_6
- BIN-07 — delinquent_months
- BIN-08 — max_delinquency
- BIN-09 — severe_delinquent_months

Every figure contains vertically aligned Default Rate and Train Count panels. Each section states the Question, Why it matters, Observed factual pattern, and Human decision needed. No external figure file was written.

## 17. Explicit Out-of-scope Confirmations

Confirmed:

- no PAY state merge;
- no delinquency value merge;
- no categorical merge;
- no final bin labels or definitions;
- no smoothing or smoothing decision;
- no variable ranking/selection/retain/drop;
- no continuous predictor analysis or binning;
- no optimal/decision-tree/ChiMerge/monotonic automatic binning;
- no Logistic, VIF, coefficients, Odds Ratio, model, KFold, CV, ROC, AUC, KS, calibration, threshold optimization, sampling, or class weights;
- no Holdout Test target analysis;
- no processed CSV, WOE mapping CSV, bin definition file, ranking file, model, scorecard, source module, test module, HTML, PDF, checkpoint, or external figure;
- no Stage 4.2B or Stage 4.3 work.

## 18. Notebook Clean-run Evidence

| Check | Result |
|---|---|
| Notebook | `notebooks/05_pay_categorical_binning.ipynb` |
| Environment | Existing project `.venv`; Python 3.9.6 |
| Execution | Fresh `python3` kernel from raw CSV |
| Cells | 40 total: 20 markdown + 20 code |
| Execution counts | Sequential 1–20 |
| Saved outputs | 53 |
| Inline PNG figure outputs | 9 |
| Error outputs | 0 |
| Tracebacks | 0 |
| Raw diagnostic tables | 12 |
| Required figure IDs | BIN-01 through BIN-09, unique |
| Notebook size | 540,112 bytes |
| Notebook SHA-256 | `839f27b9b21dc2470a897144d363e0138d5dfc416f69e784a6e5b611dbb41628` |

Matplotlib built its font cache in a system temporary directory during the fresh run. A harmless `jupyter_client` destructor warning occurred after explicit kernel shutdown. Neither is a Notebook cell error or repository artifact. No package was installed, upgraded, or removed.

## 19. Files Created or Modified

Created:

- `/Users/yu/2.工作/Risk Analysis Project/notebooks/05_pay_categorical_binning.ipynb`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_4.2_PAY_Categorical_Binning.md`

Modified existing files: none.

## 20. Protected-input Final Integrity

All protected inputs retained the exact sizes and SHA-256 values recorded in Section 4 after the Notebook clean-run and report creation. Tracked and staged diffs remained empty. HEAD, history, Git index, raw CSV, Stage 0–4.1 artifacts, README files, requirements, and Luca reference were unchanged.

## 21. Final Git State

Expected and verified handoff state:

- branch: `main`;
- HEAD: `9d22109b9db4abfafbb653e0cb7b9068bda6a8d3`;
- working tree changes: exactly the two untracked Stage 4.2 artifacts;
- tracked unstaged diff: empty;
- staged diff: empty;
- remote: none;
- no git add;
- no commit;
- no push;
- no remote change.

## 22. Deviations, Limitations, and Unexpected Artifacts

- Deviations from the Architect Specification: none identified.
- Incomplete Stage 4.2A items: none identified.
- Unexpected repository artifacts/modifications: none observed.
- Limitation: raw rates and WOE for extremely sparse high-severity codes are unstable; their raw evidence is intentionally preserved rather than silently smoothed or grouped.
- Infinite raw IV totals for PAY_2 through PAY_6 are diagnostic artifacts of explicitly displayed zero-event states and cannot be used for selection.
- No statement in this report is a final grouping decision.

## 23. Human Checkpoint — Mandatory Stop

Status:

`Stage 4.2A complete — final grouping pending Architect/user Human Checkpoint.`

The Architect/user must review month-specific PAY evidence, PAY=1 asymmetry, code 2 boundaries, raw high-severity support, adjacent delinquency values, and sparse categorical special groups before authorizing Stage 4.2B.

Codex stops here. Stage 4.2B has not started.

## 24. Reviewer Status

No Stage 4.2 Reviewer verdict or milestone commit has been produced or claimed. Reviewer validation occurs only after Stage 4.2B is complete and separately authorized.

# REVIEWER RESULT

## Verdict

PASS

This verdict applies only to **Stage 4.2A — PAY + Categorical Raw Risk Diagnostics**. It is an interim Phase A readiness result, not the final Reviewer verdict for Stage 4.2. Final grouping remains intentionally undecided, Stage 4.2B has not been authorized or implemented, and no Stage 4.2 milestone commit is permitted at this checkpoint.

## Requirement Check

- The complete Stage 4.2A immutable Architect Specification is present, and the artifact status correctly stops at the Human Checkpoint.
- Git baseline is correct: `main` at Stage 4.1 milestone `9d22109b9db4abfafbb653e0cb7b9068bda6a8d3`, expected parent/message and artifact hashes, no tracked or staged diff, and no remote.
- The locked 80/20 stratified split reproduces 24,000 Train and 6,000 Test rows, disjoint/full-coverage IDs, and both required stable ID-set hashes.
- Holdout Test is used only for technical row/ID/hash assertions and then deleted. No Test target count, risk table, WOE/IV, PAY/category analysis, or Train/Test risk comparison is present.
- The exact frozen Train mappings and three delinquency-summary formulas are reproduced without altering raw PAY, row/ID alignment, or target.
- Exactly the 12 authorized variables are analyzed, in the prescribed fixed order. AGE, LIMIT_BAL, BILL_AMT, PAY_AMT, and utilization variables receive no diagnostics or binning.
- All six PAY tables retain every observed raw code in numeric order and include Count, Train share, Good, Bad, Default Rate, Dist Good, Dist Bad, WOE, IV Contribution, and explicit zero-event flags. No code is merged or semantically redefined.
- PAY=1 presence/support/rate/WOE is reported month by month; code 2 boundaries and every observed high-severity code 3–8 remain separately visible with sample support.
- All three delinquency-summary tables and figures preserve every observed integer value in ascending order. No value is merged.
- SEX, EDUCATION_GROUP, and MARRIAGE_GROUP tables are complete. SEX merging is not proposed, and both sparse Other / Unknown categories are explicitly flagged without final treatment.
- All six zero-Good/zero-Bad raw PAY states are visibly reported with mathematically infinite raw WOE/IV where appropriate. No silent smoothing or smoothing decision is applied.
- Candidate observations use non-binding Human-review language, preserve ordinal adjacency and month-specific differences, and do not define final bins, variable ranks, retain/drop decisions, or selection recommendations.
- Nine uniquely numbered figures BIN-01 through BIN-09 display Default Rate and Train support together. Each associated section provides the required question, rationale, observed factual evidence, and Human decision boundary.
- No automatic binning, continuous binning, variable selection, modeling, KFold/CV, AUC/KS, sampling, processed artifact, Stage 4.2B work, or Stage 4.3 work is present.
- The Notebook clean-runs from raw data with execution counts 1–20, 53 outputs, nine embedded figures, and no error output. Protected inputs and earlier-stage artifacts remain unchanged.

## Independent Verification

- Read the complete Stage 4.2A Architect Specification and all Codex execution evidence.
- Inspected every Notebook source/Markdown cell, saved output, execution count, metadata field, target/Test reference, table-construction rule, candidate-observation statement, and prohibited-operation pattern.
- Independently replayed the split and reproduced Train hash `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` and Test hash `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Independently reproduced all 12 diagnostic table totals and raw-state IV values, the 18,691/5,309 Good/Bad totals, PAY=1 monthly evidence, all six zero-event states, and the 386/302 sparse-category counts.
- Extracted and visually inspected all nine embedded diagnostic figures; numbering, labels, rate panels, support panels, and factual annotations are readable and consistent with the saved tables.
- Independently reran all 20 code cells in a fresh local kernel without overwriting the Notebook; all cells succeeded and reproduced 53 outputs and nine PNG figures with no cell errors.
- Rechecked Notebook SHA-256 `839f27b9b21dc2470a897144d363e0138d5dfc416f69e784a6e5b611dbb41628`, protected-file hashes, Git branch/HEAD, tracked/staged/untracked scope, and remote state after verification.

## Issues Found

No issues affecting **Stage 4.2A** acceptance.

Stage 4.2 as a whole is not complete because final Human-approved groupings and Stage 4.2B implementation remain pending. This is the Architect-required checkpoint state, not a Phase A defect.

## Required Fixes

None for Stage 4.2A.

## Architect Decision Needed

Architect/user must complete the mandatory variable-by-variable Human Checkpoint, freeze the final PAY, delinquency-summary, and categorical grouping decisions, and separately authorize Stage 4.2B.

No local commit was created because the Architect Specification explicitly reserves the Stage 4.2 milestone commit for completion of Stage 4.2B followed by final Reviewer PASS.


---

# Stage 4.2B — Human-Approved Final Binning Implementation

## 25. Overall Stage 4.2 Status

`Stage 4.2B implementation complete — awaiting final Stage 4.2 Reviewer validation.`

This appendix records implementation evidence only. It does not claim final Stage 4.2 Reviewer PASS, closure, staging, or a milestone commit. The preceding Stage 4.2A report, immutable specification, raw diagnostics, and interim Reviewer Result remain historical and unchanged.

## 26. Human Checkpoint Decision Record

The Architect/user froze the final groupings. Codex implemented them exactly and did not search, optimize, rank, or redesign bins.

Approved PAY rationale: `-2/-1/0` are grouped by observed modeling risk behavior, not by a claim of identical real-world semantics. PAY_0 code 1 retains a standalone intermediate-risk bin. PAY_2–PAY_4 code 1 joins the low bin because standalone WOE support is too sparse. PAY_5/PAY_6 code 1 is absent from Train but is deliberately pre-mapped to the low bin for domain completeness. Code 2 remains a major boundary, and sparse high-severity codes are consolidated to remove unstable tiny-state/zero-event WOE.

Explicitly rejected and not implemented:

- PAY_5/PAY_6 `[-2,-1,0] / [2] / [3+]`, because it omits the allowed code 1 domain treatment.
- `delinquent_months` and `severe_delinquent_months` `0 / 1 / 2 / 3 / 4-5 / 6`, because it overfits a local Train coincidence.

Approved instead:

- PAY_5/PAY_6 `[-2,-1,0,1] / [2] / [3+]`, with synthetic code 1 mapped low.
- Both persistence summaries `0 / 1 / 2 / 3 / 4+`, providing stable, interpretable persistence thresholds.

## 27. Final Frozen Mapping Definitions

| Variable(s) | Ordered final bins | Raw-domain rule |
| --- | --- | --- |
| PAY_0 | `[-2,-1,0] / [1] / [2+]` | `2+ = {2,3,4,5,6,7,8}` |
| PAY_2–PAY_6 | `[-2,-1,0,1] / [2] / [3+]` | `3+ = {3,4,5,6,7,8}` |
| delinquent_months | `0 / 1 / 2 / 3 / 4+` | `4+ = {4,5,6}` |
| max_delinquency | `0 / 1 / 2 / 3+` | `3+ = {3,4,5,6,7,8}` |
| severe_delinquent_months | `0 / 1 / 2 / 3 / 4+` | `4+ = {4,5,6}` |
| SEX | raw categories 1 and 2 | unchanged |
| EDUCATION_GROUP | Graduate School / University / High School / Other / Unknown | unchanged |
| MARRIAGE_GROUP | Married / Single / Other / Unknown | unchanged |

`delinquent_months = 4+` means at least four of six observed months have PAY > 0. `severe_delinquent_months = 4+` means at least four have PAY ≥ 2. `max_delinquency = 3+` is the sparse high-severity tail.

## 28. Synthetic Domain and Mapping Integrity

- Complete approved PAY domain `{-2,-1,0,1,2,3,4,5,6,7,8}` was asserted synthetically for every PAY variable.
- PAY_5 synthetic code 1 → `[-2,-1,0,1]`: PASS.
- PAY_6 synthetic code 1 → `[-2,-1,0,1]`: PASS.
- Synthetic code 99 raises an explicit validation error: PASS. No fallback/high assignment exists.
- Synthetic coverage does not claim that any state occurs in Train or Test. No Test lookup was used for code 1.
- Train rows/ID/order/target, six raw PAY fields, and three raw summaries remained identical. Mapping reruns were deterministic; no Train row was unmapped.
- Values 4/5/6 map to `4+` for both persistence summaries; values 3–8 map to `3+` for max_delinquency; no `4-5` bin exists.

## 29. Final Grouped WOE / IV Tables

Convention: `0 = Good`, `1 = Bad`, `WOE = ln(Dist Good / Dist Bad)`. The following 12 tables are in fixed scope order and are not ranked.

### PAY_0

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [-2,-1,0] | -2, -1, 0 | 18,506 | 77.108333% | 15,966 | 2,540 | 13.725278% | 85.420791% | 47.843285% | 0.579658749 | 0.217821300 |
| [1] | 1 | 3,000 | 12.500000% | 1,969 | 1,031 | 34.366667% | 10.534482% | 19.419853% | -0.611642036 | 0.054346666 |
| [2+] | 2, 3, 4, 5, 6, 7, 8 | 2,494 | 10.391667% | 756 | 1,738 | 69.687249% | 4.044727% | 32.736862% | -2.091087561 | 0.599977656 |

Final grouped total IV: `0.872145621429`

### PAY_2

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [-2,-1,0,1] | -2, -1, 0, 1 | 20,473 | 85.304167% | 17,144 | 3,329 | 16.260441% | 91.723289% | 62.704841% | 0.380337669 | 0.110368090 |
| [2] | 2 | 3,145 | 13.104167% | 1,396 | 1,749 | 55.612083% | 7.468835% | 32.944057% | -1.484071823 | 0.378070591 |
| [3+] | 3, 4, 5, 6, 7, 8 | 382 | 1.591667% | 151 | 231 | 60.471204% | 0.807875% | 4.351102% | -1.683776505 | 0.059660015 |

Final grouped total IV: `0.548098696196`

### PAY_3

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [-2,-1,0,1] | -2, -1, 0, 1 | 20,642 | 86.008333% | 17,096 | 3,546 | 17.178568% | 91.466481% | 66.792240% | 0.314385679 | 0.077572282 |
| [2] | 2 | 3,049 | 12.704167% | 1,469 | 1,580 | 51.820269% | 7.859398% | 29.760784% | -1.331481581 | 0.291612921 |
| [3+] | 3, 4, 5, 6, 7, 8 | 309 | 1.287500% | 126 | 183 | 59.223301% | 0.674121% | 3.446977% | -1.631842877 | 0.045248647 |

Final grouped total IV: `0.414433849225`

### PAY_4

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [-2,-1,0,1] | -2, -1, 0, 1 | 21,190 | 88.291667% | 17,388 | 3,802 | 17.942426% | 93.028730% | 71.614240% | 0.261614438 | 0.056023399 |
| [2] | 2 | 2,542 | 10.591667% | 1,204 | 1,338 | 52.635720% | 6.441603% | 25.202486% | -1.364165246 | 0.255929452 |
| [3+] | 3, 4, 5, 6, 7, 8 | 268 | 1.116667% | 99 | 169 | 63.059701% | 0.529667% | 3.183274% | -1.793417496 | 0.047590252 |

Final grouped total IV: `0.359543102661`

### PAY_5

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [-2,-1,0,1] | -2, -1, 0, 1 | 21,636 | 90.150000% | 17,639 | 3,997 | 18.473840% | 94.371623% | 75.287248% | 0.225929649 | 0.043117261 |
| [2] | 2 | 2,102 | 8.758333% | 954 | 1,148 | 54.614653% | 5.104061% | 21.623658% | -1.443751536 | 0.238501938 |
| [3+] | 3, 4, 5, 6, 7, 8 | 262 | 1.091667% | 98 | 164 | 62.595420% | 0.524317% | 3.089094% | -1.773537580 | 0.045487292 |

Final grouped total IV: `0.327106490815`

### PAY_6

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [-2,-1,0,1] | -2, -1, 0, 1 | 21,526 | 89.691667% | 17,510 | 4,016 | 18.656508% | 93.681451% | 75.645131% | 0.213847133 | 0.038570153 |
| [2] | 2 | 2,230 | 9.291667% | 1,100 | 1,130 | 50.672646% | 5.885185% | 21.284611% | -1.285546084 | 0.197966713 |
| [3+] | 3, 4, 5, 6, 7, 8 | 244 | 1.016667% | 81 | 163 | 66.803279% | 0.433364% | 3.070258% | -1.957939677 | 0.051628802 |

Final grouped total IV: `0.288165668499`

### delinquent_months

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 15,896 | 66.233333% | 14,066 | 1,830 | 11.512330% | 75.255471% | 34.469768% | 0.780805940 | 0.318457186 |
| 1 | 1 | 3,572 | 14.883333% | 2,502 | 1,070 | 29.955207% | 13.386122% | 20.154455% | -0.409206867 | 0.027696484 |
| 2 | 2 | 1,532 | 6.383333% | 920 | 612 | 39.947781% | 4.922155% | 11.527595% | -0.850997243 | 0.056212109 |
| 3 | 3 | 936 | 3.900000% | 465 | 471 | 50.320513% | 2.487828% | 8.871727% | -1.271459319 | 0.081168677 |
| 4+ | 4, 5, 6 | 2,064 | 8.600000% | 738 | 1,326 | 64.244186% | 3.948424% | 24.976455% | -1.844616977 | 0.387886624 |

Final grouped total IV: `0.871421079861`

### max_delinquency

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 15,896 | 66.233333% | 14,066 | 1,830 | 11.512330% | 75.255471% | 34.469768% | 0.780805940 | 0.318457186 |
| 1 | 1 | 1,380 | 5.750000% | 1,034 | 346 | 25.072464% | 5.532074% | 6.517235% | -0.163887351 | 0.001614554 |
| 2 | 2 | 5,785 | 24.104167% | 3,240 | 2,545 | 43.993086% | 17.334546% | 47.937465% | -1.017195951 | 0.311291649 |
| 3+ | 3, 4, 5, 6, 7, 8 | 939 | 3.912500% | 351 | 588 | 62.619808% | 1.877909% | 11.075532% | -1.774579355 | 0.163219118 |

Final grouped total IV: `0.794582506999`

### severe_delinquent_months

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 17,276 | 71.983333% | 15,100 | 2,176 | 12.595508% | 80.787545% | 40.987003% | 0.678567784 | 0.270073653 |
| 1 | 1 | 2,799 | 11.662500% | 1,887 | 912 | 32.583065% | 10.095768% | 17.178376% | -0.531535076 | 0.037646548 |
| 2 | 2 | 1,279 | 5.329167% | 708 | 571 | 44.644253% | 3.787919% | 10.755321% | -1.043583747 | 0.072710673 |
| 3 | 3 | 861 | 3.587500% | 400 | 461 | 53.542393% | 2.140067% | 8.683368% | -1.400572127 | 0.091643642 |
| 4+ | 4, 5, 6 | 1,785 | 7.437500% | 596 | 1,189 | 66.610644% | 3.188700% | 22.395931% | -1.949265861 | 0.374399997 |

Final grouped total IV: `0.846474512620`

### SEX

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 1 (unchanged) | 9,486 | 39.525000% | 7,174 | 2,312 | 24.372760% | 38.382109% | 43.548691% | -0.126288203 | 0.006524783 |
| 2 | 2 (unchanged) | 14,514 | 60.475000% | 11,517 | 2,997 | 20.649029% | 61.617891% | 56.451309% | 0.087573785 | 0.004524571 |

Final grouped total IV: `0.011049354693`

### EDUCATION_GROUP

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Graduate School | Graduate School (unchanged) | 8,455 | 35.229167% | 6,824 | 1,631 | 19.290361% | 36.509550% | 30.721416% | 0.172613856 | 0.009991121 |
| University | University (unchanged) | 11,256 | 46.900000% | 8,582 | 2,674 | 23.756219% | 45.915146% | 50.367301% | -0.092547123 | 0.004120341 |
| High School | High School (unchanged) | 3,903 | 16.262500% | 2,927 | 976 | 25.006405% | 15.659943% | 18.383876% | -0.160367931 | 0.004368315 |
| Other / Unknown | Other / Unknown (unchanged) | 386 | 1.608333% | 358 | 28 | 7.253886% | 1.915360% | 0.527406% | 1.289689845 | 0.017900302 |

Final grouped total IV: `0.036380079009`

### MARRIAGE_GROUP

| Final Bin | Raw Values Included | Count | % Train | Good | Bad | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Married | Married (unchanged) | 10,892 | 45.383333% | 8,354 | 2,538 | 23.301506% | 44.695308% | 47.805613% | -0.067274534 | 0.002092443 |
| Single | Single (unchanged) | 12,806 | 53.358333% | 10,112 | 2,694 | 21.037014% | 54.100904% | 50.744020% | 0.064057128 | 0.002150324 |
| Other / Unknown | Other / Unknown (unchanged) | 302 | 1.258333% | 225 | 77 | 25.496689% | 1.203788% | 1.450367% | -0.186343651 | 0.000459485 |

Final grouped total IV: `0.004702252234`

## 30. Fixed-Order Grouped IV Totals

| Variable | Final grouped total IV |
| --- | ---: |
| PAY_0 | 0.872145621429 |
| PAY_2 | 0.548098696196 |
| PAY_3 | 0.414433849225 |
| PAY_4 | 0.359543102661 |
| PAY_5 | 0.327106490815 |
| PAY_6 | 0.288165668499 |
| delinquent_months | 0.871421079861 |
| max_delinquency | 0.794582506999 |
| severe_delinquent_months | 0.846474512620 |
| SEX | 0.011049354693 |
| EDUCATION_GROUP | 0.036380079009 |
| MARRIAGE_GROUP | 0.004702252234 |

These totals are reported without ranking, thresholding, Keep/Drop, or selection.

## 31. Anchor Validation and Reconciliation

- PAY_0 exact bin Count/Good/Bad and rounded Default Rate/WOE/total IV anchors: PASS.
- PAY_2–PAY_6 exact Count and rounded Default Rate/WOE/total IV anchors: PASS.
- delinquent_months `4+`: 2,064 Count / 738 Good / 1,326 Bad, rounded rate/WOE and total IV: PASS.
- max_delinquency `3+`: 939 / 351 / 588, rounded rate/WOE and total IV: PASS.
- severe_delinquent_months `4+`: 1,785 / 596 / 1,189, rounded rate/WOE and total IV: PASS.
- EDUCATION_GROUP Other / Unknown = 386 (1.6083%); MARRIAGE_GROUP Other / Unknown = 302 (1.2583%): PASS.
- Every table sums to 24,000 Count, 18,691 Good, and 5,309 Bad; `% Train`, `Dist Good`, and `Dist Bad` each sum to 1 within floating-point tolerance.

## 32. Zero-Event Resolution and Smoothing

All Human-approved final PAY tail consolidations contain both Good and Bad. Across all 12 final tables:

- no final bin has Good = 0;
- no final bin has Bad = 0;
- every WOE is finite;
- every IV contribution and total IV is finite;
- no 0.5 or other smoothing was applied.

The six raw zero-event states remain visible in the preserved Stage 4.2A diagnostics; they were not silently rewritten.

## 33. Final Monotonicity Diagnostics

| Variable | Default Rate broad direction | WOE broad direction |
| --- | --- | --- |
| PAY_0 | PASS | PASS |
| PAY_2 | PASS | PASS |
| PAY_3 | PASS | PASS |
| PAY_4 | PASS | PASS |
| PAY_5 | PASS | PASS |
| PAY_6 | PASS | PASS |
| delinquent_months | PASS | PASS |
| max_delinquency | PASS | PASS |
| severe_delinquent_months | PASS | PASS |

This was diagnostic only. Frozen order was retained without automatic merge, reorder, alternative search, or selection.

## 34. Final Figure Inventory

Notebook contains the preserved Phase A figures BIN-01–BIN-09 and the following new inline final-grouping figures, each showing grouped Default Rate and Train support in frozen order:

- BIN-10 — PAY_0 Final Grouping
- BIN-11 — PAY_2 Final Grouping
- BIN-12 — PAY_3 Final Grouping
- BIN-13 — PAY_4 Final Grouping
- BIN-14 — PAY_5 Final Grouping
- BIN-15 — PAY_6 Final Grouping
- BIN-16 — delinquent_months Final Grouping
- BIN-17 — max_delinquency Final Grouping
- BIN-18 — severe_delinquent_months Final Grouping

No external figure file was created. Each figure note defers variable retention, redundancy, and stability decisions to Stage 4.4/4.6.

## 35. Split Identity and Holdout Protection

- Raw / Train / Test rows: 30,000 / 24,000 / 6,000.
- ID overlap: 0; union coverage: 30,000.
- Train ID SHA-256: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`.
- Test ID SHA-256: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Test was deleted after technical row/ID/hash assertions in the clean run.
- No Test target distribution/default rate, PAY/category occurrence, code-1 lookup, grouped-bin frequency, WOE, IV, or predictor-target relationship was inspected. Final bins were not applied to Test.

## 36. Notebook Clean-Run and Phase A Preservation

- Notebook: `/Users/yu/2.工作/Risk Analysis Project/notebooks/05_pay_categorical_binning.ipynb`.
- Clean kernel: existing project `.venv`; no package changes.
- Cells: 78 total = 40 Markdown + 38 code.
- Execution counts: sequential 1–38.
- Saved outputs: 117; inline PNG outputs: 18; error outputs/tracebacks: 0.
- Phase A first 40 cells retained source SHA-256 `23a39acd9e33b164933ce51ca863867a3b57024cd54a3cbf675bde01c852d25a`.
- Phase A first 40 cells retain 53 outputs, 12 raw diagnostic tables, and BIN-01–BIN-09 after fresh execution.
- Final Notebook SHA-256 at report generation: `79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796`.
- A harmless `jupyter_client` destructor warning occurred only after explicit kernel shutdown; it is not a Notebook cell output or repository artifact.

## 37. Categorical Decisions

SEX remains two raw categories. EDUCATION_GROUP remains four categories, including Other / Unknown as a separate sparse special group. MARRIAGE_GROUP remains three categories, including its separate Other / Unknown group. No category was renamed, merged, or dropped. Their future stability/selection is deferred to Stage 4.4 and Stage 4.6.

## 38. Scope Exclusions

Confirmed: no final-bin redesign; no new feature engineering; no variable ranking/selection; no continuous-variable analysis/binning; no Logistic, coefficients, odds ratios, VIF, ROC, AUC, KS, calibration, thresholding, sampling, class weighting, or KFold/CV; no Test target/risk analysis; no Stage 4.3 work; no mapping CSV, processed data, pickle/joblib, model, external figure, HTML/PDF, source/test module, checkpoint, or project cache artifact.

## 39. Protected Inputs and Git Handoff

The 18 tracked protected files recorded before execution retain their original size/SHA-256 values. Phase A source and report history are preserved. The only project artifacts changed are the two authorized untracked Stage 4.2 files.

- repository root: `/Users/yu/2.工作/Risk Analysis Project`
- branch: `main`
- HEAD: `9d22109b9db4abfafbb653e0cb7b9068bda6a8d3`
- tracked unstaged diff: empty
- staged diff: empty
- untracked: exactly this Notebook and this Task Report
- remote: none
- no git add, commit, push, or remote change

## 40. Deviations, Limitations, and Final Reviewer Handoff

- Deviations from frozen groupings: none.
- Incomplete implementation items: none identified.
- Unexpected repository artifacts: none observed.
- Limitation: Holdout performance and grouped-bin behavior remain intentionally unknown until the authorized final evaluation stage.
- Reviewer status: awaiting final Stage 4.2 Reviewer validation; Codex does not claim PASS or CLOSED and did not create a milestone commit.

## 41. Immutable Architect Specification — Stage 4.2B

================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.2B ================

# Stage 4.2B — Final PAY + Categorical Binning Implementation

## 1. Stage Context

Stage 4 固定结构：

* Stage 4.1 — WOE / IV / Binning Method Design ✅ CLOSED
* Stage 4.2A — PAY + Categorical Raw Risk Diagnostics ✅ INTERIM REVIEWER PASS
* Stage 4.2B — Final PAY + Categorical Binning Implementation ← CURRENT
* Stage 4.3 — Continuous Variable Supervised Binning
* Stage 4.4 — WOE / IV + Variable Screening
* Stage 4.5 — Candidate WOE Logistic Regression
* Stage 4.6 — Stratified 5-Fold Model Stability Test
* Stage 4.7 — Final Logistic Freeze + Full-Train Refit

Stage 4.2A 已完成并通过限定范围 Reviewer PASS。

Architect/user 已完成 Mandatory Human Checkpoint。

本 Specification 中的 grouping decisions 现已：

> **FROZEN**

Codex 的任务是实施并验证这些决定。

Codex 不得重新设计 bins。

---

# 2. Stage 4.2B Objective

本阶段只完成：

1. 实现 Human-approved PAY grouping；
2. 实现 Human-approved delinquency-summary grouping；
3. 保持现有 categorical grouping；
4. 计算 final grouped Count / Good / Bad / Default Rate / WOE / IV；
5. 验证所有 final bins 有效、有限、完整；
6. 验证 mapping 对允许的 PAY code domain 完整；
7. 为后续 Stage 4.4/4.5 正式记录 final grouping definitions。

本阶段不做：

* variable selection；
* continuous binning；
* Logistic；
* KFold；
* Test evaluation。

---

# 3. Git Pre-flight

Project root：

`/Users/yu/2.工作/Risk Analysis Project`

当前正式 milestone 仍应为 Stage 4.1：

`9d22109b9db4abfafbb653e0cb7b9068bda6a8d3`

Branch：

`main`

Expected handoff state after approved Stage 4.2A:

* HEAD = Stage 4.1 milestone；
* tracked unstaged diff = empty；
* staged diff = empty；
* remote = none；
* exactly two Stage 4.2 artifacts exist as untracked work:

  * `notebooks/05_pay_categorical_binning.ipynb`
  * `task_records/Stage_4.2_PAY_Categorical_Binning.md`

Stage 4.2A Notebook approved SHA-256 before Phase B:

`839f27b9b21dc2470a897144d363e0138d5dfc416f69e784a6e5b611dbb41628`

Stage 4.2 Task Report must contain the Stage 4.2A Reviewer Result showing:

> PASS limited to Stage 4.2A

If baseline differs unexpectedly:

> BLOCKED.

Do not automatically:

* reset
* restore
* stash
* clean
* checkout
* amend
* rebase

---

# 4. Data Boundary

Replay the same frozen split:

* Train = 24,000
* Holdout Test = 6,000
* `random_state=42`
* stratified by target

Verify only technical Test integrity:

Train ID hash:

`bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`

Test ID hash:

`d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

Holdout Test remains sealed.

Stage 4.2B must not calculate or inspect:

* Test target distribution；
* Test default rate；
* Test WOE；
* Test IV；
* Test PAY state frequencies；
* Test occurrence of PAY=1；
* Test grouped-bin frequencies；
* Test predictor-target relationship。

Important:

> The PAY_5/PAY_6 code=1 mapping rule must be validated synthetically, not by inspecting whether code 1 exists in Test.

---

# 5. Frozen Good / Bad / WOE Convention

* `0 = Good / Non-default`
* `1 = Bad / Default`

[
WOE_j=
\ln
\left(
\frac{DistGood_j}{DistBad_j}
\right)
]

Positive WOE = relatively lower risk.

Negative WOE = relatively higher risk.

[
IV_j=
(DistGood_j-DistBad_j)\times WOE_j
]

No convention change is allowed.

---

# 6. FINAL PAY GROUPING — FROZEN

## PAY_0

Final bins:

### Bin 1

`[-2, -1, 0]`

### Bin 2

`[1]`

### Bin 3

`[2+]`

where `[2+]` means all approved raw PAY codes:

`2,3,4,5,6,7,8`

No other grouping is authorized.

---

# 7. PAY_2 / PAY_3 / PAY_4 / PAY_5 / PAY_6 — FROZEN

All five use the same **domain-complete grouping rule**:

### Bin 1

`[-2, -1, 0, 1]`

### Bin 2

`[2]`

### Bin 3

`[3+]`

where `[3+]` means:

`3,4,5,6,7,8`

This applies to:

* PAY_2
* PAY_3
* PAY_4
* PAY_5
* PAY_6

---

# 8. Explicit PAY_5 / PAY_6 Code=1 Rule

Stage 4.2A Train contained no observed `PAY_5=1` or `PAY_6=1`.

Nevertheless, the final mapping is intentionally defined as:

`1 → [-2,-1,0,1]`

for both variables.

Reason:

> A production-style binning rule must define valid treatment for an allowed PAY state even if that state is absent from the current Train sample.

This is a Human-approved design decision.

Codex must not:

* create a separate code=1 bin；
* map it to `[2]`；
* map it to `[3+]`；
* treat it as missing；
* inspect Test to decide where it belongs。

Because no Train row contributes code=1 for PAY_5/PAY_6:

> the WOE of the low bin is estimated from the Train observations actually present in `-2/-1/0`.

If future unseen data contains code=1:

> it receives that already learned low-bin WOE.

---

# 9. PAY Domain Coverage Validation

For each PAY variable, validate mapping over the complete approved raw domain:

`-2,-1,0,1,2,3,4,5,6,7,8`

Use synthetic values / mapping assertions.

Do not use Holdout Test.

Expected mapping:

## PAY_0

* -2 → low
* -1 → low
* 0 → low
* 1 → middle
* 2–8 → high

## PAY_2–PAY_6

* -2 → low
* -1 → low
* 0 → low
* 1 → low
* 2 → middle
* 3–8 → high

Any unexpected code outside the approved domain must:

> raise / flag an explicit validation error.

Do not silently assign unexpected codes to a bin.

---

# 10. PAY Grouping Rationale — Record, Do Not Re-open

The approved rationale must be documented:

### `-2/-1/0`

Across months these states occupy broadly similar lower-risk regions.

Their grouping is based on:

> modeling risk behavior

not a claim that their real-world semantics are identical.

### PAY_0 = 1

PAY_0 code=1 has substantial Train support and materially intermediate risk.

Therefore it remains its own bin.

### PAY_2–PAY_4 = 1

Code=1 is too sparse to support standalone WOE estimation and is assigned to the lower-state bin.

### PAY_5/PAY_6 = 1

Not observed in Train, but explicitly pre-mapped to the lower-state bin for domain completeness.

### Code 2

A major risk boundary is preserved.

### High-severity tail

Sparse high codes are consolidated to avoid unstable tiny-state WOE and zero-event infinities.

No further debate or optimization occurs in Codex.

---

# 11. Expected PAY Final-bin Verification Anchors

From the approved Stage 4.2A Train counts, final results should reproduce approximately the following.

Use these as validation anchors, not hardcoded outputs.

## PAY_0

| Bin         |  Count |   Good |   Bad | Default Rate |       WOE |
| ----------- | -----: | -----: | ----: | -----------: | --------: |
| `[-2,-1,0]` | 18,506 | 15,966 | 2,540 |     13.7253% |  0.579659 |
| `[1]`       |  3,000 |  1,969 | 1,031 |     34.3667% | -0.611642 |
| `[2+]`      |  2,494 |    756 | 1,738 |     69.6872% | -2.091088 |

Final IV approximately:

`0.872146`

---

## PAY_2

| Bin           |  Count | Default Rate |       WOE |
| ------------- | -----: | -----------: | --------: |
| `[-2,-1,0,1]` | 20,473 |     16.2604% |  0.380338 |
| `[2]`         |  3,145 |     55.6121% | -1.484072 |
| `[3+]`        |    382 |     60.4712% | -1.683777 |

Final IV approximately:

`0.548099`

---

## PAY_3

| Bin           |  Count | Default Rate |       WOE |
| ------------- | -----: | -----------: | --------: |
| `[-2,-1,0,1]` | 20,642 |     17.1786% |  0.314386 |
| `[2]`         |  3,049 |     51.8203% | -1.331482 |
| `[3+]`        |    309 |     59.2233% | -1.631843 |

Final IV approximately:

`0.414434`

---

## PAY_4

| Bin           |  Count | Default Rate |       WOE |
| ------------- | -----: | -----------: | --------: |
| `[-2,-1,0,1]` | 21,190 |     17.9424% |  0.261614 |
| `[2]`         |  2,542 |     52.6357% | -1.364165 |
| `[3+]`        |    268 |     63.0597% | -1.793417 |

Final IV approximately:

`0.359543`

---

## PAY_5

| Bin           |  Count | Default Rate |       WOE |
| ------------- | -----: | -----------: | --------: |
| `[-2,-1,0,1]` | 21,636 |     18.4738% |  0.225930 |
| `[2]`         |  2,102 |     54.6147% | -1.443752 |
| `[3+]`        |    262 |     62.5954% | -1.773538 |

Final IV approximately:

`0.327106`

Note:

> Train count for low bin does not include code=1 because Train contains none, but the mapping rule still includes code=1.

---

## PAY_6

| Bin           |  Count | Default Rate |       WOE |
| ------------- | -----: | -----------: | --------: |
| `[-2,-1,0,1]` | 21,526 |     18.6565% |  0.213847 |
| `[2]`         |  2,230 |     50.6726% | -1.285546 |
| `[3+]`        |    244 |     66.8033% | -1.957940 |

Final IV approximately:

`0.288166`

Same domain-complete code=1 rule applies.

---

# 12. FINAL delinquent_months GROUPING — FROZEN

Final bins:

* `0`
* `1`
* `2`
* `3`
* `4+`

where:

`4+ = {4,5,6}`

Do not use the earlier candidate:

`4-5 / 6`

That option was explicitly rejected during Human Checkpoint.

---

# 13. delinquent_months Rationale

The raw risk progression was strongly ordinal.

The final `4+` tail is chosen because it has a stable, generalizable interpretation:

> at least four of the six observed months show PAY > 0.

It is **not** chosen merely because raw values 4 and 5 happened to have similar default rates in this Train sample.

This distinction must be documented.

Expected `4+` Train result:

* Count = 2,064
* Good = 738
* Bad = 1,326
* Default Rate ≈ 64.2442%
* WOE ≈ -1.844617

Expected final total IV:

`≈ 0.871421`

---

# 14. FINAL max_delinquency GROUPING — FROZEN

Final bins:

* `0`
* `1`
* `2`
* `3+`

where:

`3+ = {3,4,5,6,7,8}`

Rationale:

> severity 3+ forms a high-severity tail, while individual high states are too sparse and noisy for stable standalone WOE.

Expected `3+` result:

* Count = 939
* Good = 351
* Bad = 588
* Default Rate ≈ 62.6198%
* WOE ≈ -1.774579

Expected final total IV:

`≈ 0.794583`

---

# 15. FINAL severe_delinquent_months GROUPING — FROZEN

Final bins:

* `0`
* `1`
* `2`
* `3`
* `4+`

where:

`4+ = {4,5,6}`

Do not implement:

`4-5 / 6`.

The final `4+` interpretation is:

> at least four of the six observed months have PAY ≥ 2.

This provides a structurally meaningful repeated-severe-delinquency group rather than a Train-specific local merge.

Expected `4+` result:

* Count = 1,785
* Good = 596
* Bad = 1,189
* Default Rate ≈ 66.6106%
* WOE ≈ -1.949266

Expected final total IV:

`≈ 0.846475`

---

# 16. Categorical Grouping — FROZEN

No category merging.

## SEX

Keep raw two categories unchanged.

## EDUCATION_GROUP

Keep:

* Graduate School
* University
* High School
* Other / Unknown

`Other / Unknown` remains a sparse special category.

Do not merge it into another education level.

## MARRIAGE_GROUP

Keep:

* Married
* Single
* Other / Unknown

`Other / Unknown` remains a sparse special category.

Do not merge it into Married or Single.

---

# 17. Sparse Categoricals

Record the Stage 4.2A evidence:

EDUCATION Other/Unknown:

* Count = 386
* Train share ≈ 1.61%

MARRIAGE Other/Unknown:

* Count = 302
* Train share ≈ 1.26%

They remain separate because:

> no sufficiently defensible semantic mapping exists into another category.

Their future stability/selection is explicitly deferred to:

* Stage 4.4 variable screening；
* Stage 4.6 stability analysis。

Do not solve that question now.

---

# 18. Final WOE / IV Tables

For each of exactly 12 variables, produce final grouped table with:

* Final Bin
* Raw Values Included
* Count
* % Train
* Good
* Bad
* Default Rate
* Dist Good
* Dist Bad
* WOE
* IV Contribution

Also report:

> Final grouped total IV

for each variable.

Do not rank variables.

Do not add Keep/Drop recommendations.

---

# 19. Infinite WOE Resolution

Stage 4.2A had six zero-Good / zero-Bad raw PAY states.

After Human-approved tail consolidation:

> all final PAY bins are expected to contain both Good and Bad observations.

Therefore:

* WOE must be finite；
* IV contribution must be finite；
* no 0.5 smoothing should be necessary。

Codex must explicitly assert:

* no final grouped bin has Good=0；
* no final grouped bin has Bad=0；
* no final WOE is ±inf；
* no final IV contribution is inf/NaN。

If this unexpectedly fails:

> BLOCKED。

Do not silently apply 0.5 smoothing because it should not be required under the approved grouping.

---

# 20. Final Bin Monotonicity Diagnostic

For ordinal predictors:

* six PAY variables；
* delinquent_months；
* max_delinquency；
* severe_delinquent_months；

report the ordered final-bin:

* Default Rate
* WOE

and verify the intended broad risk ordering.

Do not alter bins if a tiny numerical imperfection occurs.

Recall Stage 4.1:

> monotonicity preferred, not mandatory.

This check is diagnostic, not a new optimization step.

---

# 21. Final Diagnostic Figures

Append to the existing Notebook.

Create final-binning diagnostic figures for:

* PAY_0
* PAY_2
* PAY_3
* PAY_4
* PAY_5
* PAY_6
* delinquent_months
* max_delinquency
* severe_delinquent_months

Continue numbering after Phase A:

* `BIN-10`
  through
* `BIN-18`

Each should show:

* final grouped Default Rate；
* final grouped Train support。

Include concise:

* Final grouping
* Why this grouping was approved
* Observed result
* Stage 4.4 note

Do not turn the note into variable selection.

Categorical final figures are optional.

---

# 22. In-memory Bin Columns

Inside Notebook only, create clearly named final grouped variables, for example:

* `PAY_0_BIN`
* `PAY_2_BIN`
* ...
* `delinquent_months_BIN`
* `max_delinquency_BIN`
* `severe_delinquent_months_BIN`

Categorical fields may remain as existing categorical columns.

Names may differ slightly if needed for clean code, but must be explicit and deterministic.

Do not overwrite raw PAY or raw summary columns.

---

# 23. Mapping Validation

Assert:

* no Train row unmapped；
* row count remains 24,000；
* target unchanged；
* ID unchanged；
* raw PAY unchanged；
* raw delinquency summaries unchanged；
* all grouped-bin counts sum to 24,000；
* all Good counts sum to 18,691；
* all Bad counts sum to 5,309；
* final mapping deterministic。

For PAY_5/PAY_6, additionally perform synthetic assertion:

`raw PAY code 1 → low bin`

without reading Holdout Test.

---

# 24. No New Feature Engineering

Stage 4.2B does not authorize creation of new predictive concepts.

Only approved bin representations may be added in memory.

Do not create:

* new PAY summaries；
* weighted delinquency；
* PAY trend；
* transition variables；
* interactions；
* new demographic variables。

---

# 25. No Variable Selection

Forbidden:

* IV thresholding；
* ranking as model candidates；
* Keep / Drop；
* redundancy screening；
* correlation screening；
* VIF；
* selecting raw PAY vs summaries；
* selecting categoricals。

All 12 survive Stage 4.2 as candidate predictors.

Stage 4.4 decides selection.

---

# 26. No Continuous Binning

Do not analyze or bin:

* AGE
* LIMIT_BAL
* BILL_AMT1–6
* PAY_AMT1–6
* current_utilization
* avg_utilization
* max_utilization

These belong to Stage 4.3.

---

# 27. No Modeling

Do not run:

* Logistic Regression
* coefficients
* Odds Ratios
* VIF
* ROC
* AUC
* KS
* calibration
* confusion matrix
* thresholding
* sampling
* class weighting
* KFold / CV

---

# 28. Notebook Artifact

Extend, do not replace with another notebook:

`notebooks/05_pay_categorical_binning.ipynb`

Preserve the approved Stage 4.2A material.

Append a clearly separated:

> `Stage 4.2B — Human-Approved Final Binning Implementation`

section.

Suggested Phase B structure:

1. Human Checkpoint decisions
2. frozen mapping definitions
3. domain-completeness assertions
4. apply PAY grouping
5. PAY final WOE/IV tables
6. PAY final figures
7. apply delinquency-summary grouping
8. delinquency final WOE/IV tables
9. delinquency final figures
10. categorical final confirmation
11. zero-event / finite-WOE validation
12. final bin integrity checks
13. frozen Stage 4.2 binning summary
14. Stage 4.2 completion boundary

Notebook must clean-run from raw CSV with all Phase A + Phase B outputs.

---

# 29. Task Report Artifact

Extend existing:

`task_records/Stage_4.2_PAY_Categorical_Binning.md`

Do not create a separate Stage 4.2B report.

Append:

* Human Checkpoint decision record；
* explicit rejected alternatives；
* final PAY groupings；
* PAY_5/PAY_6 unseen-code-1 rule；
* domain-completeness rationale；
* final delinquency groupings；
* why `4+` replaced prior `4-5 / 6` candidate；
* categorical final decisions；
* grouped WOE/IV tables；
* grouped IV totals；
* zero-event resolution；
* confirmation no smoothing needed；
* final figures；
* mapping assertions；
* Holdout protection；
* scope exclusions；
* Notebook clean-run；
* protected-input integrity；
* Git state；
* final Reviewer handoff status。

Update status to:

`Stage 4.2B implementation complete — awaiting final Stage 4.2 Reviewer validation.`

---

# 30. Immutable Architect Specification

Append this full Stage 4.2B Specification to the same Task Report using:

`================ BEGIN ARCHITECT SPECIFICATION — STAGE 4.2B ================`

and

`================ END ARCHITECT SPECIFICATION — STAGE 4.2B ================`

Preserve verbatim.

Do not alter the existing Stage 4.2A specification or Reviewer Result.

---

# 31. Explicit Rejected Alternatives

Task Report must record that Human Checkpoint considered and rejected:

### Rejected

`PAY_5/PAY_6 = [-2,-1,0] / [2] / [3+]` as an incomplete domain rule.

### Approved instead

`[-2,-1,0,1] / [2] / [3+]`

even though code=1 is absent from Train for PAY_5/PAY_6.

Also rejected for:

* `delinquent_months`
* `severe_delinquent_months`

the Train-specific grouping:

`0 / 1 / 2 / 3 / 4-5 / 6`

Approved instead:

`0 / 1 / 2 / 3 / 4+`

Reason:

> avoid overfitting a local Train coincidence and prefer a stable, interpretable persistence threshold.

---

# 32. Holdout Test Protection

Stage 4.2B may not answer:

* whether final bins work well on Test；
* whether Test contains PAY_5/PAY_6 code=1；
* whether Test bin frequencies differ；
* Test IV / WOE；
* Test default rates。

The first true final Holdout evaluation remains Stage 5.

---

# 33. Files Allowed to Change

Only:

* `notebooks/05_pay_categorical_binning.ipynb`
* `task_records/Stage_4.2_PAY_Categorical_Binning.md`

No other file changes authorized.

Do not create:

* CSV mappings；
* pickle/joblib；
* processed datasets；
* model files；
* external figures；
* `src/`
* `tests/`
* Stage 4.3 notebook/report。

---

# 34. Git Rules

Codex:

* no git add
* no commit
* no push

After Codex completion:

> final Stage 4.2 artifacts go to Reviewer.

Reviewer workflow:

### FAIL

* no commit；
* return fixes。

### PASS

In the same Reviewer round:

1. append final Stage 4.2 Reviewer PASS result；
2. create Stage 4.2 local milestone commit；
3. verify post-commit state。

Expected commit scope:

exactly:

* `notebooks/05_pay_categorical_binning.ipynb`
* `task_records/Stage_4.2_PAY_Categorical_Binning.md`

Recommended commit message:

`Finalize PAY and categorical binning`

No push.

---

# 35. Stage 4.2B Completion Criteria

All must pass:

1. correct Stage 4.1 HEAD；
2. expected Phase A handoff state；
3. Phase A Reviewer PASS present；
4. Holdout sealed；
5. frozen PAY mapping implemented exactly；
6. PAY_0 uses `[-2,-1,0] / [1] / [2+]`；
7. PAY_2–PAY_6 use `[-2,-1,0,1] / [2] / [3+]`；
8. synthetic PAY_5 code1 maps low；
9. synthetic PAY_6 code1 maps low；
10. no Test lookup used for code1 decision；
11. unexpected PAY domain values explicitly rejected；
12. delinquent_months uses `0/1/2/3/4+`；
13. max_delinquency uses `0/1/2/3+`；
14. severe_delinquent_months uses `0/1/2/3/4+`；
15. prior `4-5/6` option not implemented；
16. SEX unchanged；
17. EDUCATION_GROUP unchanged；
18. MARRIAGE_GROUP unchanged；
19. 12 final WOE/IV tables produced；
20. grouped counts reconcile；
21. grouped Good/Bad reconcile；
22. all WOE finite；
23. all IV contributions finite；
24. no smoothing used；
25. PAY raw fields unchanged；
26. raw summary fields unchanged；
27. final diagnostic figures BIN-10–BIN-18 produced；
28. no variable selection；
29. no continuous binning；
30. no Logistic；
31. no KFold；
32. no AUC/KS；
33. no Test target analysis；
34. Notebook clean-runs；
35. Task Report updated；
36. Stage 4.2A history preserved；
37. Stage 4.2B immutable spec preserved；
38. only two authorized artifacts changed；
39. no staging；
40. no commit；
41. no push；
42. stop for final Reviewer。

---

# 36. Codex Completion Report

Return concise report including:

* pre-flight
* branch / HEAD
* Phase A Reviewer PASS verification
* final PAY grouping implemented
* PAY_5/PAY_6 synthetic code1 validation
* final delinquency grouping implemented
* categorical grouping preserved
* grouped WOE/IV completion
* grouped IV totals
* zero-event resolution
* confirmation no smoothing
* final monotonicity diagnostics
* BIN-10–BIN-18 status
* Holdout sealing
* Notebook clean-run
* protected-input integrity
* Git status
* confirmation:

  * no variable selection
  * no continuous binning
  * no Logistic
  * no KFold
  * no Test target analysis
  * no commit
  * no push

Final line:

`Stage 4.2B implementation complete — awaiting final Stage 4.2 Reviewer validation and milestone commit.`

================ END ARCHITECT SPECIFICATION — STAGE 4.2B ================

# REVIEWER RESULT

## Verdict

PASS

## Requirement Check

- Stage 4.2A remains complete and its previously reviewed Notebook cells, saved outputs, figures, Task Report history, and interim Reviewer PASS are preserved unchanged.
- Stage 4.2B implements the frozen Human-approved mappings exactly: PAY_0 uses `[-2,-1,0] / [1] / [2+]`; PAY_2–PAY_6 use `[-2,-1,0,1] / [2] / [3+]`; PAY_5/PAY_6 synthetic code 1 maps to the low bin; unknown PAY-domain values are explicitly rejected.
- The three delinquency summaries use the required `0/1/2/3/4+`, `0/1/2/3+`, and `0/1/2/3/4+` groupings. The rejected `4-5 / 6` alternative is absent. SEX, EDUCATION_GROUP, and MARRIAGE_GROUP remain unchanged.
- All 12 required final Train-only WOE/IV tables are present with the required schema and complete Count/Good/Bad reconciliation. Every final bin contains both Good and Bad, every WOE/IV value is finite, and no smoothing is used.
- Final monotonicity diagnostics and inline figures BIN-10 through BIN-18 are present. No variable ranking/selection, continuous binning, modeling, KFold/CV, AUC/KS, sampling, or Stage 4.3 work was performed.
- Holdout use is limited to the authorized split-identity assertions before the Test frame is deleted. No Test occurrence, frequency, target, WOE/IV, or risk analysis is present.
- The Notebook clean-runs from a fresh kernel, the Task Report contains the complete immutable specifications for both phases, and only the two authorized Stage 4.2 artifacts changed.

## Independent Verification

- Rechecked repository pre-flight on `main` at `9d22109b9db4abfafbb653e0cb7b9068bda6a8d3`: tracked and staged diffs were empty, exactly the Stage 4.2 Notebook and Task Report were untracked, and no remote exists.
- Compared the current Task Report prefix byte-for-byte with the post-Phase-A reviewer backup and compared the current Notebook's first 40 cell objects with the reviewed Phase A Notebook; both comparisons were exact.
- Parsed the saved Notebook independently: 78 cells (40 Markdown, 38 code), sequential execution counts 1–38, 117 saved outputs, 18 inline PNG outputs, and zero error outputs.
- Recreated the frozen 80/20 stratified split independently and reproduced the Train/Test ID hashes `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` and `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Independently reconstructed all approved mappings and recomputed all 12 final tables from the raw CSV. Counts, Good/Bad, Default Rate, WOE, IV contributions, and total IV values matched the Notebook; every table reconciled to 24,000 Count, 18,691 Good, and 5,309 Bad.
- Confirmed all nine ordered PAY/delinquency groupings have monotonically increasing Default Rate and monotonically decreasing WOE in the frozen bin order.
- Visually inspected BIN-10 through BIN-18 and confirmed titles, bin labels, rates, support counts, ordering, and plotted trends match the independently recomputed results.
- Executed all 38 code cells in order from a separate fresh local kernel without saving over the project Notebook: execution counts were 1–38, all 18 PNG figures were regenerated, and no execution error occurred. The fresh run emitted 116 outputs; the saved run's one additional output is a non-error stderr environment warning in the import cell and does not affect results.
- Verified all 18 protected tracked files against the recorded pre-execution SHA-256 snapshot; every file matched. Rechecked Notebook SHA-256 `79db85a8bbc85ba2ccd636e60dd98d2920b9b60dbd44cf8c204301f9ac000796` before appending this Reviewer Result.

## Issues Found

No issues affecting Stage 4.2 acceptance were found.

## Required Fixes

None

## Architect Decision Needed

None
