# DAS-91 — Pacchetto calendario legale di raccolta

## Stato

`PARTIAL_IMPLEMENTABLE`

Il pacchetto contiene calendari ufficiali per 17 Regioni italiane e per la Provincia autonoma di Trento. Sardegna, Valle d’Aosta e Provincia autonoma di Bolzano sono intenzionalmente `UNKNOWN` e non devono ricevere fallback.

## File

- `DAS_91_LEGAL_COLLECTION_CALENDAR_V1.json`: input machine-readable.
- `DAS_91_LEGAL_COLLECTION_CALENDAR_V1.sha256`: identità del file da verificare nel checkout.

## Semantica

Il dataset determina esclusivamente se la raccolta è consentita dalla normativa applicabile. Non rappresenta la crescita, la presenza o la maturazione biologica della specie.

Non deve modificare:

- `phenology-v1`;
- Habitat;
- Annata;
- Ricerca oggi;
- Affidabilità;
- `overall-v1`.

## Formato delle finestre

- `MM-DD/MM-DD`: estremi inclusivi.
- Se la data finale precede quella iniziale, l’intervallo attraversa l’anno.
- `LAST_SUNDAY_MM`: ultima domenica del mese.
- `LAST_DAY_02`: ultimo giorno di febbraio, incluso il 29 negli anni bisestili.

## Precedenza

1. area speciale;
2. Comune;
3. provvedimento temporaneo vigente;
4. Provincia;
5. Regione/Provincia autonoma;
6. `UNKNOWN`.

## Vincoli di integrazione

- Gli atti temporanei scaduti rimangono nel dataset come regressione negativa e non vanno applicati nel 2026.
- Le limitazioni che richiedono geometrie o inventari locali non ancora presenti devono produrre `UNKNOWN` nell’area dubbia, non una decisione approssimata.
- `ALLOWED` e `NOT_ALLOWED` richiedono sempre una fonte e una regola applicabile.
- I link istituzionali sono locator delle fonti; durante l’integrazione Codex deve validarli sintatticamente ma non sostituirli con fonti commerciali.

## Caso Bianchetto in agosto

Nelle giurisdizioni che dispongono di una finestra ufficiale incompatibile con agosto, il calendario legale restituisce `NOT_ALLOWED`. Il punteggio `overall-v1` rimane invariato e la fenologia biologica conserva il proprio stato indipendente.

## Limitazione dichiarata

Il locator digitale dell’Ordinanza Borgorose 81/1995 non è disponibile nel pacchetto. La regola è registrata con estremi dell’atto, ma deve essere mantenuta come `SOURCE_METADATA_INCOMPLETE` fino all’acquisizione di una copia ufficiale o di un URL comunale stabile. Non eliminare l’override e non promuoverlo silenziosamente a fonte completa.
