# Stage 4.1 — WOE / IV / Binning Method Design

## 1. Task Identification

- Project: Credit Card Default Risk
- Stage: `Stage 4.1 — WOE / IV / Binning Method Design`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Execution role: Codex local project executor
- Status: execution complete; awaiting Architect/user Human Learning Checkpoint
- Authorized scope: methodology documentation and transparent synthetic toy validation only
- Holdout Test: sealed and unused
- Next stage: Stage 4.2 is not authorized and was not started

## 2. Stage Objective and Boundary

Stage 4.1 freezes how Good/Bad, WOE, IV, supervised binning, ordinary/special bins, zero-event handling, PAY/ordinal/categorical grouping, and future fold-specific WOE estimation will work.

It does not answer where any real predictor should be cut. No project CSV, Train frame, Holdout Test frame, real predictor, or real target was loaded into Stage 4.1 calculation code. No real WOE/IV table, cut point, ranking, variable recommendation, model, or validation metric was produced.

## 3. Immutable Architect Specification

================ BEGIN ARCHITECT SPECIFICATION ================

# Stage 4.1 — WOE / IV / Binning Method Design

## 1. Stage Context

项目固定宏观结构：

- Stage 0 — Current-State Audit
- Stage 1 — Project Foundation
- Stage 2 — Luca Notebook Audit & Data Understanding
- Stage 3 — Data Preparation, EDA & Feature Engineering
- Stage 4 — WOE / Logistic Regression, Interpretable Credit Risk Model
- Stage 5 — Challenger Model + Model Comparison + Final Evaluation
- Stage 6 — GitHub + Final Documentation + Resume/Interview Packaging

Stage 3 已完成并通过最终 Reviewer。

Stage 4 拆分为：

- Stage 4.1 — WOE / IV / Binning Method Design
- Stage 4.2 — PAY + Categorical Binning
- Stage 4.3 — Continuous Variable Supervised Binning
- Stage 4.4 — WOE / IV + Variable Screening
- Stage 4.5 — Candidate WOE Logistic Regression
- Stage 4.6 — Stratified 5-Fold Model Stability Test
- Stage 4.7 — Final Logistic Freeze + Full-Train Refit

当前仅授权：

> **Stage 4.1 — WOE / IV / Binning Method Design**

本阶段只冻结和验证“以后如何做”。

本阶段不回答：

> “某个真实 predictor 最终应该切在哪里？”

真实变量的 target-aware binning 从 Stage 4.2 / 4.3 才开始。

---

# 2. Stage 4.1 Objective

Stage 4.1 只解决以下方法论问题：

1. 固定 Good / Bad / Event 定义；
2. 固定 WOE 公式和符号方向；
3. 固定 IV 公式和解释方法；
4. 固定 IV 在 variable screening 中的角色；
5. 固定 continuous supervised binning workflow；
6. 固定 ordinary bin size 原则；
7. 固定 special-bin 处理原则；
8. 固定 monotonicity 原则；
9. 固定 zero-good / zero-bad bin 的处理；
10. 固定 PAY / ordinal discrete variable 的 binning 方法；
11. 固定 categorical variable 的处理原则；
12. 记录 Stage 4.6 后续 KFold 与 WOE 的正确关系。

本阶段必须通过简单、透明、可手算的 toy examples 证明这些方法定义实现正确。

不得提前进入真实变量分析。

---

# 3. Git Pre-flight

Project root：

`/Users/yu/2.工作/Risk Analysis Project`

Codex 开始任何修改前必须实际检查：

1. repository root 正确；
2. branch = `main`；
3. working tree clean；
4. staged diff empty；
5. no remote；
6. 当前 HEAD 是 Stage 3.3 最终 Reviewer PASS 后创建的 local milestone commit；
7. 该 HEAD 的 parent 应为 Stage 3.1 milestone：
   `a8ca9707b93e27f4f2f310349640fd7efb0e098d`
8. 该 Stage 3.3 milestone 的改动范围应只对应：
   - `notebooks/03_feature_engineering_and_sanity_checks.ipynb`
   - `task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`
9. Stage 3.3 Task Report 明确包含最终 Reviewer `PASS`；
10. Stage 0–3 protected artifacts 均存在。

Architect 当前未提供 Stage 3.3 最终 commit hash。

因此：

> Codex 必须从本地 Git 实际读取并报告真实 HEAD，不得猜测 commit hash。

如 Stage 3.3 Reviewer PASS 后没有形成 expected milestone commit、working tree 不 clean、存在额外 drift 或 Git baseline 存在冲突：

> 返回 BLOCKED。

不得：

- reset；
- restore；
- stash；
- checkout；
- clean；
- amend；
- rebase；

来自动修复。

---

# 4. Protected Inputs

不得修改：

- `data/raw/UCI_Credit_Card.csv`
- Luca reference Notebook
- Stage 0–3 Notebooks
- Stage 0–3 Task Reports
- README
- requirements
- unrelated project files

本阶段可以读取 Stage 3.3 report 以确认：

- Train = 24,000
- Holdout Test = 6,000
- final predictor-ready schema = 29 predictors

但不得重新设计 Stage 3 feature engineering。

---

# 5. Holdout Test Protection

Stage 4 总原则：

> 6,000-row Holdout Test 继续保持 sealed，直到 Stage 5 final comparison。

Stage 4.1 不得：

- 查看 Test target distribution；
- 查看 Test predictor-target relationship；
- 计算 Test WOE；
- 计算 Test IV；
- 对 Test 做 supervised binning；
- 用 Test 设计任何 method rule。

如果为了验证 split identity 而重新 replay split：

只能做：

- row count；
- ID overlap；
- known ID-set hash；

完成 integrity check 后必须再次 seal Test。

但 Stage 4.1 如果不需要 replay split，也无需无意义地加载 Test。

---

# 6. Good / Bad Definition — Frozen

项目 target：

`default.payment.next.month`

固定定义：

- `0 = Good = Non-default = Non-event`
- `1 = Bad = Default = Event`

所有后续 WOE / IV 计算均遵守这个定义。

不得反转。

---

# 7. WOE Definition — Frozen

对于变量的第 (j) 个 bin：

\[
DistGood_j=\frac{Good_j}{TotalGood}
\]

\[
DistBad_j=\frac{Bad_j}{TotalBad}
\]

项目正式 WOE 定义：

\[
WOE_j=
\ln
\left(
\frac{DistGood_j}{DistBad_j}
\right)
\]

符号解释固定为：

- `WOE > 0` → Good representation 相对更高 → 相对低风险；
- `WOE < 0` → Bad representation 相对更高 → 相对高风险；
- `WOE ≈ 0` → Good / Bad representation 接近 → 该 bin 区分能力弱。

