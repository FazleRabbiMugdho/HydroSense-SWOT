# Data Sources — HydroSense-SWOT

## NASA & Partner Datasets

| ID | Type | Name | Link | Version | Used For |
|---|---|---|---|---|---|
| D1 | Dataset | SWOT L2 HR RiverSP Reach & Node | [PO.DAAC](https://podaac.jpl.nasa.gov) / [Hydroweb](https://hydroweb.next.theia-land.fr/catalog/details/SWOT_L2_HR_RIVERSP_REACH) | Baseline C/D | Core input — WSE, width, slope for Manning calibration |
| D2 | Dataset | SWOT L4 Sword of Science Discharge V3 | [PO.DAAC](https://podaac.jpl.nasa.gov/dataset/SWOT_L4_HR_DAWG_SOS_DISCHARGE_V3) | V3, Dec 2025 | Prior art comparison |
| D3 | Dataset | GPM IMERG Early Run V07 | [GES DISC](https://gpm.nasa.gov/resources/documents/imerg-v07-technical-documentation) | V07 | Near-real-time rainfall input |
| D4 | Database | SWORD River Database | [hydrosheds.org](https://www.hydrosheds.org) | v16 | Reach geometry, width pre-screening |
| D5 | Database | HydroSHEDS | [hydrosheds.org](https://www.hydrosheds.org) | v1 | Sub-catchment polygons |
| D6 | Gauge | BWDB/FFWC gauge series | [bwdb.gov.bd](http://www.bwdb.gov.bd) | TBD | Validation target |

## API & Access Points

| Service | Endpoint | Purpose |
|---|---|---|
| Hydrocron | PO.DAAC Hydrocron API | SWOT reach time series |
| earthaccess | Python library | SWOT/IMERG bulk download |
| GES DISC OPeNDAP | disc.gsfc.nasa.gov | IMERG sub-setting |

## Reference Sources

| ID | Type | Name | Link | Used For |
|---|---|---|---|---|
| S1 | News | Dhaka Tribune — Netrokona flash flood losses | [Link](https://www.dhakatribune.com/amp/bangladesh/nation/19349/flash-flood-causes-tk1-000cr-loss-in-netrakona) | 2017 context |
| S2 | Paper | Effects of 2017 early flash flooding in Haor areas, Sunamganj — J. Environ. Sci. & Nat. Resour. 12(1&2), 2019 | [Link](https://banglajol.info/index.php/JESNR/article/view/52007) | 2017 loss figures |
| S3 | Analysis | Think Global Health — Haor flash floods 2026 | [Link](https://www.thinkglobalhealth.org/article/bangladesh-faces-rice-shortages-amid-flash-floods-and-global-fuel-crisis) | 2026 loss figures |
| S4 | News | The Financial Express — Haor flash flood, April 2022 | [Link](https://thefinancialexpress.com.bd/national/floods-now-creeping-into-northern-dists-1649645421) | Meghalaya rainfall context |
| S5 | Web | ICIMOD / SERVIR-HKH — Flood preparedness in Bangladesh | [Link](https://servir.icimod.org/news/underpinning-flood-preparedness-in-bangladesh/) | FFWC and HIWAT context |
| S6 | News | The Business Standard — Causes behind recent floods | [Link](https://www.tbsnews.net/bangladesh/beyond-rain-causes-behind-recent-floods-927551) | Upstream data sharing |
| S7 | Slides | BWDB FFWC presentation | [Link](https://pani.bwdb.gov.bd/archive/pdf/190.pdf) | FFWC limitations |
| S8 | Web | NASA Earthdata — Global river discharge release | [Link](https://www.earthdata.nasa.gov/news/nasa-cnes-release-unprecedented-global-river-discharge-water-quality-estimates) | SWOT discharge availability |
| S9 | Code | SWOT-Confluence | [GitHub](https://github.com/SWOT-Confluence) | Prior art |
| S10 | Web | CNES SWOT mission | [Link](https://cnes.fr/en/projects/swot) | Mission specifications |
| S11 | Paper | HESS 30, 4141 (2026) — SWOT in narrow rivers | [Link](https://hess.copernicus.org/articles/30/4141/2026/) | Revisit frequency, accuracy |
| S12 | Doc | GPM IMERG V07 technical documentation | [Link](https://gpm.nasa.gov/resources/documents/imerg-v07-technical-documentation) | Latency specification |
| S13 | Web | PO.DAAC webinar — SWOT hydrology services (Hydrocron) | [Link](https://earthdata.nasa.gov/s3fs-public/2025-07/ED%20Webinar%20-%20PODAAC%20SWOT%20Hydro%20Tools%20-%20July%202025.pdf) | Data access |

## Claims Ledger

| Claim | Source | Value | Status |
|---|---|---|---|
| 2017: Boro affected in Sunamganj haors | S2 | 149,224 ha; 393,855 t; 315,084 farmers | VERIFIED |
| 2026: basin rainfall above normal | S3 | ~76% (cited to The Daily Star) | SECONDARY |
| 2026: Boro flooded | S3 | >130,000 acres; ~214,000 t; ~230,000 farmers | SECONDARY |
| FFWC five-day forecasts | S5 | 25 stations in flash-flood areas | VERIFIED |
| FFWC upstream data limited | S5, S7 | Listed as limitation in FFWC materials | VERIFIED |
| HIWAT forecast range | S5 | 54-hour, minor river segments | VERIFIED |
| SWOT discharge published | S8 | L4 SoS V3, reanalysis mode | VERIFIED |
| SWOT river width limit | S10 | >100 m (goal 50 m) | VERIFIED |
| SWOT revisit | S10, S11 | 21-day repeat | VERIFIED |
| SWOT height error | S11 | ~0.18 m (rivers >100 m) | VERIFIED |
| IMERG Early latency | S12 | ~4 hours; 0.1°, half-hourly | VERIFIED |
