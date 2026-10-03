# Stage 2 — Luca Notebook Audit & Data Understanding

## 1. Task Identification

- Task: `Stage 2 — Luca Notebook Audit & Data Understanding`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Execution date: 2026-08-09
- Role: Codex local project executor
- Delivery status: Completed for independent Reviewer assessment; not committed
- Evidence convention:
  - `Stage 2 independently executed data check` means an output produced by the newly created Stage 2 Notebook against the formal raw CSV.
  - `Saved output from Luca reference notebook` means content read from the reference file without executing it.

## 2. Stage Objective and Scope

This Stage establishes a traceable factual record of Luca Basanisi's reference workflow and independently checks the formal UCI CSV. It covers reference identification, cell-order workflow mapping, saved-output review, field-definition comparison, lightweight data understanding, validation/leakage review, and an audit decision log.

This is a reference-audit and data-understanding deliverable. It does not reproduce Luca's workflow, create production features, split data for this project, resample, train models, tune thresholds, or begin Stage 3.

## Immutable Architect Specification

Authoritative source: original Stage 2 Codex Prompt supplied from the Architect-02 specification. The immutable block below is copied verbatim.

================ BEGIN ARCHITECT SPECIFICATION ================

# Stage 2 — Luca Notebook Audit & Data Understanding

## 目标

本 Stage 的目标是：

> 系统理解 Luca reference Notebook 实际完成的完整分析流程，并将其与正式 UCI 数据的字段定义、真实取值和数据行为进行对照，形成后续项目设计与优化所依赖的事实底稿。

本 Stage 是：

**Reference Audit + Data Understanding**

不是复现、优化或建模 Stage。

项目路径：

`/Users/yu/2.工作/Risk Analysis Project`

当前稳定 Git 基线：

- Branch：`main`
- Stage 1：PASS / CLOSED
- HEAD：`aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c`
- Commit：`Establish project foundation`
- Stage 1 closeout 后 working tree clean
- 当前无 remote、无 GitHub push

正式数据：

`data/raw/UCI_Credit_Card.csv`

Luca reference Notebook 位于现有 `references/` 管理体系中。必须读取本地真实文件，不得凭记忆猜测其实现。

---

# 一、完整审查 Luca Notebook

按照 Notebook 的**真实 section / cell 顺序**完整梳理其 workflow。

需要识别并记录实际存在的：

- 数据读取；
- initial inspection；
- column / datatype inspection；
- missing-value inspection / handling；
- abnormal / special-code handling；
- observation deletion；
- variable recoding；
- EDA；
- variable relationship / correlation analysis；
- feature engineering；
- categorical treatment；
- scaling / transformation；
- train/test preparation；
- validation / cross-validation；
- class-imbalance handling；
- model training；
- model comparison；
- evaluation；
- final conclusions。

如果 Luca 没有执行某一步，要明确记录其不存在，不得根据常规 Data Science workflow 自行补全。

---

# 二、建立 Luca Workflow Map

需要形成一份按照 Notebook 实际顺序排列的 Workflow Map。

对于每个重要步骤，尽可能记录：

- section / Markdown heading；
- cell index 或其他可重新定位位置；
- Luca 做了什么；
- 使用哪些变量；
- 如何转换；
- 创建了哪些 derived variables；
- 输出了什么；
- Luca 自己如何解释；
- 是否存在 saved output；
- 与下一步骤之间的关系。

Stage 2 Report 应达到：

> 后续 Architect 不重新逐 cell 阅读 Luca Notebook，也可以准确理解 Luca 的整体分析和建模设计。

---

# 三、允许读取 Luca 已保存结果，但不重新运行其完整流程

允许读取并引用 Luca Notebook 中已有的：

- saved model outputs；
- metrics；
- tables；
- charts；
- printed results；
- Markdown conclusions；
- execution state。

**不得为了本 Stage 整体重新运行 Luca Notebook。**

引用 Luca 结果时必须清楚区分其来源，例如：

`Saved output from Luca reference notebook`

不得将其描述为本项目重新训练或 reproduced 的结果。

---

# 四、独立理解正式 UCI 数据

使用：

`data/raw/UCI_Credit_Card.csv`

运行必要、轻量、可重复的数据检查。

目的仅限于：

1. 确认数据实际上是什么；
2. 验证 Luca 对数据的理解和处理；
3. 为后续 Architect 设计建立事实基础。

至少检查：

- shape；
- column names；
- dtypes；
- missing / null；
- duplicate rows；
- ID uniqueness / duplicate IDs（如适用）；
- target distribution；
- 明显的数据完整性问题。

不得修改 raw CSV。

---

# 五、关键变量组审查

至少系统审查以下变量组。

## Demographics

- `SEX`
- `EDUCATION`
- `MARRIAGE`
- `AGE`

确认：

- actual unique values；
- frequency；
- documented meaning；
- non-standard / special codes；
- Luca 如何处理。

## Credit exposure

- `LIMIT_BAL`

确认：

- range；
- distribution；
- Luca 如何使用。

## Repayment / delinquency status

审查所有 `PAY_*` variables。

这是 Stage 2 的重点。

至少确认：

- 实际有哪些编码；
- 各编码频数；
- documented meaning；
- Luca 如何解释；
- Luca 如何转换；
- 是否 binary 化；
- 是否合并 categories；
- 是否创建 aggregated variables；
- 是否保留原始 `PAY_*`；
- 是否存在潜在信息压缩。

用户当前怀疑 Luca 对部分 `PAY_*` 的处理可能过度粗化并损失 delinquency severity 信息。

该观点只是**审查线索，不是预设结论**。

必须依据实际 Notebook 和数据判断。

## Billing history

审查全部 `BILL_AMT*`。

## Payment history

审查全部 `PAY_AMT*`。

## Target

审查：

`default.payment.next.month`

---

# 六、三方对照

对关键字段和处理尽可能建立：

`Official / documented meaning`

vs.

`Actual CSV behavior`

vs.

`Luca treatment`

三方对照。

字段定义的证据优先级：

1. UCI 官方数据集记录；
2. 正式 CSV 实际表现；
3. 项目内已经验证的 documentation；
4. Luca Notebook 作为 reference implementation。

Luca Notebook 是被审查对象，不自动作为字段定义的最高权威。

如出现：

- 官方定义存在歧义；
- observed values 与官方描述不完全一致；
- Luca interpretation 无法确认；
- 数据自身无法支持确定判断；

必须记录 uncertainty，不得自行创造确定结论。

---

# 七、Audit Decision Framework

对 Luca 的重要：

- data cleaning；
- recoding；
- variable treatment；
- feature engineering；
- preprocessing；
- model setup；
- evaluation decision；

进行分类。

仅使用以下四种标签：

### Keep

当前判断合理，可以作为后续项目候选继承做法。

### Needs Validation

当前不足以判断优劣，后续需要数据或模型实验验证。

### Candidate for Improvement

已有合理理由认为值得后续重新设计，但本 Stage 不实施修改。

### Incorrect / Risky

仅在存在较强证据时使用，例如：

- leakage；
- 明确字段解释错误；
- train/test contamination；
- 明显不合理 evaluation；
- 数学或代码逻辑错误。

不得为了体现本项目比 Luca 更复杂或“更高级”而强行批评其方法。

---

# 八、Stage 2 允许的 Data Understanding / EDA

允许：

- `value_counts`；
- descriptive statistics；
- category frequencies；
- missingness；
- distributions；
- target distribution；
- simple crosstabs；
- 必要的简单 plots；
- 为核实 Luca 描述而执行的小型检查。

这些操作必须服务于：

**理解数据或验证 Luca。**

