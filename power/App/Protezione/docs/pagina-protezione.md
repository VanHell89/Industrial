# Protezione – Rischi & Coperture (draft v0.1)

Obiettivo: fornire una vista unica sui rischi finanziari/personali e sulle coperture attive, evidenziando gap e priorità di intervento. Includere assicurazioni, fondi emergenza e protezione del reddito.

## Ambito
- Rischi coperti: vita/invalidità, infortuni, salute, casa, responsabilità civile, cyber/identità, viaggi, professionali.
- Coperture considerate: polizze assicurative, fondi emergenza, benefit aziendali, garanzie carte/banche.
- Collegamento con altre pagine: mutui (protezione rata), immobili (assicurazioni casa), PGRF per stress test cashflow.

## Dati di base da tracciare
- Polizza: tipo, compagnia, numero, data inizio/fine, premio, massimale, franchigia, beneficiari.
- Rischio coperto: categoria, evento assicurato, esclusioni note.
- Premio: importo, frequenza, metodo pagamento (link a Ledger).
- Stato: attiva, in rinnovo, disdetta programmata.
- Fondi emergenza: saldo attuale, target (mesi spesa), copertura percentuale.

## Sezioni UI
- **Radar “Copertura rischi”**: per categoria (vita, salute, casa, RC, cyber, viaggio, professionale) mostra livello copertura vs priorità.
- **Card Polizze**: elenco polizze con badge stato, premio annuo, massimale, prossima scadenza; CTA `+ Polizza`.
- **Card Fondo Emergenza**: saldo, target mesi, gap; collegamento a transazioni di funding.
- **Stress test cashflow**: simula impatto di evento (es. infortunio, perdita lavoro) su PGRF: mesi di tenuta con coperture + fondo emergenza.
- **Alert scadenze**: polizze in rinnovo entro 60/30/7 giorni.

## Nuovi tipi/etichette transazione
- `Insurance Premium` (outflow) collegato a polizza_id.
- `Emergency Fund Deposit/Withdrawal` (in/out) con tag `emergency_fund`.
- Opzione “associa polizza” per mutuo/immobile per legare premio a quell’asset.

## KPI
- Copertura vita/invalidità: massimale totale vs 10–15× reddito annuo (parametrico).
- Fondo emergenza: mesi coperti = saldo / spesa mensile (da PGRF o Ledger).
- Rapporto premio/massimale per polizza.
- % rischi critici coperti (peso per priorità utente).

## Roadmap rapida
1) MVP: elenco polizze + premi (Ledger taggati) + radar coperture manuale + fondo emergenza.
2) Integrazione con PGRF per spesa mensile e stress test.
3) Alert scadenze e suggerimenti gap coverage.