整个项目不得在后续 Stage 中更换成相反方向，避免 coefficient interpretation 混乱。

---

# 8. Default Rate vs WOE

Notebook 必须明确区分：

\[
DefaultRate_j=
\frac{Bad_j}{Good_j+Bad_j}
\]

和 WOE。

解释：

- Default Rate 回答：
  “这个 bin 内有多少比例客户违约？”
- WOE 回答：
  “这个 bin 在总体 Good 与 Bad population 中的相对 representation 有何差异？”

Stage 4.2 / 4.3 的正式 bin tables 应同时展示：

- Count
- Good Count
- Bad Count
- Default Rate
- Dist Good
- Dist Bad
- WOE
- IV Contribution

Stage 4.1 仅用 toy examples 展示该格式。

---

# 9. IV Definition — Frozen

每个 bin 的 IV contribution：

\[
IV_j=
(DistGood_j-DistBad_j)
\times WOE_j
\]

变量整体 IV：

\[
IV=\sum_j IV_j
\]

Notebook 必须通过 toy example 验证：

- bin-level contribution；
- total IV；
- 手工计算与 helper calculation 一致。

---

# 10. IV Interpretation — Frozen

可以记录行业/教材中常见的粗略参考范围：

| IVDescriptive interpretation |                                    |
| ---------------------------- | ---------------------------------- |
| < 0.02                       | Very weak                          |
| 0.02–0.10                    | Weak                               |
| 0.10–0.30                    | Moderate                           |
| 0.30–0.50                    | Strong                             |
| > 0.50                       | Very strong; investigate carefully |

但必须明确：

> **这些区间仅作为 descriptive reference。**

本项目不得使用自动 hard rule，例如：

`IV < 0.02 → automatically drop`

最终 variable selection 在 Stage 4.4 综合：

- IV；
- risk pattern；
- business interpretability；
- redundancy；
- multicollinearity；
- later Logistic stability。

IV 是：

> important screening evidence

不是：

> automatic variable-selection judge。

---

# 11. Continuous Supervised Binning — Frozen Workflow

普通连续变量正式采用：

> **Quantile Pre-binning → Target-aware Adjacent Merging**

而不是：

- fixed-width blind binning；
- fully automatic opaque optimal binning；
- arbitrary manual cut points；
- decision-tree black-box cut-point search。

目标是：

> 保持方法透明、可解释、可人工审核。

---

# 12. Pre-binning Rule

对于普通连续变量：

> 最多约 20 个 quantile-based candidate pre-bins。

目标：

- 每个 pre-bin 样本量相对均衡；
- 为后续 supervised merging 提供较细候选结构；
- 不把 pre-bin 当作 final bin。

若由于变量只有较少 unique values，无法合理形成 20 bins：

> 应尊重实际 unique structure，不强行生成 20 bins。

不得人为 jitter 数值。

---

# 13. Final Bin Count Principle

普通连续 predictor：

> final bins 通常控制在 3–6 个。

AGE：

> 倾向 3–5 个 final bins。

这不是绝对数学硬限制。

如果真实数据支持偏离：

必须在 Stage 4.3：

- 明确报告；
- 给出 sample-size / business / risk-pattern reason；
- 由 Architect/user Human Checkpoint 决定。

Codex 不得自行把变量扩展成大量 final bins。

---

# 14. Ordinary Final-bin Sample-size Principle

对 24,000 Train：

普通 final bin 的设计原则：

### Preferred

\[
\ge 5\%
\]

即约：

\[
\ge 1200
\]

rows。

### Acceptable with justification

\[
3\%-5\%
\]

即约：

\[
720-1200
\]

rows。

### Normally too small

\[
<3\%
\]

原则上应与合理相邻 bin 合并。

但是：

> 明确的 special/business bin 可以例外。

因此这不是机械 `min_bin_size=5%` 的硬算法。

Stage 4.2 / 4.3 应把小 bin 显式标记供人工判断。

---

# 15. Adjacent Merging Criteria

Stage 4.2 / 4.3 对 bins 的合并主要考虑：

1. adjacent default rates 是否非常接近；
2. adjacent WOE 是否非常接近；
3. 某 bin 是否过小；
4. 合并后 risk structure 是否更稳定；
5. 合并是否保持 business interpretation；
6. 是否减少明显随机波动。

不得：

- 只为了提高 IV 无限试切点；
- 只为了制造漂亮图反复合并；
- 根据 Holdout Test 修改 bins。

---

# 16. Monotonicity Principle — Frozen

正式规则：

> **Monotonicity preferred, not mandatory.**

意思是：

- 如果变量具有合理总体风险方向，应尽量形成可解释的风险趋势；
- 允许现实数据存在小幅局部反弹；
- 不强制所有 predictor WOE / Default Rate 完美单调；
- 不得为了完美 monotonicity 进行过度合并。

最终优先级是：

> stability + sample adequacy + interpretability + reasonable risk structure

而不是：

> perfect monotonic curve。

---

# 17. Candidate Special Bins — BILL_AMT

对：

- `BILL_AMT1`
- `BILL_AMT2`
- `BILL_AMT3`
- `BILL_AMT4`
- `BILL_AMT5`
- `BILL_AMT6`

Stage 4.3 初始应分别考虑：

- `BILL_AMT < 0`
- `BILL_AMT = 0`

作为：

> candidate special bins。

`BILL_AMT > 0` 部分再进行 quantile pre-binning。

注意：

> candidate special bin 不等于永久禁止合并。

如果 Stage 4.3 发现：

- 样本极少；
- risk behavior 接近；
- WOE 接近；

仍可以提出合并建议，但必须由 Human Checkpoint 决定。

---

# 18. Candidate Special Bin — PAY_AMT

对：

`PAY_AMT1`–`PAY_AMT6`

已知：

- 无 negative；
- 有大量 zero；
- positive distribution 右偏。

因此：

> `PAY_AMT = 0`

作为 candidate special bin。

positive values 再 quantile pre-bin + supervised adjacent merge。

---

# 19. Candidate Business Anchors — Utilization

对：

- `current_utilization`
- `avg_utilization`
- `max_utilization`

Stage 4.3 应重点保持可见：

- `utilization = 0`
- `utilization > 1`

作为 candidate business anchors。

中间：

\[
0<utilization\le1
\]

再进行 supervised binning。

不得在 Stage 4.1 提前固定：

- 25%
- 50%
- 75%
- 80%
- 90%

等 cut points。

实际内部 cut points 必须由 Stage 4.3 Train evidence 决定。

---

# 20. PAY Binning Method — Frozen

