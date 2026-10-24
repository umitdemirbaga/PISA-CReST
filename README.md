# PISA-CReST

Open-source code and reproducibility materials for **Creative-thinking Representation via a Self-supervised Transformer (CReST)**, an assessment-design-aware, self-supervised multi-view Transformer framework for examining hidden-item transfer and domain-relative predictive information in PISA 2022 Creative Thinking data across education systems.

The final experiments reported in the associated manuscript were implemented in **Python 3.11.10** using **PyTorch 2.9.0+cpu** and were conducted entirely in a **CPU-only computing environment**.

## Associated Manuscript

**Beyond Overall Proficiency: Assessment-Design-Aware Domain-Relative Prediction in PISA 2022 Creative Thinking**

**Authors:** Kubra Kirca Demirbaga and Umit Demirbaga

The study analyses PISA 2022 Creative Thinking data from **142,564 students across 63 education systems** and uses **32 scored Creative Thinking items** spanning Written, Visual, Social, and Scientific domains.

CReST learns representations from multiple assessment data views, using a design-calibrated general representation and separate contextual and domain-specific components. The framework aims to reduce recoverable assessment-design information while preserving predictive information for hidden-item transfer. These controls do not establish complete design invariance or measurement invariance across education systems.

## Repository Structure

```text
PISA-CReST/
│
├── notebooks/
│   ├── main/
│   │   ├── 01_data_preparation_and_design_audit.ipynb
│   │   ├── 04_final_architecture_lock.ipynb
│   │   ├── 05_final_cross_system_training.ipynb
│   │   ├── 06_final_cross_system_evaluation.ipynb
│   │   ├── 07_contextual_robustness_and_subgroup_analysis.ipynb
│   │   ├── 08_survey_weighted_population_analysis.ipynb
│   │   ├── 09_domain_profiles_beyond_overall_proficiency.ipynb
│   │   ├── 10_cross_system_profile_stability.ipynb
│   │   ├── 11_final_sensitivity_and_robustness.ipynb
│   │   ├── 13_computational_reproducibility_audit.ipynb
│   │   ├── 15_CReST_Ridge_Baseline_Comparison.ipynb
│   │   └── 23_CReST_vs_Ridge_Domain_Relative_Comparison.ipynb
│   │
│   ├── development/
│   │   ├── 02_model_development_v5_v6.ipynb
│   │   └── 03_design_robustness_refinement.ipynb
│   │
│   └── publication/
│       ├── 12_manuscript_results_assembly.ipynb
│       └── 14_supplementary_material_assembly.ipynb
│
└── README.md
```

The repository distinguishes between the **locked CReST evaluation workflow**, **post-hoc baseline comparisons reported in the manuscript**, **development-stage analyses**, and **publication-assembly notebooks**. Notebooks 15 and 23 are located in `notebooks/main/` because they support the manuscript, but their comparisons were conducted **after** the original CReST evaluation and should not be described as pre-specified confirmatory experiments.

## Data

This repository does **not** redistribute OECD PISA 2022 student-level microdata or processed participant-level datasets.

The study uses five official PISA 2022 public-use data sources:

- Creative Thinking data
- Student Questionnaire data
- School Questionnaire data
- Student Cognitive data
- Questionnaire Timing data

The corresponding source files used by the preprocessing workflow are:

```text
CY08MSP_CRT_COG.SAV
CY08MSP_STU_QQQ.SAV
CY08MSP_SCH_QQQ.SAV
CY08MSP_STU_COG.SAV
CY08MSP_STU_TIM.SAV
```

These files should be obtained directly from the official **OECD PISA 2022 public-use database**.

After downloading the files, place them in a local directory such as:

```text
PISA-CReST/
└── data/
    └── raw/
        ├── CY08MSP_CRT_COG.SAV
        ├── CY08MSP_STU_QQQ.SAV
        ├── CY08MSP_SCH_QQQ.SAV
        ├── CY08MSP_STU_COG.SAV
        └── CY08MSP_STU_TIM.SAV
```

Raw and processed student-level files should **not** be committed to the repository.

## Main Reproducibility Workflow

For reproducing the **original locked CReST analysis**, the recommended notebook order is:

```text
01_data_preparation_and_design_audit.ipynb
        ↓
04_final_architecture_lock.ipynb
        ↓
05_final_cross_system_training.ipynb
        ↓
06_final_cross_system_evaluation.ipynb
        ↓
07_contextual_robustness_and_subgroup_analysis.ipynb
        ↓
08_survey_weighted_population_analysis.ipynb
        ↓
09_domain_profiles_beyond_overall_proficiency.ipynb
        ↓
10_cross_system_profile_stability.ipynb
        ↓
11_final_sensitivity_and_robustness.ipynb
        ↓
13_computational_reproducibility_audit.ipynb
```

The post-hoc manuscript comparisons can be run after their respective source analyses have produced the required intermediate files:

```text
01 → 06 → 15    General hidden-target Ridge baseline
01 → 06 → 09 → 23    Domain-relative Ridge baseline (all 10 plausible values)
```

These are **dependency paths**, not a replacement for the full locked workflow above. Notebook 23 does not depend on Notebook 15's exported results; it independently reconstructs the corresponding source-only summary features. Neither baseline notebook retrains CReST.

### Notebook 01 — Data Preparation and Design Audit

Constructs the analytical cohort from the official PISA public-use files and performs the assessment-design audit.

The final primary cohort contains:

- 142,564 students
- 63 education systems
- 32 Creative Thinking items
- 30 assessment booklets
- 10 distinct Creative Thinking design signatures

This notebook also constructs the fixed cross-system splits and the model-ready analytical handoff used by subsequent notebooks.

### Notebook 04 — Final Architecture Lock

Documents the final CReST architecture selected during development and subsequently fixed for the seven-fold held-out-system evaluation.

### Notebook 05 — Final Cross-System Training

Runs the final seven-fold cross-system training procedure.

Each of the 63 education systems serves exactly once as an entirely unseen test system. Each outer fold contains:

- 45 training systems
- 9 validation systems
- 9 unseen test systems

For each final outer-fold model, its designated test systems were excluded from parameter fitting and checkpoint selection. Architecture development used Fold 0 training and validation systems that subsequently served as test systems in other folds. Thus, architecture selection was not fully nested within the seven-fold evaluation.

### Notebook 06 — Final Cross-System Evaluation

Evaluates the locked CReST checkpoints on their corresponding held-out education systems, including reconstruction performance, assessment-design predictability, and disjoint-item hidden-target transfer.

### Notebook 07 — Contextual Robustness and Subgroup Analysis

Evaluates hidden-target representation robustness across contextual subgroups.

These analyses assess **performance stability across groups** and should not be interpreted as establishing causal fairness or demographic parity.

### Notebook 08 — Survey-Weighted Population Analysis

Performs survey-weighted population analyses using the PISA final student weight and BRR-Fay replicate weights.

This notebook also evaluates convergent alignment between the CReST cross-item transfer score and official PISA 2022 Creative Thinking plausible values.

### Notebook 09 — Domain Profiles Beyond Overall Proficiency

Tests whether domain-relative information remains after conditioning hidden-target performance on official overall PISA Creative Thinking proficiency.

### Notebook 10 — Cross-System Profile Stability

Evaluates the continuous domain-relative profile geometry, plausible-value stability, cross-system stability, and the matched unimodal Gaussian safeguard against overinterpreting stable clusters as discrete learner types.

### Notebook 11 — Final Sensitivity and Robustness

Runs the final sensitivity analyses, including representation variants, the both-direction complete-case cohort, and expansion of the domain-relative profile space to include the Visual domain.

### Notebook 13 — Computational Reproducibility Audit

Audits:

- training time
- inference efficiency
- parameter counts
- checkpoint selection
- software environment
- computational hardware

## Additional Manuscript Baseline Comparisons (Post Hoc)

The following two notebooks are available in [`notebooks/main/`](notebooks/main/). They evaluate conventional **source-item summary features** against previously learned CReST representations under the same held-out-system and disjoint-item protocols. They were conducted **post hoc** and do not change the trained CReST checkpoints or the original locked evaluation.

### Notebook 15 — General Hidden-Target Ridge Baseline

