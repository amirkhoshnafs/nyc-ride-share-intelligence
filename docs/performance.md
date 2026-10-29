# Performance and validation

## Observed optimization

| Test | Before | After | Interpretation |
|---|---:|---:|---|
| Model in-memory size | ~1.99 GB | 782.51 MB | Approximately 61% smaller |
| Fact datetime fields removed | 0 | 4 raw datetimes pruned | Derived keys/durations retained |
| Rows in `Fact_Rides` | 39,023,078 | 39,023,078 | Same source trip count |

The size comparison comes from the author's prior VertiPaq diagnostics. It is a **memory measurement**, not the PBIX size on disk. The published PBIX is larger than 1 GB.

## Query benchmark

A borough-by-month query involving three measures was previously benchmarked at **~6 ms**. This is a measure of that specific query and test environment, **not** an average chart load time or a comprehensive performance guarantee.

## Why the model is fast

- High-cardinality datetime data was pruned from the large fact table after integer keys and durations were created.
- Dimensions carry descriptive attributes and unique keys; the fact table retains lean foreign keys.
- Model uses explicit DAX measures and targeted inactive relationship activation.
- No extra aggregation table was required for the measured use case.

## Regression checks

- All rows = **39,023,078**
- January = **19,663,930**
- February = **19,359,148**
- WAV Requested ≈ WAV Fulfilled ≈ **91K**
- Average on-scene wait = **3.07 min**
- Average request-to-pickup = **4.16 min**

This repository does not contain the imported semantic model or the executable benchmarking environment. Re-run these checks in the full Desktop file after a refresh.