以下六个：

- `PAY_0`
- `PAY_2`
- `PAY_3`
- `PAY_4`
- `PAY_5`
- `PAY_6`

属于：

> ordered discrete credit-behavior status

不使用 quantile binning。

Stage 4.2 必须：

1. 每个月 PAY 单独分析；
2. 每个 raw code 先分别计算：
   - count
   - Good
   - Bad
   - default rate
   - WOE
   - IV contribution
3. 再根据 risk similarity / sample size 进行 adjacent-state merging。

只允许具有 ordinal adjacency / coherent status range 的 grouping。

不得产生无逻辑组合，例如：

- `-2` 与 `3` 合并，但排除 `-1/0/1/2`；
- `1` 与 `5` 单独组成一个 group；
- 其他破坏 ordinal structure 的 grouping。

---

# 21. PAY -2 / -1 / 0 — Explicit Rule

Stage 4.1 不提前规定：

> `-2/-1/0` 一定合并。

Stage 4.2 才根据 24,000 Train 的：

- count；
- default rate；
- WOE；
- stability；
- sample support；

判断是否足够相似。

即使最终合并：

不得宣称：

> `-2/-1/0` 在真实业务语义上完全相同。

正确解释应是：

> 它们在当前 Train 的 risk-modeling behavior 上足够接近，因此被归入同一 modeling bin。

---

# 22. PAY=1 and High PAY Codes

由于 Stage 3 已发现 `PAY=1` 在不同月份出现频率高度不对称：

> 六个月 PAY 不要求采用相同 grouping。

每个月必须独立评估。

对于高 severity：

- 3
- 4
- 5
- 6
- 7
- 8

如果样本稀少且 risk behavior 接近，可以考虑形成类似：

`3+`

的 merged group。

但具体 grouping 留到 Stage 4.2。

---

# 23. Delinquency Summary Binning — Frozen

以下变量：

- `delinquent_months`
- `max_delinquency`
- `severe_delinquent_months`

属于低-cardinality ordinal predictors。

不使用 quantile pre-binning。

Stage 4.2：

1. 逐 raw integer value 计算：
   - count
   - default rate
   - WOE
   - IV contribution
2. 只允许 adjacent-value merging。

例如：

`3+`

可能成立。

但是不允许无序组合，例如：

`0 + 4`

或者：

`1 + 6`

作为一个 bin。

---

# 24. Existing Categorical Predictors

以下变量初始保持现有类别：

### SEX

保留现有 raw categories。

### EDUCATION_GROUP

- Graduate School
- University
- High School
- Other / Unknown

### MARRIAGE_GROUP

- Married
- Single
- Other / Unknown

Stage 4.2：

- 先分别计算 WOE / IV；
- 不主动合并。

仅当出现：

- sample extremely sparse；
- zero Good/Bad；
- extreme unstable WOE；
- clearly redundant risk behavior；

才提出 merge candidate。

任何合并仍需 Human Checkpoint。

---

# 25. Zero-Good / Zero-Bad Bin Rule

如果某 bin：

- `Good = 0`
  或
- `Bad = 0`

则原始 WOE 会趋向无穷。

正式处理顺序：

### First preference

优先判断 bin 是否：

- 太小；
- 可以和相邻 / 合理类别合并。

如可合理合并：

> 优先 merge。

### Second preference

只有当：

- bin 有明确保留理由；
- 无法合理合并；

才允许：

> **0.5 additive smoothing**

即对 Good / Bad count calculation 加：

`0.5`

以避免 infinite WOE。

任何 smoothing 必须：

- 显式记录；
- 标记变量与 bin；
- 不得静默处理。

---

# 26. Stage 4.6 Future KFold Policy — Record Now, Do Not Execute

Stage 4.1 需要正式记录后续已经确认的 CV 设计，但不得实际运行 KFold。

Stage 4.2–4.5：

> 使用完整 24,000 Train 完成 bin design、WOE/IV analysis、variable screening 与 model specification freeze。

Stage 4.6：

> 使用 Stratified 5-Fold 检查 model stability。

KFold 中：

### Fixed across all folds

不重新做：

- feature engineering；
- variable selection；
- bin-boundary selection；
- PAY grouping selection。

这些已经冻结。

### Re-estimated inside each fold

每个 fold 使用约 19,200 fold-training rows：

1. 按固定 bins 重新计算 WOE values；
2. fit Logistic coefficients；
3. 将该 training-fold WOE mapping 应用于约 4,800 validation rows；
4. 预测；
5. 计算 validation AUC / KS；
6. 保存 coefficient / WOE stability evidence。

因此 Stage 4.6 的 KFold 主要用于：

> model stability evidence

而不是把 Stage 4 重做五遍。

---

# 27. KFold Methodological Limitation — Must Be Documented

由于：

> final bin boundaries 和 variable selection 在完整 24,000 Train 上设计，

后续 KFold validation folds 的 target 曾间接参与 model specification design。

因此 Stage 4.6 CV metrics：

不得描述为：

> perfectly unbiased final out-of-sample performance estimate。

应描述为：

> internal stability / robustness evidence under a fixed model specification。

真正 untouched final generalization evaluation：

> 留给 Stage 5 的 6,000 Holdout Test。

---

# 28. Stage 4.1 Toy WOE / IV Demonstration

Notebook 必须创建一个小型、完全透明的 toy example。

不得使用真实 predictor 的 target relationship。

Toy example 应包含：

- 至少 3–4 个 bins；
- 每个 bin 有 Good / Bad counts；
- 至少一个 WOE > 0；
- 至少一个 WOE < 0；
- 至少一个 WOE near 0；
- 手工公式；
- programmatic result；
- assertion 两者一致。

输出表至少包含：

- Bin
- Count
- Good
- Bad
- Default Rate
- Dist Good
- Dist Bad
- WOE
- IV Contribution

并输出 total IV。

---

# 29. Stage 4.1 Toy Adjacent-Merging Demonstration

再创建一个简短 toy continuous example，展示：

> pre-bins → adjacent risk comparison → conceptual merging

例如：

- 若两个 adjacent bins default rate / WOE 非常接近；
- 合并后 sample 更稳定；

展示为什么合并合理。

这只是教学 demonstration。

不得：

- 实现真实变量最终 cut points；
- 声称 toy thresholds 对 UCI 数据有效；
- 根据 toy example 新增方法规则。

---

# 30. Toy Zero-event Demonstration

可以额外用小型 toy counts 展示：

- `Bad=0` 导致 raw WOE infinite；
- why merging is preferred；
- 0.5 smoothing 如何避免 infinity。

但必须明确：

