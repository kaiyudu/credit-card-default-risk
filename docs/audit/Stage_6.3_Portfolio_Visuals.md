# Stage 6.3 — Portfolio Visuals

## Task Identification

- Stage: `Stage 6.3 — Portfolio Visuals`
- Project root: `/Users/yu/2.工作/Risk Analysis Project`
- Execution status: implementation complete; awaiting independent Reviewer validation
- Authorized scope: create exactly two portfolio PNGs and this Task Report; minimally modify `README.md`

## Stage Objective

Strengthen the accepted Stage 6.2 portfolio README with exactly two presentation-quality visuals based exclusively on frozen project evidence: a Development/Holdout modeling workflow and a final same-sample Holdout ROC comparison.

## Pre-Execution State

- Repository root: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD: `5158b5f5b7466c512cd289c19f5c939d35130029`
- Parent: `7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`
- HEAD message: `Finalize portfolio README`
- Initial tracked diff: empty
- Initial staged diff: empty
- Initial untracked list: empty
- Git remote: none
- Stage 6.2 Reviewer result: `PASS`
- Stage 6.3 artifacts present before execution: none

## README Baseline

The accepted Stage 6.2 `README.md` was 7,135 bytes with SHA-256 `1c0b5025b6d7f9d04ba741f8280a3413dc0d89afc46c40635846d355496c0055`. Its existing section structure, facts, result table, business framing, modeling explanation, notebook path, limitations, and reproducibility prose were retained. The Stage 6.3 diff contains eight added lines and zero removed lines, solely for the two image embeds and concise captions.

## Protected-File Baseline

