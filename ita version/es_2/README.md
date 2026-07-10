# Requisito 2 — Più campagne, ambiente stocastico, Combinatorial-UCB con budget

## Problema

Consideriamo quattro campagne con valutazioni (0.95, 0.85, 0.78, 0.90), orizzonte T = 3000 e budget condiviso B = 700. Per ogni campagna scegliamo un bid da una griglia di 11 valori. Due campagne adiacenti nel conflict graph 0-1-2-3 non possono ricevere contemporaneamente un bid positivo. Dopo aver escluso i bid sopra la valutazione restano 33 arm atomici e 250 super-azioni ammissibili.

I massimi bid concorrenti hanno marginali Beta diverse nelle quattro campagne. Nel 30% dei round le campagne condividono lo stesso quantile, così gli esiti sono correlati pur mantenendo invariate le marginali. Il benchmark è il clairvoyant stocastico che risolve un LP sulle super-azioni con le medie vere e costo medio al più B/T; il suo reward atteso è circa 0.548 per round.

## Versione base

La versione base usa un Combinatorial-UCB con budget. Per ogni arm atomico stima reward e costo medi dal feedback semi-bandit, costruisce un UCB sul reward e un LCB sul costo. A ogni round un LP sceglie una distribuzione sulle super-azioni con costo stimato al più B/T.

L'apprendimento degli arm funziona, ma il controllo della spesa no: il costo LCB sottostima i costi reali e l'LP tende a spendere troppo presto. Il budget viene esaurito prima della fine dell'orizzonte, poi viene scelta la super-azione nulla. Il pseudo-regret finale è circa 595, contro circa 0.435 per round del riferimento random; il budget non viene mai superato.

## Versione migliorata

La versione migliorata mantiene ambiente, benchmark e super-azioni, ma modifica il learner:

- stima per ogni arm la probabilità di vittoria e da questa ricava reward e costo attesi;
- usa bound KL invece di Hoeffding per la probabilità di vittoria;
- usa il costo empirico nell'LP al posto del costo LCB;
- aggiorna il limite di spesa a ogni round usando budget residuo diviso round residui e limita l'LP alle super-azioni ancora pagabili.

Il costo empirico evita di rendere artificialmente permissivo il vincolo dell'LP, mentre il pacing dinamico recupera eventuali scostamenti di spesa durante l'esplorazione iniziale. I bound KL aiutano a ordinare gli arm quando le probabilità di vittoria sono piccole.

## Risultati

Sulle stesse realizzazioni dell'ambiente, il pseudo-regret finale scende da circa 595 a circa 70 e il learner raggiunge circa il 96% del reward atteso del benchmark. La spesa segue il budget fino alla fine dell'orizzonte senza violarlo. Nelle prove di ablation, il costo empirico riduce il pseudo-regret da circa 166 a 70 rispetto al costo LCB, mentre il bound KL riduce il valore da circa 100 a 70 rispetto a Hoeffding.
