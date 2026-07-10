# Online Learning and Advertising

Questa consegna raccoglie quattro esercizi di online learning applicati a problemi di bidding con budget. Ogni cartella contiene due notebook: una versione base e una versione migliorata.

## Metodo di lavoro

Per ogni esercizio siamo partiti da una soluzione il più possibile vicina agli algoritmi presentati nel corso. La versione base serve a mostrare l'applicazione diretta del metodo e a verificare come si comporta sull'istanza scelta.

Abbiamo poi analizzato i risultati della base, guardando soprattutto regret, uso del budget, azioni selezionate e andamento nel tempo. Quando emergeva un limite concreto, la versione migliorata interviene solo su quel punto, senza cambiare il problema o il benchmark. In questo modo il confronto tra le due versioni permette di capire sia cosa funziona nella soluzione iniziale sia da dove arriva il miglioramento.

Le modifiche non sono uguali in tutti gli esercizi: dipendono dal comportamento osservato. In alcuni casi il problema principale è stimare bene probabilità di vittoria piccole; in altri è distribuire la spesa lungo tutto l'orizzonte o adattarsi a cambiamenti dell'ambiente. Per ogni esercizio il README nella relativa cartella descrive le due versioni e riporta i risultati principali.

## Struttura

- `es_1`: singola campagna in ambiente stocastico;
- `es_2`: più campagne in ambiente stocastico;
- `es_3`: metodo primal-dual in ambiente stocastico e non stazionario;
- `es_4`: metodi con memoria limitata e change detection in ambiente leggermente non stazionario.
