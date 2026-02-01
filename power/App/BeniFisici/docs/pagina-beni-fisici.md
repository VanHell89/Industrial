# Beni Fisici – Veicoli, Lusso, Arte, Metalli (draft v0.1)

Obiettivo: tracciare e valutare beni fisici (auto, moto, barche, orologi, arte, gioielli, oro/argento) con costi di possesso, manutenzione e assicurazione, integrandoli nel patrimonio e nel cashflow.

## Ambito e principi
- Ogni bene è un asset con identificativo, categoria e valore corrente stimato.
- Tracciamento costi ricorrenti: assicurazioni, bollo/tassa di possesso, manutenzione, storage, carburante (opzionale), refit/restauro.
- Ricavi/eventi: vendita, noleggio/charter (se applicabile), plus/minusvalenze.
- Integrazione con PGRF e Protezione per flussi e coperture assicurative.

## Categorie supportate (estendibili)
- Auto, Moto, Barche, Camper, Bici, Orologi, Arte, Gioielli, Oro/Metalli fisici, Altro bene durevole.

## Dati per bene
- Identificativi: nome, categoria, marca/modello, anno, serial/VIN (se presente).
- Valori: costo di acquisto, data acquisto, valore corrente stimato, fonte valutazione, ammortamento/dep. annua stimata.
- Coperture: polizza collegata (dalla pagina Protezione), massimale, scadenza.
- Costi ricorrenti: assicurazione, bollo, manutenzione, storage.
- Stato: in uso, in deposito, in vendita.

## Nuovi tipi di transazione (tag Ledger)
- `Asset Purchase (Physical)` — prezzo, fees, eventuale trade-in.
- `Asset Maintenance` — categoria costi (ordinaria/straordinaria).
- `Asset Insurance Premium` — collegato a polizza_id.
- `Asset Tax` — bollo/tassa possesso.
- `Asset Storage` — rimessaggio/porto/garage.
- `Asset Fuel/Usage` (opz.) — per TCO analitico.
- `Asset Sale` — valore di realizzo, costi vendita, plus/minusvalenza.
- `Asset Rental/Charter Income` — incassi collegati al bene.

## UI proposta
- Header bene: valore corrente, delta vs costo, stato, copertura assicurativa.
- Sezione “Cost of Ownership”: tabella e grafico stacked dei costi ricorrenti per periodo.
- Sezione “Valutazione & Deprezzamento”: curva valore stimato nel tempo, LTV se bene finanziato.
- Sezione “Cashflow bene”: entrate (noleggi) e uscite (manutenzioni, assicurazioni) filtrabili.
- Sezione “Asset list”: griglia con filtri per categoria/stato, CTA `+ Bene fisico`.

## KPI
- `TCO annuo` = assicurazione + tasse + manutenzione + storage (+ fuel opz.).
- `Net Yield` per beni a reddito (charter/noleggio) = (entrate - costi ricorrenti) / valore medio.
- `Deprezzamento annuo stimato` = (valore acquisto - valore corrente) / anni possesso.
- `Coverage gap` = massimale polizza / valore corrente (per capire sotto/over-insurance).

## Integrazioni
- Ledger: tag `physical_asset` + `asset_id` per tutte le transazioni del bene.
- Protezione: link diretto alla polizza; reminder scadenza premi.
- PGRF: i costi ricorrenti entrano tra le Uscite; eventuali noleggi/charter tra le Entrate.
- Portfolio: i beni fisici compaiono come categoria “Physical Assets” o sottocategoria Alternatives.

## Roadmap
1) MVP: anagrafica bene + costi ricorrenti + TCO annuo + collegamento polizza.
2) Step 2: curve deprezzamento/valutazione storica e comparatori per categoria.
3) Step 3: income da noleggio/charter e KPI di rendimento netto.
4) Step 4: alert scadenze bollo/assicurazione e suggerimenti coverage gap.