不得在本 Stage：

- 大规模寻找预测力最强变量；
- 正式 feature selection；
- 自动寻找 optimal transformations；
- 根据 target 不断尝试新特征；
- 根据 EDA 直接进入模型优化。

---

# 九、Feature Engineering 边界

Stage 2 可以：

- 记录 Luca 已经创建的 features；
- 评价这些 features；
- 指出可能值得新增或改进的方向。

但不得正式实现新的 risk-oriented features，例如但不限于：

- utilization；
- payment ratio；
- delinquency count；
- maximum delinquency；
- repayment trend；
- balance trend；
- volatility；
- 其他金融领域 derived features。

本 Stage **不要创建正式 `feature_catalog.md`**。

---

# 十、Class Imbalance 边界

Stage 2 可以：

- 描述 target imbalance；
- 记录 Luca 是否进行了 imbalance handling；
- 指出这是后续模型设计需要考虑的问题。

不得：

- SMOTE；
- oversampling；
- undersampling；
- 改变 target distribution；
- class-weight model training；
- imbalance model experiments。

---

# 十一、Modeling 边界

不得训练新的正式模型。

不得开展：

- Logistic Regression baseline；
- WOE；
- Scorecard；
- Random Forest；
- XGBoost；
- LightGBM；
- model selection；
- hyperparameter tuning；
- threshold optimization。

可以审查 Luca 已保存的 modeling workflow 和结果。

必须严格区分：

`Luca saved result`

与

`Stage 2 independently executed data check`

---

# 十二、Train/Test / Validation 审查

Stage 2 要审查 Luca：

- 是否 split；
- 如何 split；
- random seed；
- 是否 stratify；
- 是否使用 CV；
- preprocessing 是否发生在 split 前后；
- test data 是否参与 feature / preprocessing decisions；
- 是否存在 leakage risk。

但是：

**Stage 2 不正式建立本项目未来 Modeling 的最终 split / CV pipeline。**

尤其不要直接因为旧 AI Studio 方案中曾提出：

`80/20 + StratifiedKFold`

就在本 Stage 实施该结构。

该事项可以作为后续 Architect Decision 记录。

---

# 十三、正式产物

Stage 2 至少形成以下两个正式文件。

## 1. Data Understanding / Audit Notebook

建议路径：

`notebooks/01_luca_audit_and_data_understanding.ipynb`

用途：

- 对正式 CSV 执行独立检查；
- 保存必要统计；
- 保存必要简单图表；
- 为 Stage 2 Report 中的判断提供可重复运行证据。

Notebook 必须：

- 可以从正式 raw CSV 重新运行；
- 不依赖人工修改的数据；
- 不修改 raw data；
- 不复制 Luca Notebook；
- 不重新运行 Luca 全流程；
- 不进入正式 feature engineering 或 modeling。

## 2. Stage 2 Task Report

路径：

`task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`

该 Report 是 Stage 2 最重要的事实底稿。

至少包含：

- Stage objective；
- files inspected；
- Luca Notebook identification；
- Luca workflow map；
- important Luca decisions；
- variable-group findings；
- official/data/Luca comparison；
- Luca saved model / evaluation results；
- Audit Decision Log；
- uncertainties / ambiguities；
- suspected issues；
- potential future improvement directions；
- explicit out-of-scope items；
- files created / modified；
- reproducibility checks；
- Git status。

---

# 十四、工程边界

本 Stage 不提前创建：

- `src/`
- `tests/`

Notebook 用于实际分析、图表和教学型解释。

Stage 2 不需要为了“工程化”而过度模块化。

优先保持当前项目结构简洁。

---

# 十五、结果真实性

所有 Stage 2 自己生成的统计、表格和图表必须来自实际运行。

所有 Luca 数字必须来自其 Notebook 已保存输出并清楚标记来源。

严禁：

- 编造 AUC；
- 编造 KS；
- 编造 Recall；
- 编造 feature importance；
- 编造 model metrics；
- 将 Luca saved output 冒充 reproduced result。

---

# 十六、项目保护

不得：

- 修改 `data/raw/UCI_Credit_Card.csv`；
- 修改 Luca reference Notebook；
- 覆盖 reference files；
- 删除或改写 Stage 0 / Stage 1 records；
- amend Stage 1 commit；
- rebase；
- reset Git history；
- 添加 remote；
- push GitHub；
- 开始后续 Feature Engineering；
- 开始后续 Modeling。

---

# 十七、Git / Reviewer 流程

Codex 完成 Stage 2 后：

**不要 commit。**

保留 working tree changes 交给 Reviewer。

当前正式流程：

- Reviewer `FAIL` → no commit；
- Reviewer `PASS` → Reviewer 在同一轮执行 local commit，并进行 post-commit verification。

Codex 不得提前 commit 或 push。

---

# 十八、Stage 2 完成标准

只有以下条件全部满足，Stage 2 才可交 Reviewer：

1. 已基于真实 Luca Notebook 建立完整 workflow map；
2. 关键 Luca decisions 可以重新定位和追溯；
3. 正式 UCI 数据关键变量已经独立检查；
4. official meaning、actual data、Luca treatment 已形成系统对照；
5. Luca 的重要处理已进入 Audit Decision Framework；
6. Luca saved outputs 和 Stage 2 自己运行结果严格区分；
7. 已记录 uncertainty 和 unresolved questions；
8. 已形成后续可能改进方向，但没有实施；
9. 没有进入正式 Feature Engineering；
10. 没有训练新的正式模型；
11. raw data 和 Luca reference 均未被修改；
12. 所有 Stage 2 自己生成的数字均来自真实运行；
13. Notebook 可以重复运行；
14. Stage 2 Report 足以作为后续 Architect 决策的事实基础；
15. Codex 未 commit；
16. Codex 未 push。

================ END ARCHITECT SPECIFICATION ================

## 3. Pre-Execution State

The required baseline was verified before any project write:

| Item | Pre-execution fact |
|---|---|
| Project root | Existing, readable, writable: `/Users/yu/2.工作/Risk Analysis Project` |
| Git top-level | `/Users/yu/2.工作/Risk Analysis Project` |
| Branch | `main` |
| HEAD | `aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c` |
| HEAD subject | `Establish project foundation` |
| Working tree | Clean |
| Staged / unstaged diff | Empty / empty |
| Remote | None configured |
| `.git/index` SHA-256 | `b91d045d66df39a2d803324b9f5f0ffc194a16c96806acfaae485d8777b0dbd1` |
| Stage 1 state | Task record contains Reviewer `PASS`; HEAD is the accepted Foundation commit, so Stage 1 is closed |
| Stage 2 output paths | Both absent before execution |

Existing ignored local items included `.DS_Store`, `.venv/`, and the Luca reference Notebook. There were no pre-existing tracked or untracked changes.

## 4. Files Inspected

- `/Users/yu/2.工作/Risk Analysis Project/data/raw/UCI_Credit_Card.csv`
- `/Users/yu/2.工作/Risk Analysis Project/references/credit-card-default-a-very-pedagogical-notebook.ipynb`
- `/Users/yu/2.工作/Risk Analysis Project/README.md`
- `/Users/yu/2.工作/Risk Analysis Project/data/README.md`
- `/Users/yu/2.工作/Risk Analysis Project/references/README.md`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/TASK_REPORT_Task_0.1_Current-State_Audit.md`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_1_Project_Foundation.md`
- Git metadata through read-only Git commands; `.git` objects were not traversed or modified.
- Official primary source: UCI Machine Learning Repository, **Default of Credit Card Clients**, `https://archive.ics.uci.edu/dataset/350/default%2Bof%2Bcredit%2Bcard%2Bclients`, accessed 2026-08-09. It supports the dataset dimensions, no-missing-data statement, variable descriptions, target coding, creator, DOI, and license used in this report.