> smoothing 是 fallback，不是默认每个 bin 都加 0.5。

---

# 31. Stage 4.1 Real-data Boundary

Stage 4.1 不得输出真实 29 predictors 的：

- default-rate tables；
- WOE；
- IV；
- pre-bins；
- candidate cut points；
- final cut points；
- rankings；
- variable recommendations。

尤其不得提前分析：

- PAY codes vs default；
- utilization vs default；
- AGE vs default；
- LIMIT_BAL vs default；
- BILL/PAY_AMT vs default。

这些分别属于 Stage 4.2 / 4.3 / 4.4。

---

# 32. No Variable Selection

本 Stage 不允许：

- IV ranking of actual predictors；
- IV threshold filtering；
- correlation screening；
- VIF；
- multicollinearity removal；
- feature elimination；
- candidate Logistic feature selection。

全部留到 Stage 4.4 / 4.5。

---

# 33. No Modeling

Stage 4.1 严禁：

- Logistic Regression fitting；
- Decision Tree；
- Random Forest；
- Gradient Boosting；
- XGBoost；
- LightGBM；
- KFold execution；
- cross-validation；
- AUC；
- KS；
- calibration；
- confusion matrix；
- coefficient estimation；
- threshold selection；
- class weighting；
- sampling。

本 Stage 是 Method Design，不是 Model Development。

---

# 34. Notebook Artifact

创建：

`notebooks/04_woe_iv_binning_method_design.ipynb`

建议结构：

1. Stage 4.1 objective and boundaries
2. Good / Bad convention
3. WOE formula
4. Toy WOE worked example
5. WOE sign interpretation
6. Default Rate vs WOE
7. IV formula
8. Toy IV worked example
9. IV interpretation and non-hard-threshold policy
10. Supervised binning concept
11. Quantile pre-binning principle
12. Adjacent-merging toy demonstration
13. Sample-size rules
14. Special-bin policy
15. Monotonicity policy
16. Zero-event / smoothing demonstration
17. PAY / ordinal-discrete policy
18. Categorical policy
19. Future 5-Fold WOE policy
20. Stage 4.1 frozen methodology summary
21. explicit Human Checkpoint

Notebook 必须：

- clean run；
- sequential execution counts；
- saved outputs；
- no error outputs；
- no actual predictor WOE/IV；
- no real supervised binning；
- no model fitting。

---

# 35. Task Report Artifact

创建：

`task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md`

必须记录：

- Stage objective；
- Git pre-flight；
- prior-stage baseline；
- Good / Bad convention；
- exact WOE definition；
- WOE sign interpretation；
- Default Rate vs WOE distinction；
- exact IV definition；
- IV reference ranges；
- explicit non-hard-threshold policy；
- continuous pre-binning method；
- final-bin count guidance；
- bin-size guidance；
- adjacent-merging criteria；
- monotonicity policy；
- special-bin policy；
- PAY methodology；
- delinquency-summary methodology；
- categorical methodology；
- zero-event handling；
- 0.5 smoothing fallback；
- future KFold policy；
- KFold limitation；
- toy validation results；
- explicit confirmation no real predictor binning performed；
- explicit confirmation no variable selection；
- explicit confirmation no modeling；
- explicit confirmation Holdout Test remained sealed；
- files created/modified；
- protected-input integrity；
- Git status；
- Human Checkpoint status。

---

# 36. Immutable Architect Specification

Task Report 必须保存完整本 Architect Specification。

使用：

`================ BEGIN ARCHITECT SPECIFICATION ================`

与：

`================ END ARCHITECT SPECIFICATION ================`

中间逐字保存。

不得 paraphrase。

---

# 37. Human Learning Checkpoint

Codex 完成后：

> 必须停止。

不要直接进入 Stage 4.2。

用户会把：

- Stage 4.1 Notebook；
- Stage 4.1 Task Report；

交给 Architect。

Architect + 用户将共同确认用户能够理解：

1. WOE formula；
2. WOE 正负方向；
3. Default Rate 与 WOE 的区别；
4. IV formula；
5. IV 为什么不能机械筛变量；
6. supervised binning 为什么是 target-aware；
7. pre-bin 与 final bin 的区别；
8. 为什么只合 adjacent bins；
9. special bin 的意义；
10. 为什么 monotonicity 不强制；
11. smoothing 为什么只是 fallback；
12. 后续 KFold 为什么固定 bins 但重新估 WOE。

只有用户明确确认后：

> 才进入 Stage 4.2。

---

# 38. Reviewer / Commit Workflow

Codex：

- 不 git add；
- 不 commit；
- 不 push。

Human Checkpoint 完成并获得 Architect/user 接受后：

> 将 Stage 4.1 artifacts 交给 Reviewer 独立检查。

Reviewer：

- FAIL → 不 commit，返回 fixes；
- PASS → 在同一 review round 创建 Stage 4.1 local milestone commit，并完成 post-commit verification。

无单独 commit-closeout round。

不得 push GitHub，除非用户另行明确授权。

---

# 39. Project Protection

本 Stage 原则上只新增：

- `notebooks/04_woe_iv_binning_method_design.ipynb`
- `task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md`

不得创建：

- processed datasets；
- binning CSV；
- WOE mapping CSV；
- feature-ranking file；
- model artifact；
- scorecard；
- `src/`
- `tests/`
- Stage 4.2 artifacts；
- external figures。

如 Notebook 内需要 helper functions：

> 仅保留在 Notebook 内部。

不要建立 production code framework。

---

# 40. Completion Criteria

全部满足才算 Codex execution complete：

1. Git baseline valid；
2. Stage 3.3 PASS milestone verified；
3. Holdout Test unused for methodology；
4. Good/Bad convention documented；
5. WOE formula correct；
6. WOE sign direction correct；
7. toy WOE calculation verified；
8. Default Rate distinction explained；
9. IV formula correct；
10. toy IV calculation verified；
11. IV reference ranges documented；
12. no automatic IV drop rule；
13. continuous pre-binning policy documented；
14. max ~20 pre-bin principle documented；
15. 3–6 final-bin guidance documented；
16. AGE 3–5 guidance documented；
17. 5% / 3–5% / <3% size guidance documented；
18. special-bin exception documented；
19. adjacent-merging criteria documented；
20. monotonicity preferred-not-mandatory documented；
21. BILL special-bin policy documented；
22. PAY_AMT zero policy documented；
23. utilization anchor policy documented；
24. PAY state-by-state methodology documented；
25. PAY monthly grouping allowed to differ；
26. delinquency adjacent-value methodology documented；
27. categorical policy documented；
28. zero-event merge-first rule documented；
29. 0.5 smoothing fallback documented；
30. future KFold fixed-bin policy documented；
31. fold-specific WOE re-estimation policy documented；
32. CV methodological limitation documented；
33. toy adjacent-merging demonstration valid；
34. no actual predictor WOE/IV；
35. no actual predictor binning；
36. no variable selection；
37. no Logistic；
38. no KFold execution；
39. no Test analysis；
40. Notebook clean-runs；
41. Task Report complete；
42. immutable spec preserved；
43. protected inputs unchanged；
44. only two authorized artifacts created；
45. no Git staging；
46. no commit；
47. no push；
48. stopped at Human Checkpoint。

