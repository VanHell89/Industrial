# Cedolare – Add-on alla tabella calendario (grafico + snapshot statistico)

## Testo da girare (versione definitiva, senza ambiguità)
> Su questa pagina la **tabella cedolare attuale è perfetta** e **deve rimanere esattamente così com’è**, senza modifiche.  
> Vorrei **aggiungere due elementi** sotto (o sopra) la tabella:
>
> **1️⃣ Grafico a barre – cedole cumulate per sottostante (anno in corso)**  
> • una barra per ogni asset  
> • valore = **somma delle cedole ricevute nell’anno selezionato**  
> • ordinabile (default: decrescente per importo)  
> • stesso schema colori degli asset già usato altrove  
> → Serve per capire **a colpo d’occhio chi contribuisce di più al reddito annuale**.
>
> **2️⃣ Tabella di breakdown statistici chiave (sempre anno in corso)**  
> Una tabella riassuntiva con metriche tipo:  
> • totale cedole anno  
> • media mensile  
> • miglior mese / peggior mese  
> • numero di asset paganti  
> • concentrazione (es. % del totale generata dal primo asset)  
> → Snapshot statistico rapido, complementare alla tabella dettagliata mensile.

## Sintesi “product-minded” (facoltativo da includere)
- tabella attuale = **dettaglio operativo** (immutata)  
- nuovo grafico = **visione immediata della composizione del reddito**  
- nuova tabella statistica = **lettura strategica dell’anno**

## Note di delivery
- Posizionare i due nuovi elementi sopra o sotto la tabella esistente, senza alterarne struttura e logica.  
- Stesso dataset e filtri della tabella (anno selezionato, account/asset).  
- Il grafico mostra solo l’anno selezionato; la tabella statistica usa lo stesso perimetro dati.  
- Ordinamento grafico: decrescente per importo di default; se si aggiunge ordinamento, mantenere un toggle chiaro.  
- Palette colori: riusare quella asset esistente per coerenza visiva.
