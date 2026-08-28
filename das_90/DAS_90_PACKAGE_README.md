# DAS-90 — External scientific review for canonical phenology

## Purpose

This package is a closed, machine-readable evidence input for DANTE AI Scout. It evaluates whether the seven registered taxa have sufficiently precise scientific support for deterministic biological phenology rules in Italy.

It does **not** contain legal collection calendars. Legal collection is owned separately by DAS-91 and must never be used as a proxy for biological presence, maturation or search suitability.

## Files

- `DAS_90_CANONICAL_PHENOLOGY_EVIDENCE_V1.json` — evidence inventory, claim-level limitations and species decisions.
- `SHA256SUMS` — immutable digest for every deliverable in this package.

## Decision standard

A runtime boundary is authorized only when a source supports all of the following:

1. the exact taxon;
2. biological rather than legal semantics;
3. transferability to Italy;
4. an explicit start and end boundary;
5. an independently verifiable locator.

Observed harvest months, production peaks and market seasons are retained as evidence but are not silently converted into inclusive biological boundaries.

## Result

The external review is complete, but the runtime input package remains partial:

| Taxon | Evidence retained | Runtime boundary |
|---|---|---|
| `Tuber aestivum` | Central-European fruiting observations, climate sensitivity | Not authorized for all Italy |
| `Tuber uncinatum` | Taxonomic evidence of conspecificity with `T. aestivum` | Separate rule not authorized |
| `Tuber borchii` | Italian production peak in February–March | Exact season not authorized |
| `Tuber magnatum` | Italian autumn/early-winter season, September–December | Exact day boundaries not authorized |
| `Tuber melanosporum` | Italian orchard developmental stages and winter ripeness | Nationwide binary season not authorized |
| `Boletus edulis` | Weather-, habitat- and management-dependent fruiting | Static national boundary not authorized |
| `Cantharellus cibarius` | Climate-sensitive European fruiting observations | Static Italian boundary not authorized |

Therefore the package must **not** cause `phenology-v1` to emit `OUT_OF_SEASON` from unsupported calendar boundaries. The safe current runtime result remains `UNKNOWN` when no separately authorized rule exists.

## Required invariants

- no runtime network access;
- no frontend-derived phenology;
- no use of DAS-91 legal dates as biological evidence;
- no change to Today, `overall-v1` or Affidabilità;
- no species boundary inferred from a production peak;
- no independent `Tuber uncinatum` rule until taxonomy and product semantics are reconciled;
- no backfill of historical assessments.

## Source policy

The JSON includes peer-reviewed primary research and reviews used for the decision. Commercial calendars, nursery pages, Wikipedia and generic field guides were inspected only during discovery and excluded as authorities.

## Integration gate

Before integration, Codex must:

1. verify the package checksum;
2. validate JSON syntax and required fields;
3. preserve each source limitation;
4. refrain from implementing month/day boundaries because this package authorizes none;
5. document the mixed evidence outcome in DAS-90, DAS-87 and the architecture audit;
6. keep DAS-85 blocked.

## Final gates

```text
SEVEN SPECIES EXTERNAL REVIEW = COMPLETE
CLAIM-BOUND SOURCE INVENTORY = COMPLETE
LEGAL / BIOLOGICAL SEPARATION = PASS
NO INVENTED WINDOWS = PASS
TAXONOMIC CONFLICT = ASSESSED
ITALY-WIDE EXACT BOUNDARIES = NOT AUTHORIZED
PHENOLOGY-V1 RUNTIME UPDATE = BLOCKED
DAS-87 INPUT PACKAGE = PARTIAL / NOT RUNTIME-READY
DAS-85 REOPENING = NOT AUTHORIZED
```
