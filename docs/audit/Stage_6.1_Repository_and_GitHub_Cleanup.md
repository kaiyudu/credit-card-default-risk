# Stage 6.1 — Repository / GitHub Cleanup

## 1. Task Identification and Status

- Task: `Stage 6.1 — Repository / GitHub Cleanup`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Execution status: `READY FOR STAGE 6.1 REVIEWER`
- Starting/current branch: `main`
- Starting/current HEAD: `68c80e07ab780b7c1b4ceff03876b534dc3ca954`
- Short HEAD: `68c80e0`
- HEAD message: `Complete XGBoost challenger comparison`
- Parent: `1a413c2d4d871541d73c75589e5b3715b9c09b3e`
- Stage 5 state: closed at the expected milestone commit
- Codex commit/push: none

## 2. Objective and Boundaries

Stage 6.1 reorganized the completed development repository into a clearer portfolio structure while retaining all Stage 0–5 notebooks, analytical results, Git history, and audit evidence. It added navigation and provenance documentation only. It did not begin the Stage 6.2 executive README, Stage 6.3 visuals/model documentation, or Stage 6.4 resume/interview packaging.

No analytical notebook was executed or edited. No data analysis, Holdout access, modeling, metric calculation, feature change, threshold work, calibration, or new visualization was performed.

## 3. Prompt-Source Validation

Before project modification, the supplied prompt was checked mechanically:

- Exact standalone BEGIN markers: `1`
- Exact standalone END markers: `1`
- BEGIN precedes END: `PASS`
- Marker lines begin directly with `=`: `PASS`
- Leading escape/space/blockquote/bullet/inline-code delimiter: none
- Complete Stage 6.1 block extractable: `PASS`
- Placeholder replacing a section: none observed

The authoritative block is appended at the end of this report. Its fingerprint and exact-comparison result are recorded in Section 15.

## 4. Pre-Execution Git State

The required clean baseline was confirmed before any write or move:

- Repository top-level: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD / parent / message: exact matches listed above
- `git status --short`: empty
- tracked working-tree diff: empty
- staged diff: empty
- untracked files: none
- Git remote: none

No restore, reset, checkout, stash, clean, amend, rebase, history rewrite, fetch, pull, push, staging, or commit operation was used.

## 5. Repository Structure Before Cleanup

```text
Risk Analysis Project/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   └── raw/UCI_Credit_Card.csv
├── notebooks/                    # 13 historical analytical notebooks
├── references/
│   ├── README.md
│   └── credit-card-default-a-very-pedagogical-notebook.ipynb  # ignored local copy
├── reports/figures/
└── task_records/                 # 13 audit records
```

No `src/`, `pipelines/`, `configs/`, `models/`, `tests/`, `services/`, or `api/` directory existed or was created.

## 6. Notebook Inventory and Six-Notebook Curation

All 13 existing `.ipynb` files were parsed and inspected read-only. Cell types, saved outputs, errors, headings, and principal code purposes were reviewed. Every historical notebook remains in its original path.

### Recommended Reading

| Order | Conceptual role | Selected notebook | Evidence and rationale |
|---:|---|---|---|
| 1 | Data Preparation & EDA | `02_train_test_split_and_eda.ipynb` | Establishes the frozen stratified Development/Holdout split and provides extensive Train-only EDA with explicit isolation controls. Its 36 cells and saved outputs directly explain the data conditions that drive later feature and modeling choices. |
| 2 | Feature Engineering | `03_feature_engineering_and_sanity_checks.ipynb` | Implements the exact frozen category mappings, delinquency summaries, and utilization definitions reused downstream. It includes Train and authorized Holdout transformation checks, formula-level assertions, schema validation, and 46 cells of reproducibility evidence. |
| 3 | WOE / Binning / Variable Screening | `07_woe_iv_variable_screening.ipynb` | Integrates the already-approved categorical and continuous bins, rebuilds all 29 WOE/IV tables, evaluates IV/correlation/VIF evidence, and freezes the ten-variable candidate matrix. It provides the strongest bridge from detailed bin construction to the final Logistic pipeline without duplicating the longer bin-development notebooks. |
| 4 | Final WOE Logistic Primary Model | `10_final_woe_logistic_freeze.ipynb` | Preferred file exists and is valid. It records the final Human variable decision, complete WOE specification, final unpenalized refit, coefficients, diagnostics, and frozen handoff. |
| 5 | XGBoost Challenger Development | `11_xgboost_challenger_development.ipynb` | Preferred file exists and is valid. It contains the complete deterministic Development-only search, boundary evidence, final 1-SE selection, freeze record, and full-Development refit. |
| 6 | Final Holdout Model Comparison | `12_final_holdout_model_comparison.ipynb` | Preferred file exists and is valid. It verifies both frozen models and performs the single same-sample sealed-Holdout AUC/KS comparison without tuning. |

### Candidate Evidence for Roles 1–3

- **Role 1 candidates:** `01_luca_audit_and_data_understanding.ipynb` and `02_train_test_split_and_eda.ipynb`. The Luca audit provides useful raw-data context but predates the final split controls. Notebook 02 is more aligned with the frozen pipeline, separates Development from Holdout explicitly, contains clearer project-owned EDA outputs, and is more useful for explaining leakage governance to a risk interviewer.
- **Role 2 candidates:** `03_feature_engineering_and_sanity_checks.ipynb` and the feature-oriented portions of the Luca audit. Notebook 03 is the only project-owned artifact that implements and validates the exact feature definitions later consumed by both final model families; it therefore has materially stronger reproducibility and recruiter relevance.
- **Role 3 candidates:** `04_woe_iv_binning_method_design.ipynb`, `05_pay_categorical_binning.ipynb`, `06_continuous_binning.ipynb`, and `07_woe_iv_variable_screening.ipynb`. Notebook 04 is a methodology design record, while 05 and 06 provide detailed family-specific bin construction. Notebook 07 is the best single public-facing synthesis because it consumes those frozen bins, evaluates the complete variable universe, documents multicollinearity and governance evidence, and produces the ten-variable candidate set used by the final Primary. The other three remain valuable supporting records.

No integrity reason required deviation from the preferred Notebook 10/11/12 choices.

## 7. Notebook Navigation

Created `notebooks/README.md` with:

- a short explanation of the preserved development history;
- exactly six recommended-reading links in the frozen conceptual order;
- concise role descriptions;
- a short supporting-notebook explanation without duplicating the future Stage 6.2 root README.

All six recommended links were resolved with exact case against existing files.

## 8. Audit Relocation

- Old location: `task_records/`
- New location: `docs/audit/`
- Existing records moved: `13`
- Method: content-preserving filesystem directory move
- Reason ordinary `mv` was used instead of preferred `git mv`: `git mv` would stage index changes, conflicting with the Stage 6.1 hard requirement that the staged diff remain empty. Reviewer can stage the final approved relocation later.
- Runtime/import/script dependencies on `task_records/`: none found
- Live navigation affected outside audit bodies: root `README.md` only
- Historical plain-text references inside audit bodies: preserved unchanged

### Moved-Record Integrity Baseline

| Audit record | Bytes | SHA-256 |
|---|---:|---|
| `Stage_1_Project_Foundation.md` | 32,398 | `0c065730997f8ba20cbfc0a2f6cbab11c0eebd8e5f59ab48254cce1fd57e820d` |
| `Stage_2_Luca_Audit_and_Data_Understanding.md` | 62,868 | `3f3cb2a981585993bc2c008763164c35fd2105f0749bbefa8d570f41e53c2ab7` |
| `Stage_3.1_Train_Test_Split_and_EDA.md` | 43,454 | `62a4c1e80ec747f2048d1e545e628ec7dad7ef64554002ed608b447a06517ea0` |
| `Stage_3.3_Feature_Engineering_and_Sanity_Check.md` | 77,812 | `b8d225689e9dab561f0769212e84199019cf15368fa120ac224119e379fe751b` |
| `Stage_4.1_WOE_IV_Binning_Method_Design.md` | 50,459 | `2347f089168675eba9a3eb198b9418b771f682c222bfd8abf161207d86cae851` |
| `Stage_4.2_PAY_Categorical_Binning.md` | 102,407 | `9474b88e43c4744474cc0d08c2167912580d0d0320dc3b5ddb43fe2c05c5df60` |
| `Stage_4.3_Continuous_Variable_Binning.md` | 154,540 | `a1a70c923ffc158db74cf586a8eb5d0b0091c4e6776781a88f90a297ead1c3d5` |
| `Stage_4.4_WOE_IV_Variable_Screening.md` | 185,676 | `637fd696317a9f18620052dbb45a140e8aa8a196e9ed36767091d8e225fcc288` |
| `Stage_4.5_Candidate_WOE_Logistic.md` | 61,711 | `89d2441c2873fc10968d365faadbd4318e252ca7b193634fac26d31642bca47e` |
| `Stage_4.6_Stratified_5Fold_Model_Stability.md` | 74,895 | `9c1c699bb1d7fbe11a2a21bc392c6a08791b4fa2b7ea832b9625cced8f0bcea7` |
| `Stage_4.7_Final_Logistic_Freeze_and_Full_Train_Refit.md` | 69,071 | `389c579dac4e9879ad1bf29c975822e5b63f565991a8033e16b1bd9a6f2172c4` |
| `Stage_5_XGBoost_Challenger_and_Final_Holdout_Comparison.md` | 218,882 | `c1e459e9956d613a72ded60c15e60e7c594efeb79c7e010aefad15f2f56aa872` |
| `TASK_REPORT_Task_0.1_Current-State_Audit.md` | 25,094 | `615b0f1ed13f163b32c61962061f0860f86d62ed383b7b31c6a1f35df84e6fd8` |

Post-move recomputation confirmed all 13 byte sizes and hashes are identical to this baseline. No audit body was rewritten.

## 9. Audit Index and Stage 6.1 Task Report

Created `docs/audit/README.md` as a concise stage-grouped index for technical, reproducibility, governance, and model-risk readers. It links all 13 relocated historical records and this Stage 6.1 report. The index explicitly notes that historical paths inside the immutable records remain historical facts.

Created this Task Report at `docs/audit/Stage_6.1_Repository_and_GitHub_Cleanup.md`. Reviewer Result is intentionally not prewritten.

## 10. Data Documentation and Raw-Data Decision

Updated `data/README.md` narrowly to add:

- a live relative link to `raw/UCI_Credit_Card.csv`;
- exact expected local path and filename;
- UCI acquisition/placement instructions;
- an explicit statement that GitHub should not be treated as the primary data-distribution source;
- the factual current tracking/history condition.

Raw CSV findings:

- Path: `data/raw/UCI_Credit_Card.csv`
- Shape documented from the frozen project record: `30,000 × 25`
- Target: `default.payment.next.month`
- Git state: tracked in current HEAD
- Historical state: present since commit `ed1020c` (`chore: initialize credit risk project`)
- Decision: retain the local file and frozen history; do not untrack, delete, or rewrite history
- Local raw-data destruction: none
- False claim of removal from history: none

The existing dataset name, creator, UCI source, DOI, attribution, and license statement were preserved from the previously reviewed project documentation; no new licensing claim or external attribution was invented.

## 11. `.gitignore` Decision

No `.gitignore` change was required. It already covers:

- `.venv/`, `venv/`, and `env/`;
- Python caches and bytecode;
- Jupyter checkpoints;
- `.DS_Store` and common editor/temp files;
- the exact optional local Luca Notebook path.

It does not broadly ignore analytical notebooks, audit records, README files, or portfolio artifacts. Because the raw CSV is already tracked in frozen history, no raw-data ignore rule or index manipulation was introduced. The exact external-reference rule remains useful to prevent an optional local download from being accidentally committed.

## 12. External Reference Decision

The source identity of `references/credit-card-default-a-very-pedagogical-notebook.ipynb` was established from existing repository evidence:

- Author: Luca Basanisi
- Title: *Credit Card Default: a very pedagogical notebook*
- Existing recorded source: Kaggle
- Existing recorded URL: `https://www.kaggle.com/code/lucabasa/credit-card-default-a-very-pedagogical-notebook`
- Local filename and Notebook content correspond to that pedagogical reference
- Git tracking/history: the copied Notebook was ignored and never tracked
- Pre-removal local bytes: `805,686`
- Pre-removal SHA-256: `6905231fbc3e6f514c7dd0776844cda9cde06a51e6eef6b2605c3af89b53b9bd`

