# Stage 6.2 — Final Portfolio README / Executive Summary

## 1. Task Identification

- Task: `Stage 6.2 — Final Portfolio README / Executive Summary`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- Starting/current HEAD: `7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`
- HEAD message: `Curate repository for portfolio presentation`
- Execution status: `READY FOR STAGE 6.2 REVIEWER`
- Primary deliverable: `/Users/yu/2.工作/Risk Analysis Project/README.md`
- Task Report: `/Users/yu/2.工作/Risk Analysis Project/docs/audit/Stage_6.2_Final_Portfolio_README.md`
- Codex commit/push: none

## 2. Stage Objective and Boundaries

Stage 6.2 replaced the Foundation-era root README with the completed project's public-facing portfolio landing page and executive summary. The new README is designed for credit-risk and portfolio-risk hiring managers, risk analytics interviewers, and technically informed recruiters. It exposes final Holdout evidence early, preserves the interpretability/performance tradeoff, and provides a concise path into the six curated project Notebooks.

No modeling, data analysis, metric calculation, feature work, model change, Notebook execution, new visual, resume content, or interview packaging was performed. Stage 6.3 and Stage 6.4 were not started.

## 3. Corrected Re-issue and Prompt Validation

This execution followed the corrected Stage 6.2 re-issue. The previous root-level `TASK_REPORT_Task_0.1_Current-State_Audit.md` was explicitly identified as an erroneous artifact from a non-current task and was not used as the Stage 6.2 contract or Task Report.

Prompt-source validation before project inspection confirmed:

- Exact standalone Stage 6.2 BEGIN markers: `1`
- Exact standalone Stage 6.2 END markers: `1`
- BEGIN precedes END: `PASS`
- Both markers begin directly with `=`: `PASS`
- Complete numbered Sections 1–41: `PASS`
- Task identity: Stage 6.2, not Task 0.1
- Placeholder or omitted section: none; the phrase “broken image placeholders” appears only as an explicit prohibition in Section 24
- Authoritative block UTF-8 bytes: `31,894`
- Authoritative block SHA-256: `780d631f1905ef529adde302a294dbef94a7d3f40fe2d57fdd6b4f34d92faecc`

The complete authoritative block is appended at the end of this report. Section 14 records the final exact-comparison method and result.

## 4. Corrective Pre-flight and Clean Baseline

The Reviewer-confirmed corrective starting state matched exactly:

- Repository top-level: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD: `7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`
- HEAD message: `Curate repository for portfolio presentation`
- Tracked diff: empty
- Staged diff: empty
- Remote: absent
- Only untracked artifact: `TASK_REPORT_Task_0.1_Current-State_Audit.md`

The exact root-level erroneous file was verified before deletion:

- Absolute path: `/Users/yu/2.工作/Risk Analysis Project/TASK_REPORT_Task_0.1_Current-State_Audit.md`
- Type: regular file
- Git state: untracked and unstaged
- Size: `17,824` bytes
- SHA-256: `6e2fd4840b0f5c35874ced864261252bc66e9db563a89c01c8226477e472ab63`

The separate accepted historical record was verified before deletion of the root artifact:

- Protected path: `docs/audit/TASK_REPORT_Task_0.1_Current-State_Audit.md`
- Git state: tracked and unchanged
- Size: `25,094` bytes
- SHA-256: `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8`

Only the exact root-level erroneous file was deleted. No wildcard, recursive deletion, or approximate path was used. Immediately afterward, branch and HEAD remained unchanged; tracked, staged, and untracked states were empty; remote remained absent; and the historical file remained present with the same hash. This restored the clean Stage 6.2 baseline.

## 5. Root README Before and After

Before Stage 6.2, `README.md` was a 2,256-byte Foundation-era overview. It stated that the project remained in Foundation and reported no model or EDA results. Its data, validation, reference, and audit-navigation links were useful, but its stage status and portfolio narrative no longer represented the completed project.

After Stage 6.2, `README.md` is a 7,135-byte, approximately 904-word professional-English portfolio landing page. It preserves useful provenance/navigation links while replacing obsolete Foundation wording with a concise completed-project executive summary.

Final information flow:

1. Informative project title
2. Two-paragraph quick summary
3. Compact final Holdout results table and interpretation
4. Behavioral credit-default business problem
5. Dataset and validation design
6. WOE Logistic Primary and XGBoost Challenger approaches
7. Four high-value modeling decisions
8. Six-Notebook recommended path
9. Scope and limitations
10. Reproducibility entry points

The final README exposes the project identity, model roles, and final performance within the opening section and results table, while retaining enough technical detail for a three-to-five-minute portfolio read.

## 6. Factual Sources and Accepted Evidence

The implementation followed the frozen source hierarchy:

1. Reviewer-approved final Stage reports
2. Final analytical Notebooks
3. Earlier audit records where needed
4. Stage 6.1 navigation and provenance documentation

Principal sources inspected:

