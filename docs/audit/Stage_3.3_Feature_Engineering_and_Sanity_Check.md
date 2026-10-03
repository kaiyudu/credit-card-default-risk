# Stage 3.3 — Feature Engineering and Sanity Check

## 1. Task Identification and Phase Status

- Phase: `Stage 3.3 — Phase A`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Execution date: 2026-08-13
- Status: `Stage 3.3 — Phase A completed; Test application pending Architect/user confirmation.`
- Scope: replay the locked split, seal Test, implement the frozen transformations on Train only, and complete sanity checks.
- This report does not claim that Stage 3.3 is complete or Reviewer-approved.

## 2. Immutable Architect Specification

The immutable block below is copied verbatim from the formal Phase A Prompt.

================ BEGIN ARCHITECT SPECIFICATION ================

# Stage 3.3 — Phase A
# Train Feature Engineering Implementation & Sanity Check

## 1. Stage Context

Stage 3 已固定为：

- Stage 3.1 — Train/Test Split + Train-only EDA
- Stage 3.2 — Feature Engineering Design
- Stage 3.3 — Feature Engineering Implementation + Sanity Check + Apply to Test

Stage 3.1 已完成并通过 Reviewer。

Stage 3.2 已由 Architect 与用户基于：

- Stage 2 Luca Audit；
- Stage 3.1 实际 Train-only EDA；
- 用户学习目标；
- 信用风险业务解释；

完成讨论，并正式冻结 Feature Engineering Specification。

当前进入：

> **Stage 3.3 Phase A**

Phase A 只负责：

> 在 Train 上实现已经冻结的规则，并完成 Sanity Check。

**本轮严禁将 Feature Engineering 应用到 Holdout Test。**

用户和 Architect 在查看 Phase A 实际结果并确认无误后，才会另行授权 Phase B 将完全相同的规则应用到 Test。

---

# 2. Git Baseline

项目路径：

`/Users/yu/2.工作/Risk Analysis Project`

Stage 3.1 的 parent baseline 为：

`4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`

Stage 3.1 已通过 Reviewer，并按照项目规则应已有独立 local milestone commit。

由于当前 Architect 未取得 Stage 3.1 最终 commit hash，Codex 不得猜测 hash。

Pre-flight 必须实际确认：

1. branch = `main`；
2. working tree clean；
3. staged diff empty；
4. no remote；
5. current HEAD 是已审核通过的 Stage 3.1 commit；
6. current HEAD 的 parent 为：
   `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`；
7.该 HEAD 的变更范围仅对应 Stage 3.1 已审核产物，而无额外未授权内容；
8. Stage 3.1 Notebook 和 Task Report 存在。

如任一条件不满足：

- 不 reset；
- 不 stash；
- 不 checkout；
- 不覆盖文件；
- 返回 BLOCKED 并报告实际 Git 状态。

---

# 3. Required Inputs

Codex 应读取：

- `data/raw/UCI_Credit_Card.csv`
- `notebooks/02_train_test_split_and_eda.ipynb`
- `task_records/Stage_3.1_Train_Test_Split_and_EDA.md`
- `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`

可以读取 Stage 3.1 / Stage 2 结果作为证据。

不得修改这些文件。

---

# 4. Locked Train/Test Split

必须重新从正式 raw CSV 使用 Stage 3.1 已锁定的 deterministic split：

- Train = 80%
- Test = 20%
- `random_state=42`
- `stratify=default.payment.next.month`

必须重新验证：

- Train rows = 24,000
- Test rows = 6,000
- no ID overlap
- union covers all 30,000 IDs

必须重现 Stage 3.1 的 stable ID-set hashes：

Train:

`bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`

Holdout Test:

`d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

如果不能精确重现：

> 返回 BLOCKED，不继续 Feature Engineering。

---

# 5. Holdout Test Protection — Phase A

虽然 split 时需要确认 Test ID hash，但完成 split integrity assertions 后：

> **Phase A 不得对 Test 执行 Feature Engineering。**

不得：

- 创建 Test utilization；
- 创建 Test delinquency summary；
- recode Test EDUCATION；
- recode Test MARRIAGE；
- 查看 Test 新 feature distribution；
- 查看 Test target relationship；
- 根据 Test 修改任何规则。

建议在完成 split assertions 后：

- 明确将 Test frame 隔离；
- 后续所有 Phase A transformation 和 sanity checks 只引用 Train。

---

# 6. Frozen Feature Engineering Decisions

以下规则已经由 Architect + 用户正式确认。

Codex 不得重新判断是否值得加入。

Codex 不得根据数据结果擅自增加、删除或修改 feature。

---

# 7. ID Treatment

`ID`：

- 保留作为 row identifier / audit key；
- 用于 split integrity 和 traceability；
- **不进入 predictor feature set**。

不得将 ID 数值作为模型候选变量。

不得删除 raw dataset 中的 ID。

---

# 8. EDUCATION Treatment

正式建模 grouping：

- `1` → `Graduate School`
- `2` → `University`
- `3` → `High School`
- `0 / 4 / 5 / 6` → `Other / Unknown`

必须明确：

- 官方 code 4 表示 Others；
- 0 / 5 / 6 的准确业务语义没有得到确认；
- 此处是 modeling grouping；
- 不宣称 0 / 4 / 5 / 6 在真实业务意义上完全相同。

建议生成清晰的 cleaned categorical field，例如：

`EDUCATION_GROUP`

Raw `EDUCATION` 可以保留用于 audit，但：

> predictor-ready feature specification 中使用 cleaned group，而不是同时使用 raw EDUCATION + cleaned EDUCATION造成重复表达。

---

# 9. MARRIAGE Treatment

正式 grouping：

- `1` → `Married`
- `2` → `Single`
- `0 / 3` → `Other / Unknown`

建议生成：

`MARRIAGE_GROUP`

同样：

- raw `MARRIAGE` 可保留用于 audit；
- predictor-ready view 使用 cleaned grouping；
- 不同时把 raw + grouped 两份作为两个 predictor。

---

# 10. AGE Treatment

`AGE`：

- 保持原始连续值；
- 不创建 AgeBin；
- 不手工分年龄段；
- 不做 transformation；
- 不做 target-driven cut。

正式 AGE binning 留到 Stage 4 WOE。

---

# 11. SEX and LIMIT_BAL

保留原始：

- `SEX`
- `LIMIT_BAL`

本阶段：

- 不创建 SEX interaction；
- 不创建 LIMIT_BAL bins；
- 不 scale；
- 不 transform。

---

# 12. Demographic Interactions

Stage 3.1 已复核 Luca 的 SEX × MARRIAGE idea。

Stage 3.2 已决定：

> **不创建 `SE_MA`。**

同时不继续搜索：

- `SE_AG`
- SEX × AGE
- AGE × MARRIAGE
- EDUCATION × SEX
- EDUCATION × MARRIAGE
- 其他 demographic interaction

不得进行 interaction fishing。

---

# 13. Raw PAY Treatment

六个月原始 repayment status：

- `PAY_0`
- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`

全部：

> **原值保留。**

不得：

- binary mapping；
- 将 `-2/-1/0` 合并；
- 给 `-2` 或 `0` 编造业务定义；
- Stage 3 做 PAY binning。

PAY grouping / WOE binning 留到 Stage 4。

---

# 14. New Delinquency Features

仅创建以下三个 delinquency summary features。

使用六个月：

`PAY_0, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6`

## A. `delinquent_months`

定义：

过去六个月中 `PAY > 0` 的月份数量。

取值理论范围：

`0–6`

它表达：

> delinquency frequency / persistence

---

## B. `max_delinquency`

定义：

对每个月先只在这个 summary calculation 中计算：

`max(PAY_i, 0)`

然后取六个月最大值。

也就是说：

- 所有六个月都 <= 0 → `max_delinquency = 0`
- 存在正 delinquency → 取最大正 severity

该计算：

> 不修改 raw PAY columns。

---

## C. `severe_delinquent_months`

定义：

过去六个月中：

`PAY >= 2`

的月份数量。

理论范围：

`0–6`

表达：

> repeated severe delinquency

---

# 15. Raw BILL_AMT Treatment

全部保留：

- `BILL_AMT1`
- `BILL_AMT2`
- `BILL_AMT3`
- `BILL_AMT4`
- `BILL_AMT5`
- `BILL_AMT6`

包括：

- zero；
- negative values；
- extreme positive values。

不得：

- 删除 negative BILL；
- 把 raw BILL negative 改为 0；
- winsorize；
- clip；
- log-transform raw predictors；
- 删除 extreme observations。

负 BILL 的信息继续由 raw BILL variables 自己保留。

---

# 16. Utilization Design

正式创建三个 utilization features。

重要：

> utilization 的业务含义定义为“已使用的授信额度比例”，因此用于 utilization calculation 的 BILL_AMT 使用 zero floor。

对每个月，仅在 utilization calculation 内定义：

`positive_bill_i = max(BILL_AMTi, 0)`

这些 `positive_bill_i` 是计算中间量。

除非技术上确有必要：

> 不要作为新的正式 predictor 单独保存。

Raw BILL_AMT 保持不变。

---

## A. `current_utilization`

定义：

`max(BILL_AMT1, 0) / LIMIT_BAL`

---

## B. `avg_utilization`

定义：

六个月每月 floored bill 的平均值，再除以 LIMIT_BAL：

`mean(max(BILL_AMT1,0), ..., max(BILL_AMT6,0)) / LIMIT_BAL`

等价实现可以接受，但必须数学一致。

---

## C. `max_utilization`

定义：

六个月 floored bill 的最大值，再除以 LIMIT_BAL：

`max(max(BILL_AMT1,0), ..., max(BILL_AMT6,0)) / LIMIT_BAL`

---

# 17. Utilization Important Rules

Stage 2 / 3.1 已确认：

`LIMIT_BAL > 0`

因此理论上不存在 division-by-zero。

但仍必须 assertion。

