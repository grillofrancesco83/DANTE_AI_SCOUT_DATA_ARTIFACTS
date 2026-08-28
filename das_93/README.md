# DAS-93 — Observational phenology evidence package v1

## Purpose

This package records public observational evidence for seven DANTE AI Scout species without converting incomplete observations into biological season boundaries.

It is a scientific decision package, not a runtime calendar.

## Files

- `DAS_93_OBSERVATIONAL_PHENOLOGY_EVIDENCE_V1.json`: canonical machine-readable decision artifact.
- `SOURCES.csv`: compact source inventory.
- `SHA256SUMS`: integrity manifest.

## Evidence vocabulary

- `PRESENCE_EVIDENCE`: the species was observed at the reported place and time.
- `LOCAL_PEAK`: repeated monitoring supports a peak only for the studied place, years and protocol.
- `BOUNDARY_NOT_AUTHORIZED`: available evidence cannot define a complete biological window or prove absence outside observed dates.

## GBIF snapshot

The GBIF section was acquired on 2026-08-28 using:

```text
country=IT
basisOfRecord=HUMAN_OBSERVATION
hasCoordinate=true
taxon_key=<species key>
```

Only monthly counts are retained. GBIF observations are presence-only and heterogeneous. Their distribution reflects observation effort, platform participation, accessibility and identification practices. Zero records in a month means “no retained record in this snapshot”, not biological absence.

## Scientific interpretation

The strongest Italian field evidence in this release concerns `Tuber borchii`: five natural production areas in the Maremma Regional Park were surveyed weekly from January through April for four years, yielding 1,033 morphologically identified fruitbodies and a local February–March production peak. Because May–December were not surveyed, the study cannot establish absence outside January–April.

Published `Tuber magnatum` studies provide geographically attributable mature and summer fruitbody observations, but use targeted sampling rather than uniform year-round monitoring. They demonstrate why biological presence, commercial maturity and legal harvestability must remain separate.

For `Boletus edulis` and `Cantharellus cibarius`, GBIF provides substantial Italian occurrence coverage, dominated by presence-only observations. These data can describe observed dates but cannot establish absence or a veto.

## Runtime decision

This package does **not** authorize:

- changes to `phenology-v1`;
- `IN_SEASON` or `OUT_OF_SEASON` boundaries;
- changes to Today, Habitat, Annata, `overall-v1` or Affidabilità;
- reopening or implementing `overall-v2`;
- any fallback from the DAS-91 legal collection calendar;
- frontend-derived phenology.

`phenology-v1` must remain fail-closed where no later, separately approved claim-bound rule exists.

## Required next evidence

A future boundary decision requires, at minimum:

1. year-round or explicitly complete seasonal sampling;
2. documented sampling effort and non-detections;
3. multiple years;
4. multiple Italian territories, elevations and habitats;
5. verified species identification and maturity stage;
6. explicit uncertainty and transferability analysis;
7. separation from legal collection periods.

