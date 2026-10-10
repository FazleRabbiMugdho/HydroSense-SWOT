# HydroSense-SWOT

**Satellite-Assisted Flood Timing & Boro Harvest Decision Support for the Haor Basin**

NASA Space Apps Challenge 2026 · Bangladesh · Team AUSTrology

---

> **Pre-Hackathon Phase.** This repository contains research, architecture, and project scaffolding prepared before the hackathon. The data pipeline, analytical engine, and interactive dashboard will be implemented during the official 48-hour hackathon window (November 13–14, 2026).

---

## The Problem

Every April, heavy rainfall over India's Meghalaya highlands can raise the Surma and Kushiyara rivers within days, breaching embankments across Bangladesh's northeastern Haor basin before the Boro rice harvest is complete.

In April 2026, rainfall across the basin's seven districts ran approximately 76% above normal. Floodwater covered more than 130,000 acres of Boro, with reported losses of approximately 214,000 tonnes of rice and approximately 230,000 affected farmers. In 2017, early flash flooding damaged Boro on 149,224 hectares in Sunamganj district alone.

A farmer has one decision to make before the water arrives: **cut grain that is not fully ripe, or leave it and risk losing all of it.**

Bangladesh's Flood Forecasting and Warning Centre (FFWC) issues five-day forecasts for 25 stations in flash-flood areas, but cites limited upstream data and insufficient lead time among its own limitations. Most of these floods start in rivers outside Bangladesh where few cross-border gauge stations report. Existing tools such as SERVIR HIWAT provide 54-hour forecasts for small rivers, but estimate flow from rainfall and a model with few measurements of the river channel itself.

## Our Solution

HydroSense-SWOT uses NASA's SWOT satellite river measurements to set channel parameters for Surma–Kushiyara reaches near the border, drives them with GPM IMERG rainfall data (available approximately 4 hours after observation), and checks the result against downstream BWDB gauge records.

The output is a three-state harvest timing flag — **GREEN** (normal), **AMBER** (prepare to harvest early), **RED** (harvest immediately) — for a named user such as an upazila agriculture officer in Sunamganj.

### What Makes This Different

- **SWOT as a channel-parameter source** — SWOT width, height, and slope calibrate channel roughness for ungauged transboundary reaches. NASA already publishes SWOT discharge; our contribution is the rainfall-driven forecast layer and harvest decision.
- **Harvest-timing output** — Not just a water level, but a decision linked to Boro ripening stage.
- **Honest validation** — Time-split replay of the April 2026 flood with computed error metrics against a rainfall-only baseline.
- **Visible limits** — What SWOT cannot see is stated on every screen.

## System Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│  TIER 1 — Data Ingestion                                         │
│  SWOT L2 HR RiverSP (PO.DAAC)  ·  GPM IMERG Early Run V07        │
│  BWDB/FFWC Gauge Series  ·  SWORD v16 / HydroSHEDS               │
├──────────────────────────────────────────────────────────────────┤
│  TIER 2 — Pre-processing & ETL                                   │
│  Quality gating  ·  Slope filter  ·  IMERG sub-basin aggregation │
│  Gauge alignment (UTC)  ·  Fixture generation                    │
├──────────────────────────────────────────────────────────────────┤
│  TIER 3 — Core Analytical Engine                                 │
│  Manning roughness calibration  ·  Muskingum-Cunge routing       │
│  Harvest flag logic  ·  Rainfall-only baseline  ·  Metrics       │
├──────────────────────────────────────────────────────────────────┤
│  TIER 4 — User Delivery Interface                                │
│  Map-first dashboard  ·  Time slider  ·  Hydrograph              │
│  Bilingual harvest flag (EN/বাংলা)  ·  Limits panel               │
└──────────────────────────────────────────────────────────────────┘
```

## NASA & Partner Data Sources

| Dataset | Provider | Product ID | Used For |
|---|---|---|---|
| SWOT L2 HR RiverSP Reach | NASA PO.DAAC | SWOT_L2_HR_RIVERSP_2.0 | Channel width, height (WSE), slope for Manning calibration |
| GPM IMERG Early Run V07 | NASA GES DISC | GPM_3IMERGHHE | Near-real-time rainfall input (~4h latency) |
| SWOT L4 SoS Discharge V3 | NASA PO.DAAC | SWOT_L4_HR_DAWG_SOS_DISCHARGE_V3 | Prior art comparison |
| SWORD River Database v16 | HydroSHEDS | — | Reach geometry pre-screening |
| HydroSHEDS v1 | WWF / McGill | — | Sub-catchment polygons for rainfall aggregation |
| BWDB/FFWC Gauge Series | Bangladesh BWDB | — | Validation target (Kanaighat, Sunamganj stations) |

## Known Limitations

- SWOT resolves rivers wider than approximately 100 m; narrow Meghalaya headwater channels are not resolved.
- SWOT revisits a given river approximately 1–3 times per 21-day cycle — it is used for channel parameter estimation, not live monitoring.
- Lead time comes from IMERG observed rainfall routing (~22–30 hours), not from SWOT passes.
- IMERG may overestimate intense orographic rainfall over Meghalaya; calibration corrects for this implicitly.
- Muskingum-Cunge routing models rising limb only — floodplain inundation extent is not modelled.
- All displayed performance metrics (NSE, KGE, timing error) are computed from code on the test period, never hard-coded.

## Repository Structure

```
HydroSense-SWOT/
├── docs/
│   ├── problem-statement.md
│   ├── solution-architecture.md
│   ├── research-briefing.md
│   ├── data-sources.md
│   └── risk-register.md
├── pipeline/
│   └── (to be implemented during hackathon)
├── ui/
│   └── (to be implemented during hackathon)
├── scripts/
│   └── (to be implemented during hackathon)
├── data/
│   └── fixtures/
├── .env.example
├── .gitignore
├── requirements.txt
├── LICENSE
└── README.md
```

## How to Run (Post-Hackathon)

```bash
1. pip install -r requirements.txt
2. cp .env.example .env  # Add your NASA Earthdata credentials
3. python scripts/download_fixtures.py
4. python pipeline/run_pipeline.py
5. streamlit run ui/app.py
```

## Team AUSTrology

| Name | Role |
|---|---|
| Fazle Rabbi Mugdho | Systems & Backend Algorithm Developer |
| Taisha Tasnim | Geospatial & Domain Researcher |
| Mahim Abdullah Rianto | Data Infrastructure & Pipeline |
| Samia Rahman Arpita | Technical Quality & Compliance |
| Redwana Binte Zaman Tonima | Product Impact & Media Strategist |
| Arko Saha | Frontend & Product Experience Developer |

## Prescreening Video

[240-Second Qualification Video (YouTube)](https://www.youtube.com/watch?v=4eO_aO8zpO8)

## References

See [`docs/data-sources.md`](docs/data-sources.md) for the full source ledger with verification status.

## License

[MIT License](LICENSE)

## Acknowledgements

Built for the NASA International Space Apps Challenge 2026 — Bangladesh.
Uses open data from NASA, CNES, and the Bangladesh Water Development Board.

---

*This project complements existing services (FFWC, SERVIR-HKH) and does not claim to replace them.*