Following the preferred public-facing rule, the ignored third-party copy was removed from the current local tree. It can be obtained from the cited authoritative Kaggle page; no Git history was rewritten. `references/README.md` was updated to make the third-party background role and the project's own frozen workflow distinction explicit, without attacking the reference or overstating originality.

## 13. Root README Boundary

The root `README.md` received only the necessary live-navigation correction caused by the audit move:

- project tree: `task_records/` → `docs/audit/`
- formal-report location sentence: `task_records/` → `docs/audit/`

No executive summary, model-comparison table, business narrative, resume content, result rewrite, or other Stage 6.2 substantive work was added. All other existing root README content was preserved.

## 14. Frozen Analytical Artifact Integrity

| Notebook | Pre-change bytes | Pre-change SHA-256 | Post-change result |
|---|---:|---|---|
| `notebooks/10_final_woe_logistic_freeze.ipynb` | 268,129 | `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863` | Identical |
| `notebooks/11_xgboost_challenger_development.ipynb` | 2,949,131 | `b59f4905330f16a0aa744dc2f2d26cfd578911302bd6f6a8a61daac282cf3fc2` | Identical |
| `notebooks/12_final_holdout_model_comparison.ipynb` | 37,175 | `ae3d867e212cfa3561cfbac823c10d0debc7ad8aecc63fe3abdb65ab1ecab5a3` | Identical |

All historical notebooks remain present. No Notebook was edited or rerun, so source, outputs, execution counts, and analytical conclusions are unchanged.

## 15. Immutable Architect Specification Verification

The authoritative prompt block is bounded inclusively by the unique standalone lines:

- `================ BEGIN AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.1 ================`
- `================ END AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.1 ================`

Comparison policy and derived evidence:

- Encoding: UTF-8
- Boundary: exact standalone BEGIN line through exact standalone END line, inclusive, excluding any following line terminator
- Source standalone marker counts: BEGIN `1`, END `1`
- Embedded standalone marker counts: BEGIN `1`, END `1`
- Derived byte count: `32,380`
- Derived SHA-256: `1cd962ffe4ed7ad292c0fa768f6c6222268e998395cef8d13ae0c2ce4e2b79e5`
- Exact byte-for-byte comparison: `PASS`
- Normalization/trimming/semantic comparison: none

The fingerprint is mechanically derived evidence; it was not treated as independent authority and the specification was not altered to fit it.

## 16. Link Validation

Repository-relative link validation covered:

- all six recommended links in `notebooks/README.md`;
- all current audit-index links in `docs/audit/README.md`;
- the current local raw-data link in `data/README.md`;
- the external Kaggle citation in `references/README.md` as a recorded external URL;
- current navigation outside immutable audit bodies affected by `task_records/` relocation.

Result: `PASS` — all intended local targets exist with exact case. No active runtime/import dependency requires the old audit path. Historical plain-text path mentions inside audit files were left unchanged as required.

## 17. File Operations

### Added

- `notebooks/README.md`
- `docs/audit/README.md`
- `docs/audit/Stage_6.1_Repository_and_GitHub_Cleanup.md`

### Modified

- `README.md` — two narrow current-path corrections only
- `data/README.md` — acquisition, placement, and tracked-history clarification
- `references/README.md` — external-reference and final-pipeline distinction plus handling record

### Moved Content-Preservingly

- all 13 files from `task_records/` to `docs/audit/`

### Removed from Current Local Tree

- `references/credit-card-default-a-very-pedagogical-notebook.ipynb` — ignored, never-tracked third-party copy with verified existing source citation

### Unchanged

- `.gitignore`
- `requirements.txt`
- raw CSV
- every analytical Notebook
- all audit-record bodies
- Git history/config/remotes/index

## 18. Final High-Level Repository Tree

```text
Risk Analysis Project/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   └── raw/UCI_Credit_Card.csv
├── notebooks/
│   ├── README.md
│   └── 13 preserved historical analytical notebooks
├── docs/
│   └── audit/
│       ├── README.md
│       ├── 13 preserved Stage 0–5 audit records
│       └── Stage_6.1_Repository_and_GitHub_Cleanup.md
├── references/
│   └── README.md
└── reports/
    └── figures/.gitkeep
```

## 19. Acceptance and Scope Confirmation

- Stage 5 frozen commit verified: `PASS`
- Exactly six recommended notebooks: `PASS`
- Selection grounded in actual contents: `PASS`
- All historical notebooks preserved: `PASS`
- Audit relocation safe and content-identical: `PASS`
- Current navigation links valid: `PASS`
- Analytical results changed: none
- Notebook source/output drift: none
- Holdout re-analysis: none
- New modeling or feature work: none
- New analytical or portfolio visuals: none
- Git history rewrite: none
- Raw-data deletion/untracking: none
- Fabricated attribution or license claim: none
- Fake production architecture: none
- Stage 6.2–6.4 substantive work: not started
- Git staging/commit/push/remote creation: none

## 20. Deferred Items and Limitations

- The root README intentionally remains a Foundation-era document except for the necessary audit-path correction; Stage 6.2 owns its substantive portfolio rewrite.
- The raw CSV remains tracked because it is already present in frozen history. No history rewrite or current-index cosmetic removal was authorized or appropriate.
- The copied Luca Notebook was removed from the current local tree and is recoverable from the existing Kaggle citation; its specific code license remains unconfirmed.
- Git may display the unstaged audit relocation as deletions plus untracked files until Reviewer stages the approved scope; this is expected because Codex preserved an empty staged diff.
- No unexpected artifact or incomplete Stage 6.1 item was identified.

### Final Git Working-Tree State

- Branch: `main`
- Current HEAD: `68c80e07ab780b7c1b4ceff03876b534dc3ca954` (unchanged from pre-flight)
- Tracked modified files: `README.md`, `data/README.md`, `references/README.md`
- Tracked deletions displayed by Git: the 13 former `task_records/*.md` paths; matching byte-identical files exist at the new untracked `docs/audit/` paths
- Untracked additions: `notebooks/README.md` and the `docs/audit/` tree containing its README, 13 relocated records, and this Stage 6.1 report
- Staged diff: empty
- Remote: none
- Commit/push: none

## 21. Reviewer Handoff

Reviewer should independently validate:

