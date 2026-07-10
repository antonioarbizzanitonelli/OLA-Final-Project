# Requirement 3 — Best-of-both-worlds with multiple campaigns, primal-dual method

## Problem

The instance has four campaigns, a grid of 11 bids, budget B = 700, horizon T = 3000, and 250 admissible super-actions. Two environments are studied: a stochastic one with maximum competitor bids following Beta distributions correlated across campaigns, and a highly non-stationary one, generated in advance, where the attractive campaigns change over time.

Full feedback is available after every round: thresholds are observed, hence so are the counterfactual rewards and costs of all super-actions. The benchmark is the best fixed distribution in hindsight that satisfies the average budget constraint, computed by solving an LP on the means of the realized sequence.

## Baseline version

The baseline applies a primal-dual method to the 250 super-actions:

- the primal side applies Hedge to the Lagrangian, which combines reward and cost;
- the dual side uses projected online gradient descent to update the constraint multiplier;
- if the remaining budget cannot cover the maximum cost of a round, the null super-action is selected.

The scores used by Hedge are normalized with fixed bounds valid over the entire horizon. The method clearly outperforms the random reference in both environments and never violates the budget, but it is conservative: it spends about 569 of 700 in the stochastic environment and 558 of 700 in the non-stationary one. Regret per round is about 0.249 and 0.222, respectively.

## Improved version

The primal-dual structure remains the same. The changes are:

- Hedge is updated only with the part of the Lagrangian that depends on the super-action;
- the learning rates for Hedge and OGD are selected according to the bounds used in normalization;
- budget control is applied to the sampled super-action: if it is not affordable, it is replaced with the null super-action without preventing all possible actions.

The first change does not alter the distribution selected by Hedge because the removed term is the same for all actions. The other two make the update less cautious and allow better budget use.

## Results

With the same environments and seeds, regret per round falls from 0.249 to 0.121 in the stochastic environment and from 0.222 to 0.134 in the non-stationary one. Expenditure rises from about 569 to 676 and from 558 to 597, respectively, always without violating the budget. Repeating the experiments over horizons from 100 to 3000 rounds, normalized regret decreases in both environments, in line with the required sublinear behavior.