- `docs/audit/Stage_6.1_Repository_and_GitHub_Cleanup.md` — final Reviewer verdict `PASS`, curated repository state, and Notebook path selection
- `notebooks/README.md` — exact six-Notebook order, titles, roles, and paths
- `notebooks/10_final_woe_logistic_freeze.ipynb` — final ten-variable WOE Logistic specification and Development refit
- `docs/audit/Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md` — final Reviewer verdict `PASS` and Primary-model evidence
- `notebooks/11_xgboost_challenger_development.ipynb` — frozen 26-conceptual-predictor / 29-column Challenger evidence
- `notebooks/12_final_holdout_model_comparison.ipynb` — direct saved output for both final Holdout metrics and deltas
- `docs/audit/Stage_5_XGBoost_Challenger_and_Final_Holdout_Comparison.md` — final Reviewer verdict `PASS`, accepted comparison, and limitations
- `docs/audit/Stage_3.1_Train_Test_Split_and_EDA.md` — frozen 24,000/6,000 split evidence
- `data/README.md` — dataset name, UCI attribution, dimensions, target, and existing official source URL
- The authoritative Stage 6.2 Architect Specification — required Taiwan-associated population framing and public-claim boundaries

The existing UCI dataset URL was reused; no generic industry benchmark, AUC grading scheme, regulatory assertion, or new external comparative research was added.

## 7. Verified Facts Used in the README

### Dataset and Validation

- Dataset: UCI Machine Learning Repository, *Default of Credit Card Clients*
- Source population framing: 30,000 credit-card customers associated with Taiwan
- Raw dimensions: `30,000 × 25`
- Target: `default.payment.next.month`
- Development sample: `24,000`
- Holdout sample: `6,000`
- Internal validation: fixed stratified five-fold cross-validation where appropriate
- Final evaluation: one comparison on the same 6,000-row Holdout after both model specifications were frozen

### Model Roles

- Primary: interpretable ten-variable WOE Logistic Regression
- Challenger: nonlinear XGBoost using 26 conceptual predictors represented by 29 encoded columns

### Accepted Final Holdout Results

| Model | Exact AUC | Exact KS |
|---|---:|---:|
| WOE Logistic Primary | `0.758535259474` | `0.405185813870` |
| XGBoost Challenger | `0.779242005131` | `0.421655549501` |

- Challenger minus Primary AUC: `+0.020706745657`
- Challenger minus Primary KS: `+0.016469735631`

The public README displays four-decimal metrics and deltas, while this report preserves the accepted full-precision values. Notebook 12 saved output and the final Stage 5 PASS record agree.

## 8. Six-Notebook Navigation

The root README uses exactly the current Stage 6.1 curated paths:

| Role | Link target |
|---|---|
| Data Preparation & EDA | `notebooks/02_train_test_split_and_eda.ipynb` |
| Feature Engineering | `notebooks/03_feature_engineering_and_sanity_checks.ipynb` |
| WOE / Binning / Variable Screening | `notebooks/07_woe_iv_variable_screening.ipynb` |
| Final WOE Logistic Primary | `notebooks/10_final_woe_logistic_freeze.ipynb` |
| XGBoost Challenger Development | `notebooks/11_xgboost_challenger_development.ipynb` |
| Final Holdout Model Comparison | `notebooks/12_final_holdout_model_comparison.ipynb` |

The README also links to `notebooks/README.md`, `docs/audit/README.md`, `data/README.md`, `references/README.md`, and `requirements.txt`. All introduced local links were resolved relative to the root README with exact filename case and existing targets.

## 9. Claims and Wording Safeguards

The README deliberately:

- frames the problem as behavioral default prediction/discrimination for existing customers;
- states observed Holdout differences without claiming statistical significance;
- preserves the WOE Logistic transparency/governance value despite lower Holdout discrimination;
- does not claim causal improvement, universal XGBoost superiority, or state-of-the-art performance;
- does not describe the project as a production, deployed, bank-validated, regulatory-compliant, Basel, IFRS 9, or official PD system;
- does not present the model as an application scorecard, approval engine, pricing system, limit engine, or formal risk-band system;
- does not expose internal Architect/Prompt Engineer/Codex/Reviewer workflow in the public portfolio narrative;
- avoids full tuning grids, parameters, coefficients, bins, hashes, Reviewer logs, and package-debugging history;
- explains Development/Holdout discipline once rather than repeating seal mechanics throughout the document.

## 10. Scope and Limitations Included

The README concisely records:

- public benchmark/case-study data rather than a current live bank portfolio;
- focus on discrimination using AUC and KS;
- absence of pricing, profitability optimization, business cutoff selection, expected-loss estimation, formal calibration, and regulatory PD validation;
- the higher-discrimination/higher-complexity XGBoost tradeoff against the more transparent WOE Logistic model;
- portfolio case-study scope rather than deployment or production-readiness claims.

## 11. Link Validation

The root README contains 13 Markdown links:

- 12 local repository links, all resolving to existing exact-case targets;
- 1 reused external UCI dataset URL already present in accepted `data/README.md`.

Exactly six links target the recommended analytical Notebooks. Local validation result: `PASS`.

