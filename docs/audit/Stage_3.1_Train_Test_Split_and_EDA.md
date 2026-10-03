# Stage 3.1 — Train/Test Split & Train-only EDA

## 1. Task Identification, Objective, and Scope

- Task: `Stage 3.1 — Train/Test Split & Train-only EDA`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Execution date: 2026-08-11
- Delivery state: Completed for independent Reviewer assessment; not committed
- Objective: verify the formal raw input, create the locked deterministic Train/Holdout Test split, seal Test, and conduct the authorized target-aware EDA on Train only.
- Scope boundary: no feature-engineering decision or implementation, WOE, formal binning, sampling, model, cross-validation, tuning, threshold, or Stage 3.2/3.3 work.

## 2. Immutable Architect Specification

The following immutable block is copied verbatim from the formal Stage 3.1 Codex Prompt.

================ BEGIN ARCHITECT SPECIFICATION ================

# Stage 3.1 — Train/Test Split & Train-only EDA

## 1. Stage 3 总体结构

Stage 3 已被正式拆分并锁定为：

### Stage 3.1
Train/Test Split + Train-only EDA

### Stage 3.2
Feature Engineering Design

由 Architect 与用户基于 Stage 2 Audit + Stage 3.1 实际 EDA 结果讨论并冻结最终 feature specification。

### Stage 3.3
Feature Engineering Implementation + Sanity Check + Apply to Test

Stage 3.3 只能按照已经冻结的 Stage 3.2 specification 实施。

当前只执行：

> **Stage 3.1**

不得提前执行 Stage 3.2 或 Stage 3.3。

---

# 2. 项目当前稳定状态

项目路径：

`/Users/yu/2.工作/Risk Analysis Project`

正式 raw dataset：

`data/raw/UCI_Credit_Card.csv`

当前 Git 基线应为：

- Branch: `main`
- Stage 2: PASS / CLOSED
- HEAD:
  `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`
- Short hash:
  `4d29c19`
- Commit message:
  `Audit reference notebook and dataset`
- Parent:
  `aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c`
- Working tree 应为 clean
- 当前无 remote
- 当前未 push GitHub

Stage 2 已确认的正式产物包括：

- `notebooks/01_luca_audit_and_data_understanding.ipynb`
- `task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`

Stage 2 已建立 Luca workflow、UCI 数据事实和 Audit Decision Log。

Stage 3.1 可以读取 Stage 2 Report 作为事实参考，但不得修改 Stage 2 文件。

---

# 3. Stage 3.1 核心目标

本阶段只完成：

1. 快速确认正式 raw data 状态；
2. 建立正式、可重复的 Train / Holdout Test split；
3. 从 split 完成后封存 Test；
4. 只在 Train / Development data 上进行 target-aware EDA；
5. 对 Luca 中有意义的 EDA 方法进行保留和修正；
6. 形成清晰、有限、编号化的 EDA 图表与事实结论；
7. 为之后 Architect + 用户逐图复盘提供正式依据；
8. 为 Stage 3.2 Feature Engineering Design 提供事实输入。

Stage 3.1 的核心理念：

> **看懂 Train 数据，但不开始决定或实现 Feature Engineering。**

---

# 4. Stage 3.1 明确禁止事项

不得：

- 训练 Logistic Regression；
- 进行 WOE；
- 正式 binning；
- 训练任何 machine-learning model；
- Random Forest；
- XGBoost；
- LightGBM；
- Decision Tree；
- model selection；
- hyperparameter tuning；
- threshold optimization；
- model evaluation；
- SMOTE；
- oversampling；
- undersampling；
- class weighting；
- 正式 feature selection；
- 创建最终 Feature Engineering pipeline；
- 创建 utilization features；
- 创建 delinquency summary features；
- 创建 `SE_MA` feature；
- 创建 `SE_AG`；
- 创建 Luca 的 `Avg_exp_*`；
- 创建 Luca 的 `Closeness_*`；
- 创建 `AgeBin`；
- 创建任何新的正式 predictor；
- 根据 Test 数据决定任何分析、清洗或 feature；
- 修改 raw CSV；
- 修改 Luca reference Notebook；
- 修改 Stage 0 / 1 / 2 records；
- 提前创建 Stage 3.2 或 Stage 3.3 产物；
- 创建 `src/`；
- 创建 `tests/`；
- commit；
- push；
- 添加 remote；
- amend / rebase / reset。

本阶段：

> **EDA 可以提出问题，但不得替 Stage 3.2 做最终 feature decision。**

---

# 5. Pre-flight

Codex 开始写入前必须确认：

1. 当前 project root 正确；
2. Git top-level 正确；
3. branch = `main`；
4. HEAD =
   `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`；
5. working tree clean；
6. 无 staged changes；
7. formal CSV 存在；
8. Stage 2 Notebook 存在；
9. Stage 2 Task Report 存在；
10. Luca reference Notebook 存在；
11. 无 remote。

如 baseline 与此不一致：

- 不修改项目；
- 不自行 reset；
- 不自行 stash；
- 不覆盖用户修改；
- 返回 BLOCKED 并准确报告实际状态。

---

# 6. Step 1 — Raw Data Integrity Check

在 split 前允许对完整 raw dataset 做：

> **非 target-aware 的结构与完整性检查。**

目的是确认 Stage 2 后数据没有漂移，而不是重新做 Stage 2 Audit。

至少确认：

