# Dashboard – Riepilogo Patrimonio (bozza)

Scopo: dare in apertura una vista unica del patrimonio netto e della salute finanziaria, con indicatori sintetici e spiegabili.

## Utente e bisogni
- Power user che vuole controllare in pochi secondi stato attuale e variazione vs ultimo periodo.
- Rilevazione immediata di warning: leva eccessiva, cassa negativa, burn rate fuori target.

## Metriche chiave (widget testuali con trend)
- Patrimonio netto (Net Worth): asset totali – passivi totali; mostra valore corrente, variazione % vs mese precedente, sparkline 12 mesi.
- Leva finanziaria: debt / net worth; soglia alert configurabile; badge colore (verde <0.3, giallo 0.3–0.6, rosso >0.6).
- Free Cash Flow (mensile): cash inflow operativi + finanziari – outflow ricorrenti; mostra media 3 mesi e forecast mese in corso.
- Liquidita immediata: cassa + conti correnti / spese medie mensili; espressa in mesi di runway.
- Risparmio netto: saving rate = (entrate nette – uscite ricorrenti)/entrate nette; mostra target utente se definito.
- Performance portafoglio: rendimento YTD e MTD, separato da effetto contributi (money-weighted return vs price return).
- Concentrazione asset: peso top 3 asset / totale; alert se >40%.
- Copertura impegni debito: rapporto tra FCF e servizio del debito mensile; alert se <1.2x.

## Dati e regole di calcolo
- Valorizzazioni asset e passivi a data T; FX deterministico con stessa fonte usata in Portfolio.
- Cashflow classificati (operativi, finanziari, una tantum) provenienti da Transaction/Ledger.
- Spese medie mensili: media mobile 3 o 6 mesi (configurabile), esclusi one-off > soglia.
- Servizio debito: ratei capitale+interessi attesi nei prossimi 30 giorni.
- Contributi vs rendimento: usare cashflow di tipo "apporto" per isolare effetto prezzo.
- Gestire multi-valuta via valuta di reporting scelta in Settings; esporre conversion rate usato (tooltip).

## Layout proposto
- Header: net worth e variazione %; pulsante "Vedi movimenti" punta a Ledger filtrato ultimo mese.
- Griglia 2x2 di KPI principali: leva, FCF, liquidita (runway), saving rate.
- Riga grafica: sparkline net worth 12 mesi; grafico barre stacked per inflow/outflow mensili.
- Sezione rischi: card con alert (leva alta, runway <3 mesi, copertura debito <1.2x, concentrazione >40%).
- Pulsante "Drill-down" per ogni KPI che apre vista dedicata (Portfolio, Cashflow, Liabilities) con filtro preimpostato.

## Interazioni e UX
- Hover/tooltip mostra formula, data di ultimo aggiornamento e fonte dati.
- Toggle "Nascondi valori" per privacy in mobilita.
- Segmentazione rapida per profilo: consolidato famiglia vs singolo account (switch in header).

## Aggiornamento e qualità
- Refresh: ogni sync dati o almeno 1 volta al giorno; indicare timestamp ultimo calcolo.
- Validazioni: net worth deve conciliare con somma asset-passivi esposta in Portfolio; FCF deve conciliare con cash delta in Ledger per periodo.
- Tracciamento errori: se un feed e degradato, mostra etichetta "dati parziali" e disabilita trend.

## Edge case
- Patrimonio negativo: mostra valore con colore neutro, ma leva e runway diventano alert automatici.
- Nessun dato storico: mostra placeholder e guida a importare transazioni o collegare conti.
- Multi-valuta con asset illiquidi: se manca FX recente, marcare stima e usare ultimo cambio disponibile.