| Protected file | Bytes before | SHA-256 before | Required frozen hash matched |
|---|---:|---|---|
| `notebooks/10_final_woe_logistic_freeze.ipynb` | 268,129 | `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863` | Yes |
| `notebooks/11_xgboost_challenger_development.ipynb` | 2,949,131 | `b59f4905330f16a0aa744dc2f2d26cfd578911302bd6f6a8a61daac282cf3fc2` | Yes |
| `notebooks/12_final_holdout_model_comparison.ipynb` | 37,175 | `ae3d867e212cfa3561cfbac823c10d0debc7ad8aecc63fe3abdb65ab1ecab5a3` | Yes |
| `data/raw/UCI_Credit_Card.csv` | 2,862,995 | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` | Yes |

No protected Notebook was executed, cleared, modified, or resaved.

## Evidence Inspection and Figure-Generation Approach

The accepted Stage 6.2 README, Stage 5 audit record, frozen Stage 4.7 primary definition, Stage 5 challenger definition, and the saved code/evidence in Notebook 12 were inspected read-only. A temporary script outside the repository reconstructed the frozen split and both frozen prediction paths, generated the ROC coordinates only after the numerical gate passed, and created the two authorized PNGs. Predictions, fitted model objects, and code were not persisted. Temporary generation material was removed after generation.

The visuals were produced with a consistent restrained navy/blue/green/gold palette, white opaque background, high-DPI rendering, and no gradients, 3-D effects, model-selection labels, or unnecessary legend elements.

## Workflow Visual

- Output: `reports/figures/modeling_workflow.png`
- Dimensions: 2,000 × 1,200 pixels
- File size: 167,050 bytes
- SHA-256: `589b1a7b8b466176fcce48fa8c48c921133d8b9efbc43651bcccc5548b2b3a42`
- Structure: Raw Dataset (30,000) splits into Development (24,000) and sealed Holdout (6,000). EDA/feature development and model development appear only in the Development lane. Development produces WOE Logistic Primary and XGBoost Challenger roles. The reserved Holdout reconnects with those frozen roles only at Final Holdout Evaluation, where AUC and KS are compared.
- Development-only verification: no Holdout arrow enters EDA, feature development, or model development; the Holdout path terminates at the final evaluation junction.

## Frozen Holdout Numerical Reproduction

The raw CSV was read in the isolated temporary context and the frozen outer split was replayed with `random_state=42` and target stratification.

- Raw rows: 30,000
- Development rows: 24,000
- Holdout rows: 6,000
- Development ID SHA-256: `bcb37af63ae7005df78e6140837594b5d650d27148b46b5d51354430ad958857`
- Holdout ID SHA-256: `d6748249f9f9fb2855746e0ed980b5013047267104090a14aa8138a700fe6f0f`
- Same Holdout observations used for both frozen models: Yes
- Absolute tolerance fixed before comparison: `1e-9`

The Primary was reconstructed using the frozen ten-predictor feature definitions, bin assignments, WOE mappings, intercept, coefficients, and probability transformation. The Challenger was reconstructed using the frozen 26-conceptual-feature universe, four-column education encoding, 29-column order, parameters, `best_round=54`, seed, and thread setting. No alternative specification was fitted or selected.

| Metric | Reproduced | Frozen Stage 5 reference | Absolute difference | Gate |
|---|---:|---:|---:|---|
| Primary Holdout AUC | 0.7585352594737264 | 0.758535259474 | 2.736699755701011e-13 | PASS |
| Primary Holdout KS | 0.40518581386989444 | 0.405185813870 | 1.0558220964185239e-13 | PASS |
| Challenger Holdout AUC | 0.7792420051310491 | 0.779242005131 | 4.9182879990894435e-14 | PASS |
| Challenger Holdout KS | 0.4216555495010459 | 0.421655549501 | 4.590772206825022e-14 | PASS |

All four metrics passed before ROC coordinates were generated.

## ROC Visual

- Output: `reports/figures/final_holdout_roc_comparison.png`
- Dimensions: 2,000 × 1,200 pixels
- File size: 198,447 bytes
- SHA-256: `f71b5549884e060bc63a7eb7518f4ae9c6c53d437e261172801b20b88dd361a0`
- Prediction source: actual reproduced probabilities for the same 6,000-row frozen Holdout
- Content: both actual ROC curves, random-classifier diagonal, correctly labeled axes, model identities with Holdout AUC values, and restrained KS/difference context
- Boundary: no winner, best-model, production-readiness, causality, or statistical-significance claim was added

## Image-Quality Validation

Both images were rendered and visually inspected at original resolution. Checks passed for crisp text, controlled margins, professional aspect ratio, high-DPI/Retina suitability, readable normal README width, consistent visual language, correct labels, no clipped or overlapping text, and clear logical flow. Both PNGs are RGBA with alpha extrema `(255, 255)`, confirming fully opaque pixels and no transparency readability issue. File sizes are proportionate to the 2,000 × 1,200 resolution.

## README Integration and Link Validation

- ROC placement: immediately after the existing Final Holdout Results interpretation.
- ROC alt text: `Final Holdout ROC comparison for the frozen WOE Logistic primary and XGBoost challenger`
- Workflow placement: immediately after the existing Data and Validation Design explanation.
- Workflow alt text: `Development and sealed Holdout workflow through final model evaluation`
- Image paths: repository-relative and present on disk.
- Public README absolute `/Users/yu/...` paths: none.
- README substantive prose deletion or rewrite: none; the diff is insertion-only.
- Final README: 7,611 bytes; SHA-256 `e8d9bb11325a3c19f3cfbb0e4008f5707b4e407a4ebefac020a2205fb7d0f733`.

## Protected-File Post-Execution Integrity

| Protected file | SHA-256 after | Matches baseline |
|---|---|---|
| `notebooks/10_final_woe_logistic_freeze.ipynb` | `c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863` | Yes |
| `notebooks/11_xgboost_challenger_development.ipynb` | `b59f4905330f16a0aa744dc2f2d26cfd578911302bd6f6a8a61daac282cf3fc2` | Yes |
| `notebooks/12_final_holdout_model_comparison.ipynb` | `ae3d867e212cfa3561cfbac823c10d0debc7ad8aecc63fe3abdb65ab1ecab5a3` | Yes |
| `data/raw/UCI_Credit_Card.csv` | `a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1` | Yes |

## File Operations and Scope Confirmation

Modified:

- `README.md` — eight insertion-only lines for two image embeds and captions.

Created:

- `reports/figures/modeling_workflow.png`
- `reports/figures/final_holdout_roc_comparison.png`
- `docs/audit/Stage_6.3_Portfolio_Visuals.md`

Confirmed:

- exactly two visuals created; no third visual;
- no prediction, score, model, JSON, data, coefficient, or temporary code artifact persisted;
- no Model Documentation page created;
- no model development, tuning, selection, feature change, or new analytical conclusion;
- no Stage 6.4 work started;
- no protected artifact changed;
- no staging, commit, push, pull, fetch, reset, rebase, or remote change.

## Immutable-Block Verification

- Source standalone BEGIN markers: 1
- Source standalone END markers: 1
- Embedded standalone BEGIN markers: 1
- Embedded standalone END markers: 1
- Encoding: UTF-8
- Boundary: exact standalone BEGIN marker through exact standalone END marker, inclusive
- Following line terminator: excluded
- UTF-8 bytes: 32,055
- SHA-256: `ae2208000ca9d167bbf12cd6637b223b77d39e7efe5fe6880a8d6f4780db3f6d`
- Deterministic exact comparison: PASS

## Final Git State

Final verification is recorded after the immutable block below. Expected handoff state is branch `main`, unchanged HEAD `5158b5f5b7466c512cd289c19f5c939d35130029`, only `README.md` modified, exactly the two PNGs and this Task Report untracked, staged diff empty, and no remote.

## Reviewer Handoff

Reviewer result: **Pending independent review.** Codex does not prewrite PASS/FAIL.

## Immutable Architect Specification

================ BEGIN AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.3 ================

# STAGE 6.3 — PORTFOLIO VISUALS

## 0. STAGE CONTEXT

Project:

`Risk Analysis Project`

Project root:

`/Users/yu/2.工作/Risk Analysis Project`

Current macro status:

- Stage 0 — CLOSED
- Stage 1 — CLOSED
- Stage 2 — CLOSED
- Stage 3 — CLOSED
- Stage 4 — CLOSED
- Stage 5 — CLOSED
- Stage 6.1 — Repository / GitHub Cleanup — CLOSED
- Stage 6.2 — Final Portfolio README / Executive Summary — CLOSED
- Stage 6.3 — Portfolio Visuals — CURRENT
- Stage 6.4 — Resume / Interview Packaging — FUTURE

Stage 6.2 final checkpoint:

Commit:

`5158b5f5b7466c512cd289c19f5c939d35130029`

Short:

`5158b5f`

Commit message:

`Finalize portfolio README`

Parent:

`7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`

Expected branch:

`main`

Expected working tree:

clean

Expected remote:

none

Codex must independently verify the actual starting state.

If branch / HEAD / tracked state differs materially and cannot be safely explained:

return BLOCKED.

Do not reset, stash, checkout, clean, amend, rebase, discard, or rewrite history.

\===============================================================================

1. STAGE 6.3 OBJECTIVE
   \===============================================================================

Stage 6.2 already produced a sufficiently comprehensive portfolio README.

Stage 6.3 must NOT expand the written project narrative.

The purpose of Stage 6.3 is narrowly:

> visually strengthen the existing README using exactly two presentation-quality portfolio visuals based on already-frozen project evidence.

The two visuals are:

1. Development / Holdout Modeling Workflow
2. Final Holdout ROC Comparison

Stage 6.3 is therefore:

`visual communication enhancement`

NOT:

- new modeling；
- new feature engineering；
- new model interpretation；
- new hyperparameter analysis；
- new technical documentation；
- new portfolio prose；
- a new analytical stage。

\===============================================================================
2\. SCOPE — EXACTLY TWO VISUALS
===============================

Create exactly two portfolio visuals.

No third visual is authorized.

Do NOT additionally create:

- SHAP plots；
- XGBoost feature-importance plots；
- KS curves；
- calibration curves；
- lift charts；
- decile charts；
- WOE plots；
- coefficient plots；
- confusion matrices；
- score distributions；
- architecture banners；
- decorative GitHub headers；
- additional diagrams。

If existing Notebook figures already contain any of the above, they remain historical analytical artifacts and should not be promoted into new Stage 6.3 portfolio outputs unless separately authorized.

\===============================================================================
3\. VISUAL 1 — DEVELOPMENT / HOLDOUT MODELING WORKFLOW
======================================================

Purpose:

Allow a recruiter or technical reviewer to understand the project validation architecture within several seconds.

The central methodological message must be visually unambiguous:

> EDA, Feature Development, and Model Development occur only within the Development sample.

The Holdout sample must NOT appear to participate in:

- EDA；
- feature design；
- binning decisions；
- feature selection；
- model selection；
- hyperparameter tuning。

The Holdout branch should remain separate until final evaluation.

\===============================================================================
4\. REQUIRED WORKFLOW FACTS
===========================

The workflow must correctly represent:

Raw Dataset:

`30,000 customers`

followed by a Development / Holdout split:

Development:

`24,000`

Holdout:

`6,000`

Development branch should communicate, at high level:

`EDA & Feature Development`

then:

`Model Development`

then the two frozen model roles:

`WOE Logistic Primary`

and:

`XGBoost Challenger`

The Holdout branch should communicate that it is:

`Reserved for Final Evaluation`

or an equivalent concise phrase.

The Development model branch and Holdout branch should only reconnect at:

`Final Holdout Evaluation`

or an equivalent concise label.

Final evaluation should communicate:

`AUC / KS`

and/or final model comparison.

\===============================================================================
5\. WORKFLOW VISUAL — MODEL DETAIL BOUNDARY
===========================================

The two model boxes must remain deliberately simple.

Do NOT put detailed internal modeling procedures into the workflow figure.

In particular, do NOT clutter the model boxes with:

- binning；
- WOE formula；
- IV thresholds；
- variable-screening mechanics；
- Logistic coefficients；
- XGBoost parameters；
- exact grid sizes；
- early stopping；
- 1-SE rule；
- `best_round`；
- exact feature counts；
- exact feature names。

The workflow visual is about:

`development-validation architecture`

not:

`model internals`.

Model-detail explanation already exists in README and notebooks.

\===============================================================================
6\. WORKFLOW VISUAL — STRUCTURAL INTENT
=======================================

The intended conceptual structure is approximately:

Raw Dataset
→ Split

then two visibly separate branches:

Development
→ EDA & Feature Development
→ Model Development
→ WOE Logistic Primary / XGBoost Challenger

versus:

Holdout
→ Reserved for Final Evaluation

then reconnect:

Frozen Development Models + Holdout
→ Final Holdout Evaluation
→ Model Comparison

Codex may improve exact visual layout, typography, box shape, spacing, arrows, and wording.

The Architect freezes the logic, not the exact graphic design.

Critical visual requirement:

A viewer must NOT be able to interpret the figure as:

`Development + Holdout → Feature Development`

Feature Development must visually belong only to Development.

\===============================================================================
7\. WORKFLOW VISUAL — PRESENTATION STANDARD
===========================================

The workflow should be:

- clean；
- modern；
- professional；
- suitable for a GitHub portfolio；
- readable at normal README width；
- understandable without zooming；
- visually restrained；
- not excessively colorful；
- not decorative for its own sake。

Prefer:

- simple shapes；
- consistent typography；
- strong spacing；
- clear arrows；
- limited visual hierarchy。

Avoid:

- tiny text；
- excessive icons；
- gradients；
- 3D effects；
- dense annotations；
- unnecessary legends；
- overly stylized infographic design。

\===============================================================================
8\. VISUAL 2 — FINAL HOLDOUT ROC COMPARISON
===========================================

The second visual must be:

`Final Holdout ROC Comparison`

It should compare the two frozen final models on the SAME final Holdout sample:

- WOE Logistic Primary
- XGBoost Challenger

The curve must be computed from actual frozen-model Holdout predictions.

It must NOT be approximated, invented, reconstructed geometrically from AUC alone, or visually fabricated.

\===============================================================================
9\. ROC SOURCE-OF-TRUTH
=======================

The primary source of truth for final evaluation is the accepted Stage 5 artifact, especially:

`notebooks/12_final_holdout_model_comparison.ipynb`

and the accepted Stage 5 Task Report.

Codex must inspect the actual current repository and identify the exact accepted frozen implementation before reproducing predictions.

Expected final Holdout reference results:

WOE Logistic Primary:

AUC:

`0.758535259474`

KS:

`0.405185813870`

XGBoost Challenger:

AUC:

`0.779242005131`

KS:

`0.421655549501`

Observed deltas:

AUC:

`+0.020706745657`

KS:

`+0.016469735631`

The Stage 6.3 ROC reproduction must agree with the frozen Stage 5 results to tight numerical tolerance.

If it does not:

STOP.

Do not plot a mismatching curve.

Investigate first.

If the discrepancy cannot be reconciled from accepted frozen artifacts:

return BLOCKED.

\===============================================================================
10\. ROC REPRODUCTION IS NOT MODEL REDEVELOPMENT
================================================

Stage 6.3 is authorized to reproduce the already-frozen final Holdout scores strictly for visualization.

This does NOT authorize:

- feature changes；
- re-binning；
- variable reselection；
- XGBoost retuning；
- changing XGBoost parameters；
- changing `best_round`；
- changing model coefficients；
- changing preprocessing；
- class weighting；
- resampling；
- calibration；
- threshold optimization；
- selecting a different random seed；
- selecting a different Holdout；
- any post-Holdout model decision。

The purpose is only:

> faithfully reproduce frozen predictions and convert them into a presentation-quality ROC figure.

\===============================================================================
11\. FROZEN PRIMARY MODEL
=========================

Expected frozen Primary:

`Final 10-Variable WOE Logistic`

Expected predictor count:

`10`

Expected final Primary Holdout:

AUC:

`0.758535259474`

KS:

`0.405185813870`

Codex should use the accepted frozen Stage 4.7 / Stage 5 implementation.

Do not refit an alternative Logistic model.

Do not create a reduced model.

Do not alter WOE definitions.

\===============================================================================
12\. FROZEN CHALLENGER MODEL
============================

Expected frozen Challenger:

XGBoost

Expected conceptual predictor count:

`26`

Expected encoded matrix width:

`29`

Expected frozen selected specification includes:

- `max_depth = 4`
- `min_child_weight = 1`
- `subsample = 0.70`
- `colsample_bytree = 0.70`
- `eta = 0.10`
- `best_round = 54`
- `tree_method = hist`
- `reg_alpha = 0`
- `reg_lambda = 1`
- `gamma = 0`
- `scale_pos_weight = 1`
- `seed = 42`
- `nthread = 4`

These values are factual reproduction controls.

They do NOT all need to appear in the visual.

Expected final Challenger Holdout:

AUC:

`0.779242005131`

KS:

`0.421655549501`

Do not retune.

\===============================================================================
13\. ROC FIGURE CONTENT
=======================

The final ROC visual should include:

- ROC curve for WOE Logistic Primary；
- ROC curve for XGBoost Challenger；
- random-classifier diagonal reference；
- clear x-axis:
  `False Positive Rate`
- clear y-axis:
  `True Positive Rate`
- title clearly indicating:
  `Final Holdout`
  and
  `ROC`
- readable legend with model identity and Holdout AUC。

The figure should also communicate the final KS values without creating a separate KS plot.

Acceptable approaches include:

- concise annotation；
- small subtitle；
- compact footer；
- caption in README。

Avoid overloading the plot itself.

\===============================================================================
14\. ROC INTERPRETATION BOUNDARY
================================

The ROC figure may visually support the factual conclusion:

XGBoost shows stronger Holdout discrimination than the WOE Logistic Primary.

Do NOT label:

- `Winner`；
- `Best Model`；
- `Superior Model`；
- `Production Model`。

Do NOT imply:

- statistical significance；
- causal superiority；
- universal superiority；
- production readiness。

The interpretability / complexity tradeoff remains part of the overall README narrative.

\===============================================================================
15\. DO NOT CREATE A KS CURVE
=============================

KS remains an important reported metric.

However, Stage 6.3 does NOT need a third visual.

Therefore:

- report final KS values textually；
- do not create a separate KS curve；
- do not create a combined multi-panel ROC + KS dashboard unless that effectively creates additional visual complexity beyond the authorized design。

The second portfolio visual should remain primarily:

`Final Holdout ROC Comparison`.

\===============================================================================
16\. README BASELINE
====================

Stage 6.2 already produced the final portfolio landing page.

Do NOT rewrite it.

Do NOT substantially expand it.

Do NOT redesign the project narrative.

Stage 6.3 may modify:

`README.md`

only as needed to:

- embed the two visuals；
- add minimal figure headings / captions / alt text；
- make minimal spacing or section-placement adjustments required for readability。

Existing README prose, facts, result tables, business framing, modeling explanation, Repository Guide, and Scope & Limitations should remain substantively unchanged unless a genuine factual defect is discovered.

If a factual defect unrelated to Stage 6.3 is discovered:

do not silently rewrite the README.

Return it for Architect attention unless the correction is trivial and directly necessary to integrate the visual.

\===============================================================================
17\. README VISUAL PLACEMENT
============================

Codex should inspect the actual Stage 6.2 README before deciding exact placement.

Preferred conceptual placement:

### Final Holdout ROC Comparison

Near:

`Final Results`

because it visualizes the final model-performance comparison.

### Development / Holdout Modeling Workflow

Near:

`Dataset & Validation Design`

and/or:

`Modeling Approach`

because it explains the development architecture.

Codex may optimize exact placement based on the current README structure.

Do NOT create a giant separate:

`Gallery`

section.

The visuals should strengthen the existing reading flow.

\===============================================================================
18\. NO NEW MODEL DOCUMENTATION
===============================

Stage 6.3 must NOT create a new general model-documentation page.

Do NOT create:

- `docs/model_documentation.md`
- model cards
- new methodology reports
- technical walkthrough documents
- interview guides

Existing project documentation is already sufficient:

README
→ core notebooks
→ audit reports

Stage 6.3 adds visuals only.

\===============================================================================
19\. OUTPUT LOCATION
====================

Use the existing portfolio figure directory:

`reports/figures/`

Expected visual outputs:

`reports/figures/modeling_workflow.png`

`reports/figures/final_holdout_roc_comparison.png`

Codex may use a different concise descriptive filename only if there is a strong repository-consistency reason.

If filenames differ, document the reason in the Task Report.

Do not scatter images across the repository.

Do not put image files in the project root.

\===============================================================================
20\. IMAGE QUALITY
==================

Both images must be suitable for GitHub README display.

Requirements:

- crisp text；
- sufficient resolution；
- no clipped labels；
- no overlapping labels；
- no excessive white margins；
- readable when rendered at normal GitHub width；
- professional aspect ratio；
- consistent visual style across both figures。

Raster output should be sufficiently high resolution for Retina/high-DPI viewing.

Do not create enormous image files without visual benefit.

\===============================================================================
21\. ACCESSIBILITY / README MARKDOWN
====================================

README embeds should use meaningful alt text.

Examples of intent:

- modeling workflow showing Development-only feature/model development and reserved Holdout；
- final Holdout ROC comparison between WOE Logistic and XGBoost。

Do not use alt text such as:

`image`

or:

`chart`.

\===============================================================================
22\. FIGURE-GENERATION IMPLEMENTATION
=====================================

Codex may choose the safest reproducible local method to generate the two figures.

The Architect does NOT prescribe a plotting library.

The preferred principle is:

> use existing project dependencies where practical and do not change the analytical environment unnecessarily.

Do NOT install packages unless a genuine blocker exists.

Do NOT modify:

`requirements.txt`

merely for presentation convenience.

If temporary generation code is needed, prefer running it outside the tracked repository or otherwise avoid creating unnecessary persistent engineering structure.

Do not create:

- `src/`
- `tests/`
- a new application package
- a visualization framework

solely for these two images.

Stage 6.3 should remain small.

\===============================================================================
23\. ROC REPRODUCTION PROCEDURE
===============================

Codex must not infer a curve from summary metrics.

The safe process is:

1. inspect the frozen final Holdout Notebook and Stage 5 evidence；
2. identify the exact frozen split, feature transformations, WOE Primary, Challenger preprocessing, and Challenger specification；
3. reproduce the exact same 6,000 Holdout predictions in an isolated / temporary execution context；
4. recompute AUC and KS；
5. compare reproduced metrics against accepted Stage 5 references；
6. only if reproduction passes, compute ROC coordinates；
7. generate the ROC figure；
8. do not write prediction CSVs or new model artifacts unless explicitly necessary, which is not expected。

Do not modify or resave Notebook 12.

\===============================================================================
24\. NO PREDICTION / MODEL ARTIFACT EXPANSION
=============================================

Do not persist new:

- prediction CSV；
- score CSV；
- pickle；
- joblib；
- JSON model artifact；
- XGBoost model file；
- WOE mapping export；
- coefficient export；
- temporary datasets。

Only the two final portfolio image files should remain as new presentation artifacts, apart from the Stage 6.3 Task Report.

\===============================================================================
25\. ANALYTICAL NOTEBOOK PROTECTION
===================================

Do NOT modify any existing analytical Notebook.

At minimum, verify pre/post SHA-256 for:

`notebooks/10_final_woe_logistic_freeze.ipynb`

`notebooks/11_xgboost_challenger_development.ipynb`

`notebooks/12_final_holdout_model_comparison.ipynb`

Expected Stage 6.2-verified hashes:

Notebook 10:

`c7879f33b12307885ce7918627efc31a3c1db8f93628abe90b279fa4d7bd5863`

Notebook 11:

`b59f4905330f16a0aa744dc2f2d26cfd578911302bd6f6a8a61daac282cf3fc2`

Notebook 12:

`ae3d867e212cfa3561cfbac823c10d0debc7ad8aecc63fe3abdb65ab1ecab5a3`

If any protected Notebook changes unexpectedly:

STOP and return BLOCKED.

Do not overwrite it with a newly executed copy.

\===============================================================================
26\. DATA PROTECTION
====================

Do not modify:

`data/raw/UCI_Credit_Card.csv`

Expected SHA-256:

`a0f0ab49d6326671d6cd83be5c88dcf18007025fe9a53ecd699119c871176ca1`

The dataset may be read to reproduce frozen predictions and generate ROC coordinates.

It must not be rewritten, sorted, cleaned, transformed in place, or replaced.

\===============================================================================
27\. NO NEW ANALYTICAL CLAIMS
=============================

Stage 6.3 does not authorize new findings.

Do not add conclusions such as:

- one feature is more important than another；
- one subgroup is riskier；
- model calibration is good/bad；
- a specific cutoff is optimal；
- XGBoost gains come from a particular variable；
- one model is statistically significantly better；
- a particular business policy should be adopted。

Only visualize already-accepted project conclusions.

\===============================================================================
28\. FILE-SCOPE EXPECTATION
===========================

Expected Stage 6.3 substantive file changes:

MODIFY:

`README.md`

CREATE:

`reports/figures/modeling_workflow.png`

CREATE:

`reports/figures/final_holdout_roc_comparison.png`

CREATE:

`docs/audit/Stage_6.3_Portfolio_Visuals.md`

No other persistent file change is expected.

If Codex believes another persistent file is genuinely required:

STOP and evaluate whether it is necessary.

If not necessary, do not create it.

Any additional persistent file must be explicitly justified in the Task Report and Reviewer handoff.

\===============================================================================
29\. STAGE 6.3 TASK REPORT
==========================

Create:

`docs/audit/Stage_6.3_Portfolio_Visuals.md`

The Task Report should record:

- Stage objective；
- starting branch / HEAD；
- starting cleanliness；
- README baseline；
- figure-generation approach；
- Workflow visual structure；
- evidence used for Workflow facts；
- ROC reproduction method；
- frozen Primary verification；
- frozen Challenger verification；
- Holdout identity；
- reproduced AUC / KS；
- differences from frozen references；
- ROC-generation result；
- figure paths；
- image dimensions / file sizes；
- README placement；
- alt text；
- local Markdown/image-link validation；
- protected Notebook pre/post hashes；
- raw CSV integrity；
- confirmation no new modeling occurred；
- confirmation no new model selection occurred；
- confirmation no new analytical conclusions；
- confirmation no Model Documentation was created；
- confirmation no Stage 6.4 work occurred；
- files modified / created；
- final Git status；
- Reviewer handoff。

\===============================================================================
30\. IMMUTABLE ARCHITECT SPECIFICATION
======================================

The final Codex execution prompt generated by Prompt Engineer must include this complete Stage 6.3 Architect Specification in a clearly delimited authoritative block.

Codex must append the complete authoritative Stage 6.3 Architect Specification verbatim to:

`docs/audit/Stage_6.3_Portfolio_Visuals.md`

Use clear standalone BEGIN / END markers.

The authoritative text inside the block must not be:

- paraphrased；
- shortened；
- normalized；
- grammar-corrected；
- reformatted；
- rewritten。

Execution notes remain outside the immutable specification block.

\===============================================================================
31\. README LINK / IMAGE VALIDATION
===================================

After integration, validate:

- Workflow image path；
- ROC image path；
- all local image references；
- all existing local Markdown links affected by edits；
- no broken image；
- no accidental absolute local filesystem paths in public README。

README must use repository-relative paths.

Do not expose:

`/Users/yu/...`

inside the public README.

\===============================================================================
32\. VISUAL REVIEW CHECK — WORKFLOW
===================================

Before completion, Codex must explicitly verify that the Workflow figure cannot reasonably be read as:

Holdout
→ Feature Development

or:

Holdout
→ Model Development

The Holdout should visibly bypass all development activities.

The reconnection point must be final evaluation.

This is a hard acceptance requirement.

\===============================================================================
33\. VISUAL REVIEW CHECK — ROC
==============================

Before completion, verify:

- both curves represent exactly the same 6,000 Holdout observations；
- WOE Logistic AUC matches frozen reference；
- XGBoost AUC matches frozen reference；
- KS values match frozen reference；
- random baseline is correct；
- axes are correct；
- legend identifies models correctly；
- no model labels are swapped；
- no Development/CV predictions are accidentally plotted；
- no threshold-specific result is presented as globally optimal。

\===============================================================================
34\. README CONTENT BOUNDARY
============================

The existing Stage 6.2 README should remain concise.

Do NOT use Stage 6.3 to add long explanations around the figures.

Figure captions should be short.

The reader should obtain:

Workflow visual:
`how the project was developed and validated`

ROC visual:
`how the frozen models compare on final unseen Holdout data`

The existing prose remains responsible for deeper interpretation.

\===============================================================================
35\. NO WEB RESEARCH
====================

No external research is required.

Do not browse for:

- ROC design examples；
- credit-risk benchmarks；
- industry model-performance thresholds；
- visual templates；
- external GitHub projects；
- regulatory guidance。

Use existing project evidence only.

\===============================================================================
36\. GIT OPERATIONS
===================

Codex may inspect Git and create authorized working-tree changes.

Codex must NOT:

- git add；
- commit；
- push；
- pull；
- fetch；
- rebase；
- amend；
- reset；
- restore protected files；
- rewrite history；
- configure remote。

Leave Stage 6.3 changes uncommitted for Reviewer.

\===============================================================================
37\. BLOCKED CONDITIONS
=======================

Return BLOCKED rather than improvising if:

- starting HEAD is not the accepted Stage 6.2 checkpoint and the difference is unexplained；
- working tree contains unexplained changes；
- protected Notebook hashes differ from expected frozen artifacts；
- raw CSV identity differs；
- frozen Stage 5 Holdout metrics cannot be reproduced；
- ROC predictions cannot be faithfully reconstructed from frozen project evidence；
- producing the ROC would require changing a frozen model；
- producing the ROC would require new model selection；
- the actual six-thousand-row Holdout cannot be verified；
- README integration would require major prose redesign；
- image generation would require substantial new dependency/environment changes；
- a requested figure would require a new analytical conclusion。

Do NOT fabricate a ROC curve.

Do NOT substitute CV or Development ROC if Holdout ROC reproduction fails.

\===============================================================================
38\. REVIEWER HANDOFF
=====================

After successful execution, provide a focused Stage 6.3 Reviewer handoff containing:

- branch；
- starting HEAD；
- current HEAD；
- confirmation HEAD unchanged；
- README path；
- Task Report path；
- Workflow figure path；
- ROC figure path；
- image dimensions / sizes；
- Workflow factual structure；
- ROC reproduction procedure；
- reproduced Primary AUC / KS；
- reproduced Challenger AUC / KS；
- differences versus frozen Stage 5 references；
- Holdout row count；
- protected Notebook pre/post hashes；
- CSV hash；
- README image/link validation；
- all changed / created files；
- confirmation exactly two portfolio visuals created；
- confirmation no third visual；
- confirmation no model documentation；
- confirmation no model redevelopment；
- confirmation no feature changes；
- confirmation no tuning；
- confirmation no new analytical conclusions；
- confirmation no Stage 6.4；
- Git status；
- no commit；
- no push。

\===============================================================================
39\. REVIEWER VALIDATION SCOPE
==============================

Reviewer should independently validate:

### Git / scope

- correct Stage 6.2 starting checkpoint；
- expected working-tree scope；
- no unauthorized files；
- no Notebook drift；
- no data drift。

### Workflow figure

- Development / Holdout clearly split；
- Feature Development only under Development；
- Model Development only under Development；
- Holdout reserved until final evaluation；
- model boxes remain high-level；
- no unnecessary model-internal detail；
- professional readability。

### ROC figure

- based on final Holdout；
- same 6,000 rows for both models；
- frozen models only；
- correct ROC calculation；
- AUC values reproduce accepted Stage 5 results；
- KS values correct；
- curves / labels not swapped；
- no fabricated coordinates；
- presentation quality acceptable。

### README

- exactly two new portfolio visuals integrated；
- placement improves rather than disrupts reading flow；
- existing Stage 6.2 narrative remains substantively intact；
- no excessive new prose；
- paths work；
- alt text useful。

### Boundary

- no new model documentation；
- no SHAP；
- no feature importance；
- no KS curve；
- no calibration；
- no threshold optimization；
- no Stage 6.4 work。

\===============================================================================
40\. REVIEW / COMMIT WORKFLOW
=============================

Codex does NOT commit.

Reviewer independently validates Stage 6.3.

If Reviewer returns:

FAIL

then:

- no commit；
- apply only approved fixes；
- re-review。

If Reviewer returns:

PASS WITH FIXES

then:

- no commit until focused re-review returns final PASS。

If Reviewer returns final:

PASS

then create a local Stage 6.3 checkpoint commit in the same review round.

Preferred commit message:

`Add portfolio visuals`

After commit verify:

- branch remains `main`；
- expected files only；
- working tree clean；
- no push。

\===============================================================================
41\. STAGE 6.3 SUCCESS CONDITION
================================

Stage 6.3 is ready for Reviewer only when ALL are true:

- Stage 6.2 checkpoint verified；
- exactly two new portfolio visuals exist；
- Workflow visual is accurate；
- Feature Development is visually Development-only；
- Holdout is visibly reserved until final evaluation；
- model boxes remain simple；
- ROC uses actual frozen final Holdout predictions；
- Primary AUC / KS reproduce Stage 5；
- Challenger AUC / KS reproduce Stage 5；
- no curve fabrication；
- visuals are presentation quality；
- README embeds both correctly；
- README remains concise；
- no major README prose rewrite；
- no third visual；
- no model documentation；
- no analytical Notebook modification；
- no data modification；
- no model redevelopment；
- no feature changes；
- no tuning；
- no new analytical conclusions；
- no Stage 6.4 work；
- Task Report complete；
- Architect Specification preserved verbatim；
- links and images validated；
- changes remain uncommitted；
- no push occurred。

\===============================================================================
42\. COMPLETION REPORT
======================

If successful, Codex should return:

`READY FOR STAGE 6.3 REVIEWER`

with a concise Completion Report containing:

### Pre-flight

- branch
- HEAD
- cleanliness

### Workflow Visual

- output path
- dimensions
- structure
- Development-only development confirmation

### ROC Visual

- output path
- dimensions
- Holdout row count
- reproduction method
- Primary AUC / KS
- Challenger AUC / KS
- numerical comparison against frozen Stage 5 references

### README

- where Workflow was embedded
- where ROC was embedded
- alt text
- link/image validation

### Integrity

- Notebook 10 pre/post hash
- Notebook 11 pre/post hash
- Notebook 12 pre/post hash
- raw CSV hash

### Scope

- exactly two visuals
- no third visual
- no Model Documentation
- no new modeling
- no tuning
- no feature changes
- no new conclusions
- no Stage 6.4

### Files

- modified
- created
- any deviations

### Git

- branch
- HEAD
- status
- no commit
- no push

================ END AUTHORITATIVE ARCHITECT SPECIFICATION — STAGE 6.3 ================

## Post-Implementation Final Verification

- Repository root: `/Users/yu/2.工作/Risk Analysis Project`
- Branch: `main`
- HEAD: `5158b5f5b7466c512cd289c19f5c939d35130029`
- Parent: `7e34a0f5f99556ac6b9e674ac6be6e3f8d2722ec`
- HEAD message: `Finalize portfolio README`
- Tracked diff: only `README.md`, with eight additions and zero deletions
- Staged diff: empty
- Untracked files: exactly the two authorized PNGs and this Task Report
- Remote: none
- Protected Notebook and raw CSV hashes: unchanged from baseline
- Immutable source/report block comparison: exact, 32,055 UTF-8 bytes, SHA-256 `ae2208000ca9d167bbf12cd6637b223b77d39e7efe5fe6880a8d6f4780db3f6d`
- README image references: both repository-relative paths resolve to readable 2,000 × 1,200 PNGs
- Temporary generation material: absent
- Commit/push: none
- Reviewer status: pending independent validation