- shape；
- column names；
- dtypes；
- missing values；
- duplicate rows；
- ID uniqueness；
- duplicate IDs；
- non-finite numeric values；
- target column 名称及合法取值集合；
- `SEX` actual codes；
- `EDUCATION` actual codes；
- `MARRIAGE` actual codes；
- `AGE` basic range；
- `LIMIT_BAL` basic range；
- `PAY_*` raw code sets；
- `BILL_AMT*` 是否存在 zero / negative / extreme values；
- `PAY_AMT*` 是否存在 zero / negative / extreme values。

不得在 split 前使用完整数据计算：

- category default rates；
- target relationship；
- correlation with target；
- feature usefulness；
- interaction usefulness；
- target-driven bins。

---

# 7. PAY Sanity Check

Stage 3.1 需要特别继续理解 `PAY_*`。

六个月字段：

- `PAY_0`
- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`

当前原则：

> 不因为 `-2`、`0` 的官方业务语义存在不确定性，就把 PAY 直接二元化。

Stage 3.1 要检查的是：

### Raw code behavior

- 每个月实际出现哪些 codes；
- 每个 code 的 frequency；
- code 是否在不同月份系统性存在；
- 有没有只出现极少次数、明显可疑的 code。

### Adjacent-month transitions

查看相邻月份的状态变化：

- `PAY_6 → PAY_5`
- `PAY_5 → PAY_4`
- `PAY_4 → PAY_3`
- `PAY_3 → PAY_2`
- `PAY_2 → PAY_0`

确认常见 transition patterns。

### Repayment trajectories

检查六个月 raw repayment trajectories：

- distinct trajectory count；
- most common trajectories；
- representative patterns。

目的：

> 判断这些特殊状态是否是系统性数据行为，而不是少量明显脏数据。

不得：

- 给 `-2` / `0` 编造官方没有确认的业务含义；
- 将 PAY 转成 binary；
- 修改 PAY raw values；
- 基于 sanity check 创建正式 features。

---

# 8. Step 2 — 正式 Train/Test Split

Stage 3.1 正式固定：

- Train / Development = 80%
- Holdout Test = 20%
- `random_state=42`
- `stratify=y`

使用正式 target：

`default.payment.next.month`

Split 完成后：

> **Test 必须被封存。**

后续 target-aware EDA 只能使用 Train。

Test target 不得用于：

- EDA；
- default-rate comparison；
- feature design；
- category decisions；
- interaction decisions；
- outlier decisions；
- predictor decisions。

允许对 Test 做的只有必要技术性验证，例如：

- row count；
- schema；
- ID overlap assertion；
- split integrity；
- 后续阶段转换是否成功。

不得生成 Test target EDA 图。

---

# 9. Split Reproducibility

必须保存足够的信息，使 Stage 3.3 可以准确复现同一 split。

至少记录：

- split method；
- seed；
- stratification target；
- Train row count；
- Test row count；
- Train/Test ID 不重叠；
- Train + Test 恰好覆盖全部 ID；
- Train ID set 的稳定 hash；
- Test ID set 的稳定 hash。

无需为了 split 单独创建大量数据文件。

除非确有必要，否则：

> 不创建 train.csv / test.csv 等冗余数据副本。

后续阶段可依据同一 raw data + 同一 deterministic split logic 重现。

---

# 10. 已确认的 EDA-only Category View

Stage 3.1 为了让 EDA 图表可理解，可以在 Train 中建立用于分析显示的 category mapping。

这不是最终 modeling pipeline。

## EDUCATION

用于 EDA 显示：

- 1 → Graduate School
- 2 → University
- 3 → High School
- 0 / 4 / 5 / 6 → Other / Unknown

必须说明：

- 官方 4 为 Others；
- 0 / 5 / 6 的准确官方语义没有确认；
- 合并只是当前分析 / 后续建模上的 grouping choice；
- 不宣称这些原始 codes 在真实业务语义上完全相同。

## MARRIAGE

用于 EDA 显示：

- 1 → Married
- 2 → Single
- 0 / 3 → Other / Unknown

同样不得宣称 code 0 官方含义就是 Other。

## AGE

保留 continuous。

Stage 3.1：

- 不创建 AgeBin；
- 不手工分年龄段。

正式年龄 binning 留给 Stage 4 WOE。

---

# 11. Step 3 — Train-only EDA

所有下列 target-aware EDA 必须只基于 Train。

EDA 要：

- 有明确问题；
- 有适当图表；
- 有必要统计；
- 有简短、客观、基于实际输出的 observed finding；
- 不为了“丰富 Notebook”制造无意义图。

总体上可以借鉴 Luca 的 EDA 骨架，因为用户已经熟悉 Luca Notebook。

但必须修正 Luca 的 full-data target-aware design：

> 所有 target-aware EDA 只允许使用 Train。

---

# 12. EDA Figure Numbering

所有正式 EDA 图统一编号：

`EDA-01`
`EDA-02`
`EDA-03`
...

每张图必须在 Notebook 中明确标注编号和标题。

每张图附近必须有 Markdown 说明：

### Question
这张图在回答什么问题？

### Why
为什么值得看？

### Observed finding
根据实际运行结果观察到什么？

### Stage 3.2 question
这个发现给后续 Feature Engineering Design 留下什么问题？

注意：

`Stage 3.2 question` 只是待讨论问题。

不得直接写：

> “因此我们必须创建某 feature。”

真正的 Feature Engineering 决策由 Stage 3.2 的 Architect + 用户讨论完成。

---

# 13. Required EDA Figures

控制图表数量，建议形成以下有限图组。

如果实际数据表现导致某一种 visualization 技术上不合适，可以使用更合适的等价图，但不得随意扩张图数。

## EDA-01 — Train Target Distribution

展示：

- Train default / non-default count；
- Train class proportion。

目的：

建立 Train 风险背景。

---

## EDA-02 — SEX Distribution and Default Rate

展示：

- SEX sample count；
- default rate by SEX。

可使用一个 figure 内合理组合。

目的：

复核 Luca demographic EDA 的基础发现。

---

## EDA-03 — EDUCATION Distribution and Default Rate

基于已经定义的 EDA grouping。

展示：

- sample count；
- default rate。

目的：

理解不同 education groups 的风险差异。

---

## EDA-04 — MARRIAGE Distribution and Default Rate

基于已经定义的 EDA grouping。

展示：

- sample count；
- default rate。

---

## EDA-05 — AGE Distribution by Target

AGE 保持 continuous。

合理展示：

- age distribution；
- default vs non-default age pattern。

不得：

- 手工 AgeBin；
- 因图形结果创建年龄段 feature。

---

## EDA-06 — SEX × MARRIAGE Sample Size

展示：

- 各 SEX × MARRIAGE combination 的人数。

优先使用容易理解的 cross-tab / heatmap。

目的：

判断 interaction group 的 sample size 是否足够。

---

## EDA-07 — SEX × MARRIAGE Default Rate

展示：

- 各组合的 default rate。

目的：

重新检查 Luca `SE_MA` idea 是否在本项目 Train 中仍显示出明显联合风险结构。

重要：

本 Stage 只形成证据。

不得创建 `SE_MA` feature。

Stage 3.2 再由 Architect + 用户决定。

---

## EDA-08 — LIMIT_BAL Distribution and Target Relationship

展示：

- LIMIT_BAL distribution；
- default vs non-default 的基本差异。

不得在本阶段手工分箱。

---

## EDA-09 — PAY Raw Code Frequency by Month

展示六个月 PAY code distribution。

推荐：

- month × raw code frequency heatmap
或其他等价、清晰的 compact visualization。

目的：

帮助用户直观看到每个月状态编码如何分布。

---

## EDA-10 — PAY Raw Code Default Rate by Month

展示：

- 每个月；
- 每个 raw PAY code；
- 对应 Train default rate。

目的是回答：

> 不同 repayment severity / status 是否呈现不同的风险水平？

这张图非常重要。

不得因为结果看起来有层级关系就在 Stage 3.1 创建 feature。

---

## EDA-11 — PAY Adjacent-Month Transition Patterns

用 compact、可解释方式展示相邻月份 PAY transition。

可以：

- 在一个 figure 中使用多个 transition heatmap；
- 或选择其他清楚的等价 visualization。

必须覆盖完整月序转换逻辑，而不是只挑一个月。

同时在 Notebook 中保存：

- most common transitions；
- important unusual transitions；
- top raw repayment trajectories table。

目的：

帮助理解 PAY 的实际数据逻辑。

---

## EDA-12 — BILL_AMT Monthly Distribution

展示六个月 BILL_AMT 的：

- central distribution；
- negative values；
- zeros；
- extreme tail。

由于 BILL_AMT 存在极端值：

可以为了 visualization 使用合理的 percentile-limited view，

但必须：

- 明确说明图形显示范围被限制只是为了可视化；
- 同时保存完整 min/max/quantile statistics；
- 不删除任何 observation；
- 不修改 raw values。

---

## EDA-13 — PAY_AMT Monthly Distribution

展示：

- six-month payment distributions；
- zeros；
- extreme tail。

如果使用 log / log1p 仅用于 visualization：

必须明确说明：

> 这只是图形尺度变化，不是正式 feature transformation。

不得因此改变数据。

---

# 14. Supporting Tables

除正式图表外，应保存少量有学习价值的表格，例如：

- categorical group counts + default rates；
- PAY raw code counts；
- PAY raw code default-rate table；
- most common PAY trajectories；
- BILL_AMT monthly quantiles；
- PAY_AMT monthly quantiles。

不得生成几十张冗余统计表。

---

# 15. Luca 借鉴原则

Stage 3.1 不是推翻 Luca。

采用：

> Luca 有意义的方法保留；有问题的地方按 Stage 2 Audit 修正。

应体现以下关系：

### 保留 / 借鉴

- shape / info / describe / value counts；
- categorical EDA；
- demographic default-rate analysis；
- SEX × MARRIAGE analysis；
- PAY status analysis；
- BILL_AMT / PAY_AMT inspection；
- 不因为大额 payment 就任意删除记录。

### 修正

- Luca full-data target-aware EDA → 本项目只在 Train；
- Luca PAY binary compression → Stage 3.1 保留 raw codes；
- Luca feature design before split → 本项目 split 后才分析 Train。

### 本阶段不复制

- AgeBin；
- SE_MA feature 本身；
- SE_AG；
- Avg_exp_*；
- Closeness_*；
- sampling；
- modeling；
- KFold；
- GridSearch。

---

# 16. Stage 3.1 不做 Feature Engineering

尤其要明确：

以下目前只是未来可能讨论的候选概念：

- delinquent_months；
- max_delinquency；
- severe_delinquent_months；
- current_utilization；
- avg_utilization；
- max_utilization；
- SE_MA。

**Stage 3.1 一个都不要正式创建。**

即使 EDA 看起来明显支持，也必须：

1. 在报告中记录事实；
2. 留作 `Stage 3.2 question`；
3. 等 Architect + 用户逐图复盘后再决定。

---

# 17. Notebook 正式产物

创建：

`notebooks/02_train_test_split_and_eda.ipynb`

Notebook 应该是可学习的，而不只是代码容器。

推荐逻辑：

1. Stage objective / scope；
2. imports；
3. raw data integrity check；
4. split；
5. split integrity / reproducibility；
6. Train-only category views；
7. Train-only EDA；
8. PAY sanity analysis；
9. summary of observed findings；
10. Stage 3.2 open questions；
11. scope / no-modeling confirmation。

Notebook：

- 必须从 raw CSV 可重复运行；
- 必须保存真实 execution outputs；
- 必须无 error output；
- 必须保存所有 EDA figures；
- 图必须编号；
- 图旁必须有解释 Markdown；
- 不复制 Luca Notebook；
- 不执行 Luca Notebook；
- 不修改 raw data。

---

# 18. Task Report 正式产物

创建：

`task_records/Stage_3.1_Train_Test_Split_and_EDA.md`

Task Report 至少记录：

- Stage objective；
- pre-flight state；
- files inspected；
- raw integrity confirmation；
- split specification；
- Train/Test row counts；
- split reproducibility evidence；
- Train/Test ID-set hashes；
- explicit Test isolation statement；
- EDA figure inventory；
- 每张 EDA figure:
  - ID
  - question
  - variables
  - chart type
  - actual observed finding
  - Stage 3.2 question
- PAY sanity-check findings；
- Luca methods retained；
- Luca methods modified；
- Luca methods deferred / not copied；
- uncertainties；
- EDA findings requiring Architect/user discussion；
- explicit out-of-scope；
- files created / modified；
- Notebook reproducibility；
- protected-input integrity；
- Git status。

---

# 19. Immutable Architect Specification in Task Report

Stage 3.1 Task Report 必须完整保存本次 Architect Specification。

必须在 Report 中加入：

`================ BEGIN ARCHITECT SPECIFICATION ================`

和：

`================ END ARCHITECT SPECIFICATION ================`

中间保存本次正式 Architect Specification 原文。

不得：

- summary；
- paraphrase；
- 重写；
- 根据实际结果修改 Specification。

Task Report 必须能脱离 ChatGPT 历史，独立保留 authoritative task contract。

---

# 20. Learning-oriented Requirement

这是一个求职项目，同时也是用户学习项目。

因此 Notebook 和 Task Report 不应只有：

> “图表结果为 X。”

对于每个主要 EDA 图，应使用户之后能够理解：

1. 这张图是什么；
2. 为什么做；
3. 横纵轴是什么；
4. 它帮助判断什么；
5. 实际观察到什么；
6. 这个观察可能对 Stage 3.2 提出什么问题。

但不得提前替 Stage 3.2 做 feature decision。

---

# 21. Post-EDA Human Checkpoint

这是 Stage 3.1 最重要的流程边界。

Codex 完成 Stage 3.1 后：

> **必须停止。**

不得自动继续：

- Feature Engineering；
- Stage 3.2；
- Stage 3.3。

后续流程是：

1. Reviewer 独立审核 Stage 3.1；
2. Reviewer PASS 后按项目规则同轮 local commit；
3. 用户将 Stage 3.1 Notebook / Report 提供给 Architect；
4. Architect 与用户逐张 EDA 图复盘；
5. 解释：
   - 为什么画；
   - 看到了什么；
   - 是否与 Luca 一致；
   - 对后续 feature design 有什么含义；
6. 然后才进入 Stage 3.2。

---

# 22. Project Protection

不得修改：

`data/raw/UCI_Credit_Card.csv`

不得修改 Luca reference Notebook。

不得修改：

- Stage 0 record；
- Stage 1 report；
- Stage 2 report；
- Stage 2 Notebook；
- README 等其他现有 tracked files，除非本 Specification 明确要求。

Stage 3.1 原则上只应新增：

- Stage 3.1 Notebook；
- Stage 3.1 Task Report。

如确实必须修改其他文件：

必须有明确技术理由，并在 Completion Report 中说明；不得顺手“整理项目”。

---

# 23. Git / Reviewer Workflow

Codex：

**不要 commit。**

完成后保留 changes 给 Reviewer。

正式项目规则：

- Reviewer FAIL → no commit；
- Reviewer PASS → Reviewer 在同一轮直接 local commit；
- Reviewer commit 后必须 post-commit verification。

Codex：

- 不 git add；
- 不 commit；
- 不 push；
- 不加 remote；
- 不改历史。

---

# 24. Completion Criteria

Stage 3.1 只有以下全部满足才能交 Reviewer：

1. 正确确认 Stage 2 Git baseline；
2. raw data integrity 已快速复核；
3. 正式执行 80/20 split；
4. `random_state=42`；
5. `stratify=y`；
6. Test 从 split 后未参与 target-aware EDA；
7. Train/Test 无 ID overlap；
8. Train/Test union 覆盖完整数据；
9. split reproducibility 已记录；
10. EDA 全部只基于 Train；
11. AGE 未分箱；
12. PAY 未二元化；
13. 未创建正式 Feature Engineering features；
14. SEX × MARRIAGE 只做 EDA，不创建 SE_MA；
15. PAY raw-code / default-rate / transition behavior 已检查；
16. BILL/PAY_AMT inspection 已完成且未随意删除极端值；
17. 正式 EDA 图已编号；
18. 每张图有 Question / Why / Observed finding / Stage 3.2 question；
19. 图表数量有限且有明确用途；
20. Luca 有意义的 EDA 方法被合理借鉴；
21. Stage 2 已发现的问题没有被复制；
22. Notebook 可以从 raw data 干净重跑；
23. Notebook 无 error output；
24. Task Report 完整；
25. Task Report 保存完整 immutable Architect Specification；
26. raw CSV 未修改；
27. Luca reference 未修改；
28. Stage 0–2 artifacts 未修改；
29. 未进入 Feature Engineering implementation；
30. 未训练模型；
31. 未处理类别不平衡；
32. Codex 未 commit；
33. Codex 未 push。

---

# 25. Codex Completion Report

完成后 Codex 返回简明 Completion Report，至少包括：

- pre-flight result；
- files created；
- files modified；
- split settings；
- Train/Test row counts；
- split integrity result；
- Train/Test ID-set hashes；
- EDA figure count；
- EDA figure ID list；
- PAY sanity-check completion；
- Notebook clean-run status；
- protected input hash/integrity status；
- Test isolation confirmation；
- confirmation that no Feature Engineering was performed；
- confirmation that no modeling was performed；
- git branch；
- HEAD；
- git status；
- staged state；
- explicit confirmation:
  - no commit
  - no push
  - no remote change

不要在 Completion Report 中开始替 Architect 讨论 Stage 3.2。

================ END ARCHITECT SPECIFICATION ================

## 3. Pre-flight Baseline and Result

All required conditions were checked before the first project write and passed:

| Check | Actual pre-execution result |
|---|---|
| Project/Git top-level | `/Users/yu/2.工作/Risk Analysis Project` |
| Branch | `main` |
| HEAD | `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3` |
| HEAD subject | `Audit reference notebook and dataset` |
| HEAD parent | `aa0c3e8c58202fb72bfb4c62a1292cc8212aa43c` |
| Working tree | Clean; no tracked or untracked changes |
| Staged diff | Empty |
| Remote | None |
| Formal CSV | Present |
| Stage 2 Notebook/Report | Present; Reviewer PASS/CLOSED in committed report |
| Luca reference | Present at the documented ignored local path |
| Stage 3.1 output paths | Both absent |

No Git repair, reset, stash, checkout, clean, index write, or remote operation was needed or performed.

## 4. Files Inspected

- `/Users/yu/2.工作/Risk Analysis Project/data/raw/UCI_Credit_Card.csv`
- `/Users/yu/2.工作/Risk Analysis Project/notebooks/01_luca_audit_and_data_understanding.ipynb` (saved content only; not executed or modified)
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_2_Luca_Audit_and_Data_Understanding.md`
- `/Users/yu/2.工作/Risk Analysis Project/references/credit-card-default-a-very-pedagogical-notebook.ipynb` (identity only; not executed or modified)
- Project README, data README, references README, requirements, Stage 0/1 records, and read-only Git metadata.

