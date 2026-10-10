# Solution Architecture — HydroSense-SWOT

**Conceptual System Design · Pre-Hackathon Phase**

---

## Elevator Sentence

> A Sunamganj upazila agriculture officer opens a phone screen. One flag tells her whether the Surma river is likely to breach before the Boro is ready to cut. That flag comes from SWOT measurements of river width, height and slope, combined with rainfall that fell on the Meghalaya hills four hours ago.

---

## System Overview

```
┌──────────────────────────────────────────────────────────────────┐
│  TIER 1 — Data Ingestion                                        │
│  SWOT L2 HR RiverSP (PO.DAAC)  ·  GPM IMERG Early Run V07     │
│  BWDB/FFWC Gauge Series  ·  SWORD v16 / HydroSHEDS             │
├──────────────────────────────────────────────────────────────────┤
│  TIER 2 — Pre-processing & ETL                                  │
│  Quality gating  ·  Slope filter  ·  IMERG sub-basin aggregation│
│  Gauge alignment (UTC)  ·  Fixture generation                   │
├──────────────────────────────────────────────────────────────────┤
│  TIER 3 — Core Analytical Engine                                │
│  Manning roughness calibration  ·  Muskingum-Cunge routing      │
│  Harvest flag logic  ·  Rainfall-only baseline  ·  Metrics      │
├──────────────────────────────────────────────────────────────────┤
│  TIER 4 — User Delivery Interface                               │
│  Map-first dashboard  ·  Time slider  ·  Hydrograph             │
│  Bilingual harvest flag (EN/বাংলা)  ·  Limits panel             │
└──────────────────────────────────────────────────────────────────┘
```

---

## Tier 1 — Data Ingestion

### 1.1 SWOT L2 HR RiverSP Reach Product

| Property | Value |
|---|---|
| Product short_name (Hydrocron) | SWOT_L2_HR_RIVERSP_2.0 |
| Product (PO.DAAC) | Collection Concept ID: C2799438271-POCLOUD |
| Provides | WSE (m), width (m), slope2 (m/m), wse_u, dark_frac, quality_f |
| Access | earthaccess Python library or Hydrocron time-series API |
| Reach ID type | 11-digit integer — stored as STRING always |

SWOT directly measures channel geometry (width, slope2, height). This is what calibrates Manning's roughness coefficient for ungauged border reaches.

### 1.2 GPM IMERG Early Run V07

| Property | Value |
|---|---|
| Short name | GPM_3IMERGHHE |
| Resolution | 0.1° × 0.1°, 30-minute intervals |
| Latency | ~4 hours after observation |
| Key variable | precipitationCal |
| Sub-region | 23.5°N–26.0°N, 91.0°E–93.5°E |
| Known bias | Overestimates intense orographic rainfall over Meghalaya |
| Timestamp convention | End-of-interval (shift back 30 min before joining) |

### 1.3 BWDB / FFWC Gauge Series

- Stations: Kanaighat (Surma), Sherpur (Kushiyara), Sunamganj
- All timestamps stored as UTC internally

### 1.4 SWORD v16 + HydroSHEDS

- SWORD: reach IDs, centreline widths — pre-screen reaches >100 m
- HydroSHEDS: sub-catchment polygons for IMERG spatial aggregation

---

## Tier 2 — Pre-processing

### SWOT Quality Gating

| Check | Threshold | Action |
|---|---|---|
| quality_f | 0 or 1 only | Drop if 2 or 3 |
| dark_frac | < 0.3 | Drop |
| width | > 100 m | Tag 50–100 m as uncertain |
| wse_u | < 0.25 m | Weight by 1/wse_u² in calibration |
| slope2 | > 2 × slope_u AND > 1e-5 m/m | Drop if slope unreliable |

The slope quality check is critical for Bangladesh's low-gradient rivers (~1e-5 m/m). SWOT slope noise can exceed the signal, and Manning's Q scales as slope^0.5.

### Data Model

SWOT and IMERG are different time series and are never joined by timestamp:
- **SWOT passes** (irregular, ~every 7–10 days) → Manning calibration only
- **IMERG steps** (every 30 minutes) → routing and operational flag time steps

---

## Tier 3 — Core Analytical Engine

### Manning's Equation — Roughness Calibration