Utilization：

- minimum 应 >= 0；
- **不得 cap at 1**；
- utilization > 1 是允许的；
- 不 winsorize；
- 不 clip extreme values。

例如：

bill > limit

可以产生：

`utilization > 1`

这是保留的真实数据关系。

负 raw BILL：

- raw BILL 保持 negative；
- utilization calculation 中 floor 为 0。

这样：

- raw BILL 保留 credit-balance / negative-balance 信息；
- utilization 保持“used credit proportion”这一清晰业务含义。

---

# 18. Raw PAY_AMT Treatment

全部保留：

- `PAY_AMT1`
- `PAY_AMT2`
- `PAY_AMT3`
- `PAY_AMT4`
- `PAY_AMT5`
- `PAY_AMT6`

本 Stage 不增加：

- avg_payment
- total_payment
- max_payment
- payment_std
- payment ratio
- payment / bill ratios
- Luca Avg_exp
- 其他 payment-derived feature

---

# 19. Features Explicitly Not Created

不得创建：

- AgeBin
- SE_MA
- SE_AG
- SEX × AGE
- AGE × MARRIAGE
- other demographic interactions
- Avg_exp_*
- Closeness_*
- avg_bill
- max_bill
- bill_std
- total_bill
- avg_payment
- max_payment
- total_payment
- payment_std
- payment-to-bill ratios
- trend scores
- weighted delinquency scores
- transition matrix features
- consecutive delinquency-chain features
- decay scores
- any unapproved feature

---

# 20. Predictor-ready Feature Structure

Phase A 应形成一个清晰的 Train predictor-ready view。

应包含：

### Cleaned / retained original predictors

- `LIMIT_BAL`
- `SEX`
- cleaned `EDUCATION_GROUP`
- cleaned `MARRIAGE_GROUP`
- `AGE`
- six raw PAY fields
- six raw BILL_AMT fields
- six raw PAY_AMT fields

### New approved features

- `delinquent_months`
- `max_delinquency`
- `severe_delinquent_months`
- `current_utilization`
- `avg_utilization`
- `max_utilization`

### Non-predictor fields retained separately for traceability

- `ID`
- target:
  `default.payment.next.month`

不得：

- one-hot encode；
- WOE encode；
- scale；
- standardize；
- normalize；
- PCA；
- model-driven selection。

---

# 21. Train Feature Sanity Checks

Feature Engineering 完成后必须进行系统 sanity check。

## Structural

确认：

- Train row count remains 24,000；
- ID set unchanged；
- target unchanged；
- no duplicate ID；
- no accidental row drop；
- no accidental row duplication；
- predictor column names unique；
- predictor schema exactly matches frozen specification。

---

## Category mappings

检查：

`EDUCATION_GROUP`

只能出现：

- Graduate School
- University
- High School
- Other / Unknown

`MARRIAGE_GROUP`

只能出现：

- Married
- Single
- Other / Unknown

并验证 mapping counts 与原 raw codes mathematically reconcile。

---

## Delinquency checks

必须 assertion：

- `0 <= delinquent_months <= 6`
- `0 <= severe_delinquent_months <= 6`
- `severe_delinquent_months <= delinquent_months`
- `max_delinquency >= 0`
- `max_delinquency <= max raw positive PAY code observed`
- `delinquent_months == 0 → max_delinquency == 0`
- `severe_delinquent_months > 0 → max_delinquency >= 2`

抽取少量 deterministic sample rows，展示：

- original six PAY values；
- calculated three delinquency features；

供用户人工理解和核对。

---

## Utilization checks

必须确认：

- LIMIT_BAL > 0 for all Train rows；
- no NaN；
- no infinity；
- current_utilization >= 0；
- avg_utilization >= 0；
- max_utilization >= 0；
- `current_utilization <= max_utilization`；
- `avg_utilization <= max_utilization`；
- utilization > 1 可以存在且不得当成错误自动处理。

抽取少量 deterministic sample rows，包括尽可能覆盖：

- negative BILL；
- zero BILL；
- ordinary positive BILL；
- bill > limit；

展示：

- raw BILL values；
- LIMIT_BAL；
- calculated utilization；

使用户可以理解 floor-to-zero 的作用。

---

# 22. Descriptive Output for New Features

Phase A 可以输出 Train-only：

- count；
- mean；
- std；
- min；
- quantiles；
- max；
- zero frequency；
- utilization > 1 frequency；
- relevant value counts for discrete delinquency summaries。

目的只用于：

> sanity / understanding。

不得在 Phase A：

- 用 target 比较新 feature predictive power；
- 根据 default rate 删除 feature；
- 根据 AUC/IV/correlation 修改 feature；
- 重新打开 Stage 3.2 feature selection。

Feature specification 已冻结。

---

# 23. No Class-Imbalance Handling

保持 Train 的真实 target distribution。

不得：

- SMOTE；
- oversampling；
- undersampling；
- class weights；
- target resampling。

这些留给后续 modeling Stage。

---

# 24. No Modeling

本 Phase 严禁：

- Logistic Regression；
- WOE；
- IV-based selection；
- formal binning；
- Decision Tree；
- Random Forest；
- XGBoost；
- LightGBM；
- CV；
- KFold；
- model metrics；
- feature importance；
- threshold；
- calibration。

Stage 3.3 仍属于 Feature Engineering，不是 Modeling。

---

# 25. Notebook Artifact

创建：

`notebooks/03_feature_engineering_and_sanity_checks.ipynb`

Phase A Notebook 推荐结构：

1. Stage objective / frozen specification；
2. imports；
3. raw load；
4. deterministic split replay；
5. hash/integrity assertions；
6. Test isolation；
7. Train category transformations；
8. Train delinquency features；
9. Train utilization features；
10. predictor-ready schema；
11. sanity checks；
12. deterministic sample validation；
13. Train-only descriptive outputs；
14. Phase A conclusion；
15. explicit human checkpoint。

Notebook 必须：

- 可从 raw CSV clean run；
- 保存真实 outputs；
- 无 error outputs；
- 不读取 Test target relationships；
- 不修改 raw data；
- 不运行 Luca Notebook；
- 不运行 Stage 2/3.1 Notebook；
- 只读取它们的 saved evidence / specification。

---

# 26. Task Report Artifact

创建：

`task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

本轮先记录为：

> `Stage 3.3 — Phase A completed; Test application pending Architect/user confirmation.`

Report 至少包含：

- Stage / Phase objective；
- Git pre-flight；
- Stage 3.1 split replay；
- ID hashes；
- frozen Feature Engineering Specification；
- exact category mappings；
- exact delinquency formulas；
- exact utilization formulas；
- predictor-ready schema；
- Train-only sanity-check results；
- sample manual-validation rows；
- new-feature descriptive summaries；
- confirmation Test remained sealed；
- explicit list of unimplemented/rejected features；
- no imbalance handling；
- no modeling；
- files created/modified；
- protected-input integrity；
- Git status；
- human checkpoint status。

---

# 27. Immutable Architect Specification

Task Report 必须保存本次完整 Architect Specification。

必须使用：

`================ BEGIN ARCHITECT SPECIFICATION ================`

以及：

`================ END ARCHITECT SPECIFICATION ================`

中间逐字保存本 Specification。

不得 paraphrase 或根据执行结果改写。

---

# 28. Phase A Human Checkpoint

Codex 完成 Train Feature Engineering + Sanity Check 后：

> **必须停止。**

本轮不得将任何 transformation 应用给 Test。

后续流程：

1. Codex 返回 Phase A Completion Report；
2. 用户将 Notebook / Task Report 提供给 Architect；
3. Architect + 用户检查：
   - category mapping；
   - delinquency calculations；
   - utilization calculations；
   - distributions；
   - sanity assertions；
   - sample manual checks；
4. 如果用户确认无问题：
   - Architect 再生成 Stage 3.3 Phase B instruction；
5. Phase B 才允许：
   - 将完全相同的 frozen deterministic rules 应用到 Test；
   - 验证 Train/Test transformed schema；
   - 完成 Stage 3；
   - 交 Reviewer 做最终 Stage 3.3 review；
   - Reviewer PASS 后 local commit。

因此：

> **Phase A 完成后不要交 Reviewer 做最终 Stage 3.3 milestone commit。**

Stage 3.3 尚未完成。

---

# 29. Git Rules for Phase A

Codex：

- 不 git add；
- 不 commit；
- 不 push；
- 不加 remote；
- 不 amend；
- 不 reset；
- 不 rebase；
- 不 stash；
- 不修改 history。

Phase A changes 保留在 working tree，等待 Human Checkpoint 和后续 Phase B。

---

# 30. Project Protection

不得修改：

- raw CSV；
- Luca reference；
- Stage 0/1/2 artifacts；
- Stage 3.1 Notebook；
- Stage 3.1 Task Report；
- README；
- requirements；
- unrelated project files。

本轮原则上只新增：

- `notebooks/03_feature_engineering_and_sanity_checks.ipynb`
- `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

不创建：

- `src/`
- `tests/`
- processed train/test CSV
- model artifacts
- feature catalog
- external figures
- additional analysis files

---

# 31. Phase A Completion Criteria

全部满足才可停止并交给 Architect/user：

1. Git baseline verified；
2. Stage 3.1 split hashes exactly reproduced；
3. Test sealed after split verification；
4. only Train transformed；
5. ID excluded from predictor set but retained for traceability；
6. EDUCATION mapping correct；
7. MARRIAGE mapping correct；
8. AGE continuous；
9. raw PAY unchanged；
10. raw BILL unchanged；
11. raw PAY_AMT unchanged；
12. exactly 3 approved delinquency features created；
13. exactly 3 approved utilization features created；
14. negative BILL only floored inside utilization calculation；
15. utilization not capped at 1；
16. no unauthorized feature；
17. predictor-ready schema matches frozen specification；
18. Train row/ID/target integrity preserved；
19. no NaN/inf introduced；
20. delinquency assertions pass；
21. utilization assertions pass；
22. deterministic sample rows saved；
23. Train-only descriptive sanity output saved；
24. no target-driven feature reassessment；
25. no imbalance handling；
26. no WOE/binning；
27. no modeling；
28. no Test transformation；
29. Notebook clean-runs；
30. Task Report complete；
31. protected inputs unchanged；
32. no Git write；
33. no commit；
34. no push。

