# Cedolare – Vista dividendi continua (long-term view)

## Richiesta prodotto (testo da girare al team)
Vorrei aggiungere una visualizzazione continua nel tempo dei dividendi.  
L’attuale vista anno su anno va benissimo così com’è e deve rimanere.  
In aggiunta, vorrei un grafico che mostri i dividendi in modo continuo dal primo anno disponibile fino all’ultimo, senza reset a gennaio, così da vedere l’evoluzione reale nel tempo (timeline cumulativa o serie mensile continua).

## Specifiche funzionali opzionali
- Asse X: mesi continui (es. 2019-01 → 2026-12).  
- Asse Y: dividendi (mensili oppure cumulati, da scegliere).  
- Colori: stessa palette per asset già usata nella vista annuale.

## Perché serve (contesto prodotto)
- 📊 Vista attuale → confronto anno per anno (resta invariata).  
- 📈 Nuova vista → crescita continua nel tempo per leggere trend, crescita e stagionalità che la vista annuale non mostra.

## Note di delivery
- Aggiungere la nuova vista accanto a quella esistente, non sostituirla.  
- La timeline deve usare gli stessi dati/dividendi e filtri attuali (account, asset, range date, ecc.).  
- Se implementata cumulativa, il reset avviene solo all’inizio del dataset, non ogni gennaio.  
- Se implementata mensile, la serie è semplicemente tutti i mesi senza bucket annuale.
