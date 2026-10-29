# Data quality, governance, and interpretation

The project preserves source exceptions and documents their effect on analytics.

- `wav_request_flag` and `wav_match_flag` are **text strings** `Y` / `N`, not Boolean columns.
- Missing `on_scene_datetime` values are legitimate for trips with no recorded on-scene event; do not treat null waiting-time records as zero-minute waits.
- Negative request-to-pickup durations are retained; likely timing/clock anomalies exist. The negative-duration chart and explicit measure monitor these records rather than removing them.
- `Dim_Geography` includes legitimate `N/A` borough values.
- **858 rides** associated with January pickups have request dates in December 2023; calendar dimension starts January 2023 for this reason.
- **4 request timestamps in March 2024** are recorded anomalies relative to the January–February pickup window.
- WAV Requested ≈ WAV Fulfilled in this source extract; this is a *recorded-data characteristic*, not proof that all real-world accessibility needs were met.
- Six corporate account records are synthetic examples, not identifiable real account holdings or revenue customers.
- The corporate account-to-zone bridge is overlapping. Do **not** sum account measures to estimate distinct total rides or miles.
- Report measures are pickup-date oriented unless explicitly changed with `USERELATIONSHIP`.
- Calculation-group YoY % may be blank because the fact contains no January–February 2023 trips.

See [semantic model](semantic-model.md) for relationship orientation and [reproduction](reproduction.md) for source data provenance.