---

# 41. Codex Completion Report

最终返回简明 Completion Report，包括：

- pre-flight result；
- branch / HEAD；
- confirmed Stage 3.3 milestone baseline；
- files created；
- WOE convention implemented；
- toy WOE validation result；
- IV convention implemented；
- toy IV validation result；
- binning methodology recorded；
- special-bin rules recorded；
- PAY methodology recorded；
- smoothing rule recorded；
- future KFold policy recorded；
- confirmation no actual predictor WOE/IV；
- confirmation no actual predictor binning；
- confirmation no variable selection；
- confirmation no modeling；
- confirmation no KFold；
- confirmation Holdout Test remained sealed；
- Notebook clean-run status；
- protected-input integrity；
- Git status；
- explicit confirmation:
  - no commit
  - no push

Final line:

`Stage 4.1 execution complete — awaiting Architect/user Human Learning Checkpoint before Stage 4.2.`

================ END ARCHITECT SPECIFICATION ================

## 4. Git Pre-flight

All mandatory checks passed before the first project write.

| Check | Actual result |
|---|---|
| Repository root | `/Users/yu/2.工作/Risk Analysis Project` |
| Branch | `main` |
| Actual HEAD | `8fbe2fa4b3715719e652101f694e5673abebba03` |
| HEAD parent | `a8ca9707b93e27f4f2f310349640fd7efb0e098d` |
| HEAD message | `Implement frozen feature engineering` |
| Working tree | Clean |
| Tracked unstaged diff | Empty |
| Staged diff | Empty |
| Untracked files | None |
| Remote | None |
| Stage 4.1 output paths before work | Both absent |

Current HEAD relative to its parent contained exactly:

- `A notebooks/03_feature_engineering_and_sanity_checks.ipynb`
- `A task_records/Stage_3.3_Feature_Engineering_and_Sanity_Check.md`

Commit scope: 2 files, 4,451 insertions, no unrelated path.

No reset, restore, stash, checkout, clean, amend, rebase, index write, history write, or remote operation was performed.

## 5. Prior-stage Baseline Verification

The local Stage 3.3 milestone satisfies the required baseline:

- its parent is the Stage 3.1 milestone `a8ca9707b93e27f4f2f310349640fd7efb0e098d`;
- its change scope contains only the two Stage 3.3 artifacts;
- the Stage 3.3 Task Report contains the final Reviewer verdict `PASS`;
- the final Reviewer verdict covers both Phase A and Phase B;
- the report confirms Train = 24,000, Holdout Test = 6,000, and identical Train/Test predictor-ready schemas of 29 predictors;
- all Stage 0–3 protected artifacts existed before execution.

Stage 3.3 final artifact identities:

| Artifact | Size (bytes) | SHA-256 |
|---|---:|---|
| Stage 3.3 Notebook | 142,371 | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` |
| Stage 3.3 Task Report | 77,812 | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` |

## 6. Protected-input Evidence

The following pre-execution identities were recorded. They are rechecked in final validation.

| Protected input | Size (bytes) | SHA-256 |
|---|---:|---|
| `.gitignore` | 534 | `300cd656670defcdee1bf9c7308154a861d5d4a9f09731c856e133b8e0e9bbd1` |
| Root README | 2,245 | `c537d233c9ec94f3f03a19d8be012c7296231e3bfabbea996c1db790e4607e34` |
| data README | 1,004 | `c8585d32e2748997abeac512d747c9d89dae831f8aa1ecc1c262f74a698d0ca0` |
| Raw CSV | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` |
| Legacy baseline placeholder Notebook | 1,057 | `ab806639f1d0f4e9b313eb2e89c456de7e16c081fd80ac81a58c066359b6ff20` |
| Stage 2 Notebook | 78,438 | `1033fdc890cbfae1f180e2f712842fe53b698b139a4ce79cd02743d236082d9c` |
| Stage 3.1 Notebook | 937,648 | `b60d868e161b4c576414f8817dcb7e131299a0635ff34857a6aaea023652678d` |
| Stage 3.3 Notebook | 142,371 | `68bac5ee0267da52966b07d7ab2486c4a4293a8d7784dc3078d9961460d3e256` |
| references README | 1,109 | `a6b3e00a0ed30bee18bc719a75319a6cff4992be9b3c9c0e7011ef0136cd6a3c` |
| Luca reference Notebook | 805,686 | `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd` |
| reports/figures/.gitkeep | 60 | `73b6245fb36c58396d3aead27ec614cbe753fb31de7b7065bf8322692a0ccf63` |
| requirements | 95 | `5109f3041548282e9f659af9159ed84f1757a9c6bbf81cafc5153c4850294f6b` |
| Stage 0 Task Report | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` |
| Stage 1 Task Report | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` |
| Stage 2 Task Report | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` |
| Stage 3.1 Task Report | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` |
| Stage 3.3 Task Report | 77,812 | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` |

Pre-execution Git index SHA-256: `61949dd93056087daca434a1f4be38fa14fa439718601135f264f0486dbf2b9a`.

## 7. Work Performed

Created one formal, synthetic-only learning Notebook and clean-ran it from a fresh local kernel. The Notebook:

- documents every frozen method rule;
- manually calculates WOE and IV from transparent toy counts;
- independently calculates the same table with a small in-Notebook helper;
- asserts bin-level and total-IV equality;
- distinguishes Default Rate from WOE;
- demonstrates quantile pre-bins and one conceptual adjacent merge with synthetic continuous values;
- demonstrates raw infinite WOE, preferred adjacent merging, and explicit 0.5 fallback smoothing;
- records future Stage 4.6 methodology and its limitation;
- ends at the mandatory Human Learning Checkpoint.

No helper module, production framework, dataset, mapping file, ranking file, model artifact, source directory, test directory, figure, or Stage 4.2 artifact was created.

## 8. Good / Bad / Event Convention

Frozen project convention:

- `0 = Good = Non-default = Non-event`
- `1 = Bad = Default = Event`

This direction was documented and asserted in the method summary. It was not reversed.

