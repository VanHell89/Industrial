# PoWeR – Pagina Immobiliare (draft v0.1)

Obiettivo: gestire l’intero ciclo di vita di un immobile (acquisto → gestione → dismissione) con flussi dedicati e tracciabilità completa dentro PoWeR.

## Scopo e confini
- Tracciare asset “Immobile” con dati anagrafici, valore storico e valore corrente.
- Coprire costi/ricavi ricorrenti: mutui, tasse, manutenzione, assicurazioni, affitti/locazioni.
- Consentire registrazioni veloci tramite un pulsante `+ Immobiliare` nel menù Transaction (shortcut dedicato).
- Tenere separati i flussi immobiliari dagli altri asset ma integrarli in Net Worth e Cash Flow.

## Dati chiave dell’immobile
- Identificativi: nome, indirizzo (opzionale), tipologia (residenziale, commerciale, terreno), stato (in uso, a reddito, in vendita).
- Valori: prezzo di acquisto, data rogito, valore corrente stimato, valore perizia, LTV corrente.
- Struttura costi/ricavi: canone atteso/effettivo, spese condominiali, manutenzione, assicurazioni, IMU/TASI/TARI, imposte di registro, notaio.
- Finanziamento: mutuo associato (importo, durata, tasso, rata, quota capitale/interessi), eventuale anticipo/caparra.

## Nuovi tipi di transazione (scope frontend)
- `Purchase (Real Estate)` — registra acquisto: prezzo, tasse/fees, anticipo, mutuo associato.
- `Mortgage Payment` — quota capitale + quota interessi + assicurazione mutuo.
- `Property Tax` — IMU/TASI/TARI (campi: anno, rata, importo).
- `Maintenance` — straordinaria/ordinaria (categoria selezionabile).
- `Insurance` — premio polizza casa.
- `Rent Income` — canone incassato, periodo di competenza, eventuale ritenuta/acconto imposta.
- `Rent Vacancy` (opz.) — periodi senza incasso per analisi di rendimento.
- `Sale (Real Estate)` — valore di vendita, costi di dismissione, plus/minusvalenza.

## UI: layout pagina Immobiliare
- **Header immobile**: foto/icone, stato, valore corrente, LTV, redditività lorda/netta.
- **Card “Sintesi finanziaria”**: prezzo acquisto, caparra/anticipo, mutuo residuo, equity, rendimento attuale (gross/net yield).
- **Sezione “Cash Flow”**: lista transazioni immobiliari filtrabili per tipo/data; CTA `+ Immobiliare` apre form pre-filtrato sui tipi sopra.
- **Sezione “Affitti”**: calendario competenze e incassi, vacancy rate, confronti atteso vs incassato.
- **Sezione “Mutuo”**: piano ammortamento sintetico, prossima rata, breakdown quota capitale/interessi.
- **Sezione “Tasse & Spese”**: tabella annuale (IMU/TASI/TARI), manutenzioni, assicurazioni.
- **Sezione “Valutazione”**: valore corrente, delta vs costo, IRR/ROI semplice, eventuale collegamento a valutazioni esterne (placeholder).

## Calcoli e KPI principali
- `Gross Yield` = canone annuo / prezzo di acquisto.
- `Net Yield` = (canone annuo – spese ricorrenti – tasse immobiliari) / prezzo di acquisto.
- `Cash-on-Cash` = (cash flow annuale netto) / (capitale proprio investito).
- `LTV` dinamico = mutuo residuo / valore corrente.
- `Vacancy Rate` = mesi non affittati / 12 (o periodo selezionato).

## Integrazione con modelli esistenti
- Ledger: le nuove transazioni immobiliari salvano su Ledger con tag `immobiliare` + `property_id`.
- Portfolio: l’immobile compare come asset “Real Estate” con valore corrente; mutuo come liability collegata.
- Allocation+: nuova categoria suggerita “Real Estate” (o sottocategoria di Alternatives) per riflettere il peso patrimoniale.
- Cedolare: i `Rent Income` possono alimentare viste di cashflow ricorrente (non dividend), mantenendo separazione semantica.

## Form di creazione/edizione immobile
- Campi obbligatori: nome, tipologia, data acquisto, prezzo acquisto, valuta.
- Facoltativi ma consigliati: indirizzo, metratura, capex iniziale, notaio/tasse.
- Collegamento mutuo: seleziona o crea mutuo; consente calcolo rata attesa e LTV immediato.

## Task UX iniziali
- Aggiungere CTA globale `+ Immobiliare` vicino a `+ Transaction`.
- Nel form, preimpostare tipo transazione in base al contesto (es. “Rent Income” se aperto da sezione Affitti).
- Filtri rapidi per periodo fiscale (anno solare) e per immobile se multi-property.

## Aperture / TODO
- Gestione rivalutazioni automatiche (es. indice ISTAT) — deciso in fase 2.
- Integrazione con allegati (rogito, perizia, contratti locazione).
- Gestione differenze fiscali tra mercati (IT: cedolare secca vs IRPEF; US: property tax/local).
- Esportazione dati immobile per banca/assicurazione.