---

# 32. Codex Completion Report

Codex 最终仅返回简明 Phase A Completion Report，包括：

- pre-flight result；
- current branch / HEAD；
- files created；
- split hash reproduction；
- Test isolation confirmation；
- implemented category mappings；
- implemented 6 new feature names；
- predictor-ready feature count / schema；
- sanity-check result；
- sample validation result；
- NaN/inf result；
- utilization >1 observation count；
- Notebook clean-run status；
- protected input integrity；
- Git status；
- explicit confirmation:
  - Test not transformed
  - no modeling
  - no imbalance handling
  - no commit
  - no push
- final line:

`Stage 3.3 Phase A complete — awaiting Architect/user human checkpoint before Test application.`

================ END ARCHITECT SPECIFICATION ================

## 3. Git Pre-flight

All required conditions passed before the first project write.

| Check | Actual result |
|---|---|
| Git top-level | `/Users/yu/2.工作/Risk Analysis Project` |
| Branch | `main` |
| HEAD | `a8ca9707b93e27f4f2f310349640fd7efb0e098d` |
| HEAD subject | `Establish train-only EDA baseline` |
| HEAD parent | `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3` |
| HEAD change scope | Only `notebooks/02_train_test_split_and_eda.ipynb` and `task_records/Stage_3.1_Train_Test_Split_and_EDA.md` |
| Working tree | Clean |
| Staged diff | Empty |
| Remote | None |
| Phase A output paths | Both absent |

Stage 3.1 Notebook and Task Report exist and the committed Task Report contains Reviewer `PASS`. No reset, restore, stash, checkout, clean, index write, or remote operation was performed during Phase A.

## 4. Inputs Read Without Execution

- `data/raw/UCI_Credit_Card.csv`
- `notebooks/02_train_test_split_and_eda.ipynb` — saved source/output only
- `task_records/Stage_3.1_Train_Test_Split_and_EDA.md`
- `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`
- `notebooks/01_luca_audit_and_data_understanding.ipynb` — saved evidence only
- Luca reference identity/static evidence only where needed

Luca, Stage 2, and Stage 3.1 Notebooks were not executed or saved. The Phase A Notebook independently starts from the formal raw CSV.

## 5. Stage 3.1 Split Replay

| Item | Frozen value / actual Phase A result |
|---|---|
| Method | `sklearn.model_selection.train_test_split` |
| Train/Test | 80% / 20% |
| Seed | `random_state=42` |
| Stratification target | `default.payment.next.month` |
| Raw rows | 30,000 |
| Train rows | 24,000 |
| Test rows | 6,000 |
| ID overlap | 0 — assertion passed |
| Full coverage | Train union Test equals all 30,000 IDs — assertion passed |

Stable ID-set canonicalization: convert IDs to canonical decimal integer strings, sort numerically, join with one newline, add no trailing newline, UTF-8 encode, then SHA-256.

- Train: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` — exact match.
- Test: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f` — exact match.

Feature Engineering executed only after both exact-hash assertions passed.

## 6. Holdout Test Sealing Evidence

Test was used only for split row count, ID overlap/coverage, and stable hash assertions. Immediately afterward, the Test frame, Test ID set, and full raw frame were deleted from the live Notebook state. No mapping, delinquency formula, utilization formula, sample selection, distribution, target relationship, or other Phase A transformation received Test as input.

Saved Notebook status:

`SEALED — Test deleted after row/ID/hash assertions; no Phase A transformation applied`

Phase B remains pending Architect/user confirmation.

## 7. Frozen Feature Engineering Specification Implemented

Exactly two cleaned categorical fields and six approved new features were added to the Train working frame. No frozen decision was reopened based on outputs.

### Category mappings

`EDUCATION_GROUP`:

- 1 → `Graduate School`
- 2 → `University`
- 3 → `High School`
- 0/4/5/6 → `Other / Unknown`

Official code 4 is Others. Exact business meanings of 0/5/6 remain unconfirmed. The grouping is a modeling choice and does not assert semantic identity.

`MARRIAGE_GROUP`:

- 1 → `Married`
- 2 → `Single`
- 0/3 → `Other / Unknown`

MARRIAGE code 0 remains semantically unconfirmed.

### Delinquency formulas

Using raw `PAY_0`, `PAY_2`, `PAY_3`, `PAY_4`, `PAY_5`, `PAY_6`:

- `delinquent_months = Σ 1(PAY_i > 0)`
- `max_delinquency = max(max(PAY_i, 0))`
- `severe_delinquent_months = Σ 1(PAY_i >= 2)`

Flooring is calculation-local; raw PAY columns remain unchanged.

### Utilization formulas

For calculation only, `positive_bill_i = max(BILL_AMTi, 0)`:

- `current_utilization = positive_bill_1 / LIMIT_BAL`
- `avg_utilization = mean(positive_bill_1,…,positive_bill_6) / LIMIT_BAL`
- `max_utilization = max(positive_bill_1,…,positive_bill_6) / LIMIT_BAL`

No `positive_bill_i` column enters the predictor view. Raw BILL fields retain negatives and extremes. Utilization was not capped, clipped, winsorized, or transformed.

## 8. Predictor-ready Train Schema

The predictor-ready view contains exactly 29 unique columns, in this frozen order:

1. `LIMIT_BAL`
2. `SEX`
3. `EDUCATION_GROUP`
4. `MARRIAGE_GROUP`
5. `AGE`
6. `PAY_0`
7. `PAY_2`
8. `PAY_3`
9. `PAY_4`
10. `PAY_5`
11. `PAY_6`
12. `BILL_AMT1`
13. `BILL_AMT2`
14. `BILL_AMT3`
15. `BILL_AMT4`
16. `BILL_AMT5`
17. `BILL_AMT6`
18. `PAY_AMT1`
19. `PAY_AMT2`
20. `PAY_AMT3`
21. `PAY_AMT4`
22. `PAY_AMT5`
23. `PAY_AMT6`
24. `delinquent_months`
25. `max_delinquency`
26. `severe_delinquent_months`
27. `current_utilization`
28. `avg_utilization`
29. `max_utilization`

`ID` and `default.payment.next.month` are retained in a separate 24,000 × 2 traceability/label structure. Raw EDUCATION and MARRIAGE remain in the audit working frame for reconciliation but do not enter the 29 predictors. No ID, target, duplicate raw category, intermediate positive-bill field, or unauthorized feature is present.

## 9. Structural and Raw-field Integrity Results

All saved assertions passed:

- 24,000 Train rows preserved; no row drop or duplication.
- Original index/order preserved.
- ID order/set unchanged and unique.
- Target unchanged.
- AGE, SEX, and LIMIT_BAL unchanged.
- Six raw PAY fields unchanged.
- Six raw BILL_AMT fields unchanged, including negative values.
- Six raw PAY_AMT fields unchanged.
- Exactly 2 grouping fields + 6 approved features added.
- Predictor names unique and schema exactly frozen.
- No missing or non-finite numeric predictor value.

The post-transformation preserved raw frame was compared exactly, value by value and dtype by dtype, with a deep pre-transformation Train copy.

## 10. Category Reconciliation

### EDUCATION

| Raw code | Group | Train count |
|---:|---|---:|
| 0 | Other / Unknown | 12 |
| 1 | Graduate School | 8,455 |
| 2 | University | 11,256 |
| 3 | High School | 3,903 |
| 4 | Other / Unknown | 97 |
| 5 | Other / Unknown | 235 |
| 6 | Other / Unknown | 42 |

Grouped counts: Graduate School 8,455; University 11,256; High School 3,903; Other / Unknown 386. Total = 24,000.

### MARRIAGE

| Raw code | Group | Train count |
|---:|---|---:|
| 0 | Other / Unknown | 47 |
| 1 | Married | 10,892 |
| 2 | Single | 12,806 |
| 3 | Other / Unknown | 255 |

Grouped counts: Married 10,892; Single 12,806; Other / Unknown 302. Total = 24,000.

All raw codes mapped exactly once; no missing/unmapped value exists; group counts mathematically reconcile with source-code counts.

## 11. Delinquency Sanity-check Results

Every required assertion passed:

- All three generated columns equal independent row-wise recalculations.
- `delinquent_months` and `severe_delinquent_months` stay within 0–6.
- `severe_delinquent_months <= delinquent_months`.
- `max_delinquency >= 0` and does not exceed observed raw positive maximum 8.
- `delinquent_months == 0` always implies `max_delinquency == 0`.
- `severe_delinquent_months > 0` always implies `max_delinquency >= 2`.

### Deterministic PAY manual-validation rows

| Case | ID | PAY_0 | PAY_2 | PAY_3 | PAY_4 | PAY_5 | PAY_6 | delinquent_months | max_delinquency | severe_delinquent_months |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| All six PAY <= 0 | 3 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| One positive month, max=1 | 19 | 1 | -2 | -2 | -2 | -2 | -2 | 1 | 1 | 0 |
| Multiple positive months, max=2 | 1 | 2 | 2 | -1 | -1 | -2 | -2 | 2 | 2 | 2 |
| Maximum severity 3 or 4 | 59 | -1 | 3 | 2 | 2 | 2 | 2 | 5 | 3 | 5 |
| Maximum severity at least 5 | 190 | 1 | 5 | 4 | 4 | 3 | 2 | 6 | 5 | 5 |

Rows were selected by fixed conditions and smallest eligible ID. Samples did not change any rule.

## 12. Utilization Sanity-check Results

Every required assertion passed:

