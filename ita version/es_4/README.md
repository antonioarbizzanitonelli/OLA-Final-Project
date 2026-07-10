# Requisito 4 — Ambienti leggermente non stazionari con più campagne

## Problema

L'orizzonte T = 3000 è diviso in tre segmenti da 1000 round. In ogni segmento i massimi bid concorrenti seguono distribuzioni Beta fisse, ma i parametri cambiano da un segmento al successivo; cambiano quindi anche le campagne più convenienti. L'istanza ha quattro campagne, un conflict graph a catena, budget B = 560 e una griglia di bid più fitta sui valori bassi.

Vengono confrontati SW-UCB combinatorio, CUSUM-UCB combinatorio e un metodo primal-dual con full feedback. Il benchmark principale è un oracle che risolve un LP separato in ogni segmento, mantenendo lo stesso budget medio per round.

## Versione base

SW-UCB usa una finestra di circa 219 round, mentre CUSUM-UCB usa 7 campioni di riferimento per arm, soglia circa 14.6 ed esplorazione aggiuntiva circa 0.07. Entrambi stimano reward con UCB e costo con LCB e scelgono una distribuzione sulle super-azioni tramite LP con limite B/T. Il primal-dual aggiorna Hedge e il moltiplicatore duale con full feedback.

Il gap cumulato dall'oracle locale è circa 562 per SW-UCB, 533 per CUSUM-UCB e 716 per primal-dual. I due metodi UCB consumano tutto il budget ma troppo presto, perché il costo LCB rende il vincolo poco restrittivo. Il primal-dual è invece conservativo e lascia inutilizzati circa 167 dei 560 disponibili. CUSUM rileva pochi cambiamenti perché ogni arm riceve un numero limitato di osservazioni tra due change point.

## Versione migliorata

Per SW-UCB la finestra viene portata a 500 round, pari a metà segmento. La scelta conserva dati sufficienti per ogni arm, senza includere troppi round del segmento precedente. Per SW-UCB e CUSUM-UCB, il costo nell'LP diventa quello empirico e il limite di spesa viene aggiornato a ogni round come budget residuo diviso round residui.

Il primal-dual usa learning rate calcolati dai bound dei suoi aggiornamenti e applica il controllo del budget direttamente alla super-azione estratta. Il change detector CUSUM resta invariato.

## Risultati

Il gap dall'oracle locale scende da 562 a 417 per SW-UCB, da 533 a 345 per CUSUM-UCB e da 716 a 660 per il primal-dual. Nella versione migliorata il gap medio per round è 0.139 per SW-UCB, 0.115 per CUSUM-UCB e 0.220 per primal-dual. CUSUM-UCB è il metodo migliore su questa istanza: si adatta ai cambiamenti e, con una gestione più regolare del budget, mantiene spazio per esplorare anche nei segmenti successivi. Tutti gli algoritmi rispettano il budget.
