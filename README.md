# Member Health Deterioration Risk Analysis

An end-to-end Microsoft Fabric machine-learning project that estimates whether an ICU patient is likely to deteriorate within the next six hours, using hourly clinical observations.

> This repository contains project code, aggregate results, and visualizations. It excludes raw clinical records, workspace identifiers, credentials, and patient-level result tables.

## Why this project matters

Clinical deterioration can develop between routine assessments. A short-horizon risk estimate can help a care team prioritize attention toward patients whose recent vital signs and laboratory values suggest elevated risk. This project demonstrates the data-engineering and analytical workflow for an early-warning prototype; it is not intended to replace clinical judgment.

## Objective

Build a reproducible Bronze → Silver → ML → Gold workflow that:

1. consolidates hourly ICU observations;
2. validates and prepares time-aware clinical features;
3. predicts deterioration/sepsis risk in the next six hours;
4. evaluates alert trade-offs on held-out data; and
5. publishes governed, analytics-ready model outputs.

## Dataset

The pipeline uses PhysioNet-style de-identified ICU time-series data. The supplied Bronze notebook recorded **1,552,210 hourly rows**, **40,336 patients**, and **43 fields**. Inputs include vital signs, laboratory measurements, demographics, admission context, ICU length of stay, and the deterioration/sepsis label. Raw data is intentionally excluded from this repository.

## Solution architecture

**Hourly ICU observations** → **Bronze ingestion and validation** → **Silver feature engineering** → **XGBoost early-warning evaluation** → **Gold inference and aggregate reporting**

## Modeling approach

The model is evaluated as a rare-event classification problem. It produces a risk score and applies an operating threshold to create an alert. Precision-recall metrics are emphasized because accuracy alone can be misleading when positive patient-hours are uncommon. Threshold selection balances detection of deterioration against false-alert workload.

## Key results

| Metric | Reported result |
| --- | ---: |
| Test PR-AUC | 0.1166 |
| Random PR-AUC baseline | 0.0174 |
| Test ROC-AUC | 0.8414 |
| Chosen threshold | 0.5534 |
| Patient detection rate | 50.81% |
| Patient false-alert rate | 4.66% |

PR-AUC is emphasized because deterioration-positive hours are rare. The model ranks risk above random, but it is an analytical early-warning prototype and is not clinically validated.

## Visualizations

### Precision-recall curve
![Precision-recall curve](docs/images/precision-recall-curve.png)

This shows the trade-off between identifying more positive hours and alert precision. Test PR-AUC is 0.1166 versus the 0.0174 random baseline.

### Confusion matrix
![Confusion matrix](docs/images/confusion-matrix.png)

At the selected threshold, this chart reported 1,005 detected positive hours, 3,096 missed positive hours, and 5,001 false-positive alerts. Sensitivity improvement remains necessary.

### Patient-level early-warning performance
![Patient-level early-warning performance](docs/images/patient-level-warning-performance.png)

This asks whether a deteriorating patient received at least one alert. It reports 50.81% detection of deteriorating patients and a 4.66% false-alert rate among non-deteriorating patients.

## What the project delivers

- `notebooks/` — source preparation, Bronze ingestion, Silver feature engineering, ML evaluation, and Gold inference.
- `docs/images/` — project charts used in this README.
- `pipeline/` — logical notebook orchestration template.

The Gold layer is designed to hold scored records, risk segments, and aggregate performance measures for downstream reporting. Any real implementation must apply organizational governance and disclosure controls.

## Important limitations

This is not clinical decision support. Real-world use needs clinical validation, governance approval, fairness and calibration analysis, prospective testing, drift monitoring, and a clinically selected operating threshold.

