# MigiTech – Dividend Calendar: grafico “Dividend income by month” Top 10 + Others

Contesto: nella vista Chart/Received del Dividend Calendar il grafico a barre stacked mostra tutti i sottostanti. Serve limitare a Top 10 per mese e raggruppare il resto in “Others/Altro”, mantenendo invariato il totale mensile.

## Obiettivo
Per ogni mese mostrare solo i 10 asset con maggior importo dividend/cedola; tutti gli altri aggregati in “Others”. Top10 + Others = totale mese.

## Regole di calcolo (per singolo mese)
1. Seleziona transazioni con `Type = Dividend` filtrate in base alla vista (es. Received = incassati).  
2. Filtra per il mese della barra.  
3. Raggruppa per Asset e calcola `amount_sum` per asset nel mese.  
4. Ordina per `amount_sum` desc, prendi i primi 10.  
5. Calcola `others_sum = totale_mese - somma_top10`.  
6. Aggiungi serie “Others” solo se `others_sum > 0`.

## Output grafico
- Ogni barra mensile: max 11 segmenti (10 asset + Others).  
- Legenda: nomi dei 10 asset + “Others”.  
- Totale barra invariato.

## Comportamento dinamico
- Top 10 calcolato per mese (può cambiare mese per mese).  
- Se gli asset del mese sono ≤10: mostra tutti, niente Others.  
- Others è mensile e dipende dagli asset esclusi quel mese.

## Dettagli opzionali
- Tooltip su Others: mostra valore totale e (facoltativo) “N asset aggregati”.  
- Tabella “Dividend details” sotto il grafico può restare completa; applicare Top10 solo al grafico.

## Note tecniche
- “Others” è serie virtuale calcolata a runtime (frontend o backend).  
- Se esiste già una logica Others, modificarla: deve essere esattamente la somma degli asset fuori Top10 per quel mese, non un generico “non in legenda”.
