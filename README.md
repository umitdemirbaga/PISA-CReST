# PISA-CReST

Open-source code and reproducibility materials for **CReST**, a design-robust self-supervised multi-view Transformer framework for analysing domain-relative structure in PISA 2022 Creative Thinking data across education systems.

The final experiments reported in the associated manuscript were implemented in **Python 3.11.10** using **PyTorch 2.9.0+cpu** and were conducted entirely in a **CPU-only computing environment**.

## Associated Manuscript

**Beyond Overall Proficiency: CReST Reveals Domain-Relative Structure in Creative Thinking Across Education Systems**

**Authors:** Kubra Kirca Demirbaga and Umit Demirbaga

The study analyses PISA 2022 Creative Thinking data from **142,564 students across 63 education systems** and uses **32 scored Creative Thinking items** spanning Written, Visual, Social, and Scientific domains.

CReST is designed to learn transferable creative-performance representations while reducing dependence on matrix-sampled assessment design, booklet structure, contextual information, and education-system-specific signals.

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
│   │   └── 13_computational_reproducibility_audit.ipynb
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

The repository distinguishes between the **final confirmatory workflow**, **development-stage analyses**, and **publication-assembly notebooks**.

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

For reproducing the final locked analysis, the recommended notebook order is:

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

Implements and validates the final locked CReST architecture used for confirmatory evaluation.

### Notebook 05 — Final Cross-System Training

Runs the final seven-fold cross-system training procedure.

Each of the 63 education systems serves exactly once as an entirely unseen test system. Each outer fold contains:

- 45 training systems
- 9 validation systems
- 9 unseen test systems

No held-out test-system results are used for architecture or checkpoint selection.

### Notebook 06 — Final Cross-System Evaluation

Performs confirmatory evaluation on the unseen education systems, including reconstruction performance, nuisance robustness, and disjoint-item creative-signal transfer.

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

All final training and confirmatory inference were performed on the same **CPU-only workstation**.

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

The confirmatory analysis follows several safeguards intended to reduce leakage and post-hoc model adaptation:

- preprocessing parameters are estimated using training systems only;
- education systems, rather than individual students, are held out during outer-fold evaluation;
- checkpoint selection uses validation systems only;
- confirmatory test systems are accessed only after checkpoint selection;
- model parameters are not updated during confirmatory test evaluation;
- disjoint-item evaluation hides all information associated with target items;
- survey-weighted analyses use the official PISA sampling design;
- plausible-value analyses are performed separately across the ten official Creative Thinking plausible values rather than averaging plausible values within students.

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

Generated student-level analytical arrays, latent representations, or processed microdata should not be redistributed through this repository.

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

If you use this repository, please cite the associated manuscript:

> Kirca Demirbaga, K., & Demirbaga, U. *Beyond Overall Proficiency: CReST Reveals Domain-Relative Structure in Creative Thinking Across Education Systems.*

Full bibliographic information and DOI will be added after publication.

## Authors

**Kubra Kirca Demirbaga**  
Republic of Türkiye Ministry of National Education

**Umit Demirbaga**  
Department of Software Engineering  
Ankara University, Türkiye

## Status

This repository accompanies the CReST research manuscript and is provided to support transparency and computational reproducibility.

The repository reflects the final locked analytical workflow used in the manuscript. Repository contents may be updated to reflect the final published version, bibliographic information, and DOI.