No broken image placeholder, image link, or new visual asset was introduced.

## 12. Protected Analytical Integrity

Pre-change baseline:

| Protected Notebook | Bytes | SHA-256 |
|---|---:|---|
| `notebooks/10_final_woe_logistic_freeze.ipynb` | 268,129 | `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863` |
| `notebooks/11_xgboost_challenger_development.ipynb` | 2,949,131 | `b59f4905330f16a0aa744dc2f2d26cfd578911302bd6f6a8a61daac282cf3fc2` |
| `notebooks/12_final_holdout_model_comparison.ipynb` | 37,175 | `ae3d867e212cfa3561cfbac823c10d0debc7ad8aecc63fe3abdb65ab1ecab5a3` |

Post-change recomputation produced the same byte counts and SHA-256 values for all three Notebooks. No Notebook was executed, imported, edited, cleared, or resaved during Stage 6.2.

Additional protected source fingerprints were recorded before writing, including the Stage 4.7, Stage 5, and Stage 6.1 reports; `notebooks/README.md`; `data/README.md`; `references/README.md`; `docs/audit/README.md`; and the historical Task 0.1 report. Final Git and hash checks confirm they remain unchanged.

## 13. File Operations and Scope Confirmation

### Deleted under Explicit Corrective Authorization

- `/Users/yu/2.工作/Risk Analysis Project/TASK_REPORT_Task_0.1_Current-State_Audit.md` — erroneous, root-level, untracked Task 0.1 artifact; pre-deletion fingerprint recorded in Section 4

### Modified

- `README.md` — rewritten as the final public-facing portfolio landing page and executive summary

### Created

- `docs/audit/Stage_6.2_Final_Portfolio_README.md` — this Stage 6.2 Task Report

### Unchanged

- protected historical `docs/audit/TASK_REPORT_Task_0.1_Current-State_Audit.md`
- all analytical Notebooks
- all prior-stage Task Reports
- Stage 6.1 navigation files
- data, references, requirements, `.gitignore`, and Git history/configuration

No additional link correction was necessary. Task 0.1 was not executed or recreated as part of Stage 6.2. No new modeling, data analysis, visual, Stage 6.3 artifact, or Stage 6.4 artifact was created.

## 14. Immutable Architect Specification Verification

Comparison policy:

- Encoding: UTF-8
- Boundary: exact standalone `================ BEGIN AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.2 ================` line through exact standalone `================ END AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.2 ================` line, inclusive
- Following line terminator after END: excluded
- Source marker counts: BEGIN `1`, END `1`
- Embedded marker counts after insertion: BEGIN `1`, END `1`
- Derived byte count: `31,894`
- Derived SHA-256: `780d631f1905ef529adde302a294dbef94a7d3f40fe2d57fdd6b4f34d92faecc`
- Trimming, normalization, whitespace-insensitive comparison, or semantic comparison: none

Final byte-for-byte comparison against the authoritative prompt source: `PASS`.

## 15. Final Git State

Final Git verification after the immutable block was appended confirmed:

- tracked modification: `README.md`
- untracked addition: `docs/audit/Stage_6.2_Final_Portfolio_README.md`
- staged diff: empty
- branch: `main`
- HEAD: `7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`, unchanged from pre-flight
- HEAD message: `Curate repository for portfolio presentation`
- remote: absent
- no commit and no push
- no additional tracked or untracked artifact

## 16. Reviewer Handoff

Reviewer should independently review:

`Stage 6.2 — Final Portfolio README / Executive Summary`

Reviewer validation should cover:

1. the exact corrective deletion and preservation of the historical Task 0.1 record;
2. branch, HEAD, initial clean Stage 6.2 baseline, and final Git scope;
3. recruiter-facing README quality, scanability, and professional English;
4. dataset, split, model-role, full-precision metric, and delta accuracy;
5. visibility of final results and interpretability/performance tradeoff;
6. six curated Notebook paths and all current README links;
7. claim safeguards and concise limitations;
8. absence of methodology overload and repetitive Holdout wording;
9. protected Notebook and source-artifact integrity;
10. absence of modeling, Notebook execution, new visuals, Stage 6.3, or Stage 6.4 work;
11. exact immutable Stage 6.2 block preservation;
12. no staging, commit, push, or remote change.

## 17. Reviewer Result

_Reserved for independent Reviewer. Codex has not prewritten PASS, PASS WITH FIXES, or FAIL._

## 18. Immutable Architect Specification

================ BEGIN AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.2 ================

# STAGE 6.2 — FINAL PORTFOLIO README / EXECUTIVE SUMMARY

## 0. STAGE CONTEXT

Project:

`Risk Analysis Project`

Local project root:

`/Users/yu/2.工作/Risk Analysis Project`

Macro status:

- Stage 0 — CLOSED
- Stage 1 — CLOSED
- Stage 2 — CLOSED
- Stage 3 — CLOSED
- Stage 4 — CLOSED
- Stage 5 — CLOSED
- Stage 6.1 Repository / GitHub Cleanup — CLOSED
- Stage 6.2 Final Portfolio README / Executive Summary — CURRENT