## 5. Luca Notebook Identification and Structural State

The local reference is not a single valid Notebook JSON document. A whole-file `json.loads` fails with `Extra data` at character 110611. Incremental `JSONDecoder.raw_decode` identifies exactly two adjacent complete top-level JSON objects:

| Object | Notebook structure | Cells | Markdown | Code | Saved execution state |
|---|---|---:|---:|---:|---|
| 1 | `nbformat 4.1`, valid notebook-shaped object | 194 | 58 | 136 | All execution counts null; no outputs |
| 2 | `nbformat 4.1`, valid notebook-shaped object | 194 | 58 | 136 | 123 code cells have execution counts; 109 code cells have output; 129 output items; zero saved error outputs |

### Character ranges

- First Notebook JSON object: `[0, 110611)`
- Second Notebook JSON object: `[110611, 805682)`

### UTF-8 byte ranges

- First Notebook JSON object: `[0, 110611)`
- Second Notebook JSON object: `[110611, 805685)`

The file ends with 1 trailing newline byte after the second object. Total file size is `805686 bytes`. There is no non-whitespace content between the objects. Cell type, source, and metadata content are identical between the two objects; each has canonical source/metadata SHA-256 `a1fca7f3ca237dbd2d36e4e5529be0339482408f6a014cfee6b135fc15e58af9`. Object 1 is therefore a cleared-state duplicate of the same workflow, while object 2 carries saved execution state. The file was read as-is; it was not executed, repaired, split, reserialized, or saved.

## 6. Luca Cell Inventory Method

The inventory was built by read-only incremental JSON parsing. For both objects, every index `0–193` was recorded with cell type, normalized source, first line, Markdown heading, execution count, output count/type, and references to data, variables, transformations, estimators, and metrics. Exact equality of the two source streams was checked before using object 2 as the saved-output view. The workflow map below covers every cell in order; important decisions are also identified at individual cell level.

No conclusion was inferred from brace counting or a single parse error. No Notebook API was used to rewrite the reference.

## 7. Luca Workflow Map

All locations refer to both identical source objects; saved-output statements refer only to object 2.

| Object / cells | Heading or phase | Action, variables, transformation, output, and link to next step |
|---|---|---|
| 1/2: 0 | `Introduction`; `Variables`; `Goal and plan` | Describes the credit-card default problem and variable groups; proposes several classifiers and an exploratory/modeling plan. No computation. Leads to imports and data loading. |
| 1/2: 1 | Setup | Imports NumPy, pandas, seaborn, matplotlib and `ggplot`; object 2 saves warning/stream output. No environment isolation or dependency declaration in the reference. |
| 1/2: 2 | Data loading | Reads `../input/UCI_Credit_Card.csv` and displays a sample. This Kaggle-relative path is not the current project's formal path. |
| 1/2: 3–16 | Initial inspection | Uses shape/head/info/describe/value inspection for categorical, repayment, bill, payment and limit fields. Finds no missing values. Renames target to `def_pay` and `PAY_0` to `PAY_1`. Saved target rate is `0.2212`. Leads directly to a deliberately “blind” model before cleaning. |
| 1/2: 17–31 | `Blind machine learning`; `Decision tree` | Uses all 23 predictors except `ID`. Cell 21 creates an 80/20 holdout with `random_state=42` and no explicit stratification. Fits decision trees, varies depth, and performs `GridSearchCV(cv=5)`. Saved examples include depth-10 accuracy `0.812`, depth-100 accuracy `0.7231667`, and grid test accuracy `0.821`. These are Luca saved results, not reproduced here. |
| 1/2: 32–34 | Full-data KFold and feature importance | Cell 32 then runs shuffled `KFold(5, random_state=42)` over the complete `df`, so observations assigned to the cell-21 holdout re-enter cross-validation/model development. Cell 34 reads feature importance from the classifier left fitted on the last fold of the cell-32 loop, and that importance informs subsequent analysis/feature-design direction. This development contamination means the original holdout is no longer an untouched independent final test set. This issue is distinct from the later wrong-frame `df.iloc` indexing defect in cells 144/146. |
| 1/2: 35–45 | Remaining blind-tree analysis | Plots/interprets the last-fold feature importance, evaluates a `PAY_1`-only tree, then moves from accuracy to F1 and a balanced tree. Saved examples include `PAY_1`-only accuracy `0.819333`, plain-tree F1 `0.4683168`, F1-grid test `0.4735565`, and balanced-tree test F1 `0.5203785`. These are Luca saved results, not reproduced here. |
| 1/2: 46–53 | `Data exploration`; `Categorical variables` | Counts and plots `SEX`, `MARRIAGE`, and `EDUCATION`; exposes undocumented codes (`MARRIAGE=0`, `EDUCATION=0/5/6`). Leads to numerical distributions and cleaning choices. |
| 1/2: 54–60 | `Numerical variables` | Histograms/descriptions for bill amounts, payments, repayment status, age, and credit limit. No formal transformation or row deletion. |
| 1/2: 61–68 | `Data Cleaning` | Recodes `EDUCATION` 0/5/6 to 4 and `MARRIAGE` 0 to 3. Then maps each repayment status `-2/-1/0` to 0. Examines payments above 300,000 but does not delete observations. Missing-value imputation is absent because the inspection found none. |
| 1/2: 69–96 | `Further analysis` | Uses crosstabs, default rates, count plots and selected bill/payment examples to relate demographics, status, age and payment behavior to `def_pay`. This is target-aware EDA on the full dataset and informs later feature choices. |
| 1/2: 97–98 | Repayment reinterpretation | Markdown says repayment codes are hard to interpret and proposes “late / not late.” Cell 98 maps every remaining positive `PAY_1…PAY_6` value to 1. Severity levels 2–8 are therefore collapsed while six monthly timing positions remain. |
| 1/2: 99–102 | Remaining relationship EDA | Continues age/default views and prepares for engineered interactions/bins. |
| 1/2: 103–129 | `Feature Engineering` | Creates `SE_MA` (sex–marriage interaction; final explicit six-category version), several age-bin trials then retains `AgeBin` with six equal-width bins and folds bin 6 into 5, creates `SE_AG`, creates but later omits `Client_1…Client_6`, creates cumulative inferred-expense ratios `Avg_exp_5…Avg_exp_1`, and creates credit-limit/bill “closeness” ratios `Closeness_6…Closeness_1`. No scaler, encoder, or imputer is fitted. All feature design occurs before the later split. |
| 1/2: 130–131 | `Find my balance and learn my machine` | Markdown states repayment fields will not be included, but the final `features` list does include `PAY_1…PAY_6`. Final list also includes `LIMIT_BAL`, recoded demographics, bill/payment histories, `SE_MA`, `AgeBin`, `SE_AG`, `Avg_exp_5…1`, and `Closeness_5…1` (not `Closeness_6`). Splits 80/20 with seed 42 and no explicit `stratify`. |
| 1/2: 132–140 | `Samples, samples, samples` | After the split, combines training X/y and creates upsampled minority, downsampled majority, and SMOTE training variants. Saved counts: original train 18,677/5,323; upsampled 18,677/18,677; downsampled 5,323/5,323; SMOTE 18,677/18,677. The ordering avoids directly resampling the holdout, but Stage 2 did not reproduce these operations. |
| 1/2: 141–147 | `Algorithm Selection` | Declares decision tree, random forest, AdaBoost, gradient boosting, and XGBoost candidates. Manual KFold comparison code calls `kf.split(df_train)` or a sampled frame but selects folds with `df.iloc[...]` from the full dataset. This breaks the intended sample-to-row mapping and can include records from the earlier holdout. Leads to per-model grid sections. |
| 1/2: 148–158 | `Random Forest Classifier - Grid Search` | Contains normal/upsampled/downsampled/SMOTE grid definitions. Grid cells 149, 152, 155, 157 are unexecuted; adjacent executed cells instantiate manually selected estimators and print saved scalar CV values. The printed constants cannot be treated as grid runs executed in the saved state. |
| 1/2: 159–167 | `Ada Boost Classifier - Grid Search` | Same pattern: grid cells 160, 162, 164, 166 are unexecuted; neighboring cells store estimator choices and printed saved scalar values. |
| 1/2: 168–176 | `Gradient Boost Classifier - Grid Search` | Same pattern: grid cells 169, 171, 173, 175 are unexecuted; neighboring cells store choices/printed saved values. |
| 1/2: 177–181 | `Training and testing`; `Random Forests` | Fits four random-forest variants and evaluates the same test set with accuracy, F1, confusion matrices, precision, and recall. Saved outputs are tabulated in section 14. |
| 1/2: 182–186 | `Ada Boost` | Fits/evaluates four AdaBoost variants on the same holdout. Saved outputs are tabulated in section 14. |
| 1/2: 187–191 | `Gradient Boosting` | Fits/evaluates four gradient-boosting variants on the same holdout. Saved outputs are tabulated in section 14. |
| 1/2: 192 | Conclusion | States that the resulting models have limited practical usefulness, cost-sensitive consequences matter, F1 may not be the final objective, domain knowledge is missing, and broad grid search was wasteful. It does not select one final accepted model. |
| 1/2: 193 | End | Empty code cell; no output. |