- LIMIT_BAL strictly positive for all Train rows.
- All three formulas equal independent recalculations from untouched raw BILL fields.
- No NaN or infinity.
- All utilization values non-negative.
- `current_utilization <= max_utilization`.
- `avg_utilization <= max_utilization`.
- Raw BILL values, including negatives, unchanged.
- Values above 1 retained without capping.

### Deterministic BILL/utilization manual-validation rows

| Case | ID | LIMIT_BAL | BILL_AMT1 | BILL_AMT2 | BILL_AMT3 | BILL_AMT4 | BILL_AMT5 | BILL_AMT6 | current_utilization | avg_utilization | max_utilization |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| At least one negative BILL | 8 | 100,000 | 11,876 | 380 | 601 | 221 | -159 | 567 | 0.118760 | 0.022742 | 0.118760 |
| At least one zero BILL | 1 | 20,000 | 3,913 | 3,102 | 689 | 0 | 0 | 0 | 0.195650 | 0.064200 | 0.195650 |
| Ordinary positive bills, none above limit | 3 | 90,000 | 29,239 | 14,027 | 13,559 | 14,331 | 14,948 | 15,549 | 0.324878 | 0.188246 | 0.324878 |
| At least one BILL above LIMIT_BAL | 6 | 50,000 | 64,400 | 57,069 | 57,608 | 19,394 | 19,619 | 20,024 | 1.288000 | 0.793713 | 1.288000 |

The negative value in ID 8 remains raw; it contributes zero only inside the average/max utilization calculation.

## 13. Train-only New-feature Descriptive Summaries

No target grouping, default-rate analysis, correlation, IV, AUC, ranking, or feature reselection was performed.

| Feature | Mean | Std | Min | Median | P95 | P99 | Max | Zero count | Zero proportion |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| delinquent_months | 0.835708 | 1.550828 | 0 | 0 | 5 | 6 | 6 | 15,896 | 66.2333% |
| max_delinquency | 0.682875 | 1.066937 | 0 | 0 | 2 | 4 | 8 | 15,896 | 66.2333% |
| severe_delinquent_months | 0.709458 | 1.459640 | 0 | 0 | 5 | 6 | 6 | 17,276 | 71.9833% |
| current_utilization | 0.423506 | 0.410234 | 0 | 0.314936 | 1.012805 | 1.233520 | 6.455300 | 2,080 | 8.6667% |
| avg_utilization | 0.372518 | 0.350969 | 0 | 0.284486 | 0.959133 | 1.048268 | 5.364308 | 771 | 3.2125% |
| max_utilization | 0.494320 | 0.432641 | 0 | 0.429782 | 1.033013 | 1.565131 | 10.688575 | 771 | 3.2125% |

### Utilization above 1

| Feature | Count > 1 | Proportion > 1 |
|---|---:|---:|
| current_utilization | 1,685 | 7.0208% |
| avg_utilization | 508 | 2.1167% |
| max_utilization | 3,129 | 13.0375% |

Rows with at least one utilization feature above 1: **3,129 / 24,000 (13.0375%)**. These rows were retained unchanged.

## 14. Explicit Unimplemented / Rejected Features and Operations

Not created: AgeBin, SE_MA, SE_AG, any demographic interaction, Avg_exp, Closeness, avg/max/std/total bill, avg/max/std/total payment, payment ratios, trends, weighted delinquency, transition features, consecutive chains, decay scores, positive-bill predictor fields, or any other unapproved feature.

Not performed: one-hot encoding, WOE, formal binning, scaling, normalization, PCA, feature selection, target-driven reassessment, SMOTE, over/undersampling, class weighting, resampling, model fitting, CV/KFold, GridSearch, metrics, feature importance, thresholding, or calibration.

Train target distribution was left unchanged.

## 15. Notebook Clean-run Evidence

| Check | Result |
|---|---|
| Python | `/Users/yu/2.工作/Risk Analysis Project/.venv/bin/python`, 3.9.6 |
| Environment | pandas 2.3.3; numpy 2.0.2; scipy 1.13.1; scikit-learn 1.6.1; jupyter-client 8.6.3; ipykernel 6.31.0 |
| Execution | Fresh `python3` kernel, project root working directory |
| Code cells | 14, sequential execution counts 1–14 |
| Saved outputs | 33 |
| Error outputs / traceback | 0 / none |
| Notebook SHA-256 | `b686560b826ada41e771f5299beafbe28f77f476007263963ce8cae1f7f7832e` |
| Notebook size | 73,305 bytes |

No package was installed. The existing `jupyter_client`/`ipykernel` stack was used. A harmless destructor warning appeared after explicit kernel shutdown and after the Notebook had saved; it is not a Notebook error output.

## 16. Files Created or Modified

Created:

- `/Users/yu/2.工作/Risk Analysis Project/notebooks/03_feature_engineering_and_sanity_checks.ipynb`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

Modified existing project files: none.

No processed CSV, external figure, checkpoint, cache, model, source module, test suite, feature catalog, Stage 3.2 artifact, or Phase B artifact was created.

## 17. Protected-input Integrity

Pre-flight values to be compared in the final read-only verification:

| Protected input | Size (bytes) | SHA-256 |
|---|---:|---|
| Raw CSV | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |
| Luca reference Notebook | 805,686 | `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd` |
| Stage 2 Notebook | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` |
| Stage 0 record | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` |
| Stage 1 report | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` |
| Stage 2 report | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` |
| Stage 3.1 Notebook | 937,648 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` |
| Stage 3.1 Task Report | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` |
| README | 2,245 | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` |
| requirements | 95 | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` |

All existing tracked files are additionally protected by the clean pre-flight state and expected empty tracked/staged diffs after Phase A.

## 18. Git Status and Phase A Human Checkpoint

- Expected final branch/HEAD: `main` / `a8ca9707b93e27f4f2f310349640fd7efb0e098d`.
- Expected working tree: only the two Phase A artifacts untracked; no tracked or staged diff.
- Remote: none.
- No Git add, commit, push, pull, fetch, amend, reset, rebase, stash, restore, checkout, clean, index/history write, or remote change was performed.

Phase A Human Checkpoint is reached. Test application remains pending Architect/user review. Stage 3.3 is not yet complete, and this Phase A state is not for final milestone commit.

## 19. Incomplete Items, Deviations, and Limitations

- Incomplete within Phase A scope: none identified.
- Expected pending work: Phase B Test application, only after separate authorization.
- Deviations: none identified.
- Limitations: semantic uncertainty for undocumented raw category/status codes remains; no target-driven assessment is permitted in this frozen implementation phase.
- Unexpected artifacts/modifications: none observed.

## 20. Human Checkpoint Result

`Pending Architect/user review of Phase A calculations, samples, distributions, and sanity checks.`

# REVIEWER RESULT

## Verdict

PASS

This verdict applies only to **Stage 3.3 Phase A — Train Feature Engineering Implementation & Sanity Check**. It is not a final Stage 3.3 milestone verdict: Phase B Test application remains intentionally incomplete and separately gated by the Architect/user Human Checkpoint.

## Requirement Check

- Git pre-flight is consistent with the frozen baseline: branch `main`, HEAD `a8ca9707b93e27f4f2f310349640fd7efb0e098d`, expected parent, no tracked/staged changes, and no remote.
- The locked 80/20 stratified split is exactly reproducible with `random_state=42`; Train/Test contain 24,000/6,000 rows, have no ID overlap, cover all 30,000 IDs, and reproduce both required ID-set hashes.
- Holdout Test protection is satisfied. Test is used only for row/ID/hash integrity assertions and is deleted before any mapping or feature calculation. Phase A contains no Test transformation or Test descriptive/target analysis.
- Train category mappings exactly implement the frozen EDUCATION and MARRIAGE groupings and reconcile mathematically to all raw-code counts.
- Raw AGE, SEX, LIMIT_BAL, six PAY fields, six BILL_AMT fields, and six PAY_AMT fields are preserved. ID and target are retained separately and excluded from predictors.
- Exactly the three approved delinquency features and three approved utilization features are created with the frozen formulas. Negative BILL values are floored only inside utilization calculations; raw BILL values remain unchanged; utilization is not capped at 1.
- The predictor-ready Train view contains exactly the required 29 unique columns. No unauthorized feature, interaction, encoding, scaling, WOE/binning, feature selection, class-imbalance operation, modeling, CV, processed dataset, or Phase B artifact is present.
- Structural, mapping, delinquency, utilization, missing-value, infinity, sample-row, and descriptive sanity checks satisfy the Phase A completion criteria.
- The Notebook contains a saved clean run with execution counts 1–14, 33 outputs, and no error output. Protected inputs and prior-stage artifacts remain unchanged.
- The mandatory stop condition is respected: Stage 3.3 is not declared complete, Test application remains pending, and no Git staging or commit has been performed.

## Independent Verification