Future work:

- Stage 6.3 Portfolio Visuals / Model Documentation
- Stage 6.4 Resume / Interview Packaging

Stage 6.2 must NOT perform Stage 6.3 or Stage 6.4 work.

\===============================================================================

1. EXPECTED STARTING GIT STATE
   \===============================================================================

Expected branch:

`main`

Expected HEAD:

`7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`

Short hash:

`7e34a0f`

Expected commit message:

`Curate repository for portfolio presentation`

Expected working tree:

clean

Expected remote:

none

Codex must independently verify the actual repository state before editing.

If branch, HEAD, or working tree differs materially and the difference is unexplained:

return BLOCKED.

Do not silently reset, stash, checkout, amend, rebase, discard, or rewrite history.

\===============================================================================
2\. PRIMARY OBJECTIVE
=====================

Rewrite the repository root:

`README.md`

into the primary landing page for the completed credit-risk portfolio project.

The README should enable a recruiter, hiring manager, or risk professional to:

- understand the business / modeling problem quickly；
- understand what dataset was used；
- understand the project’s model-development strategy；
- see the final out-of-sample results quickly；
- understand the tradeoff between interpretability and nonlinear predictive performance；
- navigate to the most important notebooks；
- understand the project scope and limitations。

The README should function as:

`Portfolio Landing Page + Executive Summary`

It should NOT function as:

- a research paper；
- a full methodology report；
- a development diary；
- a Git history explanation；
- an Architect / Reviewer workflow record；
- a hyperparameter appendix；
- a model validation report。

\===============================================================================
3\. TARGET AUDIENCE
===================

The primary audience is:

- Credit Risk hiring managers；
- Portfolio Risk hiring managers；
- Bank Risk hiring managers；
- Risk Analytics interviewers；
- technically informed recruiters。

The README should be understandable without requiring the reader to inspect all notebooks.

A technically interested reader should still be able to follow links into the notebooks and audit documentation.

\===============================================================================
4\. READING-TIME TARGET
=======================

Design the README so that:

### First \~30 seconds

A reader can understand:

- what the project is；
- what problem it solves；
- what the two main model roles are；
- what the final performance looks like。

### Within \~3–5 minutes

A reader can understand:

- dataset；
- validation design；
- modeling approach；
- major modeling decisions；
- final comparison；
- repository structure；
- scope / limitations。

Avoid unnecessary density.

\===============================================================================
5\. WRITING STYLE
=================

The README should be:

- concise；
- professional；
- technically accurate；
- recruiter-friendly；
- scan-friendly；
- evidence-based；
- modest in its claims。

Prefer:

- short paragraphs；
- concise bullet points where useful；
- small tables；
- clear section hierarchy。

Avoid:

- long walls of text；
- excessive mathematical notation；
- excessive implementation detail；
- marketing language；
- exaggerated claims；
- generic AI/ML buzzwords；
- tutorial-style explanations of basic concepts。

The project should read like a serious credit-risk modeling case study.

\===============================================================================
6\. LANGUAGE
============

The public-facing root README should be written in professional English.

Use terminology appropriate for credit-risk / model-development contexts.

Do not introduce terminology that the project did not actually implement.

\===============================================================================
7\. README INFORMATION ARCHITECTURE
===================================

The README should cover the following conceptual areas.

Codex has discretion over exact section titles, wording, grouping, and order where this improves readability, but the final information architecture should preserve the intended hierarchy.

Required conceptual areas:

1. Project title
2. Concise project summary
3. Final model results
4. Business problem
5. Dataset and validation design
6. Modeling approach
7. Key modeling decisions
8. Repository / notebook guide
9. Scope and limitations

The preferred high-level reading flow is:

Project identity
→ quick summary
→ final results
→ problem/data/method
→ key decisions
→ navigation
→ limitations

Codex may make small structural refinements if clearly superior.

Do not add unnecessary major sections.

\===============================================================================
8\. TITLE REQUIREMENTS
======================

The title should clearly communicate:

- credit-card / credit default risk；
- modeling；
- the comparison between an interpretable Logistic-based primary model and XGBoost challenger。

Do not use vague titles such as:

`Machine Learning Project`

or:

`Credit Risk Analysis`

if a more informative title can be constructed.

The exact wording is NOT frozen.

Codex should write the best concise title based on the actual project.

\===============================================================================
9\. PROJECT SUMMARY REQUIREMENTS
================================

Near the top of the README, include a concise project summary.

It should communicate, without excessive detail:

- this is a behavioral credit-default modeling project；
- the data concern existing credit-card customers；
- the prediction target is next-month default；
- the project compares an interpretable WOE Logistic primary model against an XGBoost challenger；
- model development and final evaluation are separated；
- the final Holdout result can be stated concisely。

The summary should be short enough to scan quickly.

Do not make this a full methodology paragraph.

\===============================================================================
10\. BUSINESS-PROBLEM FRAMING
=============================

The project must be framed accurately as:

behavioral credit default prediction / discrimination