1. starting/current branch, HEAD, parent, message, and clean baseline;
2. exactly six recommended notebooks and the role-specific evidence;
3. unchanged hashes for all analytical notebooks, especially 10/11/12;
4. the 13 audit-record pre/post byte/hash matches;
5. audit index and all local navigation links;
6. data provenance/acquisition text and the retained tracked raw CSV;
7. unchanged `.gitignore` and absence of broad hiding rules;
8. external-reference source evidence, current-tree removal, and README distinction;
9. root README's narrow two-line path-only correction;
10. exact embedded Architect Specification comparison;
11. absence of Stage 6.2 content, analytical changes, Notebook execution, staging, commit, push, or remote creation;
12. final Git change scope and high-level tree.

## 22. Reviewer Result

_Reserved for independent Reviewer. Codex has not prewritten PASS, PASS WITH FIXES, or FAIL._

## 23. Immutable Architect Specification

================ BEGIN AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.1 ================

# STAGE 6.1 — REPOSITORY / GITHUB CLEANUP

## 0. STAGE CONTEXT

当前项目宏观结构：

- Stage 0 Audit — CLOSED
- Stage 1 Foundation — CLOSED
- Stage 2 Luca Audit / Data Understanding — CLOSED
- Stage 3 Data Preparation / EDA / Feature Engineering — CLOSED
- Stage 4 WOE / Logistic Primary Model — CLOSED
- Stage 5 XGBoost Challenger + Final Holdout Comparison — CLOSED
- Stage 6 Portfolio / Documentation / Resume / Interview Packaging — CURRENT

当前 Task：

Stage 6.1 — Repository / GitHub Cleanup

后续计划：

- Stage 6.2 Final README / Executive Summary
- Stage 6.3 Portfolio Visuals / Model Documentation
- Stage 6.4 Resume / Interview Packaging

本 Task 不得开始 Stage 6.2–6.4 的实质工作。

Stage 6.1 的目标不是继续提高模型表现，而是：

> 将已经完成的开发型 repository 整理为一个更适合公开 GitHub portfolio 展示的 credit-risk modeling repository，同时完整保留 Stage 0–5 的 analytical integrity、Git history 和 audit trail。

\===============================================================================

1. CURRENT FROZEN PROJECT STATE
   \===============================================================================

Local project:

`/Users/yu/2.工作/Risk Analysis Project`

Expected branch:

`main`

Expected current HEAD:

`68c80e07ab780b7c1b4ceff03876b534dc3ca954`

Short hash:

`68c80e0`

Expected commit message:

`Complete XGBoost challenger comparison`

Expected parent:

`1a413c2d4d871541d73c75589e5b3715b9c09b3e`

Stage 5 is fully CLOSED.

Expected working tree at Stage 6.1 start:

clean

Expected remote:

none

Codex must independently verify this pre-flight state.

If branch / HEAD / working tree differs materially and the difference is unexplained:

return BLOCKED.

Do not silently repair, reset, checkout, stash, discard, amend, rebase, or modify history.

\===============================================================================
2\. STAGE 0–5 ANALYTICAL FREEZE
===============================

Everything analytical from Stage 0–5 is frozen.

Stage 6.1 MUST NOT:

- redo the outer Train/Holdout split；
- inspect Holdout for new model-development purposes；
- create a new Holdout；
- alter Stage 3 features；
- alter categorical mappings；
- alter utilization definitions；
- alter WOE bins；
- alter IV calculations；
- alter variable screening；
- alter Logistic predictors；
- alter Logistic coefficients；
- remove PAY\_AMT5；
- alter XGBoost features；
- retune XGBoost；
- alter XGBoost hyperparameters；
- alter `best_round`；
- add a new model family；
- add SHAP as a model-selection step；
- add feature importance as a model-selection step；
- add calibration；
- optimize thresholds；
- add new predictive feature engineering；
- modify AUC / KS values；
- reinterpret or overwrite existing Reviewer findings；
- cosmetically rerun analytical notebooks if doing so changes outputs or execution state；
- change any Stage 0–5 analytical conclusion。

Stage 6.1 may inspect Notebook content read-only in order to curate repository presentation.

Existing Notebook analytical cells and outputs should remain unchanged unless a purely path/navigation issue makes an edit absolutely necessary.

Any such necessity must be reported and should normally be treated cautiously.

\===============================================================================
3\. PUBLIC PORTFOLIO DESIGN PRINCIPLE
=====================================

The repository should evolve from:

Development Repository

into:

Curated Credit Risk Portfolio Repository

without destroying the historical development record.

The repository should support two reading layers:

### Layer 1 — Recruiter / Hiring Manager

A person should be able to understand the project structure quickly and identify the important modeling notebooks without reading the entire development history.

### Layer 2 — Risk / Model Validation Reviewer

A technically interested reviewer should still be able to inspect:

- full analytical development history；
- Architect decisions；
- Reviewer records；
- leakage controls；
- Holdout freeze controls；
- tuning records；
- audit trail。

Therefore:

Public presentation should be curated through navigation and directory organization.

Do NOT destroy historical artifacts merely because they are not public-facing.

\===============================================================================
4\. SIX PUBLIC-FACING NOTEBOOKS
===============================

The final portfolio navigation must identify exactly SIX recommended public-facing notebooks.

Their conceptual reading order is frozen:

1. Data Preparation & EDA
2. Feature Engineering
3. WOE / Binning / Variable Screening
4. Final WOE Logistic Primary Model
5. XGBoost Challenger Development
6. Final Holdout Model Comparison

Codex must inspect all existing notebooks in:

`notebooks/`

and select the strongest existing notebook for each role.

The user should NOT be asked to manually select notebooks.

\===============================================================================
5\. NOTEBOOK SELECTION RULES
============================

For public-facing notebooks 4–6, preferred expected files are:

- `10_final_woe_logistic_freeze.ipynb`
- `11_xgboost_challenger_development.ipynb`
- `12_final_holdout_model_comparison.ipynb`

Use these unless actual repository inspection identifies a concrete integrity reason not to.

For public-facing notebooks 1–3:

Codex must inspect existing Stage 3–4 notebooks and determine the strongest candidate based on actual content.

Selection criteria:

- strongest contribution to the final modeling narrative；
- relevance to the final frozen pipeline；
- clear outputs；
- low redundancy；
- consistency with subsequent stages；
- usefulness to a recruiter；
- usefulness to a credit-risk / bank-risk interviewer；
- ability to explain why later modeling decisions were made。