## 5. Raw Integrity Confirmation

Before splitting, the Notebook performed only non-target-aware structure/integrity checks on the full raw data.

| Check | Actual result |
|---|---|
| Shape | 30,000 rows × 25 columns |
| Columns/dtypes | Source order preserved; 12 float64 amount/limit fields and 13 int64 fields |
| Missing values | 0 |
| Exact duplicate rows | 0 |
| ID | 30,000 unique; 0 duplicate IDs |
| Non-finite numeric values | 0 |
| Target identity/allowed set | `default.payment.next.month`; values exactly `{0, 1}`; no full-data target relationship calculated |
| Actual demographic codes | SEX `{1,2}`; EDUCATION `{0,1,2,3,4,5,6}`; MARRIAGE `{0,1,2,3}` |
| AGE range | 21–79 |
| LIMIT_BAL range | 10,000–1,000,000 |
| PAY raw sets | PAY_0/PAY_2/PAY_3/PAY_4: `-2…8`; PAY_5/PAY_6: `-2,-1,0,2…8` |
| BILL_AMT | Zeros, negatives, and extreme tails present |
| PAY_AMT | Zeros and extreme tails present; no negatives |
| Raw file | 2,862,995 bytes; SHA-256 `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |

No full-data category default rate, target relationship, target correlation, usefulness analysis, interaction analysis, or target-driven bin was computed before the split.

## 6. Formal Split Specification and Reproducibility Evidence

| Item | Formal value / result |
|---|---|
| Method | `sklearn.model_selection.train_test_split` |
| Train / Holdout Test | 80% / 20% |
| Seed | `random_state=42` |
| Stratification | `stratify=raw_df['default.payment.next.month']` |
| Train rows | 24,000 |
| Test rows | 6,000 |
| Train/Test schema | Identical to raw schema |
| ID overlap | 0; assertion passed |
| Full ID coverage | Train union Test equals all 30,000 raw IDs; assertion passed |
| Duplicate assignment / loss | None; cardinality and union assertions passed |
| Deterministic replay | Second in-memory call with identical inputs/settings reproduced both ID sets; assertion passed |
| Split files | None written; no `train.csv` or `test.csv` |

### Stable ID-set hash canonicalization

For each split: convert every ID to its canonical decimal integer string, sort numerically, join with one newline, add no trailing newline, UTF-8 encode, and calculate SHA-256.

- Train: 24,000 IDs; `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`
- Holdout Test: 6,000 IDs; `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`

These rules and full hashes are saved in both Notebook code and executed output for Stage 3.3 deterministic replay.

## 7. Explicit Test Isolation Statement

Test was used only for technical row-count, schema, ID-overlap, full-coverage, and deterministic-replay assertions. The Notebook did not calculate or display Test target distribution, category default rates, target relationships, outlier decisions, interactions, plots, or feature decisions. After technical assertions, the Test frame, replay objects, and full raw frame were deleted from the live EDA state. Every subsequent target-aware table and figure is sourced from `train_df` or its EDA-only display copy.

## 8. EDA-only Views

- EDUCATION view: 1 Graduate School; 2 University; 3 High School; 0/4/5/6 Other / Unknown.
- MARRIAGE view: 1 Married; 2 Single; 0/3 Other / Unknown.
- SEX view: verified UCI labels 1 Male, 2 Female.
- These three view columns exist only on the in-memory Train analysis copy; they are not formal predictors or a pipeline.
- EDUCATION code 4 is documented as Others, while 0/5/6 remain semantically unconfirmed. MARRIAGE code 0 also remains unconfirmed. The display grouping does not assert identical real-world meaning.
- AGE remains continuous. PAY raw values remain unchanged.

## 9. EDA Figure Inventory

All 13 figures are saved as real PNG outputs inside the executed Notebook. Each figure's nearby Markdown contains Question, Why, Observed finding, and Stage 3.2 question.

| ID | Title | Question / why / variables / chart | Actual Train-only observed finding | Stage 3.2 question |
|---|---|---|---|---|
| EDA-01 | Train Target Distribution | Establish class context; target count bars with proportion annotations | 18,691 non-default (77.8792%) and 5,309 default (22.1208%) | How should later metrics/criteria reflect imbalance? |
| EDA-02 | SEX Distribution and Default Rate | Compare support and Train rate; count/rate panels using SEX | Male 9,486 at 24.3728%; Female 14,514 at 20.6490% | How should predictive, fairness, and interpretability questions be handled? |
| EDA-03 | EDUCATION Distribution and Default Rate | Compare authorized EDA groups; count/rate panels | Graduate 8,455/19.2904%; University 11,256/23.7562%; High School 3,903/25.0064%; Other/Unknown 386/7.2539% | Preserve raw codes, explicit unknown grouping, or another transparent treatment? |
| EDA-04 | MARRIAGE Distribution and Default Rate | Compare authorized EDA groups; count/rate panels | Married 10,892/23.3015%; Single 12,806/21.0370%; Other/Unknown 302/25.4967% | Retain raw codes or a documented uncertainty-aware grouping? |
| EDA-05 | Continuous AGE Distribution by Target | Compare unbinned class distributions; density-normalized histograms | Means 35.37 vs 35.65; medians both 34; substantial overlap | What continuous treatment could be validated without target-driven manual bins? |
| EDA-06 | SEX × MARRIAGE Sample Size | Check interaction support; count heatmap | Four principal combinations contain 4,154–7,588; Other/Unknown cells have only 114 and 188 | Are joint groups sufficiently supported for later interaction testing? |
| EDA-07 | SEX × MARRIAGE Default Rate | Revisit Luca interaction evidence; Train-rate heatmap | Male rates exceed Female within Married (25.85% vs 21.73%) and Single (23.04% vs 19.66%); Male × Other/Unknown is 31.58% on only 114 rows | Is joint structure stable beyond separate variables, and how should it be validated? |
| EDA-08 | LIMIT_BAL Distribution and Target Relationship | Compare continuous raw limit distributions; class density and boxplot | Non-default mean/median 178,477/150,000; default 128,243/90,000; distributions overlap | Continuous, transformed, or combined treatment only after later validation? |
| EDA-09 | PAY Raw Code Frequency by Month | Test whether raw codes are systematic; chronological log-color count heatmap | -2/-1/0 are substantial every month; code 2 occurs 2,102–3,145 times; 7/8 are rare; code 1 has strong month asymmetry | Which raw codes should remain distinct or need justified grouping? |
| EDA-10 | PAY Raw Code Default Rate by Month | Examine uncompressed status/risk ordering; chronological rate heatmap | Code 2 rates 50.67%–69.36%, code 3 57.81%–76.73%, non-positive codes mostly 12.69%–19.74%; very rare high codes have unstable rates | Test raw severity, justified groups, or parallel representations while preserving timing? |
| EDA-11 | PAY Adjacent-Month Transition Patterns | Cover all five adjacent transitions and trajectories; five log-color heatmaps plus tables | 1,023 trajectories; all-zero is 7,853 (32.7208%); constant -2 1,689; constant -1 1,563; same-code 0→0 dominates each pair; large jumps are rare | How could persistence/direction/severity/trajectory be represented without invented meanings? |
| EDA-12 | BILL_AMT Monthly Distribution | Show central range, zeros, negatives, tails; percentile-limited boxplot plus full symlog view | Zeros 1,612–3,223; negatives 468–556; p99 about 274,503–348,402; maxima 855,086–983,931 | What robust treatment could preserve negatives and legitimate extremes? |
| EDA-13 | PAY_AMT Monthly Distribution | Show zeros and extreme tails; percentile-limited boxplot plus log1p display histograms | No negatives; zeros 4,180–5,787; medians 1,500–2,100.5; p99 about 64,578–83,026; maxima 426,529–1,215,471 | Should later design test robust scale/ratio concepts after temporal/business checks? |

The EDA-12 percentile y-limit and symmetric-log axis and EDA-13 percentile y-limit/log1p coordinate are visualization-only. No observation or raw value was dropped, clipped, winsorized, or transformed into a predictor.

## 10. PAY Sanity-check Findings

- The chronological order used is `PAY_6 → PAY_5 → PAY_4 → PAY_3 → PAY_2 → PAY_0`.
- Raw code counts and Train default rates are saved for all six months without binary mapping.
- Codes -2, -1, 0, and 2 are systematic across all months. Rare 7/8 codes and the month-specific code-1 pattern are factual behaviors, not sufficient evidence of dirty data.
- All five adjacent transition matrices are saved. In every pair, 0→0 is the most common transition, with 11,258–11,871 Train rows. Constant -1/-2 and constant 2 transitions are also common.
- Objectively large code jumps (absolute raw-code change at least 5) are saved as a frequency table with no assigned business meaning; the largest listed count is only 20.
- Six-month Train trajectories total 1,023 distinct patterns. The top table preserves 15 raw patterns and their proportions.
- The exact business meanings of -2 and 0 remain unconfirmed. No semantics were invented, no raw value was altered, and no delinquency summary predictor was created.

## 11. Supporting-table Inventory

- Train target counts/proportions.
- SEX, grouped EDUCATION, and grouped MARRIAGE counts/default rates.
- Continuous AGE and LIMIT_BAL class summaries.
- SEX × MARRIAGE count and Train default-rate matrices.
- PAY raw-code count and Train default-rate matrices.
- Top five transitions per adjacent pair; observed large-code-jump table.
- Top 15 raw repayment trajectories and distinct count.
- Full-range monthly BILL_AMT and PAY_AMT tables with min, p01, p25, median, p75, p99, max, zeros, and negatives.

Target-aware tables use Train only.

## 12. Luca Methods Retained, Modified, and Deferred

### Retained / borrowed

- Shape/schema/describe/value-count style inspection.
- Focused categorical and demographic target-rate EDA.
- SEX × MARRIAGE evidence review without creating the interaction.
- Raw repayment status, BILL_AMT, and PAY_AMT inspection.
- No arbitrary deletion of large payment/bill observations.

### Modified

- Luca's full-data target-aware EDA was replaced by a deterministic stratified split followed by Train-only target-aware EDA.
- Luca's PAY binary compression was not copied; raw monthly codes, rates, transitions, and trajectories were retained.
- Luca's feature design before split was not copied; this Stage only records observations and open questions after Test isolation.

### Deferred / not copied

- AgeBin, SE_MA predictor, SE_AG, Avg_exp, Closeness, sampling, modeling, KFold, GridSearch, feature importance, and model evaluation.

## 13. Uncertainties and Human-review Questions

- Official semantics of PAY codes -2/0, EDUCATION 0/5/6, and MARRIAGE 0 remain unconfirmed.
- Small pooled categories and rare high PAY codes yield unstable descriptive rates.
- Distributional differences and joint-group patterns do not by themselves establish stable incremental predictive value or appropriate business treatment.
- Architect + user should review EDA-01 through EDA-13 individually before freezing any feature specification.
- Candidate ideas such as delinquency summaries, utilization, or SE_MA remain discussion prompts only; none was calculated as a predictor or authorized here.

## 14. Explicit Out-of-scope Confirmation

No AgeBin, PAY binary mapping, SE_MA, SE_AG, Avg_exp, Closeness, utilization, delinquency summary, predictor catalog, processed/split dataset, feature selection, WOE, formal binning, sampling, class weighting, model, KFold, GridSearch, model evaluation, tuning, threshold, `src/`, `tests/`, Stage 3.2 artifact, or Stage 3.3 artifact was created or executed. Neither Luca nor Stage 2 Notebook was executed.

## 15. Files Created or Modified

Created:

- `/Users/yu/2.工作/Risk Analysis Project/notebooks/02_train_test_split_and_eda.ipynb`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_3.1_Train_Test_Split_and_EDA.md`