- Read the complete embedded Architect Specification and the Phase A execution report.
- Inspected every Notebook source cell, saved output, execution count, metadata, and data-flow reference to Test and target.
- Independently recalculated the locked split and reproduced Train hash `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` and Test hash `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Independently reproduced the category reconciliations, all six feature formulas and summaries, deterministic validation rows, zero counts, utilization-above-one counts, and the reported 3,129 rows with at least one utilization value above 1.
- Independently reran all 14 code cells in a fresh local kernel without overwriting the Notebook; all cells completed successfully and reproduced 33 outputs with no cell errors.
- Rechecked Git branch/HEAD, tracked/staged/untracked scope, remotes, Notebook hash, and protected-file hashes after verification.

## Issues Found

No issues affecting **Phase A** acceptance.

Stage 3.3 as a whole is not complete because Phase B has not been authorized or executed. This is the Architect-required checkpoint state, not a Phase A defect.

## Required Fixes

None for Phase A.

## Architect Decision Needed

Architect/user must complete the required Human Checkpoint and decide whether to authorize a separate Stage 3.3 Phase B instruction for applying the frozen deterministic transformations to Holdout Test.

No local commit was created because the Architect Specification explicitly prohibits a Phase A milestone commit.


# STAGE 3.3 PHASE B EXECUTION RECORD

## 21. Phase B Authorization and Scope

The Architect/user Human Checkpoint authorized Phase B through the immutable contract reproduced below. Phase B applied the Phase A-approved deterministic rules to Holdout Test and performed structural, arithmetic, schema, and non-target sanity validation only.

No feature decision was reopened. Test target was retained only as a separate, untouched label for ID-alignment and exact-value preservation assertions. No Test target statistic, relationship, selection, or evaluation was computed.

## 22. Phase B Git Pre-flight

All required checks passed before the first Phase B write.

| Check | Actual result |
|---|---|
| Repository root | `/Users/yu/2.工作/Risk Analysis Project` |
| Branch | `main` |
| HEAD | `a8ca9707b93e27f4f2f310349640fd7efb0e098d` |
| HEAD parent | `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3` |
| HEAD message | `Establish train-only EDA baseline` |
| Staged diff | Empty |
| Tracked unstaged diff | Empty |
| Remote | None |
| Authorized workspace drift | Exactly the two untracked Phase A artifacts |
| Extra changed/untracked artifact | None |

The two authorized pre-modification Phase A artifacts were:

| Artifact | Git classification | Size (bytes) | SHA-256 |
|---|---|---:|---|
| `notebooks/03_feature_engineering_and_sanity_checks.ipynb` | `??` | 73,305 | `b686560b826ada41e771f5299beafbe28f77f476007263963ce8cae1f7f7832e` |
| `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md` | `??` | 38,556 | `f7cbc757fd5b0fffdaa0582974c09e14999b5df70cb77512dcdeee618c6e670f` |

No reset, restore, stash, checkout, clean, index write, history write, or remote operation was performed.

## 23. Approved Phase A Baseline Verification

- The approved Phase A Notebook SHA-256 exactly matched `b686560b826ada41e771f5299beafbe28f77f476007263963ce8cae1f7f7832e`.
- The Phase A Task Report retained its execution evidence and immutable Architect Specification.
- The Task Report contained the independent Phase A Reviewer verdict `PASS`.
- The approved Notebook contained 28 cells (14 code + 14 markdown), sequential execution counts 1–14, 33 saved outputs, and zero error outputs or tracebacks.
- Static inspection found no prior Phase B Test transformation, WOE/IV/binning, imbalance handling, or modeling evidence.
- The pre-Phase-B Phase A immutable block SHA-256 was `5f6f86122806e2b4113f73796ef9a8a8077c28cb7d24ac16b092f4219123ca5a`; it was not edited by Phase B.

## 24. Immutable Architect Specification — Phase B

================ BEGIN ARCHITECT SPECIFICATION — PHASE B ================

# Stage 3.3 — Phase B
# Apply Frozen Feature Engineering to Holdout Test + Final Stage 3.3 Sanity Check

## 1. Stage Context

Stage 3 固定结构：

- Stage 3.1 — Train/Test Split + Train-only EDA
- Stage 3.2 — Feature Engineering Design
- Stage 3.3 — Feature Engineering Implementation + Sanity Check + Apply to Test

Stage 3.3 Phase A 已完成：

> Train Feature Engineering Implementation + Sanity Check

Phase A 已经：

- 通过 Codex execution；
- 通过 Phase A Reviewer；
- 通过 Architect/user Human Checkpoint。

因此现在正式授权：

> **Stage 3.3 Phase B**

Phase B 的唯一目的：

> 将 Phase A 已验证并冻结的完全相同 Feature Engineering rules 应用到 Holdout Test，并验证 Train/Test transformation schema 和数据完整性。

本 Phase：

- 不重新设计 feature；
- 不重新评价 feature；
- 不使用 Test target 做分析；
- 不进入 Stage 4；
- 不进行任何 modeling。

---

# 2. Phase A Accepted Baseline

Project root：

`/Users/yu/2.工作/Risk Analysis Project`

Current committed milestone baseline 应仍为：

Branch:

`main`

HEAD:

`a8ca9707b93e27f4f2f310349640fd7efb0e098d`

Commit message:

`Establish train-only EDA baseline`

Parent:

`4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`

Phase A 尚未 commit。

这是设计行为。

当前 working tree 预计只存在 Phase A 两个未提交 artifact：

- `notebooks/03_feature_engineering_and_sanity_checks.ipynb`
- `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

不得因为 working tree 不 clean 而自动 BLOCKED。

Phase B 的 pre-flight 应判断：

> 当前 non-clean state 是否**精确对应已批准的 Phase A artifacts**。

---

# 3. Phase B Git Pre-flight

开始任何修改前必须检查：

1. repository root 正确；
2. branch = `main`；
3. HEAD =
   `a8ca9707b93e27f4f2f310349640fd7efb0e098d`；
4. HEAD parent =
   `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`；
5. staged diff empty；
6. no remote；
7. 除 Phase A 两个 artifact 外，没有其他 tracked/untracked workspace drift；
8. 所有 Stage 0–3.1 protected inputs 未被修改；
9. Phase A Notebook 和 Task Report 均存在；
10. Phase A Task Report 明确记录 Reviewer `PASS`；
11. Phase A Notebook 为 approved saved Phase A version。

Phase A approved Notebook SHA-256：

`b686560b826ada41e771f5299beafbe28f77f476007263963ce8cae1f7f7832e`

如 Notebook hash 不匹配：

> BLOCKED。

不得猜测差异无害。

如果存在除这两个 Phase A artifact 外的任何额外 workspace change：

> BLOCKED。

不得 reset / restore / stash / checkout / clean。

---

# 4. Protected Inputs

不得修改：

- `data/raw/UCI_Credit_Card.csv`
- `notebooks/01_luca_audit_and_data_understanding.ipynb`
- `notebooks/02_train_test_split_and_eda.ipynb`
- Stage 0 / Stage 1 / Stage 2 / Stage 3.1 Task Reports
- Luca reference Notebook
- README
- requirements
- unrelated files

Phase A report 已记录以下 important protected hashes，可用于最终验证：

Raw CSV:

`a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1`

Stage 3.1 Notebook:

`b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d`

如 protected input integrity 发生变化：

> BLOCKED / FAIL，停止执行。

---

# 5. Files Allowed to Modify

Phase B 原则上只允许修改 Phase A 已创建的：

- `notebooks/03_feature_engineering_and_sanity_checks.ipynb`
- `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

不要新建：

- Phase B Notebook
- processed Train CSV
- processed Test CSV
- feature CSV
- model artifact
- `src/`
- `tests/`
- external figures
- additional report
- Stage 4 artifact

Phase B 是对现有 Stage 3.3 artifacts 的完成，不是新的独立 Stage。

---

# 6. Locked Split Replay

Notebook 必须继续从正式 raw CSV 独立 replay Stage 3.1 split：

- Train = 80%
- Test = 20%
- `random_state=42`
- `stratify=default.payment.next.month`

必须再次确认：

- Train rows = 24,000
- Test rows = 6,000
- no ID overlap
- union = all 30,000 IDs

Stable ID hashes 必须精确为：

Train:

`bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`

Test:

`d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

任何 mismatch：

> BLOCKED，不应用 Feature Engineering。

---

# 7. Core Phase B Principle

Phase B 不再进行 Feature Engineering Design。

必须：

> **Apply exactly the Phase A frozen deterministic rules to Test.**

不得根据 Test：

- default rate；
- feature distribution；
- correlation；
- outlier pattern；
- category frequency；
- utilization range；
- PAY distribution；

改变任何 transformation。

即使 Test 出现：

- 极端 utilization；
- rare PAY codes；
- rare demographic groups；
- unusual BILL_AMT；

也只能：

1. 验证是否符合 frozen formula；
2. 报告 structural/sanity result；
3. 不改变 rule。

---

# 8. Test Target Protection

Test target：

`default.payment.next.month`

可以：

- 作为 untouched label 与对应 ID 一起保留；
- 验证 row alignment；
- 验证 transformation 前后 target values 完全 unchanged。

不得：

- 计算 Test default rate；
- 按 Test target group feature；
- 计算 Test target correlation；
- 计算 Test IV；
- 计算 Test WOE；
- 计算 Test AUC；
- 计算 Test accuracy；
- 查看 predictor 与 Test target 的关系；
- 根据 Test target 做任何决策。

Test target 在 Phase B 中仍属于：

> **sealed outcome information**

直到后续正式 Final Evaluation。

---

# 9. Frozen EDUCATION Mapping

对 Train 和 Test 使用完全相同 mapping：

- `1` → `Graduate School`
- `2` → `University`
- `3` → `High School`
- `0 / 4 / 5 / 6` → `Other / Unknown`

正式 predictor：

`EDUCATION_GROUP`

Raw `EDUCATION` 不进入 predictor-ready schema。

如果 Test 出现 mapping specification 之外的新 raw EDUCATION code：

> 不自行建立新 group；
> BLOCKED 并报告。

---

# 10. Frozen MARRIAGE Mapping

完全相同 mapping：

- `1` → `Married`
- `2` → `Single`
- `0 / 3` → `Other / Unknown`

正式 predictor：

`MARRIAGE_GROUP`

如果 Test 出现未定义 code：

> BLOCKED。

不得自行处理。

---

# 11. Frozen Raw Predictor Treatment

以下字段 Train/Test 均原样保留：