## 9. WOE Definition and Sign Interpretation

For bin (j):

[
DistGood_j = Good_j / TotalGood
]

[
DistBad_j = Bad_j / TotalBad
]

[
WOE_j = \ln(DistGood_j / DistBad_j)
]

Frozen interpretation:

- WOE > 0: relatively stronger Good representation and relatively lower risk;
- WOE < 0: relatively stronger Bad representation and relatively higher risk;
- WOE near 0: similar Good/Bad representation and weak separation.

No opposite sign definition was introduced.

## 10. Main Synthetic WOE / IV Validation

The main transparent example used only these invented counts:

| Bin | Count | Good | Bad |
|---|---:|---:|---:|
| Toy_A | 50 | 40 | 10 |
| Toy_B | 60 | 30 | 30 |
| Toy_C | 45 | 20 | 25 |
| Toy_D | 45 | 10 | 35 |
| Total | 200 | 100 | 100 |

Manual result:

| Bin | Default Rate | Dist Good | Dist Bad | WOE | IV Contribution |
|---|---:|---:|---:|---:|---:|
| Toy_A | 0.200000 | 0.400000 | 0.100000 | 1.386294 | 0.415888 |
| Toy_B | 0.500000 | 0.300000 | 0.300000 | 0.000000 | 0.000000 |
| Toy_C | 0.555556 | 0.200000 | 0.250000 | -0.223144 | 0.011157 |
| Toy_D | 0.777778 | 0.100000 | 0.350000 | -1.252763 | 0.313191 |

The example includes positive, negative, and exactly zero WOE without smoothing.

Validation results:

- manual total IV = `0.740236228026`;
- helper total IV = `0.740236228026`;
- all bin-level numeric fields agree at `rtol=1e-12, atol=1e-12`;
- total IV agrees at the same tolerance;
- Count = Good + Bad for every bin;
- Dist Good sums to 1 within tolerance;
- Dist Bad sums to 1 within tolerance.

This large synthetic IV is not evidence about a real variable and must not be interpreted as project predictive strength.

## 11. Default Rate versus WOE

Frozen distinction:

[
DefaultRate_j = Bad_j / (Good_j + Bad_j)
]

Default Rate is an inside-bin event proportion. WOE compares that bin's share of the entire Good population with its share of the entire Bad population.

The synthetic Toy_B example makes the distinction explicit:

- Default Rate = 0.50;
- Dist Good = 0.30;
- Dist Bad = 0.30;
- WOE = 0.

Thus Default Rate and WOE may relate directionally but are not the same value.

Future formal bin tables must show Count, Good, Bad, Default Rate, Dist Good, Dist Bad, WOE, and IV Contribution together.

## 12. IV Definition, Interpretation, and Screening Role

Frozen formula:

[
IV_j = (DistGood_j - DistBad_j) \times WOE_j
]

[
IV = \sum_j IV_j
]

Descriptive reference only:

| IV | Interpretation |
|---:|---|
| < 0.02 | Very weak |
| 0.02–0.10 | Weak |
| 0.10–0.30 | Moderate |
| 0.30–0.50 | Strong |
| > 0.50 | Very strong; investigate carefully |

No hard rule such as `IV < 0.02 → automatically drop` is permitted. Stage 4.4 will combine IV with risk pattern, business interpretability, redundancy, multicollinearity, and later Logistic stability. Stage 4.1 created no actual IV ranking, threshold filter, candidate list, or drop recommendation.

## 13. Continuous Supervised-binning Methodology

Frozen workflow:

> Quantile pre-binning → target-aware adjacent merging

Rules recorded:

- at most about 20 quantile-based candidate pre-bins;
- respect actual unique-value structure if it cannot support about 20 bins;
- never jitter values;
- pre-bins are candidates, not final bins;
- final ordinary continuous bins usually 3–6;
- AGE tends toward 3–5;
- no fixed-width blind binning, opaque optimal binning, arbitrary manual cuts, or decision-tree cut search;
- no repeated cut hunting solely to maximize IV;
- no Holdout Test involvement.

Later adjacent merging considers all six frozen criteria:

1. adjacent default rates are very close;
2. adjacent WOE values are very close;
3. a bin is too small;
4. merging improves risk-structure stability;
5. merging preserves business interpretation;
6. merging reduces obvious random fluctuation.

Only adjacent or logically coherent ordered ranges may merge.

## 14. Final-bin Count and Sample-size Guidance

For 24,000 Train rows:

| Ordinary final-bin support | Guidance |
|---|---|
| ≥5%, approximately ≥1,200 | Preferred |
| 3%–5%, approximately 720–1,200 | Acceptable with justification |
| <3% | Normally merge with a reasonable adjacent bin |

Explicit special/business bins may be exceptions and must be flagged for human judgment. This is not a mechanical `min_bin_size=5%` rule.

## 15. Synthetic Quantile Pre-bin and Adjacent-merge Demonstration

A completely synthetic continuous sequence of 200 values was divided by `qcut` into five 40-row candidate pre-bins. Invented event counts produced:

| Synthetic pre-bin | Count | Good | Bad | Default Rate | WOE |
|---|---:|---:|---:|---:|---:|
| (0.999, 40.8] | 40 | 36 | 4 | 0.100000 | 1.686399 |
| (40.8, 80.6] | 40 | 30 | 10 | 0.250000 | 0.587787 |
| (80.6, 120.4] | 40 | 29 | 11 | 0.275000 | 0.458575 |
| (120.4, 160.2] | 40 | 20 | 20 | 0.500000 | -0.510826 |
| (160.2, 200.0] | 40 | 10 | 30 | 0.750000 | -1.609438 |

The adjacent second and third bins have close Default Rates (25.0% and 27.5%) and WOE values. Their conceptual merge produced:

- Count = 80;
- Good = 59;
- Bad = 21;
- Default Rate = 26.25%;
- WOE = 0.522189.

All total Count/Good/Bad preservation assertions passed. This is a target-aware teaching example only. It is not an automated optimum, creates no real threshold, and makes no claim about UCI data.

## 16. Monotonicity Policy

Monotonicity is preferred, not mandatory.

A reasonable overall risk direction is desirable, but a small local reversal may be real. Perfect monotonicity must not be manufactured through over-merging. Stability, sample adequacy, interpretability, and reasonable risk structure have priority.

## 17. Candidate Special-bin Policies

Recorded but not executed on real data:

### BILL_AMT1–BILL_AMT6

- `<0`: candidate special bin;
- `=0`: candidate special bin;
- `>0`: later quantile pre-binning and adjacent merging.

Candidate special bins may still be proposed for merging if extremely small and risk/WOE behavior is close; Human Checkpoint approval is required.