Modified existing project files: none.

No external figures, CSVs, checkpoints, caches, backups, or auxiliary project files were created.

## 16. Notebook Clean-run and Reproducibility Evidence

| Check | Result |
|---|---|
| Python | `/Users/yu/2.工作/Risk Analysis Project/.venv/bin/python`, 3.9.6 |
| Core environment | pandas 2.3.3; numpy 2.0.2; scipy 1.13.1; scikit-learn 1.6.1; matplotlib 3.9.4; seaborn 0.13.2 |
| Execution | Existing `jupyter_client 8.6.3` / `ipykernel 6.31.0`; project root working directory; fresh `python3` kernel |
| Path handling | Locates `data/raw/UCI_Credit_Card.csv` from launch directory or ancestors |
| Final clean run | PASS; 18 code cells executed top-to-bottom with counts 1–18 |
| Saved outputs | 46 output objects, including exactly 13 embedded PNG figures |
| Error output | 0 |
| Placeholder output/text | 0 |
| Formal figure registry | Exactly EDA-01 through EDA-13 in order |
| Notebook SHA-256 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` |
| Notebook file size | 937,648 bytes |

The standard Jupyter launcher remains stale, so the already-installed `jupyter_client`/`ipykernel` path was used without package installation. A harmless `jupyter_client` destructor warning appeared only after explicit kernel shutdown and after the Notebook had saved; it is not a cell error or traceback output in the Notebook.

## 17. Protected-input Integrity

Pre-flight and post-execution values match:

| Protected input | Size | SHA-256 | Conclusion |
|---|---:|---|---|
| Raw CSV | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` | Unchanged |
| Luca reference Notebook | 805,686 | `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd` | Unchanged |
| Stage 2 Notebook | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` | Unchanged |
| Stage 0 record | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` | Unchanged |
| Stage 1 report | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` | Unchanged |
| Stage 2 report | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` | Unchanged |