Explicitly absent from Luca's workflow: observation deletion; missing-value imputation; standardized scaling; formal categorical one-hot/ordinal encoding; a logistic-regression baseline; AUC or KS reporting; threshold optimization; a documented final model selection; and an isolated untouched final test set after repeated model comparison.

## 8. Important Luca Decisions

- Renames the target to `def_pay` and the first repayment field from `PAY_0` to `PAY_1` for internal consistency.
- Consolidates undocumented `EDUCATION` codes 0/5/6 into 4 and `MARRIAGE=0` into 3.
- Converts all six repayment variables to binary late/not-late flags: original values `<=0 → 0`, values `>0 → 1`.
- Retains all six binary repayment positions in the final code despite a contradictory Markdown statement that they would be excluded.
- Creates demographic interactions, age bins, cumulative inferred-expense ratios, and limit-to-bill closeness ratios before splitting.
- Excludes `ID`; retains raw bill and payment amount histories and `LIMIT_BAL`.
- Uses 80/20 split and seed 42 without explicit stratification.
- Applies upsampling, downsampling, and SMOTE only to post-split training variants.
- Compares tree ensembles largely with F1 and accuracy; uses the same holdout repeatedly for variant/model assessment.
- Ends without a final chosen model and explicitly questions practical adequacy and metric choice.

## 9. Independent UCI Data Understanding

All facts in this section are `Stage 2 independently executed data check` outputs saved in `notebooks/01_luca_audit_and_data_understanding.ipynb`.

