# Research Briefing — HydroSense-SWOT

## Problem Summary

HydroSense-SWOT is a prototype flood-timing tool for the Surma–Kushiyara part of the Haor basin in north-east Bangladesh. It uses SWOT river measurements to set channel parameters for reaches near the border, drives them with GPM IMERG rainfall, and checks the result against downstream BWDB gauges. "Done" means one replayed past flood (April–May 2026) with separate calibration and test periods, computed error figures against a rainfall-only baseline, and a clear harvest-timing flag for a named user such as an upazila agriculture officer.

## Prior Art & Comparable Projects

### SWOT-Confluence and Sword of Science (SoS) Discharge Product
Open-source framework (NASA/CNES, DAWG) that runs six discharge algorithms on SWOT river data and publishes consensus discharge. Version 3 released Dec 2025, built in reanalysis mode (historical record, not a live feed). Computing discharge from SWOT is no longer novel — our contribution is the rainfall-driven, decision-oriented layer.

### SWOT L2 RiverSP Products
Reach data (~10 km) carries WSE, slope, width and mission-computed discharge. Node data (~200 m) carries elevation and width but not slope. Hydrocron is a NASA time-series API over these products.

### SERVIR-HKH / ICIMOD Tools
A customised Streamflow Prediction Tool gives 10-day forecasts for major rivers and feeds FFWC; HIWAT gives 54-hour flash-flood forecasts for small river segments. This is the closest existing NASA-supported service. We describe ourselves as complementary.

### FFWC (Bangladesh)
Issues five-day forecasts for 25 water-level stations using national rainfall forecasts and an in-house model; lists limited upstream data and insufficient lead time as limitations.

### Past Space Apps Flood Entries
Real-time flood alert and IMERG-based risk maps reached Global Nominee in 2017, 2019 and 2020 (RTFAS, Super Citizen, FLOEWS). FloodShield (team MonsoonFive) was reported as first runner-up at a Bangladesh node. A generic "flood alert app" is crowded — we lead with the SWOT and Boro-harvest angle.

### SWOT Water Height Accuracy
Validation over India (14,000+ observations, 419 stations) reports ~0.18 m error for rivers wider than 100 m and ~0.26 m for narrower ones.

## Competitive Differentiators

1. **One replayed flood that judges can scrub through.** Time slider over April–May 2026 showing rainfall arriving, modelled flow rising, and the flag turning before the downstream gauge.

2. **Harvest-timing output, not just a water level.** Link the flag to Boro ripening and give the officer a decision. Nothing in the flood entries we found does this.

3. **SWOT as a channel-parameter source.** Use SWOT width, height and slope to set reach roughness, then drive the model with near-real-time rain. The SWOT discharge products exist, but we found no entry that uses them this way for the Haor system.

4. **Visible uncertainty and honest limits.** Error band on the hydrograph and on-screen note about what SWOT cannot see.

5. **Complement existing services.** Show output next to what FFWC and SERVIR provide so judges see a gap being filled.

## Award Strategy

| Criterion | Score | Rationale |
|---|---|---|
| Impact | 4 | Named region, documented large losses in 2017 and 2026, clear user |
| Creativity | 3 | SWOT-for-discharge exists; our angle is the rainfall-driven harvest decision |
| Validity | Target 4 | Requires time-split replay with computed metrics and baseline comparison |
| Relevance | TBD | Depends on official challenge alignment (Oct 28) |
| Presentation | 3 | Strong story; needs the slider demo |

**Primary target:** Local Impact award.
**Secondary:** Best Use of Science.
