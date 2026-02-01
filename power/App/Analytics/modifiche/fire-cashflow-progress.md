# MigiTech – FIRE Cash Flow: correzione calcolo “Progress”

Contesto: nella componente FIRE Cash Flow i KPI sono corretti tranne **Progress**, che usa un valore non coerente (es. ultimo mese). Il Progress deve basarsi sul **Run Rate** medio del periodo selezionato.

## Obiettivo
Calcolare Progress come avanzamento verso il Target Income usando la media degli incassi (Run Rate) sul periodo selezionato, non l’ultimo mese.

## Definizioni
- `target_income`: target impostato (€/mese).
- `run_rate`: media mensile degli incassi “Received” (Type = Dividend) nel periodo selezionato (es. ultimi 12 mesi).

## Formula corretta
- `progress = run_rate / target_income`
- Visualizzazione: `progress_pct = (run_rate / target_income) * 100`

## Vincoli
- Se `target_income = 0` → progress = 0 (o n/a; evitare divisione per zero).
- Se `progress_pct > 100%`: opzione A mostrare >100 (es. 120%); opzione B clamp 100%. Suggerito: mostrare >100 perché informativo.

## Coerenza KPI
- Progress deve usare le stesse variabili di Gap e Time to Target (target_income, run_rate).

## Test rapido (esempio)
- Target Income = 500 €/mese
- Run Rate (12m avg) = 250 €/mese
- Progress = 250 / 500 = 0.5 → 50%
- Gap atteso: 250 €
- Cambiando Run Rate Period (es. 6 mesi), Progress si aggiorna coerentemente.

## Note di implementazione
- Run Rate va calcolato come media mensile sul periodo scelto (non ultimo mese).
- Assicurare che il datasource “Received” usi Type = Dividend e stato coerente con la vista.
