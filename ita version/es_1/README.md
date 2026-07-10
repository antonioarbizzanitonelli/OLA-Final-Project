# Requisito 1 — Singola campagna, ambiente stocastico

## Problema

Consideriamo una singola campagna di durata T = 5000 round, con budget B = 125 e valutazione v = 0.6. A ogni round si sceglie un bid da una griglia discreta. L'asta è first-price contro tre concorrenti con bid uniformi in [0, 1]: la probabilità di vincere con bid b è quindi b^3. In caso di vittoria il reward è v - b e il costo è b. I bid sopra la valutazione sono esclusi perché darebbero utilità negativa.

Il benchmark è il clairvoyant stocastico, che conosce la distribuzione delle aste e sceglie la distribuzione sui bid con reward atteso massimo, rispettando il costo medio B/T. Su questa istanza il reward atteso del benchmark è circa 0.0126 per round.

## Versione base

La versione base confronta due learner:

- **UCB1 budget-blind**, che usa UCB1 classico sui bid e si ferma quando il budget è esaurito;
- **UCB con budget**, che stima reward e costo medi di ogni bid, costruisce rispettivamente un UCB e un LCB e usa un LP per scegliere una distribuzione sui bid con costo stimato al più B/T.

I due algoritmi ottengono risultati molto simili: il regret medio finale è circa 40.2 per UCB1 e 39.6 per UCB con budget. Gli intervalli di confidenza sul costo restano larghi e il loro lower bound è spesso nullo; di conseguenza il vincolo nell'LP è poco informativo. Entrambi i learner consumano il budget prima della fine dell'orizzonte e ottengono circa il 36% del reward del clairvoyant.

## Versione migliorata

L'ambiente, il benchmark e UCB1 restano invariati. Il learner con budget viene modificato in tre punti:

- per ogni bid stima direttamente la probabilità di vittoria, da cui ricava reward e costo attesi;
- usa un intervallo di confidenza KL per la probabilità di vittoria;
- considera giocabili solo i bid che possono essere pagati con il budget residuo; il bid nullo resta l'azione di opt-out.

Stimare la probabilità di vittoria è particolarmente utile perché l'osservazione è Bernoulli e le probabilità rilevanti sono piccole. Gli intervalli KL sono più stretti degli intervalli di Hoeffding in questa zona e permettono di distinguere prima i bid che vincono poco da quelli utili.

## Risultati

La versione migliorata chiude con regret medio finale circa 11.4, contro 40.2 di UCB1. Spende circa 123 dei 125 disponibili e raggiunge circa l'82% del reward del clairvoyant. A parità di impostazione, sostituire il bound KL con Hoeffding porta il regret a circa 35.7 e lascia inutilizzata una parte importante del budget: il bound più largo favorisce a lungo bid bassi che vincono raramente.
