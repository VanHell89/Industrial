# Power – Roadmap Funzionale (bozza)

Scopo: ordinare le funzionalità per priorità (core, avanzate, nice-to-have) e sequenziarle.

## Core (MVP decisionale)
- Ingest fonti bancarie/broker API e file standard (CSV, OFX).
- Normalizzazione e deduplicazione transazioni.
- Calcolo posizione netta, NAV, liquidità disponibile.
- Dashboard stato FIRE e cash runway.

## Avanzate
- Regole di categorizzazione personalizzabili ma deterministiche.
- Scenario engine (what-if su contribuzioni, rendimento, inflazione).
- Alerting su anomalie e deviazioni da target asset allocation.
- Supporto multi-currency con rate provenance.

## Nice to have
- Integrazione fiscale base (plus/minus valenze).
- Collaboration/coach mode con accesso controllato.
- Export verso strumenti terzi (Sheets, Notion, API).

## Sequencing draft
1. Core ingest + calcolo NAV/liquidità.
2. Dashboard FIRE/cash runway.
3. Alerting baseline + categorizzazione avanzata.
4. Scenario engine.
5. Integrazioni nice-to-have.

## Dipendenze/assunzioni
- Accesso stabile alle fonti dati primarie.
- Libreria tassi FX affidabile e ripetibile.