| Check | Actual result |
|---|---|
| Shape | 30,000 rows × 25 columns |
| Source-order columns | `ID`, `LIMIT_BAL`, `SEX`, `EDUCATION`, `MARRIAGE`, `AGE`, `PAY_0`, `PAY_2`, `PAY_3`, `PAY_4`, `PAY_5`, `PAY_6`, `BILL_AMT1…6`, `PAY_AMT1…6`, `default.payment.next.month` |
| Dtypes | `LIMIT_BAL`, `BILL_AMT1…6`, `PAY_AMT1…6` are `float64`; all other fields are `int64` |
| Missingness | 0 null cells; 0 columns with nulls |
| Duplicate rows | 0 exact duplicate rows |
| Duplicate column names | 0 |
| Numeric finiteness | 0 non-finite numeric cells |
| ID | 30,000 unique IDs; 0 duplicate-ID rows |
| Target | `0: 23,364 (77.88%)`; `1: 6,636 (22.12%)`; majority/minority ratio `3.520796` |
| Raw file | 2,862,995 bytes; SHA-256 `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |

The Notebook identifies the target by actual column inspection and assertions, not by silently renaming it. It writes no processed data.

## 10. Variable-Group Findings

### Demographics

- `SEX`: values 1 (11,888; 39.6267%) and 2 (18,112; 60.3733%). Audit-only class-1 rates are 24.1672% and 20.7763% respectively.
- `EDUCATION`: 0:14, 1:10,585, 2:14,030, 3:4,917, 4:123, 5:280, 6:51. Codes 0/5/6 are observed but absent from the official page's 1–4 description. They are observed special codes, not given invented official meanings.
- `MARRIAGE`: 0:54, 1:13,659, 2:15,964, 3:323. Code 0 is observed but absent from the official 1–3 description.
- `AGE`: 21–79, mean 35.4855, median 34, 1st/99th percentiles 22/60. All 56 observed integer ages and frequencies are preserved in Notebook output.

### Credit exposure

- `LIMIT_BAL`: minimum 10,000; median 140,000; mean 167,484.3227; 95th percentile 430,000; 99th percentile 500,000; maximum 1,000,000; 81 unique values; no zero, negative, or missing values.
- Luca retains it as a predictor and denominator in ratio-like engineered variables. Division-by-zero is not present in this CSV, but semantic and stability merits of the ratios remain for later validation.

### Billing history

- `BILL_AMT1…6` contain no missing values, but each includes zeros and negative balances. Negative counts by month are 590, 669, 655, 675, 655, 688; zero counts rise from 2,008 (`BILL_AMT1`) to 4,020 (`BILL_AMT6`).
- Monthly means range from 38,871.7604 to 51,223.3309. Observed extrema include -339,603 (`BILL_AMT6`) and 1,664,089 (`BILL_AMT3`). These values were described, not cleaned or winsorized.
- Luca retains all six raw bill amounts and uses them in inferred-expense and closeness formulas.

### Payment history

- `PAY_AMT1…6` contain no missing or negative values. Zero counts range from 5,249 to 7,173 (17.4967%–23.9100%).
- Monthly medians range from 1,500 to 2,100; maxima range from 426,529 to 1,684,259.
- Luca retains all six raw payment amounts, inspects payments above 300,000 without deleting rows, and uses payment/bill relationships in engineered formulas.

### Target

- Actual target name: `default.payment.next.month`; values are exactly 0 and 1.
- Distribution: 23,364 non-default and 6,636 default. Stage 2 documents the imbalance but performs no sampling or class weighting.

## 11. Official / Actual CSV / Luca Comparison

Official evidence location for all rows: UCI page cited in section 4, accessed 2026-08-09. CSV evidence: executed Notebook cells 3–17. Luca evidence: object/cell locations below.

| Field/group | Official/documented meaning | Actual CSV behavior | Luca interpretation/treatment | Agreement, discrepancy, uncertainty, evidence |
|---|---|---|---|---|
| `SEX` | 1 male, 2 female | Exactly 1/2; counts 11,888/18,112 | Counts/plots, retains coded field initially; later uses sex interactions | Agreement on codes. Luca cells 46–53, 70+, 103–129. |
| `EDUCATION` | 1 graduate school, 2 university, 3 high school, 4 others | Also 0, 5, 6; combined 345 rows | Maps 0/5/6 to 4 | Mapping is pragmatic but undocumented meanings remain unresolved; not an official equivalence. Luca cells 46–53, 61–68. |
| `MARRIAGE` | 1 married, 2 single, 3 others | Also code 0 (54 rows) | Maps 0 to 3 | Official page does not define 0; equivalence to “others” is unverified. Luca cells 46–53, 61–68. |
| `AGE` | Age in years | Integer 21–79 | Explores raw age, creates multiple bins, keeps a collapsed equal-width `AgeBin` | Raw semantics agree; bin boundaries/merges are Luca design choices, not official definitions. Luca cells 99–129. |
| `LIMIT_BAL` | Amount of given credit in NT dollars, including individual/family or supplementary credit | 10,000–1,000,000; no zero/missing/negative | Retains raw and divides bill-derived quantities by limit | Basic interpretation agrees. Ratio meaning/value needs later validation. Luca cells 3–16, 103–131. |
| Repayment status (`PAY_0`, `PAY_2…6`) | X6–X11, September–April status; official page lists -1 duly paid, 1–8 months delayed, 9 nine months or more | All six show -2, -1, 0 and positive codes; max 8, no 9. `PAY_5/6` have no code 1 | Renames `PAY_0→PAY_1`; maps -2/-1/0→0, then every positive code→1; final code retains six binary month flags | -2 and 0 are observed but not defined on the current official page. Binary mapping directly removes severity distinctions 2–8 while preserving six-month timing. Performance impact is not established. Luca cells 61–68, 97–98, 130–131. |
| `BILL_AMT1…6` | Statement amounts for September–April | No missing; zeros and negatives; wide right tails | Retains all raw values; builds inferred-expense and closeness ratios | Month sequence and broad meaning agree. Negative balances and formula semantics are not documented by Luca. Luca cells 54–60, 69–96, 103–131. |
| `PAY_AMT1…6` | Previous-payment amounts for September–April | No missing/negative; 17.50%–23.91% zeros; wide right tails | Retains all raw values; inspects large values; combines with bills in formulas | Broad meaning agrees. Outliers are inspected but not formally justified. Luca cells 54–68, 69–96, 103–131. |
| Target | Default payment next month: yes=1, no=0 | Exactly 0/1; 22.12% positive | Renames to `def_pay`; uses as target | Agreement. Luca cells 3–16 onward. |

## 12. Repayment Status Deep Dive

`Stage 2 independently executed data check`:

| Raw code | PAY_0 | PAY_2 | PAY_3 | PAY_4 | PAY_5 | PAY_6 |
|---:|---:|---:|---:|---:|---:|---:|
| -2 | 2,759 | 3,782 | 4,085 | 4,348 | 4,546 | 4,895 |
| -1 | 5,686 | 6,050 | 5,938 | 5,687 | 5,539 | 5,740 |
| 0 | 14,737 | 15,730 | 15,764 | 16,455 | 16,947 | 16,286 |
| 1 | 3,688 | 28 | 4 | 2 | 0 | 0 |
| 2 | 2,667 | 3,927 | 3,819 | 3,159 | 2,626 | 2,766 |
| 3 | 322 | 326 | 240 | 180 | 178 | 184 |
| 4 | 76 | 99 | 76 | 69 | 84 | 49 |
| 5 | 26 | 25 | 21 | 35 | 17 | 13 |
| 6 | 11 | 12 | 23 | 5 | 4 | 19 |
| 7 | 9 | 20 | 27 | 58 | 58 | 46 |
| 8 | 19 | 1 | 3 | 2 | 1 | 2 |

The first transformation (`-2/-1/0 → 0`) and later transformation (all positive values `→1`) leave two levels in every monthly field. Counts mapped to 1 are 6,818, 4,438, 4,213, 3,510, 2,968, and 3,079. Counts with original severity above 1 are 3,130, 4,410, 4,209, 3,508, 2,968, and 3,079.

Across six months, the raw CSV contains 1,106 distinct repayment trajectories; applying Luca's final binary mapping yields 62. There are 10,069 rows with any positive delay and 8,380 with any severity above 1. Thus information compression is a directly demonstrated fact: the mapping removes both distinctions among non-positive statuses and severity distinctions among positive statuses. It does not remove month position because six binary fields remain in the final feature code. Whether the discarded distinctions improve generalization is not determined by Stage 2 and requires later controlled validation.

The current official UCI page documents `-1` and positive delay levels but does not explain the observed `-2` or `0`, so their business semantics remain unresolved. No inferred labels were assigned.

## 13. Train-Test, Validation, and Leakage Review

- Luca makes an initial 80/20 split in the blind-tree section and another 80/20 split after feature engineering; both use `random_state=42` with no explicit `stratify`.
- **Cells 32–34 holdout reuse:** after cell 21 creates the initial holdout, cell 32 applies KFold to the complete `df`. Observations originally assigned to that holdout therefore re-enter cross-validation/model development. Cell 34 reports feature importance from the classifier left fitted on the final fold of the cell-32 loop, and that importance is used in the subsequent analysis/feature-design path. The supported finding is holdout reuse, development contamination, test-informed feature analysis, and loss of untouched independent final-test status.
- Cleaning, full-data target relationship exploration, feature ideation, and feature construction precede the later split. Most formulas are row-local and do not estimate global parameters, but feature choices are informed by the entire dataset, including what later becomes the holdout. This creates test-informed design risk even where direct value leakage is not shown.
- No scaler, encoder, or imputer is fitted, so there is no fitted-transform leakage of those types.
- Sampling variants are constructed after the later split and from the training frame. That ordering is appropriate in principle; Stage 2 does not validate implementation quality or reproduce SMOTE.
- `GridSearchCV(cv=5)` is used, but stratification is not explicitly specified in Luca's code. Manual KFold uses five folds with shuffle and seed 42 and is not stratified.
- Separately from the cells 32–34 holdout-reuse issue, the later model-comparison loop in cells 144/146 is materially unsafe: it splits one frame but retrieves fold rows from the full `df` by positional index. For sampled frames this does not evaluate the intended rows and may draw records belonging to the earlier holdout. This is strong code-level evidence of train/validation contamination and invalid sample comparisons.
- Twelve ensemble/sampling variants are then evaluated on the same holdout, and outcomes inform narrative comparison. Reusing the holdout for iterative choice weakens its role as an untouched final test set.
- There is no separate final test set after those comparisons and no final accepted model. Evaluation reports accuracy, F1, confusion counts, precision and recall; no AUC, KS, calibration, or threshold optimization is saved.

Stage 2 creates no replacement split, CV, preprocessing, or evaluation pipeline.

## 14. Luca Saved Model and Evaluation Results

Every number below is a `Saved output from Luca reference notebook`; none was reproduced by Stage 2.

### Early decision-tree outputs

| Saved result | Value |
|---|---:|
| Depth-10 test accuracy | 0.812000 |
| Depth-100 test accuracy | 0.723167 |
| Accuracy-grid test accuracy | 0.821000 |
| `PAY_1`-only tree test accuracy | 0.819333 |
| Plain-tree test F1 | 0.468317 |
| F1-grid test F1 | 0.473557 |
| Balanced-tree test F1 | 0.520379 |

The unexecuted grid cells in the later ensemble sections mean adjacent printed scalar “CV” values are preserved claims/constants, not evidence that those grid searches ran in the saved state.

### Final repeated-holdout outputs

| Model / training variant | Accuracy | F1 | TP | FP | TN | FN | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Random forest / normal | 0.811 | 0.454 | 470 | 289 | 4,398 | 843 | 0.62 | 0.36 |
| Random forest / upsampled | 0.804 | 0.488 | 559 | 419 | 4,268 | 754 | 0.57 | 0.43 |
| Random forest / downsampled | 0.737 | 0.519 | 849 | 1,112 | 3,575 | 464 | 0.43 | 0.65 |
| Random forest / SMOTE | 0.810 | 0.486 | 539 | 364 | 4,323 | 774 | 0.60 | 0.41 |
| AdaBoost / normal | 0.772 | 0.401 | 458 | 516 | 4,171 | 855 | 0.47 | 0.35 |
| AdaBoost / upsampled | 0.751 | 0.524 | 821 | 1,001 | 3,686 | 492 | 0.45 | 0.63 |
| AdaBoost / downsampled | 0.734 | 0.513 | 841 | 1,125 | 3,562 | 472 | 0.43 | 0.64 |
| AdaBoost / SMOTE | 0.802 | 0.479 | 546 | 422 | 4,265 | 767 | 0.56 | 0.42 |
| Gradient boosting / normal | 0.802 | 0.450 | 488 | 366 | 4,321 | 825 | 0.57 | 0.37 |
| Gradient boosting / upsampled | 0.732 | 0.469 | 709 | 1,001 | 3,686 | 604 | 0.41 | 0.54 |
| Gradient boosting / downsampled | 0.681 | 0.480 | 884 | 1,486 | 3,201 | 429 | 0.37 | 0.67 |
| Gradient boosting / SMOTE | 0.800 | 0.453 | 498 | 388 | 4,299 | 815 | 0.56 | 0.38 |

These results are not suitable for selecting this project's model: they come from the audited reference, share a repeatedly consulted holdout, and are affected by the comparison-code concerns above.

## 15. Audit Decision Log

Only the four Architect-authorized labels are used.

| ID | Luca decision and location | Evidence / impact | Classification | Rationale and future question | Stage 2 action |
|---|---|---|---|---|---|
| AD-01 | Exclude `ID` from predictors; cells 17–19, 131 | Identifier is unique for all 30,000 rows | Keep | Prevents direct identifier use; later checks should still verify provenance/order artifacts | Independently checked against raw CSV |
| AD-02 | Rename target and `PAY_0`; cells 3–16 | Improves local naming but differs from source schema | Keep | Harmless if mapping is documented and source is preserved | Recorded only |
| AD-03 | `EDUCATION` 0/5/6→4; cells 61–68 | Codes are observed but not defined on current UCI page | Needs Validation | Pooling sparse unknowns may be practical, but semantic equivalence is unproven | Independently checked against raw CSV; deferred |
| AD-04 | `MARRIAGE` 0→3; cells 61–68 | 54 code-0 rows; official page does not define 0 | Needs Validation | “Unknown” may not equal “others” | Independently checked against raw CSV; deferred |
| AD-05 | No row deletion after large-payment review; cells 61–68 | Large values exist; no demonstrated corruption | Keep | Preserving valid extremes is safer than arbitrary deletion | Recorded only |
| AD-06 | Map non-positive repayment codes to 0; cells 61–68 | -2/-1/0 are frequent; -2/0 semantics unresolved | Candidate for Improvement | It demonstrably merges distinct observed states; usefulness must be tested later | Independently checked against raw CSV; deferred |
| AD-07 | Map all positive repayment severity to 1; cells 97–98 | 8,380 rows have severity >1; raw trajectories fall 1,106→62 | Candidate for Improvement | Severity information is certainly compressed, but model impact is not proven | Independently checked against raw CSV; deferred |
| AD-08 | Keep six binary month positions in final code; cell 131 | Code includes `PAY_1…6` despite contrary Markdown | Keep | Retains timing at a coarse level; resolve documentation contradiction | Recorded only |
| AD-09 | Create sex/marriage and sex/age interactions; cells 103–129 | Based on full-data EDA; categories manually defined | Needs Validation | Added complexity and stability/fairness implications require controlled evaluation | Deferred to later Architect decision |
| AD-10 | Equal-width age binning and merge bin 6→5; cells 103–129 | Several candidate bins tried on full data | Candidate for Improvement | Boundaries are not domain-justified and discard continuous resolution | Recorded only; deferred |
| AD-11 | `Avg_exp_*` and `Closeness_*` formulas; cells 103–131 | Row-local formulas use bills/payments/limit; names/semantics are not rigorously justified | Needs Validation | Need formula review, temporal interpretation, stability, and incremental-value tests | Recorded only; deferred |
| AD-12 | Feature design before split; cells 69–131 | Target-aware EDA and choices use full data before holdout assignment | Candidate for Improvement | Creates test-informed design risk; future design should isolate decision data | Recorded only; deferred |
| AD-13 | 80/20 seed-42 split without explicit stratify; cells 19, 131 | Binary target is 22.12% positive | Candidate for Improvement | Reproducible, but class-balance preservation is implicit rather than guaranteed | Recorded only; deferred |
| AD-14 | Sampling after split; cells 132–140 | Variants are built from the training frame | Keep | Correct ordering principle; sampling value itself remains unvalidated | Recorded only |
| AD-14A | Reuse the cell-21 holdout through full-`df` KFold and last-fold feature importance; cells 32–34 | Cell 32 folds over complete `df`, reintroducing observations originally assigned to the holdout; cell 34 uses importance from the classifier left by the final fold, and that importance informs later analysis/feature design | Incorrect / Risky | Full-`df` KFold causes development contamination, test-informed feature analysis, and loss of untouched independent final-test status | Recorded only; no rerun or repair |
| AD-15 | Manual KFold sample indexing via full `df`; cells 144/146 | Split indices from sampled frames index a different full frame and may include holdout rows | Incorrect / Risky | Code-level row-mapping error invalidates intended comparisons and creates contamination risk | Recorded only; no rerun or repair |
| AD-16 | Printed constants next to unexecuted grids; cells 148–176 | Twelve grid cells have null execution state; adjacent constants have saved output | Incorrect / Risky | Saved state does not substantiate the implied grid execution lineage | Recorded only; no rerun |
| AD-17 | Repeated use of one holdout for 12 variants; cells 177–191 | Same test set used for comparative reporting | Candidate for Improvement | Iterative consultation weakens final-test independence | Recorded only; deferred |
| AD-18 | Shift from accuracy to F1 and confusion components; cells 17–45, 177–192 | Positive class is minority and costs are asymmetric | Keep | Better than accuracy alone, while final business metric remains undecided | Recorded only |
| AD-19 | No final model; conclusion cell 192 | Luca explicitly questions practical value/metric/domain knowledge | Keep | Appropriately avoids overstating an inconclusive exercise | Recorded only |

## 16. Uncertainties and Ambiguities

- The current official UCI page does not define observed repayment codes `-2` and `0`; their exact operational meanings are unresolved here.
- The current official UCI page does not define `EDUCATION=0/5/6` or `MARRIAGE=0`; their semantics cannot be established from the CSV.
- Luca's Markdown says final repayment fields will be excluded, but the immediately following feature list includes all six; the code is treated as implementation truth, while intent remains ambiguous.
- Luca's later grid-search cells are unexecuted, so the provenance of adjacent hard-coded/printed CV numbers is not demonstrable from the saved notebook.
- Saved charts and model numbers were inspected but not reproduced; their dependency-version sensitivity is unknown.
- The direct performance effect of retaining raw repayment severity versus Luca's binary mapping is unknown and deliberately not tested in Stage 2.

## 17. Suspected Issues

- The manual model-comparison fold indexing is a concrete code defect and likely contaminates comparison data.
- Full-data, target-aware EDA and feature decisions before the later split risk optimistic design choices.
- Repeated holdout evaluation prevents that holdout from functioning as a fully untouched final assessment.
- Sparse/undocumented category pooling may obscure materially different records.
- Coded categorical variables are passed to tree models as numbers; this imposes split order relationships that need validation.
- Age and repayment-status compression may discard useful resolution.
- Engineered formula names and month alignment are insufficiently documented for credit-risk interpretation.

## 18. Potential Future Improvement Directions

These are candidates for later Architect decisions only; none was implemented:

- Obtain/confirm authoritative codebook semantics for observed `PAY_*` values -2 and 0 and undocumented demographic codes.
- Compare raw/ordinal, carefully grouped, and alternative repayment representations under a leakage-safe design, including stability and interpretability—not just predictive lift.
- Establish an isolated split/CV protocol before target-aware feature decisions; reserve a final assessment set.
- Replace the faulty sample-indexing loop and independently verify every validation frame's row identity.
- Validate category treatment, temporal alignment, and mathematical definitions for Luca's interaction and ratio features.
- Evaluate whether continuous age and raw severity should be retained alongside any grouped representations.
- Choose metrics and thresholds from explicit credit-risk costs and operational goals; consider discrimination and calibration measures only in a later modeling stage.
- Use a simple reproducible baseline before complex sampling/ensemble comparisons; treat imbalance strategy as a controlled model-selection question.

## 19. Explicit Out-of-Scope Items

No formal feature engineering, feature catalog, processed dataset, feature selection, automated transformation search, train/test pipeline, CV pipeline, SMOTE, over/undersampling, class weighting, model training, model comparison, hyperparameter tuning, threshold optimization, WOE, scorecard, Logistic Regression, tree/boosting model, `src/`, `tests/`, Git stage/commit/push, or Stage 3 work was performed. The Luca Notebook was not executed.

## 20. Files Created or Modified

Created:

- `/Users/yu/2.工作/Risk Analysis Project/notebooks/01_luca_audit_and_data_understanding.ipynb`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`