README and all other existing tracked files are protected by an empty tracked Git diff and unchanged HEAD/index. No Notebook other than the new Stage 3.1 artifact was run or saved.

## 18. Git Status

- Start: `main` at `4d29c19dfff1f2816c4019c52daaf66fee4ed3a3`; clean; empty staged diff; no remote.
- End: expected `main` at the same HEAD; no tracked or staged diff; only the two Stage 3.1 artifacts untracked; no remote.
- No Git add, commit, push, pull, fetch, reset, rebase, amend, stash, restore, checkout, clean, history/index write, or remote change was performed.

Final read-only command evidence is recorded after report creation in the completion handoff and must be rechecked by Reviewer.

## 19. Completion-criteria Check

| Criteria group | Result | Evidence |
|---|---|---|
| Baseline/raw/split settings and integrity | PASS | Sections 3, 5, 6 |
| Test isolation and ID reproducibility | PASS | Sections 6–7; executed assertions and hashes |
| Train-only target-aware EDA | PASS | Sections 7–11; Notebook data flow |
| AGE continuous / PAY raw / no SE_MA predictor | PASS | Executed scope assertions; sections 8, 10, 14 |
| PAY counts/rates/transitions/trajectories | PASS | EDA-09–11 and supporting outputs |
| BILL/PAY_AMT inspection without deletion | PASS | EDA-12–13 and full-range tables |
| 13 numbered, learning-oriented figures | PASS | EDA-01–13; each has four required Markdown fields |
| Luca methods retained/corrected without copying audited risks | PASS | Section 12 |
| Notebook clean-run, no errors | PASS | Section 16 |
| Complete Task Report and immutable Specification | PASS, subject to final verbatim block verification | Sections 1–20 |
| Protected inputs and Stage 0–2 artifacts | PASS | Section 17 and final Git/hash verification |
| No FE/model/imbalance/Git writes/Stage 3.2/3.3 | PASS | Sections 14, 18, 20 |

