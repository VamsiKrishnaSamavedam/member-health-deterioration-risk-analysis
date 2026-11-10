# Related Fabric Liquid Clustering benchmark

## Scope

This is a cleaned summary of the only supplied notebook. It compared standard Delta OPTIMIZE with Liquid Clustering on a Synthea clinical-observation table. It is a data-layout benchmark, not an early-sepsis model experiment.

## Measured setup

| Item | Observed value |
|---|---:|
| Source records | 208,292,927 |
| Original table size | 1,490.88 MB |
| Original Delta files | 225 |
| Standard optimized files | 9 |
| Liquid-clustered files | 13 |
| Liquid clustering keys | code, date, patient |
| Measured runs per configuration | 5 |

## Median execution-time comparison

| Query | Standard OPTIMIZE | Liquid clustering | Observed result |
|---|---:|---:|---|
| Creatinine filter | 2.355 s | 2.958 s | 25.6% slower |
| Multiple observation codes | 3.823 s | 4.250 s | 11.2% slower |
| Code plus date | 2.678 s | 1.769 s | 33.95% faster |
| Patient plus code | 0.692 s | 1.142 s | 65.1% slower |
| Patient plus code plus date | 1.105 s | 1.548 s | 40.1% slower |

The code-plus-date query returned 1,738,258 matching records in both configurations. The notebook did not independently verify files skipped or bytes scanned, so it does not establish why the measured latency differed.

## Cleaning applied

The published notebook has no saved outputs, Spark/Livy session data, workspace or Lakehouse IDs, table names, or patient identifiers. It uses placeholders that must be set in the target Fabric workspace.
