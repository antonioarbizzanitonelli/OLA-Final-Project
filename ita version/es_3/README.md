# Requisito 3 — Best-of-both-worlds con più campagne, metodo primal-dual

## Problema

L'istanza ha quattro campagne, una griglia di 11 bid, budget B = 700, orizzonte T = 3000 e 250 super-azioni ammissibili. Vengono studiati due ambienti: uno stocastico con massimi bid concorrenti Beta correlati tra campagne e uno altamente non stazionario, generato in anticipo, in cui le campagne convenienti cambiano nel tempo.

Dopo ogni round è disponibile full feedback: si osservano i threshold e quindi reward e costo controfattuali di tutte le super-azioni. Il benchmark è la migliore distribuzione fissa in hindsight che rispetta il vincolo di budget medio, calcolata risolvendo un LP sulle medie della sequenza realizzata.

## Versione base

La versione base applica un metodo primal-dual sulle 250 super-azioni:

- il lato primal usa Hedge sulla Lagrangiana, che combina reward e costo;
- il lato dual usa projected online gradient descent per aggiornare il moltiplicatore del vincolo;
- se il budget residuo non copre il costo massimo di un round, viene scelta la super-azione nulla.

I punteggi usati da Hedge sono normalizzati con bound fissi, validi per tutto l'orizzonte. Il metodo supera nettamente il riferimento random nei due ambienti e non viola mai il budget, ma è conservativo: spende circa 569 su 700 nell'ambiente stocastico e 558 su 700 in quello non stazionario. Il regret per round è rispettivamente circa 0.249 e 0.222.

## Versione migliorata

La struttura primal-dual rimane la stessa. Le modifiche sono:

- Hedge viene aggiornato solo con la parte della Lagrangiana che dipende dalla super-azione;
- i learning rate di Hedge e OGD sono scelti in funzione dei bound usati nella normalizzazione;
- il controllo del budget viene applicato alla super-azione campionata: se non è pagabile viene sostituita dalla super-azione nulla, senza fermare tutte le azioni possibili.

La prima modifica non cambia la distribuzione scelta da Hedge, perché il termine eliminato è uguale per tutte le azioni. Le altre due rendono l'aggiornamento meno prudente e permettono di usare meglio il budget.

## Risultati

Con gli stessi ambienti e seed, il regret per round scende da 0.249 a 0.121 nell'ambiente stocastico e da 0.222 a 0.134 in quello non stazionario. La spesa passa rispettivamente da circa 569 a 676 e da 558 a 597, sempre senza violare il budget. Ripetendo gli esperimenti su orizzonti da 100 a 3000 round, il regret normalizzato diminuisce in entrambi gli ambienti, in linea con il comportamento sublineare richiesto.
