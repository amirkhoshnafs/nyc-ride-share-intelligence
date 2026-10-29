# Semantic model

## Tables

| Table | Purpose / documented rows |
|---|---|
| `Fact_Rides` | 39,023,078 modeled trip rows, 26 columns |
| `Dim_Date` | 731 calendar dates, 2023-01-01 to 2024-12-31 |
| `Dim_Time` | 1,440 minute-of-day records, integer `TimeKey` = HHMM |
| `Dim_Geography` | 265 zone records; `LocationID`, `Borough`, `Zone`, `service_zone` |
| `Dim_Base` | 2 platform/base mapping records |
| `Dim_Account` | 6 **synthetic** corporate accounts |
| `Bridge_AccountZone` | 462 account-zone mapping rows; hidden in report view |
| `_Measures` | Explicit DAX measure holder (15 intended measures) |
| `Time Intelligence` | Calculation group with Current, MTD, YTD, YoY % items |

## Active relationships (single filter direction unless marked)

- `Dim_Base[BaseKey]` → `Fact_Rides[BaseKey]`
- `Dim_Date[DateKey]` → `Fact_Rides[PickupDateKey]`
- `Dim_Time[TimeKey]` → `Fact_Rides[PickupTimeKey]`
- `Dim_Geography[LocationID]` → `Fact_Rides[PULocationID]`
- `Dim_Account[AccountKey]` → `Bridge_AccountZone[AccountKey]`
- `Dim_Geography[LocationID]` ↔ `Bridge_AccountZone[LocationID]` (**Both** filter directions in this one designated relationship)

## Inactive role-playing relationships

- `Dim_Date[DateKey]` → `Fact_Rides[RequestDateKey]`, `Fact_Rides[DropoffDateKey]`
- `Dim_Time[TimeKey]` → `Fact_Rides[RequestTimeKey]`, `Fact_Rides[DropoffTimeKey]`
- `Dim_Geography[LocationID]` → `Fact_Rides[DOLocationID]`

These are activated in measures where necessary via `USERELATIONSHIP`.

## Many-to-many caution

The `Bridge_AccountZone` describes multiple program/account memberships per pickup zone; **199 geography zones map to multiple accounts**. Thus a ride in a shared zone may contribute to more than one account or program display. Summing account-level rows as a distinct total will overcount. The model's six account names are demonstration data, **not observed corporate customers**.

## Source availability

The included `powerbi/ubers latest september 2.Report/` is a report-only PBIR definition, not the model's source code. The report points to `../ubers latest september 2.SemanticModel` and therefore requires the original model when opened as a project. The full PBIX can be downloaded separately using the Google Drive link in the root README.
