# Proposed architecture

\`\`\`mermaid
flowchart TD
  S[Approved clinical source] --> Q[Quality and de-identification checks]
  Q --> L[Lakehouse curated zone]
  L --> F[6-hour feature windows]
  F --> T[Training and test partitions]
  T --> M[Candidate risk model]
  M --> V[Validation and calibration]
  V --> R[Aggregate review outputs]
\`\`\`

## Implementation controls

- Preserve event-time ordering when creating features and labels.
- Do not expose patient identifiers, small-cell counts, raw notes, secrets, or workspace details in Git.
- Treat benchmark latency as platform evidence only; it is not a predictive-performance metric.
- Document intended use, operating threshold, monitoring plan, and human-review workflow before deployment.