```
Q = (1/n) × A × R^(2/3) × slope2^(1/2)

Q      = discharge [m³/s]
n      = roughness [dimensionless, solved for per reach]
A      = width × (wse − z_bed) [m²]
R      = depth for wide rivers (width >> depth)
slope2 = reach-averaged slope from SWOT [m/m]
z_bed  = minimum SWOT WSE during quality-passed passes
```

- Valid n range: [0.025, 0.10]
- Calibrate only on mid-to-high flow SWOT passes
- Calibration period: Jan 2024 – Mar 2026 (pre-event)
- Test period: Apr 1 – May 31, 2026 only

### Muskingum-Cunge Routing

Valid regime: rising limb, in-channel flow only. Cannot model floodplain inundation or backwater.

```
V_mean = Q / A           (at mid-flood-stage SWOT pass)
c      = (5/3) × V_mean  (flood wave celerity)
K      = reach_length / c (travel time per reach)
```

Estimated lead time: ~22–30 hours from observed rain to flag.

### Harvest Flag Logic

| Condition | Flag | Action |
|---|---|---|
| Modelled level ≥ danger level | **RED** | Harvest immediately |
| Modelled level ≥ 80% of danger level | **AMBER** | Prepare to harvest early |
| Below 80% | **GREEN** | Normal operations |

### Rainfall-Only Baseline

6-hour accumulated MAP over Meghalaya against historical thresholds. Required to demonstrate what SWOT channel parameters add over rainfall alone.

### Performance Metrics

| Metric | Display |
|---|---|
| NSE | 2 decimal places |
| KGE | 2 decimal places |
| PBIAS | 1 decimal place with sign |
| Peak timing error | 1 decimal place, hours |
| Lead time | 1 decimal place, hours |

All metrics computed from code on the test period. Never hard-coded.

---

## Tier 4 — User Interface

```
+=================================================================+
| HydroSense-SWOT          [EN / বাংলা]      Data: 2026-04-14    |
+===================================+============================+
|                                   |  HARVEST FLAG              |
|  MAP — Surma-Kushiyara reaches    |  [AMBER icon]  AMBER      |
|  (cividis colormap)               |                            |
|  (*) = SWOT overpass date         |  Cut within 12 hours       |
|  [///] = slope quality flag       |  if level keeps rising.    |
|  Click reach -> hydrograph        |                            |
+===================================+============================+
|  HYDROGRAPH — Kanaighat (Surma)                                |
|  Level |  modelled (shaded uncertainty ±0.18 m)                 |
|   (m)  |  BWDB observed                                        |
|        |  baseline (rain-only)                                  |
|        |  ---danger level---                                    |
|        +--[Apr 1]---[Apr 15]---[May 1]---[May 31]------------>|
|                  [SLIDER] <- peak set from data                |
+=================================================================+
|  LIMITS (always visible)                                       |
|  • SWOT resolves rivers >100 m; narrow headwaters excluded     |
|  • IMERG may overestimate orographic rain over Meghalaya       |
|  • Routing models rising limb only                             |
|  • Lead time: ~22-30 h from observed rain                      |
|  • NSE: [computed]  KGE: [computed]  Timing: [computed] h      |
+=================================================================+
```

### Accessibility

| Element | Standard |
|---|---|
| Map colours | cividis sequential (CVD-safe) |
| Flag | Text + icon + colour (never colour alone) |
| Contrast | WCAG AA ≥ 4.5:1 |
| Default language | Bangla; English toggle |
| Min viewport | 360 px |

---

## What This Design Does NOT Claim

- NOT real-time SWOT monitoring
- NOT a 48-hour forecast — lead time is from observed IMERG rain
- NOT resolving narrow headwater channels (<100 m wide)
- NOT modelling floodplain inundation extent
- NOT promising any specific NSE before the code runs
- NOT the first satellite flood tool for Bangladesh — novelty is the harvest-timing flag for this ungauged transboundary corridor

---

## Technical Assumptions

| # | Assumption | Risk | Mitigation |
|---|---|---|---|
| A1 | z_bed = min SWOT WSE from quality-passed passes | Underestimates depth | ±0.18 m error band; calibrate on mid-to-high flow passes |
| A2 | Muskingum-Cunge valid for rising limb in-channel flow | Recession mismatch | Restrict claims to rising limb + threshold timing |
| A3 | IMERG bias absorbed by Manning n calibration | Bias pattern may differ between events | State in Limits panel; report PBIAS |
