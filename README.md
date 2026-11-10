# Member Health Deterioration Risk Analysis

Microsoft Fabric project scaffold for a 6-hour ICU early-sepsis prediction use case.

## Current status

This repository is publication-ready as a project shell, but not as a completed machine-learning study. No early-sepsis notebook, feature specification, cohort definition, labels, trained model, or evaluation artifact was supplied with the available materials.

The only attached notebook reviewed was a Fabric Liquid Clustering benchmark on synthetic clinical observations. A cleaned, output-free copy is included as related platform engineering material; it is not evidence of sepsis-model performance.

## Intended 6-hour prediction problem

- Population: define the ICU cohort and exclusions before training.
- Index time: define when risk is calculated.
- Prediction window: 6 hours.
- Outcome: define the sepsis label and clinical event time using an approved specification.
- Split strategy: use patient-level and time-aware validation to reduce leakage.

## Conceptual architecture

\`\`\`mermaid
flowchart LR
  A[Approved ICU events] --> B[Fabric Lakehouse]
  B --> C[Time-windowed feature engineering]
  C --> D[Train and validation splits]
  D --> E[Risk model]
  E --> F[Calibration, fairness and drift review]
  F --> G[Clinical-review reporting]
\`\`\`

## Available related material

- notebooks/related_liquid_clustering_benchmark.ipynb: sanitized Fabric query-layout benchmark.
- docs/liquid-clustering-benchmark.md: measured platform benchmark summary and limitations.

## Missing before an ML release

- Source notebook or pipeline
- Data dictionary and feature definitions
- Cohort, label, and timestamp specification
- Model configuration and training code
- Evaluation results, calibration, subgroup analysis, and clinical review

## Safety

This project must not be used for diagnosis or treatment decisions without appropriate clinical governance, prospective validation, privacy review, and approved deployment controls.