Do NOT select a notebook simply because its filename is earlier.

Do NOT create new analytical notebooks merely to create a cleaner six-part sequence.

Do NOT duplicate notebook contents.

Do NOT rename historical notebook files into artificial public numbering such as:

`01_...`
`02_...`
...
`06_...`

if those filenames are not already their historical names.

Historical paths should remain stable.

\===============================================================================
6\. PRESERVE ALL HISTORICAL NOTEBOOKS
=====================================

The six notebooks are:

Recommended Reading

They are NOT:

the only notebooks allowed to remain in the repo.

Do NOT delete other Stage 0–5 notebooks merely because they are not recommended reading.

Do NOT merge historical notebooks.

Do NOT rewrite them into a single polished notebook.

Do NOT reorder history by changing old filenames.

The complete analytical history should remain available.

\===============================================================================
7\. CREATE `notebooks/README.md`
================================

Create:

`notebooks/README.md`

Purpose:

Curated notebook navigation only.

It must contain at minimum:

### A. Short context

Explain briefly that:

- the repository preserves the complete development notebook history；
- six notebooks are highlighted as the recommended portfolio reading path；
- supporting notebooks remain for transparency and reproducibility。

### B. Recommended Reading

List exactly the selected six notebooks in this conceptual order:

1. Data Preparation & EDA
2. Feature Engineering
3. WOE / Binning / Variable Screening
4. Final WOE Logistic Primary Model
5. XGBoost Challenger Development
6. Final Holdout Model Comparison

For each:

- notebook display title；
- exact relative link；
- 1–3 concise sentences describing its role。

### C. Supporting notebooks

Briefly explain that other notebooks document intermediate analysis or development decisions.

Do not write a full project executive summary here.

Do not duplicate the future Stage 6.2 root README.

Do not turn this file into a long methodology document.

\===============================================================================
8\. TASK RECORDS → `docs/audit/`
================================

The current development audit records are expected under:

`task_records/`

For the final portfolio structure, these records should be moved down one level into:

`docs/audit/`

This is intended to remove internal project-management material from the repository root while preserving full auditability.

Preferred operation:

use `git mv`

rather than copy/delete.

Target concept:

`task_records/<existing records>`

becomes:

`docs/audit/<existing records>`

\===============================================================================
9\. AUDIT RECORD IMMUTABILITY
=============================

Existing Stage 0–5 Task Reports contain important immutable records including:

- Architect Specifications；
- Reviewer Results；
- model-development evidence；
- Holdout controls；
- prior repository paths as historical facts。

Therefore:

Do NOT rewrite the bodies of existing audit records simply because their directory moved.

Do NOT normalize them.

Do NOT rewrite historical paths that were true at the time.

Do NOT remove Architect / Reviewer content.

Do NOT remove old completion records.

Do NOT shorten files for presentation.

Audit files should remain byte-identical wherever feasible.

Codex should record pre-move and post-move SHA-256 hashes for moved audit records, or otherwise verify content integrity.

If content-preserving movement cannot be achieved:

return BLOCKED rather than silently rewriting the audit chain.

\===============================================================================
10\. AUDIT LINK / PATH VALIDATION
=================================

Before moving `task_records/`, inspect whether the move would break:

- active relative Markdown links；
- repository navigation；
- Notebook links；
- scripts；
- runtime imports；
- code paths；
- documentation paths required for reproducibility。

Historical plain-text mentions of:

`task_records/...`

inside immutable audit reports are NOT by themselves a reason to rewrite those reports.

They are historical records.

However, current live navigation outside immutable audit records should be updated where necessary.

If a meaningful runtime dependency directly requires `task_records/` at the old location:

STOP and return BLOCKED with evidence.

Do not invent a complicated compatibility layer without Architect approval.

\===============================================================================
11\. CREATE `docs/audit/README.md`
==================================

Create:

`docs/audit/README.md`

Purpose:

Provide a lightweight index for the detailed development and validation records.

This file should:

- explain that these files contain the complete project development / review audit trail；
- explain that most readers do not need them for the main portfolio narrative；
- organize or link the records by Stage where practical；
- preserve access for model-risk / technical reviewers。

Do not rewrite each audit file.

Do not summarize every parameter search.

Keep the index concise.

\===============================================================================
12\. DO NOT CREATE A FAKE PRODUCTION `src/` ARCHITECTURE
========================================================

Do NOT create directories merely to make the project look like an ML engineering repository.

Do NOT add empty or artificial:

- `src/`
- `pipelines/`
- `configs/`
- `models/`
- `tests/`
- `services/`
- `api/`

unless the current repository already has a real functional need for them.

This project is primarily:

a Credit Risk Model Development Case Study

not:

a production ML software system.

If reusable helper code already exists, leave it where currently justified unless a clear cleanup is necessary.

Do not perform broad code refactoring in Stage 6.1.

\===============================================================================
13\. DATA DIRECTORY
===================

Create or update:

`data/README.md`

The goal is to make the dataset provenance and reproducibility expectations clear.

At minimum document, based on verified existing project information:

- dataset name；
- source institution / source；
- expected local raw filename；
- expected raw dataset path；
- row count；
- column count；
- target name；
- basic instructions explaining how a future user should obtain/place the data。

Expected dataset:

UCI Default of Credit Card Clients

Expected raw data:

`data/raw/UCI_Credit_Card.csv`

Expected dimensions:

30,000 rows × 25 columns

Expected target:

`default.payment.next.month`

Do NOT invent licensing claims.

Do NOT claim redistribution rights unless verified.

Do NOT perform external research unless necessary and explicitly permitted by the execution environment.

Prefer information already frozen in the project.

\===============================================================================
14\. RAW DATA GIT HANDLING
==========================

The long-term public-facing preference is:

the repository should not rely on GitHub as the primary distribution mechanism for the raw dataset.

However, Stage 0–5 Git history is frozen.

Therefore Codex must first inspect:

- whether the raw CSV is currently tracked；
- whether notebooks require the existing local path；
- whether removing it from the current index would affect reproducibility；
- whether it already exists in historical commits。

HARD RULE:

Do NOT rewrite Git history to purge data.

Do NOT use filter-branch / filter-repo / BFG.

Do NOT destroy prior commits.