### PAY_AMT1–PAY_AMT6

- `=0`: candidate special bin;
- positive values: later quantile pre-binning and adjacent merging.

### current_utilization, avg_utilization, max_utilization

- `=0`: candidate business anchor;
- `>1`: candidate business anchor;
- `0 < utilization <= 1`: later supervised binning.

Stage 4.1 sets no 25%, 50%, 75%, 80%, 90%, or other internal utilization cut point.

## 18. PAY and Ordinal-discrete Methodology

For PAY_0, PAY_2, PAY_3, PAY_4, PAY_5, PAY_6:

- analyze every month independently;
- do not use quantile binning;
- begin with raw-state Count, Good, Bad, Default Rate, WOE, and IV Contribution;
- merge only ordinal-adjacent or coherent ranges;
- do not pre-merge `-2/-1/0`;
- allow monthly groupings to differ;
- consider sparse high-severity coherent groups such as `3+` only in Stage 4.2 evidence;
- never claim modeling-grouped statuses have identical business meanings.

No real PAY count, rate, WOE, IV, or grouping was calculated.

## 19. Delinquency-summary and Existing-categorical Methodology

For `delinquent_months`, `max_delinquency`, and `severe_delinquent_months`:

- analyze each raw integer value;
- do not quantile-bin;
- permit adjacent-value merging only;
- a coherent `3+` may later be considered;
- unordered groups such as `0 + 4` are forbidden.

Existing categorical variables begin unchanged:

- SEX raw categories;
- EDUCATION_GROUP: Graduate School, University, High School, Other / Unknown;
- MARRIAGE_GROUP: Married, Single, Other / Unknown.

Stage 4.2 first calculates category evidence without proactive merging. A merge is only a candidate for extremely sparse support, zero Good/Bad, unstable extreme WOE, or clearly redundant risk behavior, and still requires Human Checkpoint approval.

No real categorical count, rate, WOE, IV, or merge was calculated.

## 20. Zero-event Handling and 0.5 Smoothing Fallback

Synthetic counts intentionally produced:

- `Good=20, Bad=0` → raw WOE = `+inf`;
- `Good=0, Bad=5` → raw WOE = `-inf`.

Formal order:

1. merge first if an adjacent/logically valid combination is reasonable;
2. apply 0.5 additive smoothing only if the bin has an explicit reason to remain and cannot reasonably merge.

The preferred synthetic merge combined the two adjacent special bins:

- Good = 20;
- Bad = 5;
- Count = 25;
- all merged WOE values finite.

The fallback demonstration explicitly added 0.5 to every Good/Bad count used for WOE distribution denominators:

| Bin | Good | Good for WOE | Bad | Bad for WOE | Smoothed WOE |
|---|---:|---:|---:|---:|---:|
| Toy_Special_A | 20 | 20.5 | 0 | 0.5 | 3.221978 |
| Toy_Special_B | 0 | 0.5 | 5 | 5.5 | -2.889490 |
| Toy_Ordinary_Neighbor | 30 | 30.5 | 25 | 25.5 | -0.312546 |

All smoothed WOE and IV contributions were finite; fallback toy total IV = `1.775766475725`.

Smoothing is not a default step. Every future use must identify the affected variable/bin and reason.

## 21. Future Stage 4.6 KFold Policy and Limitation

Recorded now; not executed.

Stage 4.2–4.5 use the complete 24,000 Train rows to freeze feature engineering, bin boundaries, PAY grouping, variable selection, and model specification.

Stage 4.6 uses Stratified 5-Fold for internal stability evidence:

### Fixed across folds

- feature engineering;
- variable selection;
- bin boundaries;
- PAY grouping.

### Re-estimated inside each fold

1. calculate WOE values using only approximately 19,200 fold-training rows and the fixed bins;
2. apply that fold-training WOE mapping to approximately 4,800 validation rows;
3. refit Logistic coefficients on fold-training rows;
4. use validation rows only for that fold's AUC/KS and coefficient/WOE stability evidence.

Methodological limitation: full-Train targets already participated indirectly in model-specification design. Therefore Stage 4.6 metrics are internal stability/robustness evidence under a fixed specification, not a perfectly unbiased final out-of-sample estimate. The untouched 6,000-row Holdout Test remains sealed until Stage 5 final comparison.

No KFold import, split, Logistic fit, AUC, or KS calculation occurred in Stage 4.1.

## 22. Real-data Boundary and Holdout Test Protection

Confirmed:

- raw CSV not loaded;
- no Stage 3 frame replayed;
- no split replay was needed;
- no real target loaded;
- no real predictor passed to WOE/IV/default-rate/binning/ranking code;
- no real PAY, utilization, AGE, LIMIT_BAL, BILL, PAY_AMT, category, or engineered-feature target relationship computed;
- no actual WOE/IV table, pre-bin, candidate cut, final cut, ranking, or recommendation produced;
- Holdout Test remained sealed and unused.

All executable examples used variables explicitly named `toy_*` or `Toy*`.

## 23. No Variable Selection and No Modeling

Not performed:

- actual IV ranking or IV threshold filtering;
- correlation screening;
- VIF or multicollinearity removal;
- feature elimination;
- candidate Logistic feature selection;
- Logistic, tree, forest, boosting, XGBoost, or LightGBM fitting;
- KFold or cross-validation;
- AUC, KS, calibration, confusion matrix, coefficient estimation, threshold selection;
- class weighting or sampling.

Stage 4.1 is method design, not model development.

## 24. Notebook Clean-run Evidence

| Check | Result |
|---|---|
| Notebook | `notebooks/04_woe_iv_binning_method_design.ipynb` |
| Python | `/Users/yu/2.工作/Risk Analysis Project/.venv/bin/python`, 3.9.6 |
| Execution | Fresh local `python3` kernel; project root working directory |
| Cells | 25 total: 16 markdown + 9 code |
| Execution counts | Sequential 1–9 |
| Saved outputs | 21 |
| Error outputs | 0 |
| Tracebacks in Notebook | 0 |
| Manual/programmatic tolerance | `rtol=1e-12, atol=1e-12` |
| Final Notebook size | 52,998 bytes |
| Final Notebook SHA-256 | `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9` |

No package was installed, upgraded, or removed. A harmless `jupyter_client` destructor warning appeared only after explicit kernel shutdown; it is not a saved Notebook output, error, or traceback.

## 25. Files Created or Modified

Created:

- `/Users/yu/2.工作/Risk Analysis Project/notebooks/04_woe_iv_binning_method_design.ipynb`
- `/Users/yu/2.工作/Risk Analysis Project/task_records/Stage_4.1_WOE_IV_Binning_Method_Design.md`