**File:** [`15_CReST_Ridge_Baseline_Comparison.ipynb`](notebooks/main/15_CReST_Ridge_Baseline_Comparison.ipynb)

Constructs **36 source-only features** from the three ideation processes (GDI, GCI, and EII), including observed credit, response behaviour, process measures, and availability indicators. A standardised Ridge regression (`alpha=1`) is fitted on the designated validation systems and evaluated on the corresponding held-out systems in both A/B transfer directions.

- **Prerequisites:** Notebook 01's `processed/crest_v5_handoff/` outputs and Notebook 06's `processed/crest_final_test_sets/` indices and `results/creative_signal_safeguard/` prediction archives.
- **Local outputs:** `results/ridge_summary_baseline/`, including fold-level estimates, paired system comparisons, and feature metadata.
- **Purpose:** Compare the general CReST representation with an interpretable, feature-based baseline. This is not a controlled ablation of the Transformer architecture.

CReST achieved a slightly higher system-equal general hidden-item transfer correlation than the feature-based Ridge baseline (Spearman $\rho=0.515$ versus $0.502$). The paired CReST-minus-Ridge difference was $\Delta\rho=0.013$ (95% system-bootstrap CI [0.009, 0.017]). Although the confidence interval excludes zero, the absolute improvement was small and does not establish that the Transformer architecture is necessary or universally superior.

### Notebook 23 — Domain-Relative CReST vs. Ridge Comparison

**File:** [`23_CReST_vs_Ridge_Domain_Relative_Comparison.ipynb`](notebooks/main/23_CReST_vs_Ridge_Domain_Relative_Comparison.ipynb)

Compares the incremental information obtained by adding domain-specific representations to CReST's general representation against the corresponding gain from adding **12 source-domain summary features** to the feature-based Ridge model's **36 general features**. The outcome is hidden-domain performance **conditional on official PISA Creative Thinking proficiency**, assessed separately across all **10 plausible values**.

- **Prerequisites:** Notebooks 01 and 06, plus Notebook 09's saved validation/test domain latent archives and `results/profile_beyond_overall_proficiency/crest_profile_beyond_pisa_ct_system.csv`.
- **Evaluation:** Seven outer folds, two disjoint-item directions, four task domains, equal weighting of 63 education systems, and 10,000 paired system-bootstrap resamples.
- **Integrity check:** Reproduces the original CReST domain results from Notebook 09 before interpreting baseline differences.
- **Local outputs:** `results/crest_vs_ridge_domain_relative/` and the aggregate results archive `crest_vs_ridge_domain_relative_results.zip`. These contain the system-level comparisons underlying the manuscript's **Figure 13**; plotting that figure is a separate step.

The comparison finds larger **incremental domain-relative prediction gains** for CReST in Social, Scientific, and Written tasks; the Visual-domain contrast is inconclusive. These findings do not establish the superiority of the Transformer architecture or independent psychological ability dimensions. The baseline uses manually aggregated features of different dimensionality, and the domain representations may retain information about assessment exposure.

**Reproduction note:** By default, both notebooks resolve project inputs relative to the current working directory. Run them with the repository root as the working directory, or set the `CREST_PROJECT_DIR` environment variable to its absolute path before running the notebooks from `notebooks/main/`. Required intermediate student-level files must be generated locally from the official PISA sources and should not be committed to GitHub.

## Development-Stage Notebooks

The notebooks in `notebooks/development/` document analyses used during model development.

They are included for transparency but are **not part of the confirmatory evaluation sequence**.

### Notebook 02 — Model Development V5/V6

Documents intermediate representation-learning experiments and assessment-design leakage diagnostics used during architecture development.

### Notebook 03 — Design Robustness Refinement

Documents the development of residualisation and country/system-equal assessment-design calibration procedures.

Development-stage results should not be interpreted as confirmatory held-out test results.

## Publication Notebooks

The notebooks in `notebooks/publication/` assemble already-computed analytical outputs for reporting.

### Notebook 12 — Manuscript Results Assembly

Collects aggregate result objects and prepares manuscript-level summaries.

### Notebook 14 — Supplementary Material Assembly