for existing credit-card customers.

The business-problem section should explain at a high level:

- what is being predicted；
- why distinguishing higher-default-risk customers from lower-default-risk customers is useful；
- why predictive performance and interpretability are both relevant。

Do NOT claim the project built:

- an application underwriting model；
- an approval / decline engine；
- a formal regulatory PD model；
- a Basel model；
- an IFRS 9 ECL model；
- a production bank decision engine；
- a complete risk-ranking / segmentation system；
- a pricing model；
- a credit-limit decision engine。

The project does produce continuous default scores/probabilities, but no formal risk-tier system was built.

Use accurate language such as:

- default prediction；
- default discrimination；
- behavioral credit risk；
- distinguishing customers with different default-risk profiles。

\===============================================================================
11\. DATASET FACTS
==================

Codex must verify project records before writing dataset facts.

Expected frozen facts include:

- UCI Default of Credit Card Clients dataset；
- 30,000 observations；
- 25 raw columns；
- target:
  `default.payment.next.month`
- behavioral / account-history style predictors；
- source population associated with Taiwan credit-card customers。

Do not invent dataset characteristics.

Do not describe the data as U.S. bank data.

Do not imply that the dataset represents a current production portfolio.

\===============================================================================
12\. VALIDATION DESIGN
======================

Explain the validation design once, clearly and concisely.

Expected framework:

- 24,000 Development observations；
- 6,000 Holdout observations；
- feature development and model development performed using Development evidence；
- 5-fold cross-validation used within Development where appropriate；
- final Holdout evaluation performed only after model specifications were frozen。

The README should communicate validation discipline without turning this into a long leakage-control section.

Do NOT repeatedly emphasize Holdout controls in multiple sections.

Do NOT include:

- ID SHA hashes；
- exact fold hashes；
- Git hashes；
- detailed freeze-gate mechanics；
- Reviewer evidence tables；
- random seed details；

unless absolutely necessary, which is not expected.

Detailed validation evidence remains available in notebooks / audit records.

\===============================================================================
13\. FINAL RESULTS
==================

Final Holdout performance is a central README element.

Codex must verify the accepted Stage 5 final metrics from project artifacts.

Expected frozen final results:

### WOE Logistic Primary

Holdout AUC approximately:

`0.758535259474`

Holdout KS approximately:

`0.405185813870`

### XGBoost Challenger

Holdout AUC approximately:

`0.779242005131`

Holdout KS approximately:

`0.421655549501`

Expected deltas:

AUC:

`+0.020706745657`

KS:

`+0.016469735631`

The README should present these results in a compact, highly readable form, preferably a small table.

Reasonable display rounding such as four decimals is acceptable.

Do not overload the main results section with:

- full CV tables；
- all fold metrics；
- baseline search results；
- 675-grid results；
- 564 near-best models；
- exact XGBoost parameter grid；
- all Logistic coefficients。

\===============================================================================
14\. RESULT INTERPRETATION BOUNDARY
===================================

The README may state that XGBoost achieved stronger unseen / out-of-sample discrimination than the WOE Logistic primary model.

The README must also retain the interpretability / complexity tradeoff.

Do NOT claim:

- statistical significance unless actually tested；
- causal improvement；
- SOTA performance；
- production readiness；
- universal superiority of XGBoost；
- that Logistic is obsolete；
- that the Challenger should automatically replace the Primary；
- that a \~0.779 AUC is exceptional across all credit-risk datasets。

Avoid phrases such as:

`significantly outperformed`

unless referring casually to magnitude would be misleading; no formal significance test was performed.

Prefer factual descriptions of the observed metric differences.

\===============================================================================
15\. MODELING APPROACH — PRIMARY
================================

The README should describe the Primary at a high level.

Relevant ideas include:

- WOE Logistic Regression；
- supervised / structured binning；
- Weight of Evidence transformation；
- IV and redundancy-informed variable screening；
- compact final predictor set；
- directional interpretability；
- traditional credit-risk modeling orientation。

Expected final Primary:

10-variable WOE Logistic model.

Do NOT include:

- complete bin definitions；
- all WOE values；
- all coefficients；
- all p-values；
- detailed VIF tables；
- formulas unless Codex judges one extremely useful, which is not expected。

\===============================================================================
16\. MODELING APPROACH — CHALLENGER
===================================

The README should describe the XGBoost Challenger at a high level.

Relevant ideas include:

- broader behavioral feature set；
- nonlinear relationships；
- interactions；
- Development-only model selection；
- Challenger role relative to interpretable Primary。

Expected Challenger feature universe:

26 conceptual predictors / 29 encoded columns.

Do not turn the README into an XGBoost tuning report.

Do NOT include:

- all 675 joint-grid combinations；
- Phase A/B/C search tables；
- specific boundary-expansion history；
- detailed early-stopping mechanics；
- full parameter JSON；
- exact `best_round` unless Codex finds a compelling concise reason, which is not expected。

\===============================================================================
17\. KEY MODELING DECISIONS
===========================

The README should highlight a small number of high-value modeling decisions.

