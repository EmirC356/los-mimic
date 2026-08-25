# los-mimic
# Post-Discharge Risk Prediction in Older Patients (MIMIC-IV)

Calibrated machine-learning models predicting 30-day post-discharge mortality and unplanned readmission in patients aged 70 and over, developed to support advance-care-planning triage at the point of discharge.

---

## Status

**Code and results are withheld pending publication.** This repository currently contains documentation only. The full pipeline, models and evaluation results will be released here alongside the manuscript.

This is deliberate: the analysis is being prepared for publication with my supervisor at Amsterdam UMC, and the code and findings are released together with the paper so that the published results and the code that produced them arrive as one reviewable artifact.

**No patient data will be published here at any point** — see Data availability below.

---

## The question

Clinicians deciding whether to initiate advance care planning at discharge need to know which older patients are at genuine short-term risk. Existing bedside instruments — the LACE index, the HOSPITAL score — are simple enough to compute by hand, and that simplicity costs accuracy. The question here is how much is recoverable from data the hospital already holds by the time a patient is discharged, and whether the resulting risk estimates are trustworthy enough to act on.

That second half is the harder one. A model that ranks patients well can still produce probabilities that are systematically wrong, and a probability is what a clinical conversation actually turns on.

## Approach

**Cohort.** Adults aged 70+ in MIMIC-IV v3.1, drawn from a source population of 534,227 hospital admissions. Two endpoints: 30-day post-discharge mortality, and 30-day unplanned readmission.

**Feature engineering.** 77 features built from raw relational EHR tables — demographics, comorbidity burden (Charlson), laboratory trajectories, medication exposure, prior utilisation, and index-admission characteristics. Every feature was specified to be computable at the moment of discharge, and reviewed for clinical justification rather than selected by an automated search. Processing used DuckDB and polars over roughly 2.5 GB of raw tables.

**Validation.** Grouped cross-validation at the patient level, so repeated admissions from the same patient cannot appear on both sides of a split. This distinction is not cosmetic: patients readmit, and a naive row-level split will report performance the model does not have.

**Calibration treated as a first-class result.** Discrimination and calibration are reported separately, with reliability curves and proper scoring rules alongside AUROC — on the argument that a triage tool's usefulness depends on whether a stated 15% risk means 15%.

**Baselines.** Both endpoints are benchmarked against the LACE index and the HOSPITAL score, computed on the same cohort under the same validation scheme, so the comparison is like-for-like rather than against numbers quoted from other papers.

**Robustness.** The analysis includes feature-reduction studies, class-imbalance sensitivity analyses, cohort-transfer experiments testing whether models hold on a different patient population, ablations, and a label-integrity caveat analysis examining where the outcome definitions themselves are fragile.

**Reporting.** Following TRIPOD+AI, with SHAP-based attribution for per-patient explanation.

## What will be released

On publication:

- the full `acp/` pipeline — cohort construction, feature engineering, modelling, evaluation
- baseline score implementations (LACE, HOSPITAL)
- the sensitivity, ablation and robustness analyses
- the feature dictionary, with definitions and clinical rationale for all 77 features
- pinned environment specification and instructions to reproduce end to end

Not released, ever: any patient-level data, any derived dataset, and any trained model artefact fitted on restricted data.

## Data availability

MIMIC-IV is a credentialed-access database distributed by PhysioNet under a data use agreement. **No data from it appears in this repository and none will.** Researchers wishing to reproduce this work must obtain access independently: create a PhysioNet account, complete the required human-subjects research training, and sign the MIMIC-IV data use agreement at physionet.org.

MIMIC-IV-ED is used for pre-admission emergency-department context under the same terms.

## Context

Developed during a machine-learning internship at Amsterdam UMC (VUmc campus), summer 2026, supervised by a clinician-researcher in the department. Manuscript in preparation.

## Contact

Emir Ceylan — Computer Science, Sabancı University
[emirceylan.com](https://emirceylan.com) · emir.ceylan@sabanciuniv.edu
