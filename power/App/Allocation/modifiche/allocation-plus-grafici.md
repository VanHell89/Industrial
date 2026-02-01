# MigiTech – Allocation Plus: sottografici e breakdown per categorie

Contesto: ogni asset ha una **Categoria** assegnata in creazione (enum fisso: Fondi obbligazionari, Fondi bilanciati, Fondi azionari, Azioni, REIT). Allocation Plus oggi mostra KPI, grafico principale “Allocation” e tabella “Breakdown”. Si richiede di estendere la pagina con ulteriori grafici e breakdown.

## Obiettivo
Aggiungere sotto il grafico generale una sezione con più cards, ciascuna con pie/donut e relativa tabella di breakdown coerente con il grafico.

## Dati di base
Per ogni asset servono: `asset_id`, `asset_name`, `category` (enum 5 valori), `current_value` (EUR), opzionale `account/portfolio_id` se filtrato.
Tutti i grafici usano **current_value**, non i flussi.

## Grafici richiesti (nuove cards)
1) **Asset Allocation – Macro categorie (5)**  
   - Serie: somma valori per ciascuna delle 5 categorie.  
   - Breakdown: Categoria | Valore (EUR) | % su totale portafoglio | N. asset.

2) **Super Macro – Fondi / Azioni / REIT (3)**  
   - Serie: Fondi = (obbligazionari + bilanciati + azionari), Azioni, REIT.  
   - Breakdown: Categoria (Fondi/Azioni/REIT) | Valore | % su totale portafoglio | N. asset.

3) **Fondi – dettaglio per singolo fondo**  
   - Filtro: category ∈ {Fondi obbligazionari, Fondi bilanciati, Fondi azionari}.  
   - Serie: ogni spicchio = `asset_name`, value = `current_value`.  
   - Breakdown: Fondo | Categoria fondo | Valore | % su totale “Fondi” | (opzionale) % su totale portafoglio | N. asset (=1).

4) **Azioni – dettaglio**  
   - Filtro: category = Azioni.  
   - Breakdown: Azione | Valore | % su totale “Azioni” | (opzionale) % su totale portafoglio.

5) **REIT – dettaglio**  
   - Filtro: category = REIT.  
   - Breakdown: REIT | Valore | % su totale “REIT” | (opzionale) % su totale portafoglio.

## Regole di calcolo
- `total_portfolio_value = Σ current_value (asset nel filtro pagina)`.
- `% su totale portafoglio = value / total_portfolio_value`.
- `% su sottoinsieme (Fondi/Azioni/REIT) = value / total_subset_value`.
- Gestire eventuale “Unclassified”: se esistono asset senza categoria, opzionale 6ª categoria “Unclassified” oppure esclusi dai grafici (da concordare; preferito: categoria Unclassified se >0).

## Layout UI suggerito
- Cards sotto il grafico principale.  
- Riga 1: Macro 5 + Super Macro 3  
- Riga 2: Fondi + Azioni  
- Riga 3: REIT (full width o card singola)  
- Ogni card: titolo, pie/donut, tabella breakdown (sotto o a lato coerente con stile esistente).

## Comportamento dinamico
- I grafici reagiscono a filtri pagina (portafoglio/account, periodo, altri filtri esistenti).
- Valori e percentuali si aggiornano in tempo reale con i filtri.

## Note funzionali
- Serie e tabelle basate solo su valori correnti (no flussi).  
- Enum categorie hard-coded.  
- Richiesta copre solo UI + calcoli; modello dati già contiene categoria asset.  
- Retrocompatibile.