The current Architect intent is approximately four high-level themes:

### A. Interpretable Primary + Nonlinear Challenger

Explain why the project deliberately compares two different model roles rather than building a large model zoo.

### B. Credit-risk-oriented WOE / IV pipeline

Show that the Logistic model is not simply a generic sklearn classifier but follows an interpretable credit-risk modeling workflow.

### C. Behavior-based feature engineering

Show that the project uses repayment status, delinquency behavior, payments, billing / credit exposure, utilization, and related behavior-oriented information.

### D. Strict development / validation separation

Communicate that model-development decisions were made without using final Holdout predictive evidence.

These should be concise.

Do NOT create separate headline bullets for minor implementation details such as:

- 1-SE rule；
- random\_state；
- XGBoost version；
- exact early-stopping patience；
- exact grid size；
- exact tree depth。

Such details belong in technical artifacts.

\===============================================================================
18\. AVOID HOLDOUT REPETITION
=============================

Validation rigor is important but should not dominate the README.

Explain the Development / Holdout structure clearly in the relevant section.

Elsewhere, only refer back to it when needed for interpreting final results.

Do not repeat variants of:

`Holdout was sealed`

throughout the README.

The reader should remember the modeling story, not only the Holdout protocol.

\===============================================================================
19\. REPOSITORY GUIDE
=====================

Stage 6.1 already curated exactly six recommended notebooks.

Codex must inspect:

`notebooks/README.md`

and use the actual selected notebook paths rather than guessing from memory.

The root README should provide a concise navigation path to the six recommended notebooks corresponding to:

1. Data Preparation & EDA
2. Feature Engineering
3. WOE / Binning / Variable Screening
4. Final WOE Logistic Primary
5. XGBoost Challenger Development
6. Final Holdout Model Comparison

A compact table or similarly readable format is preferred.

Do not duplicate the full content of `notebooks/README.md`.

Link to it for additional navigation if useful.

\===============================================================================
20\. SCOPE & LIMITATIONS
========================

Include a concise limitations / scope section.

Its purpose is to show professional awareness of what the project does and does not establish.

At minimum, address the substance of:

### Dataset scope

This is a public benchmark / case-study dataset, not evidence from a specific live bank portfolio.

### Modeling scope

The project focuses primarily on default discrimination.

It does not implement a full downstream bank decision framework such as:

- pricing；
- business cutoff optimization；
- profitability optimization；
- expected-loss estimation；
- regulatory PD calibration。

### Complexity / governance tradeoff

XGBoost improves observed discrimination while the WOE Logistic model remains substantially more transparent and easier to explain/govern.

Keep this section short.

Do not turn limitations into a long apology or undermine the project.

\===============================================================================
21\. CLAIMS TO AVOID
====================

The README must not claim or imply:

- the model is production-ready；
- it has been deployed；
- it was validated by a bank；
- it is regulatory-compliant；
- it produces official PD estimates；
- it was trained on U.S. bank data；
- it is an application scorecard；
- it determines approvals；
- it determines credit limits；
- it created formal risk bands；
- it proves causality；
- it achieved statistically significant superiority；
- it is state of the art；
- it is representative of every credit-card portfolio；
- XGBoost is universally preferable to interpretable models。

\===============================================================================
22\. TECHNICAL DETAIL BOUNDARY
==============================

The root README should expose enough technical detail to demonstrate competence without overwhelming the portfolio narrative.

Appropriate:

- Development / Holdout size；
- 5-fold CV at a high level；
- WOE / IV；
- Logistic Primary；
- XGBoost Challenger；
- behavioral features；
- AUC / KS；
- final comparison。

Too detailed for root README:

- every feature bin；
- every variable IV；
- every coefficient；
- every p-value；
- every tuning combination；
- all CV folds；
- exact fold hashes；
- exact search boundary history；
- Reviewer validation logs；
- Architect prompts；
- Git workflow；
- package debugging；
- dependency-installation history。

These remain accessible elsewhere.

\===============================================================================
23\. ARCHITECT / CODEX / REVIEWER VISIBILITY
============================================

The public-facing root README should describe the credit-risk project itself.

Do NOT make the internal:

Architect → Prompt Engineer → Codex → Reviewer

workflow part of the main portfolio narrative.

Do not advertise AI-agent workflow as a project feature.

Detailed audit records can remain accessible under:

`docs/audit/`

but should not distract from the portfolio landing page.

\===============================================================================
24\. VISUALS — STAGE 6.3 BOUNDARY
=================================

Stage 6.2 must NOT create new analytical or presentation visuals.

Do not create:

- ROC curves；
- KS curves；
- feature importance charts；
- SHAP plots；
- model pipeline diagrams；
- portfolio graphics；
- screenshots；
- banners。

Stage 6.2 may structure README prose so that future visuals could be inserted cleanly during Stage 6.3.

Do not add broken image placeholders.

If existing repository images already exist, do not substantially redesign visual presentation unless necessary for README integrity.

\===============================================================================
25\. README QUALITY STANDARD
============================