No pre-existing project file was modified, moved, or deleted. The new Notebook was written once as a template and then saved with its own clean-kernel outputs; this is the authorized Stage 2 artifact lifecycle.

## 21. Reproducibility Checks

| Check | Result |
|---|---|
| Python executable | `/Users/yu/2.工作/Risk Analysis Project/.venv/bin/python` |
| Python version | 3.9.6 |
| Core package metadata | pandas 2.3.3; numpy 2.0.2; scipy 1.13.1; scikit-learn 1.6.1; matplotlib 3.9.4; seaborn 0.13.2 |
| Execution method | Existing `jupyter_client 8.6.3` / `ipykernel 6.31.0`, new `python3` kernel, project root as working directory |
| Clean run | PASS: all 10 code cells executed in order with counts 1–10; 40 saved output objects; zero error outputs |
| Input resolution | Notebook resolves its own launch/project directory and uses `/Users/yu/2.工作/Risk Analysis Project/data/raw/UCI_Credit_Card.csv` |
| Manual data modification dependency | None |
| Processed data written | No |
| Model trained | No |
| Sampling/splitting performed | No |
| Luca Notebook executed | No |
| Output lineage | Stage 2 statistics are saved Notebook outputs; Luca metrics are read from object 2 saved outputs and labeled separately |