All 33 Architect completion criteria are represented by these evidence groups and were factually checked before handoff.

## 20. Incomplete Items, Deviations, and Limitations

- Incomplete items: none identified within Stage 3.1 scope.
- Deviations: none identified.
- Limitations: undocumented raw-code semantics remain unresolved; sparse group/rate estimates are explicitly marked; Stage 3.1 does not test stability or predictive value because that would cross the stage boundary.
- Unexpected artifacts or modifications: none observed.

## 21. Post-EDA Human Checkpoint

The Post-EDA Human Checkpoint has been reached. Codex stops here. No Feature Engineering decision is frozen, and no Stage 3.2 or Stage 3.3 work has begun. Next steps are independent Reviewer assessment, Reviewer commit only if PASS, and then Architect + user figure-by-figure review.

## 22. Reviewer Result

`Pending independent Reviewer assessment.`

Reviewer notes:

<!-- Reviewer to append PASS/FAIL, commit evidence if PASS, and post-commit verification here. -->

# REVIEWER RESULT

## Verdict

PASS

## Requirement Check

- Stage 3.1 scope is complete: the deliverables are limited to the required Train/Test split and Train-only EDA Notebook plus this Task Report.
- The split is deterministic and reproducible: 80/20, `random_state=42`, stratified by the target, with 24,000 Train rows and 6,000 sealed Holdout Test rows. Train/Test IDs are disjoint, jointly cover all source IDs, and reproduce the recorded stable hashes.
- Holdout isolation is satisfied. The Notebook uses the Holdout Test only for structural, ID, coverage, and deterministic split assertions, then deletes it before all target-aware analysis. No Test target distribution or target relationship is inspected.
- All target-aware EDA is derived only from Train. The authorized display-only mappings for EDUCATION and MARRIAGE are used; AGE remains continuous; PAY values remain raw; and no SEX × MARRIAGE predictor is created.
- The complete finite EDA set EDA-01 through EDA-13 is present. Every figure has the required Question, Why it matters, Observed finding, and Stage 3.2 question fields. PAY counts, Train default rates, all five adjacent transitions, and full six-month trajectories are covered without binary recoding.
- BILL_AMT and PAY_AMT analyses preserve raw data and use transformations or axis limits only for display. No feature engineering, modeling, sampling, CV pipeline, processed-data export, Stage 3.2/3.3 work, `src/`, or `tests/` work is present.
- The Notebook has a clean saved run with execution counts 1–18, 46 outputs, 13 embedded figures, and no error outputs. Protected inputs and committed Stage 0–2 artifacts remain unchanged.
- Before review closeout, Git remained on `main` at the expected Stage 2 HEAD, with no staged changes, no remote, and only the two reviewed Stage 3.1 deliverables untracked.

## Independent Verification

- Read the complete embedded Architect Specification and Codex execution/verification record in this Task Report.
- Inspected every Notebook cell, its data flow, assertions, saved outputs, metadata, and all 13 embedded figures.
- Independently reran all 18 code cells from a clean kernel without saving over the reviewed Notebook; all cells completed successfully and reproduced 46 outputs and 13 PNG figures with no cell errors.
- Independently recalculated the split and stable ID hashes from the protected raw CSV. Reproduced Train hash `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857` and Test hash `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`.
- Independently reproduced key EDA evidence, including Train target counts 18,691/5,309, the raw PAY code sets, 1,023 distinct six-month trajectories, and the reported leading trajectory counts.
- Verified branch, HEAD, tracked/staged/untracked state, file scope, remotes, protected-file hashes, and absence of out-of-scope project directories or artifacts.

## Issues Found

None affecting acceptance.

## Required Fixes

None.

## Architect Decision Needed

None.
