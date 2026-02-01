# MigiTech – Classificazione Cedole / Dividendi in fase di registrazione

Contesto: in **New Transaction**, il campo **Type** include “Dividend” ma non richiede di specificare la natura del flusso. Serve distinguere subito la tipologia per usare correttamente la pagina **Cedolare**.

## Obiettivo
Classificare ogni transazione di tipo **Dividend** al momento dell’inserimento per avere flussi già etichettati, senza riclassificazioni manuali, e abilitarne l’aggregazione/analisi in Cedolare.

## Condizione di attivazione
- Si applica solo se **Type = Dividend**.
- Per gli altri tipi (Buy, Sell, Deposit, Withdrawal, Fee, Interest, Transfer) nulla cambia.

## Nuovo campo
- **Nome:** Classificazione Cedola / Dividendo  
- **Tipo:** selezione obbligatoria (dropdown o radio).  
- **Opzioni fisse (5):**
  - Cedola – Fondo obbligazionario
  - Cedola – Fondo bilanciato
  - Cedola – Fondo azionario
  - Dividendo – Azione
  - Dividendo – REIT

## Validazione
- Se **Type = Dividend**, il campo è obbligatorio.
- Se manca la selezione: bloccare salvataggio e mostrare errore “Seleziona la classificazione della cedola/dividendo”.
- Se **Type ≠ Dividend**, il campo non è visibile né richiesto.

## Logica dati
- Salvare la classificazione come attributo della transazione (enum/valori fissi).
- Nomi tecnici possibili: `dividend_classification` o `cashflow_classification`.

## Uso nella pagina Cedolare
- Aggregare i flussi per tipologia.
- Calcolare totali e percentuali.
- Analizzare serie temporali già segmentate.
- Generare grafici/report senza mapping manuale.

## Coerenza con Categoria Asset
- Questa è una classificazione del **flusso**, non dello strumento.
- Non sostituisce la categoria asset (fondi, azioni, REIT); serve solo per l’analisi delle cedole/dividendi.

## Scope
- Modifica limitata a **New Transaction**.
- Nessun impatto sugli altri tipi di transazione.
- Niente categorie personalizzabili in questa fase.
- Soluzione retrocompatibile.
