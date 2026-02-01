# PGRF – Piano di Gestione Risorse Finanziarie (ex PAC) · draft v0.1

Scopo: un’unica vista che confronta flussi in entrata e in uscita e mette in relazione portafoglio finanziario, immobili e leva (mutui/prestiti). È l’evoluzione del PAC manager, focalizzata sul bilanciamento risorse/obblighi.

## Obiettivi
- Tracciare contributi periodici, cedole/dividendi netti, affitti, altri ingressi di cassa.
- Collegare le uscite strutturali: rate mutuo immobili, prestiti, assicurazioni, piani di accumulo, spese ricorrenti.
- Misurare sostenibilità e leva: rapporto tra patrimonio (portafoglio + immobili) e debito residuo, copertura rate con cashflow ricorrente.
- Simulare aggiustamenti: aumentare contributi, estinguere mutui, cambiare leva target.

## Flussi in entrata (E)
- Contributi ricorrenti (DCA/PAC su portafoglio).
- Cedole/Dividendi netti (collegati a Ledger, filtrabili).
- Affitti netti da immobili (collegati a pagina Immobiliare).
- Altri ingressi (bonus, stipendi variabili, ecc.).

## Flussi in uscita (U)
- Rate mutuo (quota capitale + interessi + assicurazioni).
- Rate prestiti personali.
- Spese immobiliari ricorrenti (tasse property, manutenzione base).
- Versamenti PAC/DCA se considerati uscita (toggle “tratta contributo come uscita”).
- Spese fisse/variabili opzionali (placeholder per future integrazioni).

## KPI chiave
- `Net Cashflow = ΣE – ΣU`
- `Coverage Rate = ΣE / ΣU` (soglia >1 = sostenibile).
- `Debt to Assets = Debito residuo / (Portafoglio + Immobili)`
- `Cash-on-Cash after debt service = (ΣE – rate mutuo/prestiti) / capitale proprio investito`
- `Runway rate-friendly`: mesi coperti dalle entrate ricorrenti prima di intaccare cassa.

## UI proposta
- Header con riepilogo KPI (Net Cashflow, Coverage, Debt/Assets, LTV complessiva).
- Due colonne:
  - Entrate: tabella/stacked bar per fonte (Cedole nette, Affitti, Contributi, Altro).
  - Uscite: tabella/stacked bar per destinazione (Mutui, Prestiti, Spese immobiliari, PAC come uscita, Altro).
- Sezione “Leva e patrimonio”: card con patrimonio totale (portafoglio + immobili), debito residuo, LTV combinata e scenari di riduzione debito.
- CTA `+ PGRF` (ex `+ PAC`) per aggiungere contributi o legare nuovi flussi (mutui, affitti, cedole) tramite selettori esistenti in Ledger/Immobiliare.

## Integrazioni necessarie
- Ledger: taggare flussi con `income_type` / `outflow_type` e link a `property_id` o `asset_id` per aggregazioni coerenti.
- Immobiliare: importare rate mutuo e affitti netti come uscite/entrate nella vista PGRF.
- Cedolare: cedole/dividendi netti confluiscono automaticamente tra le Entrate.
- Portfolio: usa controvalore portafoglio corrente per rapporti di leva; aggiungere toggle “includi cassa”.

## Roadmap ridotta
1) MVP: somma entrate/uscite da Ledger con mapping manuale di categoria; KPI Net Cashflow & Coverage.
2) Step 2: integrazione Immobiliare (mutui/affitti) e Cedolare (cedole nette).
3) Step 3: scenari leva (estinzioni parziali, aumento contributi) con what-if.
4) Step 4: alerting soglie (coverage <1, debt/assets >60%).
