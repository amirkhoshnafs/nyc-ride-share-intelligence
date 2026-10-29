# DAX measure catalog

These are the **15 documented model measures**, recorded from the project's modeling handoff. The supplied repository contains only the report-definition export, not the `.SemanticModel` measure source. This catalog is documentation and can aid rebuilding or validating the full model.

```dax
Total Rides = COUNTROWS ( 'Fact_Rides' )

Total Trip Miles = SUM ( 'Fact_Rides'[trip_miles] )

Total Trip Hours = DIVIDE ( SUM ( 'Fact_Rides'[trip_time] ), 3600 )

Total Base Fare = SUM ( 'Fact_Rides'[base_passenger_fare] )

WAV Requested Rides =
CALCULATE ( [Total Rides], 'Fact_Rides'[wav_request_flag] = "Y" )

WAV Fulfilled Rides =
CALCULATE (
    [Total Rides],
    'Fact_Rides'[wav_request_flag] = "Y",
    'Fact_Rides'[wav_match_flag] = "Y"
)

Rides by Pickup Zone = [Total Rides]

Rides by Dropoff Zone =
CALCULATE (
    [Total Rides],
    USERELATIONSHIP ( 'Fact_Rides'[DOLocationID], 'Dim_Geography'[LocationID] )
)

Rides With On-Scene =
CALCULATE ( [Total Rides], NOT ISBLANK ( 'Fact_Rides'[WaitTimeSec] ) )

Avg On-Scene Wait (Min) =
DIVIDE ( SUM ( 'Fact_Rides'[WaitTimeSec] ), [Rides With On-Scene] ) / 60

Avg Request-to-Pickup (Min) =
DIVIDE (
    SUM ( 'Fact_Rides'[RequestToPickupSec] ),
    CALCULATE ( [Total Rides], NOT ISBLANK ( 'Fact_Rides'[RequestToPickupSec] ) )
) / 60

Rides MTD = TOTALMTD ( [Total Rides], 'Dim_Date'[Date] )

Rides YTD = TOTALYTD ( [Total Rides], 'Dim_Date'[Date] )

Rides by Request Date =
CALCULATE (
    [Total Rides],
    USERELATIONSHIP ( 'Fact_Rides'[RequestDateKey], 'Dim_Date'[DateKey] )
)

Rides With Negative Duration =
CALCULATE ( [Total Rides], 'Fact_Rides'[RequestToPickupSec] < 0 )
```

## Time Intelligence calculation group

Calculated item expression definitions from the project handoff:

| Item | DAX expression | Notes |
|---|---|---|
| Current | `SELECTEDMEASURE()` | Base result |
| MTD | `TOTALMTD(SELECTEDMEASURE(), 'Dim_Date'[Date])` | Month-to-date |
| YTD | `TOTALYTD(SELECTEDMEASURE(), 'Dim_Date'[Date])` | Year-to-date |
| YoY % | `DIVIDE(SELECTEDMEASURE() - CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Dim_Date'[Date])), CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Dim_Date'[Date])))` | Format `0.00%`; no prior-year fact for comparisons |

The documented YoY item uses `DIVIDE` to handle zero or blank prior-year denominators safely. The original 2024 dataset has no corresponding prior-year fact rows, so the YoY result is normally blank.

## Desktop note — Operations chart

In the working local Desktop project, the negative-duration chart initially referenced an absent `_Measures[Rides With Negative Duration]`. The owner created the measure above, rebound the chart's X-axis, and confirmed the chart was fixed. The report-only PBIR files in this repository continue to require that exact measure in the original semantic model.
