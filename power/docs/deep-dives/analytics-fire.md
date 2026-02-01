# Power – Deep Dive: Analytics / FIRE (bozza)

Scopo: definire cosa significa “cuore analitico” di Power e come produce insight sul percorso FIRE.

## Obiettivi analitici
- Misurare progresso verso FIRE (target capitale / spesa annua / SWR).
- Cash runway e burn rate forecast.
- Asset allocation vs policy target con raccomandazioni di ribilanciamento.

## Input dati
- Transazioni normalizzate e valorizzate.
- Valori di mercato e tassi FX.
- Parametri utente: target FIRE, SWR, inflazione attesa, contributi.

## Output attesi
- Indicatori chiave: NW attuale, gap to FIRE, mesi di runway.
- Grafici serie storiche e proiezioni.
- Alert/suggerimenti: contribuzione consigliata, ribilanciamento, rischio concentrazione.

## Metodi e regole
- Calcolo NW: asset liquidi + investimenti – liabilities.
- Proiezioni: scenari deterministici con parametri configurabili (no black box).
- Sensitivity analysis su rendimento, inflazione, spesa.

## Qualità e spiegabilità
- Ogni numero deve essere replicabile: mostrare formula e input.
- Versionare le regole di calcolo per audit e rollback.

## Prossimi passi
- Definire schema dati per parametri utente e scenari.
- Prototipo notebook o script per validare calcoli con dataset sintetico.