If raw data is already tracked in frozen historical commits:

- do not attempt to rewrite history；
- keep analytical reproducibility intact；
- document dataset acquisition in `data/README.md`；
- report the existing tracked-state issue in Completion Report；
- do not make a fake claim that the data has been removed from repository history。

If raw data is NOT tracked:

- ensure `.gitignore` appropriately excludes local raw data while preserving `data/README.md`；
- do not delete the user's local dataset。

If a simple current-HEAD untracking action would create material reproducibility or audit ambiguity:

do not do it without further Architect approval。

Stage 6.1 prioritizes frozen-history integrity over cosmetic data removal.

\===============================================================================
15\. `.gitignore`
=================

Inspect the current `.gitignore`.

Only make minimal, clearly justified cleanup changes.

Appropriate candidates may include:

- `.DS_Store`
- `.ipynb_checkpoints/`
- project `.venv/`
- local cache artifacts
- raw data if it is not historically tracked and safe to ignore

Do NOT:

- add broad wildcard rules that might hide important project artifacts；
- ignore notebooks；
- ignore audit records；
- ignore README files；
- ignore final portfolio assets；
- remove tracked analytical files through a broad ignore rule。

Any `.gitignore` change must be documented.

\===============================================================================
16\. REFERENCES / LUCA MATERIAL
===============================

Inspect:

`references/`

The Luca / external pedagogical notebook must NOT appear to be the user's final modeling pipeline.

The public presentation should make the distinction explicit:

External reference reviewed during project development

versus

User's final analytical pipeline

Create or update:

`references/README.md`

It should state concisely:

- what external reference material was used for；
- that it served as background / audit / pedagogical reference；
- that the final project architecture, feature decisions, leakage controls, WOE/Logistic pipeline, XGBoost selection, and Holdout evaluation are the project's own frozen workflow。

Do not attack or criticize the reference.

Do not overstate originality.

\===============================================================================
17\. EXTERNAL REFERENCE NOTEBOOK HANDLING
=========================================

Codex must inspect the actual reference files.

Preferred public-facing outcome:

avoid making a copied external notebook a prominent repository artifact.

If the external notebook is clearly a third-party copy AND its authoritative external source can be reliably identified from existing repository evidence:

Codex may remove that copied file from the CURRENT tree and retain only an appropriate citation / link in `references/README.md`.

But:

- no history rewriting；
- no deletion if the source identity cannot be verified；
- no invented URL；
- no invented attribution。

If there is ambiguity:

keep the reference file in `references/`

and clearly label it as external reference material.

Report the decision and rationale.

\===============================================================================
18\. ROOT REPOSITORY PRESENTATION
=================================

Stage 6.1 should make the repository root cleaner.

Expected high-level portfolio structure after Stage 6.1 should conceptually resemble:

Risk Analysis Project/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── README.md
│   └── ...
│
├── notebooks/
│   ├── README.md
│   ├── existing notebooks...
│
├── docs/
│   └── audit/
│       ├── README.md
│       └── Stage / Task records...
│
└── references/
├── README.md
└── optional verified reference material...

Do not force this exact tree if a small factual difference in the existing repository requires preservation.

Do not create empty decorative folders.

\===============================================================================
19\. ROOT `README.md` BOUNDARY
==============================

Stage 6.2 will handle the main portfolio README.

Therefore Stage 6.1 MUST NOT write the full root README.

Do NOT yet create:

- full Executive Summary；
- final business problem section；
- full methodology narrative；
- final model comparison table；
- resume-style accomplishments；
- interview talking points；
- lengthy model results explanation；
- portfolio screenshots / graphics narrative。

If an existing root README needs a tiny path/link correction because Stage 6.1 moves files, that narrow correction is allowed.

Otherwise leave substantive root README redesign for Stage 6.2.

\===============================================================================
20\. DO NOT CREATE NEW MODEL VISUALS
====================================

Stage 6.1 is directory/repository cleanup.

Do not create:

- new ROC charts；
- new KS charts；
- new feature importance plots；
- SHAP plots；
- model diagrams；
- performance dashboards；
- portfolio graphics。

Those belong to Stage 6.3 if approved later.

Existing Notebook visuals remain untouched.

\===============================================================================
21\. REQUIREMENTS / ENVIRONMENT
===============================

Do not change analytical dependencies unless necessary for repository integrity.

Expected important dependency already included:

`xgboost==2.1.4`

Do NOT:

- upgrade XGBoost；
- upgrade Python；
- upgrade pandas；
- upgrade NumPy；
- upgrade SciPy；
- upgrade scikit-learn；
- rebuild `.venv`；
- install presentation packages；
- add unnecessary dependencies。

`requirements.txt` should remain analytically equivalent to the Stage 5 accepted state unless a genuine Stage 6.1 repository issue requires a narrowly justified fix.

\===============================================================================
22\. FILE-INTEGRITY VERIFICATION
================================

Codex must verify that Stage 6.1 did not alter frozen analytical artifacts.

At minimum verify integrity for the final key notebooks:

- final WOE Logistic notebook；
- XGBoost Challenger notebook；
- Final Holdout Comparison notebook。

Prefer SHA-256 pre/post comparison.

If these files are not modified, their hashes should remain identical.

Also verify that audit files moved with `git mv` retain identical content where intended.

If an analytical notebook changes unexpectedly:

STOP and return BLOCKED.

Do not silently accept notebook drift.

\===============================================================================
23\. NOTEBOOK EXECUTION
=======================

Stage 6.1 does NOT require analytical notebooks to be rerun.

Do not rerun Notebook 10 / 11 / 12 merely because repository organization changed.

Only verify that:

- files still exist；
- links point correctly；
- no path move has broken required data references at the repository-structure level。

If a directory move would require rerunning analytical notebooks or modifying frozen outputs:

STOP and report the issue.

\===============================================================================
24\. STAGE 6.1 TASK REPORT
==========================

Create:

`docs/audit/Stage_6.1_Repository_and_GitHub_Cleanup.md`

This Task Report should document:

- objective；
- starting HEAD；
- starting working-tree state；
- repository structure before cleanup；
- six selected public-facing notebooks；
- selection rationale；
- files created；
- files moved；
- files removed from current tree, if any；
- audit relocation details；
- audit hash/integrity verification；
- data README decision；
- raw-data Git status；
- `.gitignore` changes；
- references cleanup decision；
- key analytical notebook pre/post hashes；
- link validation；
- final repository tree；
- limitations / deferred items；
- explicit confirmation Stage 6.2 was not started；
- git status；
- Reviewer handoff。