- `LIMIT_BAL`
- `SEX`
- `AGE`
- `PAY_0`
- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`
- `BILL_AMT1`–`BILL_AMT6`
- `PAY_AMT1`–`PAY_AMT6`

不得：

- bin；
- clip；
- winsorize；
- scale；
- normalize；
- log transform；
- recode PAY；
- merge PAY `-2/-1/0`；
- alter negative BILL；
- alter PAY_AMT。

---

# 12. Frozen Delinquency Features

必须在 Test 创建与 Train 完全相同三个 feature。

## `delinquent_months`

\[
\sum_i 1(PAY_i > 0)
\]

六个月 PAY：

- PAY_0
- PAY_2
- PAY_3
- PAY_4
- PAY_5
- PAY_6

---

## `max_delinquency`

对每个月只在 calculation 内：

\[
\max(PAY_i,0)
\]

然后：

\[
\max_i \max(PAY_i,0)
\]

Raw PAY 不得修改。

---

## `severe_delinquent_months`

\[
\sum_i 1(PAY_i \ge 2)
\]

不得加入任何其他 PAY-derived feature。

---

# 13. Frozen Utilization Features

Raw BILL 保持原值。

仅在 utilization calculation 内：

\[
positive\_bill_i=\max(BILL\_AMT_i,0)
\]

不得把 `positive_bill_i` 保存为正式 predictor。

创建：

## `current_utilization`

\[
\frac{\max(BILL\_AMT1,0)}{LIMIT\_BAL}
\]

## `avg_utilization`

\[
\frac{
mean(
\max(BILL\_AMT1,0),\ldots,\max(BILL\_AMT6,0)
)
}{
LIMIT\_BAL
}
\]

## `max_utilization`

\[
\frac{
\max(
\max(BILL\_AMT1,0),\ldots,\max(BILL\_AMT6,0)
)
}{
LIMIT\_BAL
}
\]

必须：

- assertion `LIMIT_BAL > 0`
- allow utilization > 1
- 不 cap at 1
- 不 clip
- 不 winsorize
- 不根据 Test extreme value 改规则

---

# 14. Predictor-ready Schema

Train 和 Test 最终必须产生完全相同的 **29-column predictor schema**：

1. `LIMIT_BAL`
2. `SEX`
3. `EDUCATION_GROUP`
4. `MARRIAGE_GROUP`
5. `AGE`
6. `PAY_0`
7. `PAY_2`
8. `PAY_3`
9. `PAY_4`
10. `PAY_5`
11. `PAY_6`
12. `BILL_AMT1`
13. `BILL_AMT2`
14. `BILL_AMT3`
15. `BILL_AMT4`
16. `BILL_AMT5`
17. `BILL_AMT6`
18. `PAY_AMT1`
19. `PAY_AMT2`
20. `PAY_AMT3`
21. `PAY_AMT4`
22. `PAY_AMT5`
23. `PAY_AMT6`
24. `delinquent_months`
25. `max_delinquency`
26. `severe_delinquent_months`
27. `current_utilization`
28. `avg_utilization`
29. `max_utilization`

必须验证：

- identical column names；
- identical order；
- unique column names；
- compatible data types；
- no Test-only feature；
- no Train-only feature。

`ID` 和 target 均单独保留，不进入 predictors。

---

# 15. Train Regression Check

Phase B 加入 Test transformation 后：

> Phase A 已批准的 Train 结果不得发生改变。

必须重新确认 Train：

- 24,000 rows；
- same ID hash；
- 29 predictors；
- all Phase A category counts unchanged；
- all Phase A new-feature summaries unchanged；
- deterministic sample rows unchanged；
- Phase A assertions still pass。

尤其至少验证以下 Phase A accepted values：

- EDUCATION Other / Unknown = 386
- MARRIAGE Other / Unknown = 302
- `delinquent_months` zero count = 15,896
- `max_delinquency` zero count = 15,896
- `severe_delinquent_months` zero count = 17,276
- rows with at least one utilization > 1 = 3,129

这些是：

> regression checks

不是重新做 feature selection。

如 Phase B 修改导致这些 Train values 变化：

> FAIL / BLOCKED。

---

# 16. Test Structural Sanity Checks

Transformation 后必须验证：

- Test rows remains 6,000；
- Test ID order/set unchanged；
- no duplicate ID；
- target unchanged；
- no row loss；
- no row duplication；
- all 29 predictors exist；
- predictor names unique；
- no NaN introduced；
- no infinity introduced；
- raw AGE unchanged；
- raw SEX unchanged；
- raw LIMIT_BAL unchanged；
- six raw PAY unchanged；
- six raw BILL unchanged；
- six raw PAY_AMT unchanged。

---

# 17. Test Category Sanity Checks

确认：

`EDUCATION_GROUP` 只能包含：

- Graduate School
- University
- High School
- Other / Unknown

`MARRIAGE_GROUP` 只能包含：

- Married
- Single
- Other / Unknown

检查：

- every row mapped exactly once；
- no missing group；
- no unexpected raw code。

可以记录 group counts 作为 transformation sanity evidence。

但：

> 不与 Test target 做任何交叉分析。

---

# 18. Test Delinquency Sanity Checks

必须 assertion：

- `0 <= delinquent_months <= 6`
- `0 <= severe_delinquent_months <= 6`
- `severe_delinquent_months <= delinquent_months`
- `max_delinquency >= 0`
- `delinquent_months == 0 → max_delinquency == 0`
- `severe_delinquent_months > 0 → max_delinquency >= 2`

必须 independently recalculate 并确认三个 feature 与 frozen formulas 一致。

可以保存少量 deterministic Test sample rows：

- six raw PAY
- three derived delinquency features

只作为 arithmetic validation。

不得查看 sample target。

---

# 19. Test Utilization Sanity Checks

必须确认：

- LIMIT_BAL > 0；
- no NaN；
- no infinity；
- utilization >= 0；
- `current_utilization <= max_utilization`
- `avg_utilization <= max_utilization`
- raw BILL unchanged；
- negative raw BILL remains negative；
- negative BILL 仅在 utilization calculation 中 floor 为 0；
- utilization > 1 retained。

必须 independently recalculate formulas。

可以保存少量 deterministic Test sample rows，覆盖如可取得：

- negative BILL；
- zero BILL；
- ordinary positive BILL；
- BILL > LIMIT_BAL。

Sample output 不显示 target。

---

# 20. Test Descriptive Output Boundary

Phase B 可以输出有限的 non-target sanity statistics，例如：

- row count；
- missing/non-finite count；
- allowed category counts；
- delinquency feature min/max；
- utilization min/max；
- utilization >1 count。

目的只能是：

> transformation validation。

不要：

- 系统比较 Train/Test feature distributions；
- 进行 drift analysis；
- 做 statistical tests；
- 根据分布差异修改 feature；
- 用 Test 参与 feature selection。

Stage 3.3 不是 drift-analysis stage。

---

# 21. Explicitly Forbidden

Phase B 仍然不得：

- WOE；
- IV；
- binning；
- Logistic Regression；
- Decision Tree；
- Random Forest；
- XGBoost；
- LightGBM；
- KFold；
- cross-validation；
- feature importance；
- correlation-based selection；
- target-based analysis；
- threshold selection；
- calibration；
- class weighting；
- SMOTE；
- over/undersampling；
- hyperparameter tuning；
- final evaluation。

不得创建任何新 feature。

---

# 22. Notebook Update

继续使用：

`notebooks/03_feature_engineering_and_sanity_checks.ipynb`

不得另建 Notebook。

保留 Phase A 内容和 saved evidence。

在其后清晰增加 Phase B sections，例如：

1. Phase B authorization / objective
2. deterministic split replay
3. Train regression validation
4. Holdout Test frozen transformation
5. Test predictor-ready schema
6. Train/Test schema equality
7. Test structural sanity checks
8. Test category sanity checks
9. Test delinquency arithmetic validation
10. Test utilization arithmetic validation
11. protected-target confirmation
12. Stage 3.3 conclusion

Notebook clean run 后应从 raw CSV 可完整重现：

> Split → Train FE → Train sanity → Test FE → final schema validation

不得依赖手工内存状态。

---

# 23. Task Report Update

继续修改：

`task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

不得创建新的 Phase B report。

必须保留 Phase A execution evidence 和 Phase A Reviewer PASS。

新增 Phase B sections，至少包括：

- Phase B authorization；
- Git pre-flight；
- Phase A approved baseline verification；
- split replay；
- frozen Test transformation；
- Test structural integrity；
- Train regression checks；
- Train/Test schema equality；
- Test category mapping verification；
- Test delinquency sanity；
- Test utilization sanity；
- Test target protection；
- no unauthorized feature；
- no modeling；
- no imbalance handling；
- Notebook clean-run；
- protected-input integrity；
- final Git status；
- Stage 3.3 completion status。

---

# 24. Immutable Phase B Architect Specification

Task Report 必须追加保存本次完整 Phase B Architect Specification。

使用：

`================ BEGIN ARCHITECT SPECIFICATION — PHASE B ================`

以及：

`================ END ARCHITECT SPECIFICATION — PHASE B ================`

中间逐字保存本 Specification。

不得覆盖或修改现有 Phase A immutable specification。

---

# 25. Stage 3.3 Completion Status

如果所有 Phase B checks 通过：

Task Report 可以更新为：

> `Stage 3.3 implementation complete — pending final Reviewer validation.`

注意：

这不等于 Reviewer PASS。

Codex 不得自行宣称 Stage 3.3 CLOSED。

真正 closure 条件：

1. Phase B execution complete；
2. final Reviewer 独立检查整个 Stage 3.3；
3. Reviewer verdict = PASS；
4. Reviewer 在同轮创建 Stage 3.3 local milestone commit；
5. post-commit verification clean。

---

# 26. Git Rules

Codex Phase B：

- 不 git add；
- 不 commit；
- 不 push；
- 不 add remote；
- 不 amend；
- 不 reset；
- 不 rebase；
- 不 stash；
- 不 checkout；
- 不 clean。

Phase B 完成时应仍保持：

HEAD:

`a8ca9707b93e27f4f2f310349640fd7efb0e098d`

Working tree 中仅 Stage 3.3 两个 artifacts 存在 authorized changes。

Commit 由最终 Reviewer 在 PASS 后执行。

---

# 27. Final Reviewer Handoff Readiness

Phase B 完成后应使最终 Reviewer 可以独立验证：

