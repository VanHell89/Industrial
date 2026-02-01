# MigiTech – Aggiunta Categoria Asset (Transaction / New Asset)

Contesto: la schermata **New Transaction** resta invariata (data, tipo, account, asset, unità, prezzo, fee, note). L’intervento è solo sul flusso di creazione di un nuovo asset/sottostante.

## Obiettivo
Associare una categoria univoca a ogni asset per aggregazione e visualizzazione corretta in **Allocation Plus**. La categoria è proprietà dell’asset, assegnata una sola volta in creazione, ereditata da tutte le transazioni future su quell’asset.

## Punto di intervento
- Flusso **Asset → New** (creazione nuovo sottostante).
- Nessuna modifica a **New Transaction**.

## Nuovo campo
- **Nome:** Categoria  
- **Tipo:** selezione obbligatoria (radio o select).  
- **Valori ammessi (fissi):**
  - Fondi obbligazionari
  - Fondi bilanciati
  - Fondi azionari
  - Azioni
  - REIT

## Validazione
- Campo obbligatorio.
- Non si può salvare un asset senza categoria.
- Messaggio suggerito: “Seleziona una categoria per il sottostante”.

## Comportamento applicativo
- La categoria **non è modificabile dalla singola transazione**.
- La categoria è modificabile **solo** dalla schermata di modifica dell’asset.
- Tutte le transazioni legate all’asset ereditano automaticamente la categoria.

## Impatto funzionale
- Allocation Plus può aggregare per categoria, calcolare percentuali/pesi, generare grafici coerenti.
- Evita classificazioni manuali postume; struttura pronta a estensioni (es. sottocategorie).

## Note
- Nessuna categoria personalizzabile in questa fase.
- Le 5 categorie sono hard-coded.
- Modifica limitata e retrocompatibile.
