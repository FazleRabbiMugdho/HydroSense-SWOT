# Problem & Challenge Statement — HydroSense-SWOT

## The Problem

Boro rice in the Haor basin of north-east Bangladesh ripens in April, and the harvest runs into May. In that window, heavy rain on the Meghalaya hills across the border can raise the Surma, Kushiyara and their tributaries within days and breach the embankments around the fields.

In April 2026, rainfall across the basin's seven districts ran about 76% above normal, floodwater covered more than 130,000 acres of Boro, and reports put the loss at about 214,000 tonnes of rice and about 230,000 farmers. In 2017 the same kind of flood damaged Boro on 149,224 hectares in Sunamganj district alone.

A farmer has one decision to make before the water arrives: cut grain that is not fully ripe, or leave it and risk losing all of it.

## The Forecasting Gap

The forecasts that exist were not built to answer that question for the border rivers:

- **FFWC (Bangladesh):** Issues five-day forecasts for 25 stations in flash-flood areas, using national rainfall forecasts and a hydrological model. It lists limited upstream data and short lead time among its own limitations. Most floods start in rivers outside Bangladesh, where it has no readings it can use.
- **SERVIR HIWAT:** Adds 54-hour forecasts for small rivers, but estimates flow from rainfall and a model with few measurements of the river channel itself.
- **SWOT satellite:** Measures water height, width and slope along 10-kilometre river reaches, and NASA already publishes discharge computed from it. But SWOT passes over a given river about once to a few times in 21 days and resolves rivers wider than about 100 metres — it cannot work as a live gauge and cannot see the narrow headwater channels.

No published tool uses SWOT to set the channel parameters, runs satellite rainfall through them within hours, and turns the result into a harvest decision for these rivers.

## What We Will Build

We take SWOT reach measurements, fit channel roughness and bed level against downstream BWDB gauge records, and route GPM IMERG Early Run rainfall (available about four hours after observation) through those reaches to flag when a flood is likely and which upazilas should harvest early.

We will replay the April 2026 flood using only data available before each date, keep calibration and test periods separate, and report Nash–Sutcliffe efficiency, bias and warning lead time against the gauges, next to a rainfall-only baseline.

## Commitments

1. **One basin, one replay.** The April–May 2026 event, within SWOT's science orbit.
2. **Error figures come from code.** No metrics are shown until computed on the test period.
3. **Baseline beside the result.** If we cannot beat a rainfall-only baseline, we say so and show what SWOT adds.
4. **Near the border means the wide rivers.** Surma and Kushiyara reaches wide enough for SWOT (checked against SWORD reach widths), not inside Meghalaya.

## Facts Used

| Claim | Source | Status |
|---|---|---|
| April 2026 basin rainfall ~76% above normal; >130,000 acres flooded; ~214,000 t lost; ~230,000 farmers | Think Global Health, 8 Jun 2026, citing Daily Star and Prothom Alo | SECONDARY |
| 2017: 149,224 ha of Boro affected in Sunamganj | J. Environ. Sci. & Nat. Resour. 12(1&2), 2019 | VERIFIED |
| FFWC five-day forecasts for 25 stations; limited upstream data | ICIMOD / SERVIR-HKH; FFWC slides | VERIFIED |
| SERVIR HIWAT 54-hour flash-flood forecasts | ICIMOD | VERIFIED |
| SWOT 10 km reaches; 21-day repeat; rivers >100 m | CNES; OSTST 2023; HESS 2026 | VERIFIED |
| IMERG Early Run ~4 hours after observation | GPM documentation | VERIFIED |
