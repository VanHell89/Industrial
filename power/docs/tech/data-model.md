# Power – Modello Dati (bozza)

Scopo: descrivere entità, relazioni e campi chiave a supporto di Power come strumento decisionale.

## Entità principali
- Asset
- Transaction
- Cashflow
- Account / Wallet
- Liability (debiti, mutui, carte)
- Category / Tagging

## Relazioni (draft)
- Asset 1–N Transaction
- Account 1–N Transaction
- Cashflow aggrega Transaction per periodo e regole
- Liability 1–N Cashflow (rimborsi)
- Category applicabile a Transaction e Asset

## Campi chiave (per entità)
- TODO: dettagliare campi minimi, tipologia, vincoli, derivati.

## Regole di business da catturare
- Valorizzazione deterministica (niente mapping manuale).
- Gestione multi–valuta e fx source.
- Tracciamento stato e qualità dati (ingest, enrichment, output).

## Prossimi passi
- Disegno E/R o UML leggero.
- Glossario attributi e unit test sui calcoli core.