\===============================================================================
25\. PRESERVE THE ARCHITECT CONTRACT VERBATIM
=============================================

The final Codex execution prompt generated by Prompt Engineer must contain a clearly delimited authoritative Stage 6.1 Architect Specification block.

Codex must append that complete authoritative Architect Specification verbatim to:

`docs/audit/Stage_6.1_Repository_and_GitHub_Cleanup.md`

for auditability.

Use clear external delimiters such as:

`================ BEGIN AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.1 ================`

and

`================ END AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.1 ================`

The specification text inside the immutable block must not be:

- paraphrased；
- normalized；
- shortened；
- reformatted；
- grammar-corrected；
- rewritten。

Any Codex commentary or execution record must remain outside that immutable block.

\===============================================================================
26\. LINK VALIDATION
====================

After cleanup, validate all newly created navigation links at minimum:

- `notebooks/README.md` → six selected notebooks；
- `docs/audit/README.md` → audit records；
- `references/README.md` → any retained local references；
- `data/README.md` → any local relative paths it mentions。

Also inspect existing live links affected by `task_records/` relocation.

No broken link should knowingly be introduced by Stage 6.1.

Historical text references inside immutable audit files are exempt if they are not intended as active current links.

\===============================================================================
27\. NO UNNECESSARY CONTENT REWRITING
=====================================

Stage 6.1 is mostly structural.

Do not take the opportunity to rewrite:

- notebook prose；
- historical Task Reports；
- model conclusions；
- Stage 5 final interpretation；
- existing analysis；
- old Reviewer Results；
- commit history。

New prose should be limited mainly to:

- `notebooks/README.md`
- `docs/audit/README.md`
- `data/README.md`
- `references/README.md`
- Stage 6.1 Task Report
- narrowly necessary current navigation corrections

\===============================================================================
28\. GIT OPERATIONS DURING CODEX EXECUTION
==========================================

Codex may use read-only and normal working-tree Git commands needed to inspect and perform authorized moves.

Allowed examples:

- `git status`
- `git log`
- `git diff`
- `git ls-files`
- `git mv`

Do NOT:

- commit；
- push；
- pull；
- fetch；
- rebase；
- amend；
- reset；
- checkout old commits destructively；
- rewrite history；
- force anything；
- create remote；
- configure GitHub；
- publish repository。

Codex should leave the Stage 6.1 changes uncommitted for Reviewer.

\===============================================================================
29\. REVIEWER HANDOFF
=====================

After Codex completes Stage 6.1, it must provide a focused Reviewer handoff.

At minimum report:

- branch；
- starting HEAD；
- current HEAD；
- confirmation current HEAD has not changed；
- six selected recommended notebooks；
- rationale for the first three selections；
- confirmation 10/11/12 status；
- `notebooks/README.md` status；
- audit records moved and method used；
- audit content-integrity verification；
- `docs/audit/README.md` status；
- Stage 6.1 Task Report status；
- `data/README.md` status；
- raw CSV tracking status；
- `.gitignore` changes；
- references decision；
- `references/README.md` status；
- any reference file removed or retained；
- key analytical notebook SHA-256 pre/post comparison；
- link-validation result；
- final high-level repository tree；
- all modified / added / moved / deleted files；
- git status；
- confirmation no analytical model result changed；
- confirmation Stage 6.2 not started；
- confirmation no commit；
- confirmation no push。

\===============================================================================
30\. REVIEWER VALIDATION SCOPE
==============================

Reviewer should independently validate Stage 6.1.

Reviewer should check at minimum:

### Repository integrity

- correct starting Stage 5 commit；
- no analytical history modification；
- expected files only；
- no accidental notebook drift。

### Six-notebook curation

- exactly six recommended notebooks；
- sequence matches frozen six conceptual roles；
- selected notebooks genuinely correspond to those roles；
- links work；
- historical notebooks remain available。

### Audit preservation

- task records successfully relocated as authorized；
- no meaningful audit content lost；
- hashes/content preserved；
- Architect Specification present verbatim in Stage 6.1 Task Report。

### Data handling

- data provenance documented；
- no history rewrite；
- no local raw-data destruction；
- no false licensing claim。

### References

- external reference clearly distinguished from final pipeline；
- no fabricated attribution or source。

### Scope

- no Stage 6.2 full README work；
- no new modeling；
- no new model visuals；
- no model metrics changed。

\===============================================================================
31\. REVIEW / COMMIT WORKFLOW
=============================

Codex does NOT commit.

Reviewer independently validates.

If Reviewer returns:

FAIL

then:

- no commit；
- fix only the identified issues；
- re-review。

If Reviewer returns:

PASS WITH FIXES

then:

- no commit until required fixes are completed and focused re-review returns PASS。

If Reviewer returns final:

PASS

then create a local Stage 6.1 checkpoint commit in the same review round.

Preferred commit message:

`Curate repository for portfolio presentation`

After commit:

- verify HEAD；
- verify branch remains `main`；
- verify expected files only；
- verify working tree clean；
- no push。

\===============================================================================
32\. NO PUSH
============

There is currently no need to publish the repository.

Do not:

- create remote；
- connect GitHub；
- push；
- publish；
- create GitHub repo。

Actual GitHub publication is a later explicitly authorized action.

Stage 6.1 only prepares repository structure.

\===============================================================================
33\. FAILURE / BLOCKED CONDITIONS
=================================

Return BLOCKED rather than improvising if:

- starting HEAD does not match expected Stage 5 commit；
- unexplained pre-existing changes exist；
- moving `task_records/` would break a real runtime dependency；
- audit files cannot be moved without content corruption；
- a required historical Notebook would need analytical modification；
- six notebook roles cannot be reliably identified from actual content；
- existing repository structure materially conflicts with the frozen Stage 6.1 plan；
- raw-data handling would require Git history rewrite；
- reference source identity would need to be invented；
- link integrity cannot be preserved without altering frozen analytical artifacts；
- frozen Notebook hashes drift unexpectedly；
- any action would require re-opening Stage 5 modeling decisions。

BLOCKED report must explain:

- exact issue；
- affected files；
- why the current authorization is insufficient；
- safest options；
- current Git state。

Do not silently expand scope.

\===============================================================================
34\. COMPLETION REPORT
======================

If successful, Codex Completion Report must clearly state:

`READY FOR STAGE 6.1 REVIEWER`

and summarize:

### Status

- successful / blocked

### Pre-flight

- branch
- HEAD
- starting cleanliness

### Six Recommended Notebooks

For each:

- conceptual role
- exact filename
- reason selected

### Repository Changes

- files added
- files moved
- files removed
- files modified

### Notebook Navigation

- `notebooks/README.md`

### Audit

- old path
- new path
- method
- integrity verification
- `docs/audit/README.md`
- Stage 6.1 Task Report

### Data

- `data/README.md`
- current raw CSV tracking status
- `.gitignore` decision

### References

- `references/README.md`
- external notebook retained/removed
- rationale

### Frozen Artifact Integrity

- key notebook pre/post hashes
- analytical changes: none

### Link Validation

- PASS / issues

### Scope Boundary

- Stage 6.2 not started
- no new modeling
- no model metrics changed

### Git

- current branch
- current HEAD
- staged changes
- unstaged changes
- untracked files
- no commit
- no push

\===============================================================================
35\. STAGE 6.1 SUCCESS CONDITION
================================

Stage 6.1 execution is ready for Reviewer only when ALL of the following are true:

- Stage 5 frozen commit verified；
- no analytical result modified；
- exactly six recommended notebooks identified；
- notebook selection grounded in actual existing contents；
- historical notebooks preserved；
- `notebooks/README.md` created；
- audit records moved to `docs/audit/` if safe and authorized；
- audit contents preserved；
- `docs/audit/README.md` created；
- Stage 6.1 Task Report created；
- authoritative Stage 6.1 Architect Specification preserved verbatim；
- `data/README.md` created/updated；
- raw data handled without history rewriting or local destruction；
- `.gitignore` minimally cleaned if needed；
- `references/README.md` created/updated；
- external reference clearly separated from final pipeline；
- no fake production `src/` architecture created；
- no new modeling performed；
- no new visuals created；
- no Stage 6.2 substantive work begun；
- navigation links validated；
- frozen analytical notebooks verified unchanged；
- Git changes remain uncommitted；
- no push occurred；
- Reviewer has a complete handoff。

Do not begin Stage 6.2 inside this task.

================ END AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.1 ================

# REVIEWER RESULT

## Verdict

PASS

## Requirement Check

- The repository remains on `main` at the required Stage 5 starting commit `68c80e07ab780b7c1b4ceff03876b534dc3ca954`; no pre-review staging, remote, push, or history rewrite was present.
- Exactly six recommended notebooks are listed in the required conceptual order. Their links resolve with exact case, their actual headings and contents support the assigned roles, preferred Notebooks 10–12 are used, and all 13 historical analytical notebooks remain available.
- All 13 historical Task Reports were relocated from `task_records/` to `docs/audit/` without content loss. The new audit index links every relocated record and this Stage 6.1 report.
- `data/README.md` documents the verified dataset identity, local path, dimensions, target, acquisition/placement guidance, and the factual tracked-history limitation. The raw CSV remains present, tracked, and unchanged; no history rewrite or local raw-data destruction occurred.
- `references/README.md` clearly distinguishes the external Luca reference from the project's frozen workflow. Removing the ignored, never-tracked local copy from the current tree was permitted because its source identity was already established by existing project evidence; no attribution, URL, or license claim was invented.
- `.gitignore` and `requirements.txt` remain unchanged. The root README change is limited to the necessary current audit-path correction. No fake production directories, Stage 6.2 substantive README work, new visuals, analytical rerun, feature/model change, or metric change was introduced.
- The complete authoritative Stage 6.1 Architect Specification is preserved verbatim in this Task Report.

The Architect described `git mv` as the preferred relocation method, not a mandatory acceptance condition. The ordinary filesystem move is accepted because it kept the Codex staged diff empty as separately required, and independent byte comparison proves that every relocated audit body is identical to its tracked predecessor.

## Independent Verification

- Compared every relocated record directly with its corresponding `HEAD:task_records/...` blob: 13 of 13 exact byte matches; the reported sizes and SHA-256 values agree.
- Compared every analytical Notebook directly with its `HEAD:notebooks/...` blob: 13 of 13 exact byte matches. Key hashes remain Notebook 10 `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863`, Notebook 11 `b59f4905330f16a0aa744dc2f2d26cfd578911302bd6f6a8a61daac282cf3fc2`, and Notebook 12 `ae3d867e212cfa3561cfbac823c10d0debc7ad8aecc63fe3abdb65ab1ecab5a3`.
- Parsed all 13 Notebook files as valid JSON and inspected the selected notebooks' actual headings. Notebook 02 covers the frozen split and Train-only EDA; Notebook 03 implements frozen feature engineering and sanity checks; Notebook 07 integrates WOE/IV, correlation, VIF, and variable screening; Notebooks 10–12 cover the frozen Primary, Challenger development, and one-time Holdout comparison respectively.
- Confirmed exactly six numbered recommended-reading entries and validated all 24 local Markdown links across the affected live navigation files; no local link failure was found. No active `task_records/` dependency remains outside immutable historical audit bodies.
- Rechecked the raw CSV independently: 30,000 data rows, 25 columns, expected target header, SHA-256 `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1`, identical to the current `HEAD` blob and still tracked since the initial repository commit.
- Confirmed the external reference copy is absent from the current tree, still covered by the exact narrow `.gitignore` rule, was never tracked, and has an existing audited identity/hash record. Its Kaggle author/title/URL were already present in the accepted Stage 5 repository documentation.
- Compared the embedded Architect block byte-for-byte with the supplied authoritative source: exact match, 32,380 UTF-8 bytes, SHA-256 `1cd962ffe4ed7ad292c0fa768f6c6222268e998395cef8d13ae0c2ce4e2b79e5`.
- Confirmed the final high-level tree contains only the intended portfolio directories; `reports/figures/` contains only its existing `.gitkeep`, and no `src/`, `pipelines/`, `configs/`, `models/`, `tests/`, `services/`, or `api/` directory was added.

## Issues Found

None.

## Required Fixes

None.

## Architect Decision Needed

None.