The final README should pass the following qualitative test:

A recruiter opening the repository should NOT need to ask:

- What is this project about?
- What is being predicted?
- What models were compared?
- Which model performed better?
- What were the final AUC / KS values?
- Was there a separate final evaluation sample?
- Where should I start reading?
- What does this project not claim to do?

At the same time, the README should NOT answer every possible technical question.

The notebooks and audit records exist for deeper inspection.

\===============================================================================
26\. EXISTING README INSPECTION
===============================

Codex must inspect the current root:

`README.md`

before rewriting.

Stage 6.1 may already contain necessary repository-path updates.

Preserve any useful factual/navigation information that remains relevant.

Do not blindly overwrite valid content without understanding it.

However, Stage 6.2 is explicitly authorized to substantially rewrite the root README into the final portfolio landing page.

\===============================================================================
27\. SOURCE-OF-TRUTH HIERARCHY
==============================

Before writing any factual claim, Codex should rely on accepted project artifacts.

Priority should generally be:

1. Reviewer-approved final Stage artifacts；
2. final notebooks；
3. Stage Task Reports / audit records；
4. curated Stage 6.1 navigation files；
5. earlier historical records only when needed。

If two project artifacts conflict:

do not guess.

Investigate which artifact is the later accepted frozen source.

If ambiguity remains material:

return BLOCKED.

\===============================================================================
28\. NO NEW EXTERNAL RESEARCH REQUIRED
======================================

Stage 6.2 should normally be completed entirely from existing project evidence.

Do not browse the web merely to add:

- generic credit-risk claims；
- industry benchmark statistics；
- AUC grading tables；
- model-performance comparisons；
- regulatory claims；
- citations that are unnecessary for this portfolio README。

If an existing verified external dataset/source URL already exists in project documentation, it may be reused appropriately.

Do not invent URLs.

\===============================================================================
29\. EXISTING ARTIFACTS MUST REMAIN FROZEN
==========================================

Do NOT modify analytical notebooks.

In particular, key final notebooks including the selected Primary, Challenger, and Holdout comparison must remain byte-identical unless a genuine structural blocker exists.

Do not rerun them.

Do not clear outputs.

Do not change notebook Markdown to match README wording.

README should describe the project; the frozen project should not be rewritten to match README.

\===============================================================================
30\. ALLOWED FILE CHANGES
=========================

Expected primary modification:

`README.md`

Create:

`docs/audit/Stage_6.2_Final_Portfolio_README.md`

Narrow link/navigation corrections elsewhere are allowed only if the new README exposes a genuine broken current link.

Do not broadly modify:

- notebooks；
- data；
- modeling code；
- requirements；
- historical audit reports；
- Stage 6.1 navigation files；

unless a concrete integrity issue requires it.

Any extra file modification must be explicitly justified in the Completion Report.

\===============================================================================
31\. STAGE 6.2 TASK REPORT
==========================

Create:

`docs/audit/Stage_6.2_Final_Portfolio_README.md`

The Task Report should record:

- Stage 6.2 objective；
- starting branch / HEAD；
- starting working-tree state；
- root README before/after role；
- factual sources used；
- final README section structure；
- final metrics used；
- six notebook links used；
- claims / wording safeguards；
- limitations included；
- link-validation result；
- confirmation no new modeling occurred；
- confirmation no new visuals were created；
- confirmation Stage 6.3 was not started；
- confirmation Stage 6.4 was not started；
- files changed；
- Git status；
- Reviewer handoff。

Do not copy the entire README into the Task Report unnecessarily.

\===============================================================================
32\. PRESERVE ARCHITECT SPECIFICATION
=====================================

The final Codex execution prompt generated by Prompt Engineer must contain the complete authoritative Stage 6.2 Architect Specification in a clearly delimited block.

Codex must append that authoritative specification verbatim to:

`docs/audit/Stage_6.2_Final_Portfolio_README.md`

Use clear external delimiters.

The authoritative text inside the block must not be:

- paraphrased；
- shortened；
- normalized；
- grammar-corrected；
- reformatted；
- rewritten。

Codex execution notes must remain outside the immutable specification block.

\===============================================================================
33\. LINK VALIDATION
====================

After README completion, verify all current local Markdown links introduced or modified by Stage 6.2.

At minimum validate:

- six recommended notebook links；
- link to `notebooks/README.md` if included；
- any `docs/` links；
- any `data/` or `references/` links used；
- dataset-source link if one is reused。

Do not knowingly leave broken relative links.

\===============================================================================
34\. ANALYTICAL INTEGRITY CHECK
===============================

Before and after Stage 6.2, verify that key frozen analytical notebooks remain unchanged.

At minimum verify the three final modeling artifacts corresponding to:

- final WOE Logistic；
- XGBoost Challenger；
- final Holdout comparison。

Prefer SHA-256 pre/post verification.

No analytical notebook should change during Stage 6.2.

\===============================================================================
35\. GIT OPERATIONS
===================

Codex may inspect Git and modify the authorized working-tree files.

Do NOT:

- commit；
- push；
- pull；
- fetch；
- rebase；
- amend；
- reset；
- rewrite history；
- configure remote；
- publish GitHub repo。

Leave changes uncommitted for Reviewer.

\===============================================================================
36\. BLOCKED CONDITIONS
=======================

Return BLOCKED rather than improvising if:

- starting Git state is materially inconsistent；
- accepted final project metrics cannot be reconciled；
- the six Stage 6.1 recommended notebooks cannot be located；
- the current repo contradicts Stage 6.1 completion；
- root README changes would require analytical notebook modification；
- a key factual claim cannot be verified from accepted artifacts；
- frozen analytical notebook integrity changes unexpectedly；
- link integrity cannot be achieved without scope expansion；
- completing README would require beginning Stage 6.3 or Stage 6.4。

A BLOCKED report must identify:

- exact issue；
- affected file / fact；
- evidence；
- safest next options；
- current Git state。

Do not silently make up missing facts.

\===============================================================================
37\. REVIEWER HANDOFF
=====================

After Codex completes Stage 6.2, provide a focused Reviewer handoff containing at minimum:

- branch；
- starting HEAD；
- current HEAD；
- confirmation HEAD unchanged；
- root README path；
- Stage 6.2 Task Report path；
- final README section structure；
- verified dataset facts；
- verified final Holdout metrics；
- verified six notebook links；
- summary of claims intentionally avoided；
- link-validation result；
- analytical notebook pre/post hashes；
- all modified / created files；
- confirmation no modeling changes；
- confirmation no new visuals；
- confirmation no Stage 6.3 work；
- confirmation no Stage 6.4 work；
- Git status；
- confirmation no commit；
- confirmation no push。

\===============================================================================
38\. REVIEW / COMMIT WORKFLOW
=============================

Codex does NOT commit.

Reviewer independently validates Stage 6.2.

If Reviewer returns:

FAIL

then:

- no commit；
- apply only approved fixes；
- re-review。

If Reviewer returns:

PASS WITH FIXES

then:

- no commit until fixes are completed and focused re-review returns final PASS。

If Reviewer returns final:

PASS

then create a local Stage 6.2 checkpoint commit in the same review round.

Preferred commit message:

`Finalize portfolio README`

After commit verify:

- branch remains `main`；
- expected files only；
- working tree clean；
- no push。

\===============================================================================
39\. REVIEWER VALIDATION SCOPE
==============================

Reviewer should independently verify at minimum:

### Accuracy

- dataset description；
- Development / Holdout counts；
- modeling roles；
- final Holdout AUC / KS；
- reported deltas；
- model complexity / interpretability framing。

### Portfolio quality

- quick readability；
- professional English；
- concise structure；
- results visible early；
- technical depth appropriate for hiring audience；
- not a methodology dump。

### Claims

- no production claim；
- no regulatory-PD claim；
- no application-underwriting claim；
- no significance claim；
- no SOTA claim；
- no formal risk-ranking claim。

### Navigation

- six notebooks correct；
- links valid；
- deeper documentation accessible。

### Scope

- no analytical notebook changes；
- no Stage 6.3 visuals；
- no Stage 6.4 resume/interview work。

\===============================================================================
40\. COMPLETION REPORT
======================

If successful, Codex should return:

`READY FOR STAGE 6.2 REVIEWER`

and provide a concise Completion Report including:

### Pre-flight

- branch
- HEAD
- initial cleanliness

### README

- path
- high-level final structure
- intended audience
- approximate scope / length characterization

### Facts Verified

- dataset
- split
- model roles
- final AUC / KS
- final deltas

### Navigation

- six notebook links
- link-validation status

### Boundaries

- no analytical changes
- no new visuals
- no Stage 6.3
- no Stage 6.4
- no unsupported claims

### Integrity

- key notebook pre/post hashes

### Files

- modified
- created
- any extra justified changes

### Git

- branch
- HEAD
- status
- no commit
- no push

\===============================================================================
41\. STAGE 6.2 SUCCESS CONDITION
================================

Stage 6.2 is ready for Reviewer only when ALL are true:

- expected Stage 6.1 checkpoint verified；
- root README rewritten as final portfolio landing page；
- project purpose is immediately understandable；
- business problem accurately framed as behavioral credit-default prediction；
- final Holdout results visible and correct；
- interpretability vs performance tradeoff accurately represented；
- validation design explained concisely；
- modeling approach explained without excessive detail；
- key decisions remain high-level；
- Holdout discussion is not repetitive；
- six recommended notebooks linked correctly；
- scope / limitations included；
- no unsupported or exaggerated claims；
- no Architect/Codex/Reviewer workflow clutter in portfolio narrative；
- no new model development；
- no analytical notebook modification；
- no new visual work；
- Stage 6.3 not started；
- Stage 6.4 not started；
- Stage 6.2 Task Report complete；
- authoritative Architect Specification preserved verbatim；
- links validated；
- changes remain uncommitted；
- no push occurred。

Do not begin Stage 6.3 inside this task.

================ END AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.2 ================