- Phase A Train-only implementation；
- frozen transformation definitions；
- Human Checkpoint authorization；
- Phase B identical Test application；
- Test target non-use；
- Train regression stability；
- Train/Test 29-column schema equality；
- clean Notebook execution；
- protected prior-stage artifacts；
- Git scope；
- no modeling / WOE / imbalance handling；
- no unauthorized features。

Codex 不执行 Reviewer 工作。

---

# 28. Phase B Completion Criteria

必须全部满足：

1. correct Git baseline；
2. only authorized Phase A workspace state exists；
3. approved Phase A Notebook hash verified；
4. protected inputs unchanged；
5. locked split hashes reproduced；
6. Train remains 24,000；
7. Test remains 6,000；
8. Train Phase A results unchanged；
9. frozen mappings applied identically to Test；
10. exactly 3 delinquency features applied to Test；
11. exactly 3 utilization features applied to Test；
12. raw PAY unchanged；
13. raw BILL unchanged；
14. raw PAY_AMT unchanged；
15. negative BILL floored only inside utilization calculation；
16. utilization not capped；
17. Test predictor schema exactly 29 columns；
18. Train/Test predictor names and order identical；
19. ID excluded from predictors；
20. target excluded from predictors；
21. Test target untouched；
22. no Test target analysis；
23. no unmapped category；
24. no NaN introduced；
25. no infinity introduced；
26. no row loss；
27. no row duplication；
28. delinquency assertions pass；
29. utilization assertions pass；
30. deterministic Test arithmetic samples pass；
31. no unauthorized feature；
32. no feature redesign；
33. no WOE / IV / binning；
34. no class-imbalance treatment；
35. no modeling；
36. no processed CSV；
37. Notebook clean-runs from raw data；
38. Task Report updated；
39. Phase B immutable Architect Specification preserved；
40. only two authorized Stage 3.3 files changed；
41. no Git staging；
42. no commit；
43. no push；
44. ready for final Reviewer。

---

# 29. Codex Completion Report

Codex 最终返回简洁 Completion Report，包括：

- Phase B pre-flight result；
- branch / HEAD；
- approved Phase A baseline verification；
- Phase A Notebook hash verification；
- split row counts and ID hash reproduction；
- Train regression-check result；
- Test transformation result；
- Test row integrity result；
- Test category mapping result；
- six derived feature names；
- Train/Test predictor schema count；
- schema equality result；
- Test NaN/inf result；
- Test delinquency sanity result；
- Test utilization sanity result；
- Test arithmetic sample validation result；
- Test target protection confirmation；
- Notebook clean-run status；
- protected-input integrity；
- files modified；
- Git status；
- explicit confirmations:
  - no new feature
  - no Test target analysis
  - no WOE/binning
  - no imbalance handling
  - no modeling
  - no processed dataset
  - no commit
  - no push

Final line:

`Stage 3.3 Phase B complete — ready for final Reviewer validation and milestone commit.`

================ END ARCHITECT SPECIFICATION — PHASE B ================

## 25. Protected-input Pre-flight and Final Integrity

All listed inputs existed, and their sizes/hashes matched Phase A records or the committed baseline before Phase B. The same values were reproduced after the Notebook clean-run and report update work; no protected input changed.

| Protected input | Size (bytes) | SHA-256 |
|---|---:|---|
| Raw CSV | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |
| Luca reference Notebook | 805,686 | `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd` |
| Stage 2 Notebook | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` |
| Stage 3.1 Notebook | 937,648 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` |
| Stage 0 Task Report | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` |
| Stage 1 Task Report | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` |
| Stage 2 Task Report | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` |
| Stage 3.1 Task Report | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` |
| Root README | 2,245 | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` |
| data README | 1,004 | `c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0` |
| references README | 1,109 | `a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c` |
| requirements | 95 | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` |
| .gitignore | 534 | `300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1` |
| Legacy baseline placeholder Notebook | 1,057 | `ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20` |
| reports/figures/.gitkeep | 60 | `73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63` |

All other tracked files were protected by the empty tracked and staged diffs.

## 26. Locked Split Replay

The Notebook independently reloaded `data/raw/UCI_Credit_Card.csv` and replayed:

- `train_test_split`
- 80% Train / 20% Test
- `random_state=42`
- `stratify=default.payment.next.month`

Results:

| Check | Actual |
|---|---|
| Raw rows | 30,000 |
| Train rows | 24,000 |
| Test rows | 6,000 |
| Train/Test ID overlap | 0 |
| ID union coverage | All 30,000 IDs |
| Train stable ID hash | `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` |
| Test stable ID hash | `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f` |

Both exact hash assertions passed before Test transformation.

## 27. Frozen Transformation Applied to Test

A single deterministic function, which cannot receive the target column, was applied separately to Train and Test. It performed only:

- `EDUCATION_GROUP` mapping;
- `MARRIAGE_GROUP` mapping;
- `delinquent_months`;
- `max_delinquency`;
- `severe_delinquent_months`;
- `current_utilization`;
- `avg_utilization`;
- `max_utilization`;
- construction of the frozen 29-column predictor-ready view.

Raw EDUCATION and MARRIAGE were excluded from predictor-ready views. Temporary positive-bill values were calculation-only and were not saved as predictors. Raw PAY, BILL, and PAY_AMT values were not altered. Utilization was not capped, clipped, winsorized, scaled, or normalized.

## 28. Train Regression Checks

All Phase A Train results remained exact after using the common frozen implementation.

| Regression check | Accepted / reproduced value |
|---|---:|
| Train rows | 24,000 |
| Train ID hash | `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` |
| Predictors | 29 |
| EDUCATION Other / Unknown | 386 |
| MARRIAGE Other / Unknown | 302 |
| `delinquent_months == 0` | 15,896 |
| `max_delinquency == 0` | 15,896 |
| `severe_delinquent_months == 0` | 17,276 |
| Rows with any utilization > 1 | 3,129 |

The Phase B Train predictor frame matched the saved Phase A `X_train` exactly by values, dtypes, index, column names, and order. All six approved feature summaries and all Phase A deterministic PAY/BILL sample rows were exactly unchanged. Phase A structural, category, delinquency, utilization, missing-value, and finite-value assertions continued to pass.

## 29. Test Structural Integrity

All assertions passed:

- Test remained exactly 6,000 rows.
- Original index/order and ID set/order were unchanged.
- IDs remained unique; no duplicate row or duplicate ID was introduced.
- Test target values and ID alignment were unchanged.
- AGE, SEX, LIMIT_BAL, six PAY, six BILL_AMT, and six PAY_AMT columns were exactly unchanged by value and dtype.
- No row was lost or duplicated.
- All 29 predictors existed with unique names.
- No predictor missing value or numeric infinity was introduced.
- Only the two frozen group fields and six approved new features were added to the Test working frame.

## 30. Test Category Mapping Verification

No unexpected raw code, unmapped row, or missing group was found. Each reconciliation totaled 6,000 rows. No target was used.

### EDUCATION

| Raw code | Frozen group | Test count |
|---:|---|---:|
| 0 | Other / Unknown | 2 |
| 1 | Graduate School | 2,130 |
| 2 | University | 2,774 |
| 3 | High School | 1,014 |
| 4 | Other / Unknown | 26 |
| 5 | Other / Unknown | 45 |
| 6 | Other / Unknown | 9 |

Grouped Test counts: Graduate School 2,130; University 2,774; High School 1,014; Other / Unknown 82.

### MARRIAGE

| Raw code | Frozen group | Test count |
|---:|---|---:|
| 0 | Other / Unknown | 7 |
| 1 | Married | 2,767 |
| 2 | Single | 3,158 |
| 3 | Other / Unknown | 68 |

Grouped Test counts: Married 2,767; Single 3,158; Other / Unknown 75.

## 31. Test Delinquency Validation

Independent recalculation reproduced all three features exactly. Every required assertion passed:

- `0 <= delinquent_months <= 6`;
- `0 <= severe_delinquent_months <= 6`;
- `severe_delinquent_months <= delinquent_months`;
- `max_delinquency >= 0`;
- zero delinquent months implies zero maximum delinquency;
- positive severe count implies maximum delinquency at least 2;
- all six raw PAY fields unchanged.

### Deterministic Test PAY samples (no target)

| Case | ID | PAY_0 | PAY_2 | PAY_3 | PAY_4 | PAY_5 | PAY_6 | delinquent_months | max_delinquency | severe_delinquent_months |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| All six PAY <= 0 | 15 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| One positive month, max=1 | 27 | 1 | -2 | -1 | -1 | -1 | -1 | 1 | 1 | 0 |
| Multiple positive months, max=2 | 2 | -1 | 2 | 0 | 0 | 0 | 2 | 2 | 2 | 2 |
| Maximum severity 3 or 4 | 534 | 2 | 0 | 0 | 3 | 3 | 2 | 4 | 3 | 4 |
| Maximum severity at least 5 | 159 | 2 | 3 | 4 | 4 | 5 | 6 | 6 | 6 | 6 |

## 32. Test Utilization Validation

Independent recalculation reproduced all three utilization features exactly. Every required assertion passed:

- all `LIMIT_BAL > 0`;
- no NaN or infinity;
- utilization values nonnegative;
- current and average utilization did not exceed maximum utilization;
- raw BILL fields were unchanged;
- negative raw BILL remained negative and was floored only in temporary arithmetic;
- utilization values above 1 were retained.

Utilization values above 1:

| Feature | Test count > 1 |
|---|---:|
| current_utilization | 430 |
| avg_utilization | 130 |
| max_utilization | 802 |

Rows with at least one utilization feature above 1: **802**.

### Deterministic Test BILL/utilization samples (no target)