Generates the final Supplementary Material tables and supporting figures from the analysis outputs.

## Computational Environment

The final experiments reported in the manuscript were conducted using the following environment:

```text
Python: 3.11.10
PyTorch: 2.9.0+cpu
Execution mode: CPU-only
Operating system: Windows 10
CPU: Intel Core i7-6700HQ @ 2.60 GHz
Physical CPU cores: 4
Logical CPU cores: 8
RAM: approximately 24 GB
CUDA: not used
GPU: not used
```

All final cross-system training and held-out-system inference were performed on the same **CPU-only workstation**.

The final seven-fold training stage comprised **125 epochs** and required approximately **33.16 hours** in total.

This corresponds to approximately:

- **4.74 hours per outer fold**
- **15.92 minutes per epoch**

Frozen latent inference across all **142,564 held-out students** required approximately **179.74 seconds**, corresponding to:

- approximately **1.261 ms per student**
- approximately **793 students per second**

No CUDA-enabled GPU was used in the reported experiments.

Runtime may differ substantially depending on the processor, memory configuration, operating system, package versions, and available computational resources.

## Reproducibility Notes

The final held-out-system evaluation follows several safeguards intended to reduce information leakage and model adaptation during evaluation:

- preprocessing parameters are estimated using training systems only;
- education systems, rather than individual students, are held out during outer-fold evaluation;
- checkpoint selection uses validation systems only;
- confirmatory test systems are accessed only after checkpoint selection;
- model parameters are not updated during confirmatory test evaluation;
- disjoint-item evaluation hides all information associated with target items;
- survey-weighted analyses use the official PISA sampling design;
- plausible-value analyses are performed separately across the ten official Creative Thinking plausible values rather than averaging plausible values within students;
- Notebook 15 and Notebook 23 are clearly identified as **post-hoc baseline comparisons**, not components of the original architecture-lock protocol;
- the baseline probes use validation systems for supervised fitting and evaluate predictions on the corresponding held-out test systems, without retraining CReST.

## Assessment Design

The PISA 2022 Creative Thinking assessment uses matrix sampling. Consequently, most missing student-item cells are structurally unavailable because an item was not assigned to a student's assessment booklet.

CReST therefore distinguishes among:

- assessment-design exposure,
- observed interaction,
- scorable performance,
- behavioural non-response, and
- administrative/system missingness.

Structural non-exposure is not treated as student disengagement.

## Data Availability and Redistribution

The original OECD PISA 2022 public-use data are not included in this repository.

The repository provides code required to reconstruct the analytical datasets from the official public-use source files.

Users are responsible for obtaining the source data directly from OECD and for complying with the applicable OECD data-use conditions.

Generated student-level analytical arrays, latent representations, individual prediction archives (including `.npz` files), or processed microdata should not be redistributed through this repository. Only sufficiently aggregated, disclosure-checked results should be shared.

## Outputs Included in the Notebooks

Selected notebook outputs are intentionally retained to support transparent inspection of the published workflow, including:

- aggregate metrics,
- cross-system evaluation summaries,
- training diagnostics,
- final statistical summaries,
- tables,
- publication figures, and
- computational audit results.

Large exploratory outputs, participant-level displays, obsolete debugging output, and machine-specific file paths have been removed from the public notebooks.

## Citation

This repository accompanies the manuscript:

> Kirca Demirbaga, K., & Demirbaga, U. *Beyond Overall Proficiency: Assessment-Design-Aware Domain-Relative Prediction in PISA 2022 Creative Thinking.* Manuscript under review.

Until the manuscript is publicly available, users may reference this repository by its GitHub URL and the relevant commit or release version.

The final journal citation and DOI will be added following publication.

## Authors

**Kubra Kirca Demirbaga**  
Republic of Türkiye Ministry of National Education

**Umit Demirbaga**  
Department of Software Engineering  
Ankara University, Türkiye

## Status

This repository accompanies the CReST research manuscript and is provided to support transparency and computational reproducibility.

The repository contains the original locked CReST workflow and two separately identified exploratory comparisons reported in the manuscript. Repository contents may be updated to reflect the final published version, bibliographic information, and DOI.
