# Project 1: Client Service Performance Dashboard

![Dashboard preview](images/dashboard.png)

## Objective

Show how well a client service team meets its response-time targets (SLAs), find where it falls short, and recommend what to fix. The audience is a Business Head who needs the answer in about 10 seconds.

## Data

500 synthetic client requests (1 Jan to 30 Sep 2026), generated with a fixed random seed. Fields: request ID, date, region, request type, priority, turnaround hours, SLA target, SLA met, satisfaction (CSAT, 1 to 5).

- Regions: APAC, EMEA, North America, LatAm
- Request types: Reporting, Client Onboarding, Portfolio Query, Document Request, Complaint / Escalation
- SLA targets (hours): 48, 120, 24, 36, 12 respectively (illustrative, editable on the Assumptions sheet)

## Workbook sheets

| Sheet | Purpose |
|-------|---------|
| README | Overview and colour legend |
| Dashboard | KPI tiles and four charts on one page |
| Assumptions | SLA target per request type (inputs) |
| Data | The 500 requests with formula columns |
| Analysis | Formula-driven summaries by region, request type and month |
| Exec Summary | "What I would tell the Business Head" |
| Design Rationale | Why each layout, colour and chart was chosen |

## Key formulas

| Purpose | Formula |
|---------|---------|
| SLA target lookup | `=INDEX(Assumptions!$B$4:$B$8, MATCH(E2, Assumptions!$A$4:$A$8, 0))` |
| SLA met? | `=IF(H2<=G2,"Yes","No")` |
| Requests per region | `=COUNTIFS(region_range, A5)` |
| SLA met % | `=IF(B5=0,0, COUNTIFS(region_range, A5, SLA_met_range, "Yes") / B5)` |
| Average turnaround | `=IFERROR(AVERAGEIFS(turnaround_range, region_range, A5), 0)` |

## Results (on the synthetic data)

- Overall SLA met: **70.4%** (352 of 500 requests)
- By region: North America 81.5%, APAC 80.8%, EMEA 67.1%, **LatAm 39.5%**
- Weakest request type: **Client Onboarding at 59.2%**
- Average CSAT: 4.19 when SLA was met, 3.01 when missed

## Recommendations

1. Review staffing and escalation routing in the weakest region first.
2. Add a weekly SLA-at-risk view to the service review.
3. Map the onboarding process end to end and remove the slowest handoff.

## Limitations

- The data is synthetic. The slower LatAm turnaround and the link between missed SLAs and lower CSAT were **built into the data generator**, so they are illustrations, not discoveries. With real data, these relationships would need to be tested.
- The Exec Summary text is typed, not formula-linked, so it does not update if the data changes.
- The summaries use formula tables rather than PivotTables. Adding a PivotTable with slicers on Region and Request Type is a natural next step.

## Design decisions

Answer first (KPI tiles on top), one page with no scrolling, two main colours with teal marking what to look at, axes fixed to the real scale (0 to 100%, 0 to 5) so differences are not exaggerated, horizontal bars for long category names, and a line for trend over time. See the Design Rationale sheet for the full list.