| Case | ID | LIMIT_BAL | BILL_AMT1 | BILL_AMT2 | BILL_AMT3 | BILL_AMT4 | BILL_AMT5 | BILL_AMT6 | current_utilization | avg_utilization | max_utilization |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| At least one negative BILL | 27 | 60,000 | -109 | -425 | 259 | -57 | 127 | -189 | 0.000000 | 0.001072 | 0.004317 |
| At least one zero BILL | 22 | 120,000 | 316 | 316 | 316 | 0 | 632 | 316 | 0.002633 | 0.002633 | 0.005267 |
| Ordinary positive bills, none above limit | 2 | 120,000 | 2,682 | 1,725 | 2,682 | 3,272 | 3,455 | 3,261 | 0.022350 | 0.023718 | 0.028792 |
| At least one BILL above LIMIT_BAL | 105 | 60,000 | 56,685 | 55,208 | 59,175 | 60,218 | 55,447 | 55,305 | 0.944750 | 0.950106 | 1.003633 |

## 33. Train/Test Predictor Schema Equality

Both views contained exactly the following same 29 unique columns in the same order:

1. `LIMIT_BAL`
2. `SEX`
3. `EDUCATION_GROUP`
4. `MARRIAGE_GROUP`
5. `AGE`
6. `PAY_0`
7. `PAY_2`
8. `PAY_3`
9. `PAY_4`
10. `PAY_5`
11. `PAY_6`
12. `BILL_AMT1`
13. `BILL_AMT2`
14. `BILL_AMT3`
15. `BILL_AMT4`
16. `BILL_AMT5`
17. `BILL_AMT6`
18. `PAY_AMT1`
19. `PAY_AMT2`
20. `PAY_AMT3`
21. `PAY_AMT4`
22. `PAY_AMT5`
23. `PAY_AMT6`
24. `delinquent_months`
25. `max_delinquency`
26. `severe_delinquent_months`
27. `current_utilization`
28. `avg_utilization`
29. `max_utilization`

Train and Test corresponding dtypes were identical: object for the two group fields, integer for SEX, AGE, PAY and delinquency features, and floating point for LIMIT_BAL, BILL_AMT, PAY_AMT and utilization features. ID, target, raw EDUCATION, raw MARRIAGE, and calculation-only positive-bill fields were absent.

## 34. Limited Non-target Test Sanity Evidence

Only transformation-validation summaries were produced.

| Feature | Minimum | Maximum | Missing |
|---|---:|---:|---:|
| delinquent_months | 0 | 6 | 0 |
| max_delinquency | 0 | 8 | 0 |
| severe_delinquent_months | 0 | 6 | 0 |
| current_utilization | 0 | 4.140600 | 0 |
| avg_utilization | 0 | 3.759125 | 0 |
| max_utilization | 0 | 4.935500 | 0 |

Across the Test predictor-ready view: missing cells = 0; numeric non-finite cells = 0.

No Train/Test feature-distribution comparison, drift analysis, statistical test, or target-based output was performed.

## 35. Test Target Protection

The Test target was:

- isolated with ID in a separate trace frame before transformation;
- absent from the transformation inputs;
- absent from the frozen transformation function;
- absent from Train/Test predictor-ready schemas;
- absent from Test mapping, arithmetic, sample, and sanity tables;
- compared only for exact pre/post values, dtype, index, and ID alignment;
- not counted, averaged, grouped, correlated, plotted, selected on, or evaluated.

All target-integrity assertions passed. No Test default rate, target distribution, target relationship, WOE/IV, metric, or model evaluation was produced.

## 36. Notebook Clean-run Evidence

| Check | Result |
|---|---|
| Python | Project `.venv/bin/python` |
| Execution | Fresh local `python3` kernel; project root working directory |
| Notebook cells | 46 total: 23 markdown + 23 code |
| Execution counts | Sequential 1–23 |
| Saved outputs | 57 |
| Error outputs | 0 |
| Tracebacks in Notebook | 0 |
| Final Notebook size | 142,371 bytes |
| Final Notebook SHA-256 | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` |

The first sandboxed kernel start was denied before any execution or Notebook rewrite because local kernel sockets were unavailable. The clean-run then succeeded using the same existing project environment with local-kernel permission. A harmless `jupyter_client` destructor warning occurred after explicit kernel shutdown; it is not a saved Notebook output, error, or traceback.

No package was installed, upgraded, or removed. No Luca, Stage 2, or Stage 3.1 Notebook was executed or saved.

## 37. Git Scope and Artifact Verification

Phase B modified only the two authorized Stage 3.3 artifacts:

- `notebooks/03_feature_engineering_and_sanity_checks.ipynb`
- `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

Expected final Git representation remains two untracked paths because Phase A was intentionally uncommitted. Tracked diff and staged diff remain empty; branch, HEAD, parent, history, index, and remote remain unchanged.

No Phase B Notebook, additional report, processed Train/Test/feature CSV, model artifact, source module, test module, figure, checkpoint, cache, HTML, PDF, or Stage 4 artifact was created.

No `git add`, commit, push, pull, fetch, amend, reset, rebase, stash, restore, checkout, clean, or remote change was performed.

## 38. Prohibited-scope Confirmation

Confirmed:

- no new feature;
- no feature redesign;
- no Test target analysis;
- no Train/Test drift analysis;
- no WOE, IV, or binning;
- no class-imbalance handling;
- no modeling or evaluation;
- no processed dataset;
- no Git staging;
- no commit;
- no push;
- no remote change;
- no Stage 4 work.

## 39. Incomplete Items, Deviations, and Limitations

- Incomplete implementation items: none identified.
- Deviations from the Architect Specification: none identified.
- Unexpected repository artifacts or modifications: none observed.
- Operational note: the initial sandboxed kernel launch was blocked before execution; the successful clean-run used the existing environment with local-kernel permission.
- Expected pending item outside Codex Phase B scope: independent final Reviewer validation and, only after Reviewer PASS, the Reviewer-created Stage 3.3 milestone commit.

## 40. Stage 3.3 Implementation Status and Final Reviewer Handoff

`Stage 3.3 implementation complete — pending final Reviewer validation.`

This is an implementation status only. Codex has not declared Reviewer PASS, closed Stage 3.3, or created a milestone commit. The complete Phase A and Phase B evidence is now ready for the final Reviewer to validate independently.

# REVIEWER RESULT

## Verdict

PASS

This is the final Reviewer verdict for the complete Stage 3.3 implementation, covering both the previously accepted Phase A Train implementation and the authorized Phase B Holdout Test application.

## Requirement Check

- The Phase A and Phase B immutable Architect Specifications are present and complete. The approved Phase A Notebook cells and Task Report prefix remain byte-for-byte reproducible from their recorded approved hashes; Phase B preserved the earlier execution evidence and Reviewer result.
- Git baseline and scope are correct: `main` at `a8ca9707b93e27f4f2f310349640fd7efb0e098d`, expected parent, no staged or tracked diff, no remote, and exactly the two authorized Stage 3.3 artifacts untracked before Reviewer closeout.
- The locked 80/20 stratified split is exactly reproducible with 24,000 Train rows and 6,000 Test rows, disjoint/full-coverage IDs, and both required stable ID-set hashes.
- Phase A Train feature engineering remains unchanged. The category counts, six new-feature summaries, deterministic samples, 29-column predictor schema, and all required regression values reproduce the accepted Phase A baseline exactly.
- Phase B applies one frozen deterministic function separately to Train and Test after removing target from transformation inputs. The function creates only the two approved group fields and six approved derived features.
- Holdout Test target protection is satisfied: target is isolated with ID, absent from transformation inputs and predictors, preserved exactly by index/value/dtype alignment, and never used for distribution, relationship, selection, metric, WOE/IV, or evaluation output.
- Test retains 6,000 rows, original index/ID order and set, unique IDs, unchanged raw AGE/SEX/LIMIT_BAL/PAY/BILL/PAY_AMT fields, and unchanged trace labels. No row loss, duplication, missing predictor, NaN, or numeric infinity is introduced.
- EDUCATION and MARRIAGE mappings are identical across Train/Test, cover every observed Test raw code exactly once, and create no unmapped or missing category.
- Delinquency and utilization formulas independently reconcile on Test. Negative BILL values remain raw and are floored only inside utilization arithmetic; utilization is not capped, clipped, winsorized, scaled, or normalized.
- Train and Test predictor-ready views contain the identical ordered 29-column schema with compatible dtypes; ID, target, raw EDUCATION/MARRIAGE, and calculation-only intermediates are excluded.
- No unapproved feature, redesign, Train/Test drift analysis, target-based analysis, WOE/IV/binning, imbalance handling, modeling, evaluation, processed dataset, extra artifact, Stage 4 work, or prohibited Git/remote action is present.
- The final Notebook clean-runs from raw data with 23 sequentially executed code cells, 57 saved outputs, and zero Notebook error outputs.

## Independent Verification

- Read both complete Architect Specifications and all Phase A/Phase B execution evidence in this Task Report.
- Inspected every Notebook source cell, saved output, execution count, metadata entry, transformation input, target reference, and final schema assertion.
- Reconstructed the current Notebook's first 28 cells and reproduced the approved Phase A SHA-256 `b686560b826ada41e771f5299beafbe28f77f476007263963ce8cae1f7f7832e`; independently verified the pre-Phase-B Task Report prefix SHA-256 `f7cbc757fd5b0fffdaa0582974c09e14999b5df70cb77512dcdeee618c6e670f`.
- Independently replayed the split and reproduced Train hash `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` and Test hash `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Independently recalculated all six features on Train/Test and reproduced the Phase A regression values, Test category counts, feature ranges, 802 Test rows with at least one utilization value above 1, and all deterministic Test sample IDs and arithmetic.
- Independently reran all 23 code cells in a fresh local kernel without overwriting the Notebook; all cells completed successfully and reproduced 57 outputs with no cell errors.
- Rechecked protected-file hashes, Notebook hash `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256`, Git branch/HEAD, staged/tracked/untracked scope, and remote state after verification.

## Issues Found

None affecting acceptance.

## Required Fixes

None.

## Architect Decision Needed

None.
