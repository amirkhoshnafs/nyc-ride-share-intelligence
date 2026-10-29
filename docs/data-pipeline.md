# Data pipeline and Power Query

## Inputs

- NYC TLC **High Volume For-Hire Vehicle (FHVHV)** Parquet files: `fhvhv_tripdata_2024-01.parquet` and `fhvhv_tripdata_2024-02.parquet`.
- NYC TLC taxi-zone lookup file (`taxi_zone_lookup.csv`, 265 zones).
- Original local path in the author's VM: `C:\RideSharing_Project\Data\` (not portable without changing parameters).
- Sources: <https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page>.

## ETL outline

1. **Power Query staging:** combine two Parquet months with parameterized folder connection and Combine Files. Helper queries and source staging queries are set to load-disabled.
2. **Fact_Rides:** reference staged trips, remove redundant source/base text attributes, derive integer date keys (`YYYYMMDD`) and time keys (`HHMM`) for request/pickup/drop-off, and calculate `WaitTimeSec` and `RequestToPickupSec`.
3. **Platform dimension:** use `Dim_Base` to map identifiers to a compact `BaseKey`; remove the redundant base strings from the fact.
4. **Cardinality optimization:** remove four raw datetime columns from `Fact_Rides` once keys and durations have been derived.
5. **Dimensions:** build calendar, 1,440 minute-of-day records, geography, two dispatch/base records, and a synthetic corporate-account sample with an account-zone bridge.
6. **Load:** 39,023,078 rows in the final Fact_Rides with 26 modeled columns.

## Performance rationale

Keeping repeated 64-bit datetime values in a very large fact can be costly. Integer keys plus reusable dimensions preserve required reporting semantics while materially lowering the model's in-memory footprint. The observed optimization is documented in [performance.md](performance.md).

## Reproducibility

The **Power Query transformation code** and full semantic-model source were not included in the supplied report ZIP. This file documents the previously tested model design but does not pretend to be an executable ETL script. For a working copy, use the full PBIX (external link in root README); for portable reproduction, obtain TLC inputs and update the source folder parameters in Power BI Desktop.
