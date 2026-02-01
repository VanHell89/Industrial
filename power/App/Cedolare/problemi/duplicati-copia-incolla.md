# Cedolare – Problema copie/incolla e voci duplicate

## Sintesi
Importando/incollando dati nella vista Cedolare, alcune righe non si allineano e compaiono voci duplicate. Serve un meccanismo che impedisca nomi identici e garantisca l’allineamento riga-per-riga.

## Problema osservato
- Copia e incolla da versione precedente genera righe disallineate.
- Le stesse voci compaiono più volte (duplicati).
- Non esiste controllo di unicità sul nome della voce cedolare/dividendo.

## Impatto
- Dati mostrati in righe sbagliate o ripetute.
- Totali e grafici possono risultare errati.
- Esperienza utente confusa (difficile capire se il dato è stato registrato correttamente).

## Ipotesi causa
- Parsing dell’input incollato che non rispetta la separazione di colonne (es. delimitatori non riconosciuti o newline incoerenti).
- Mancanza di vincolo di unicità sui nomi delle voci cedolari.

## Richiesta di fix
1. **Allineamento dati**: normalizzare l’input incollato (trim, delimitatori coerenti, controllo colonne) per garantire che ogni riga finisca nella riga corretta.
2. **Unicità voci**: introdurre vincolo (DB + UI) che impedisca l’inserimento di due voci con lo stesso nome.  
   - Se il nome esiste: bloccare salvataggio e mostrare errore “Esiste già una voce con questo nome”.
3. **Validazione preventiva**: mostrare anteprima con evidenza di duplicati/disallineamenti prima del salvataggio.

## Note
- Il bug è stato notato su dati incollati da una versione precedente; verificare compatibilità con formati legacy.
- La soluzione deve essere retrocompatibile ma rifiutare nuovi duplicati.