Modified existing files: none.

No processed dataset, binning CSV, WOE mapping CSV, feature-ranking file, model artifact, scorecard, source module, test module, external figure, checkpoint, HTML, PDF, cache, or Stage 4.2 artifact was created.

## 26. Protected-input Final Integrity

All protected-input sizes and SHA-256 values remained identical to Section 6 after Notebook execution and report creation. Tracked and staged diffs remained empty. The Git index, HEAD, history, prior-stage artifacts, raw data, README files, and requirements were unchanged.

## 27. Final Git State

Expected and verified Stage 4.1 handoff state:

- branch: `main`;
- HEAD: `8fbe2fa4b3715719e652101f694e5673abebba03`;
- working tree changes: exactly the two untracked Stage 4.1 artifacts;
- tracked unstaged diff: empty;
- staged diff: empty;
- remote: none;
- no `git add`;
- no commit;
- no push;
- no remote change.

## 28. Acceptance Criteria Check

All 48 Architect completion criteria passed:

- valid Git/Stage 3.3 Reviewer-PASS baseline;
- Holdout Test unused;
- Good/Bad, WOE direction, IV formula, Default Rate distinction, descriptive IV ranges, and non-hard-threshold policy documented;
- main toy WOE/IV hand/programmatic calculations reconciled;
- continuous, final-bin count, sample-size, special-bin, adjacent-merge, monotonicity, PAY, delinquency, categorical, and zero-event rules documented;
- synthetic adjacent merge and explicit zero-event fallback demonstrations passed;
- future fixed-bin/fold-specific-WOE KFold policy and limitation documented without execution;
- no real WOE/IV, real binning, variable selection, Logistic, KFold, or Test analysis;
- Notebook clean-run passed;
- this Task Report is complete and preserves the immutable specification;
- protected inputs unchanged;
- only two authorized artifacts created;
- no staging, commit, or push;
- execution stopped at the Human Learning Checkpoint.

## 29. Deviations, Limitations, and Unexpected Artifacts

- Deviations from the Architect Specification: none identified.
- Incomplete Stage 4.1 items: none identified.
- Unexpected repository artifacts or modifications: none observed.
- Operational note: local-kernel permission was required for Jupyter loopback communication; execution used the existing environment without package changes.
- Methodological limitation: the future Stage 4.6 metrics have the full-Train design-participation limitation documented in Section 21.
- Toy limitation: all displayed thresholds, counts, WOE, and IV values are synthetic teaching devices and convey no empirical conclusion about project predictors.

## 30. Human Learning Checkpoint

Status:

`READY FOR ARCHITECT/USER HUMAN LEARNING CHECKPOINT`

Codex stops here. Stage 4.2 has not started. The user and Architect must confirm understanding of the frozen methodology before any real predictor WOE/IV or binning work is authorized.

## 31. Reviewer Status

No Stage 4.1 Reviewer verdict has been produced or claimed. Reviewer validation occurs only after the Architect/user Human Learning Checkpoint and separate handoff. No milestone commit was created by Codex.

# REVIEWER RESULT

## Verdict

PASS

## Requirement Check

- The complete immutable Architect Specification is present, and the Stage 4.1 deliverables are limited to the required methodology Notebook and Task Report.
- Git pre-flight and prior-stage baseline are valid: branch `main`, HEAD `8fbe2fa4b3715719e652101f694e5673abebba03`, expected Stage 3.3-only commit scope and parent, clean pre-task state, empty staged diff, and no remote.
- The frozen project convention is correctly documented as `0 = Good/non-event` and `1 = Bad/event`. WOE is consistently defined as `ln(Dist Good / Dist Bad)`, with positive WOE interpreted as relatively lower risk.
- Default Rate and WOE are clearly distinguished. The toy table contains Count, Good, Bad, Default Rate, Dist Good, Dist Bad, WOE, and IV Contribution, including positive, negative, and zero WOE examples.
- The IV definition and descriptive reference ranges are correct. IV is explicitly treated as screening evidence rather than an automatic variable-drop rule.
- Manual and helper WOE/IV calculations reconcile at the required tolerance. The synthetic total IV and all bin-level values are arithmetically correct.
- The continuous workflow, approximately 20 quantile pre-bins, 3–6 ordinary final-bin guidance, AGE 3–5 guidance, ordinary-bin size principles, six adjacent-merging criteria, and preferred-not-mandatory monotonicity policy are all documented as frozen.
- BILL_AMT, PAY_AMT, utilization, PAY status, delinquency-summary, and existing-categorical policies match the Architect Specification without prematurely defining real cut points or groupings.
- The zero-event demonstration correctly shows raw infinite WOE, the merge-first preference, and explicit 0.5 additive smoothing only as a documented fallback.
- The future Stage 4.6 policy correctly fixes feature/model specification across folds, re-estimates WOE and Logistic coefficients within each fold-training subset, and limits CV interpretation to internal stability/robustness evidence.
- All executable calculations are synthetic. The Notebook contains no project-data read, real predictor/target analysis, real binning, variable selection, model fitting, KFold execution, AUC/KS, sampling, or output-file creation. Holdout Test remains sealed and unused.
- The Notebook clean-runs with sequential execution counts 1–9, 21 saved outputs, and no error output. Stage 4.2 was not started.
- Protected inputs and all prior-stage artifacts remain unchanged; only the two authorized Stage 4.1 artifacts existed as untracked changes before Reviewer closeout.

## Independent Verification

- Read the complete Architect Specification and Codex execution evidence in this Task Report.
- Inspected every Notebook Markdown/code cell, saved output, execution count, metadata field, import, file-access pattern, and prohibited-operation pattern.
- Independently recalculated the four-bin toy WOE/IV table and reproduced total IV `0.740236228026`, including the positive, negative, and zero WOE cases.
- Independently recalculated the synthetic pre-bin WOE values and the adjacent merge result: 80 rows, 59 Good, 21 Bad, 26.25% Default Rate, and WOE `0.522189`.
- Independently recalculated the zero-event 0.5 smoothing example and reproduced finite WOE values and total IV `1.775766475725`.
- Independently reran all nine code cells in a fresh local kernel without overwriting the Notebook; all cells succeeded and reproduced 21 outputs with no cell errors.
- Rechecked Notebook SHA-256 `97de91d34659a56f876e7e4fe321fdccf68dea8c0eeb2561d2f3161097afada9`, protected-file hashes, Git branch/HEAD, tracked/staged/untracked scope, and remote state after verification.

## Issues Found

None affecting acceptance.

## Required Fixes

None.

## Architect Decision Needed

None.