The installed `jupyter` launcher points to a stale interpreter path and `nbformat`/`nbclient`/`nbconvert` are not installed. No package was installed. A direct existing-kernel executor was therefore used. Its first clean-kernel attempt stopped before saving because an unrelated shell-channel reply preceded the current reply; matching was corrected and the Notebook was then rerun from another clean kernel successfully. A harmless `jupyter_client` destructor warning appeared after explicit shutdown, after all outputs had been saved; it is not a cell error.

## 22. Input Integrity Verification

The following values were checked before project writes and again after Notebook execution. Final verification after report creation is recorded here as the same values.

| Input | Path | Before / after size | Before / after SHA-256 | Before / after modification time | Conclusion |
|---|---|---:|---|---|---|
| Formal raw CSV | `/Users/yu/2.工作/Risk Analysis Project/data/raw/UCI_Credit_Card.csv` | 2,862,995 / 2,862,995 bytes | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` / same | `2019-09-20T00:04:18-0400` / same (epoch 1568952258) | Unchanged |
| Luca reference | `/Users/yu/2.工作/Risk Analysis Project/references/credit-card-default-a-very-pedagogical-notebook.ipynb` | 805,686 / 805,686 bytes | `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd` / same | `2026-08-06T20:42:25-0400` / same (epoch 1786063345.355446903) | Unchanged |

Stage 0 and Stage 1 records were also left untouched.

## 23. Git Diff and Working-Tree Summary

| Git check | Start | End |
|---|---|---|
| Branch | `main` | `main` |
| HEAD | `aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c` | `aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c` |
| Tracked working tree | Clean | Clean; no tracked diff |
| Staged diff | Empty | Empty |
| Remote | None | None |
| `.git/index` SHA-256 | `b91d045d66df39a2d803324b9f5f0ffc194a16c96806acfaae485d8777b0dbd1` | Same |

Final read-only Git results:

- `git diff --name-status`: empty.
- `git diff --stat`: empty.
- `git diff --cached --name-status`: empty.
- Untracked: only `notebooks/01_luca_audit_and_data_understanding.ipynb` and `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`.
- Ignored local items remain `.DS_Store`, `.venv/`, and `references/credit-card-default-a-very-pedagogical-notebook.ipynb`.
- No index change, stage, commit, history rewrite, remote change, fetch, pull, push, reset, or rebase occurred.

Because both Stage 2 artifacts are untracked, ordinary `git diff` is empty; `git status --short` is the authoritative current listing.

## 24. Acceptance Criteria Check

| # | Architect completion criterion | Result | Evidence |
|---:|---|---|---|
| 1 | Complete workflow map from real Luca Notebook | PASS | Sections 5–7 cover objects and every cell 0–193 in order |
| 2 | Key Luca decisions relocatable/traceable | PASS | Exact object/cell locations in sections 7, 8, 13, 15 |
| 3 | Formal UCI key variables independently checked | PASS | Executed Notebook and sections 9–12 |
| 4 | Official meaning / actual data / Luca treatment compared | PASS | Structured comparison in section 11 |
| 5 | Important treatments classified under required framework | PASS | Section 15 uses only the four authorized labels |
| 6 | Luca saved vs Stage 2 executed results separated | PASS | Explicit labels and separate sections 9/14 |
| 7 | Uncertainty and unresolved questions recorded | PASS | Sections 11, 12, 16 |
| 8 | Future directions recorded but not implemented | PASS | Sections 18–19; no derived dataset/model |
| 9 | No formal Feature Engineering | PASS | Audit-only temporary mappings; no feature artifact or export |
| 10 | No new formal model trained | PASS | Notebook source/output and scope checks |
| 11 | Raw data and Luca reference unmodified | PASS | Matching path/size/hash/mtime in section 22 |
| 12 | All Stage 2 numbers from real execution | PASS | Clean-kernel saved outputs, execution counts 1–10, no errors |
| 13 | Notebook can be rerun | PASS | Formal relative input resolution; independent clean run succeeded |
| 14 | Report sufficient for later Architect decisions | PASS | Full workflow, facts, saved results, decisions, issues, uncertainties |
| 15 | Codex did not commit | PASS | HEAD/index unchanged; artifacts remain untracked |
| 16 | Codex did not push | PASS | No remote and no push |

## 25. Incomplete Items

None identified within the authorized Stage 2 scope. Semantic uncertainties and future experiments are explicitly deferred, not incomplete Stage 2 construction.

## 26. Deviations and Limitations

- No scope deviation is known.
- The standard `jupyter` launcher is stale and higher-level Notebook execution packages are absent. The existing `jupyter_client`/`ipykernel` stack was used without installation; this is an execution-environment limitation, not a deliverable failure.
- The Luca Notebook was deliberately not executed, so its saved outputs were audited but not reproduced or validated for runtime correctness.
- Official definitions are limited to what the current UCI page states; undocumented observed codes remain unresolved rather than guessed.
- No model-performance claim is made about alternative repayment representations because such an experiment is outside Stage 2.
- No unexpected project modification was observed.

## 27. Reviewer Result

`Pending independent Reviewer assessment.`

Reviewer notes:

<!-- Reviewer to append PASS/FAIL and supporting findings here. -->

# REVIEWER RESULT

## Verdict

PASS WITH FIXES

## Requirement Check

- Luca workflow audit and traceability: Substantially satisfied. The Workflow Map covers cells 0–193 without gaps and accurately locates the major cleaning, EDA, feature-engineering, split, sampling, validation, modeling, evaluation, and conclusion steps. One important validation-risk path at cells 32–34 is described but not explicitly audited as a holdout-contamination/design-risk issue; this must be added before closeout.
- Saved-output handling: Satisfied. Luca saved outputs are clearly separated from Stage 2 independently executed results. The reported early tree metrics, unexecuted grid cells, adjacent hard-coded/printed values, and twelve final holdout result rows match the saved second Notebook object.
- Independent UCI data understanding: Satisfied. Shape, schema, dtypes, missingness, duplicate rows, ID uniqueness, target distribution, demographic codes, credit exposure, repayment-code frequencies, billing/payment behavior, and target facts were independently reproduced.
- Repayment-status deep dive: Satisfied. The raw-code tables, Luca binary mapping, 1,106-to-62 trajectory compression, 10,069 rows with any positive delay, and 8,380 rows with severity above one were independently confirmed. The report appropriately treats model impact and undocumented code semantics as unresolved.
- Official / actual CSV / Luca comparison: Satisfied. The required field groups are covered and undocumented values are preserved as uncertainties rather than assigned invented meanings.
- Audit Decision Framework: Satisfied in form and substantially in substance. All 19 entries use only the four authorized labels and are evidence-linked. A further entry or explicit amendment is required for the cells 32–34 full-data KFold/feature-importance path.
- Notebook reproducibility and scope: Satisfied. The Notebook contains 10 code cells with saved execution counts 1–10, 40 saved output objects, and zero error outputs. The Reviewer reran all 10 code cells in order from a fresh process without saving the Notebook; all passed. No split, sampling, model training, processed-data output, formal feature engineering, `src/`, `tests/`, or `feature_catalog.md` was found.
- Input and Git protection: Satisfied. The formal CSV and Luca reference retain the audited sizes, modification times, and SHA-256 values. Existing tracked files are unchanged; branch remains `main` at `aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c`; staged and tracked diffs are empty; only the two Stage 2 artifacts are untracked; index hash is unchanged; no remote exists.
- Formal Task Report contract: Not satisfied. The Stage 2 Report does not contain the required immutable Architect Specification or its begin/end markers. The Reviewer recovered the complete original Specification from the same ChatGPT Project's `Architect-02` record to make this assessment possible, but the local Task Report itself remains non-compliant.

## Independent Verification

The Reviewer independently recovered and read the original Stage 2 Architect Specification; parsed both concatenated Luca Notebook objects; checked all 194 source cells and the cited critical cell ranges; verified saved execution states and model-result outputs; recomputed the CSV integrity, demographic, exposure, repayment, billing, payment, and target results; reran the new Notebook's 10 code cells in a fresh process without saving it; recalculated protected-input hashes and timestamps; inspected the final project tree; and checked Git branch, HEAD, index hash, staged/unstaged state, untracked paths, history, and remotes. The two Stage 2 artifacts are technically sound and within scope apart from the reporting issues below.

## Issues Found

1. **Missing Immutable Architect Specification:** `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md` contains no verbatim Architect Specification and no required begin/end markers. Its locally authored acceptance table cannot replace the authoritative contract.
2. **Omitted cells 32–34 validation risk:** after Luca creates an 80/20 holdout at cell 21, cell 32 runs KFold over the full `df`, necessarily bringing rows from that earlier holdout back into cross-validation. Cell 34 then reports feature importance from the classifier left fitted on the final full-data KFold training fold, and the subsequent workflow continues to use those observations for design. The report mentions the cell-32 KFold but does not identify this specific holdout reuse or trace the downstream feature-importance/design risk in the validation review or Decision Log.
3. **Character/byte boundary conflation:** section 5 labels `0:110611` and `110611:805682` as “Byte/character boundary.” These are character offsets. The correct UTF-8 byte ranges are `[0,110611)` and `[110611,805685)`, followed by the final newline byte to total 805,686 bytes.

## Required Fixes

1. Insert the complete, exact original Stage 2 Architect Specification into the Task Report between explicit `BEGIN ARCHITECT SPECIFICATION` and `END ARCHITECT SPECIFICATION` markers. Do not paraphrase or reconstruct it from the current report.
2. Amend the Workflow/validation discussion and Audit Decision Log to explicitly record the cells 32–34 full-`df` KFold reuse after holdout creation, its inclusion of earlier holdout rows, and the lineage of the cell-34 feature importance. Classify it using the authorized framework and keep it distinct from the later cells 144/146 wrong-frame `df.iloc` defect.
3. Correct section 5 to distinguish character offsets from UTF-8 byte offsets.
4. Resubmit the corrected report without changing the verified Notebook, protected inputs, or other project files. Reviewer should then verify only the fixes plus working-tree drift before issuing final PASS and local commit.

## Architect Decision Needed

None

# REVIEWER RESULT

## Verdict

PASS

## Requirement Check

- Immutable Architect Specification: Satisfied. The complete original Stage 2 Specification is now preserved between explicit begin/end markers and was compared character-for-character with the authoritative `Architect-02` source; the text is an exact match.
- Cells 32–34 validation-risk correction: Satisfied. The validation review now explicitly records the post-holdout full-`df` KFold reuse, reintroduction of earlier holdout rows, last-fold model lineage for cell-34 feature importance, downstream test-informed design risk, and loss of untouched final-test status. `AD-14A` separately classifies this path as `Incorrect / Risky`, distinct from the later cells 144/146 wrong-frame indexing defect.
- Character/byte boundary correction: Satisfied. The report now distinguishes character ranges `[0,110611)` / `[110611,805682)` from UTF-8 byte ranges `[0,110611)` / `[110611,805685)`, followed by one newline byte for the verified 805,686-byte file.
- Previously verified Stage 2 deliverables: Remain satisfied. The Stage 2 Notebook hash is unchanged; the formal CSV and Luca reference hashes, sizes, and modification times are unchanged; no existing tracked project file changed; no new artifact or out-of-scope work appeared.
- Git readiness: Satisfied. Before Reviewer closeout, branch remained `main` at the Stage 1 commit, staged and tracked diffs were empty, only the two reviewed Stage 2 artifacts were untracked, and no remote was configured.

## Independent Verification

The Reviewer re-read only the corrected report regions, compared the inserted Specification character-for-character against the authoritative same-project Architect record, checked the new validation discussion and `AD-14A` against Luca cells 21 and 32–34, recalculated character and UTF-8 byte boundaries, rechecked the Stage 2 Notebook and protected-input hashes, and confirmed working-tree scope and Git state. No drift from the previously verified Stage 2 Notebook or inputs was found.

## Issues Found

No remaining issue affecting Stage 2 acceptance was found.

## Required Fixes

None.

## Architect Decision Needed

None
