# Requirement 2 — Multiple campaigns, stochastic environment, budgeted Combinatorial-UCB

## Problem

We consider four campaigns with valuations (0.95, 0.85, 0.78, 0.90), horizon T = 3000, and shared budget B = 700. For each campaign, a bid is selected from a grid of 11 values. Two adjacent campaigns in the conflict graph 0-1-2-3 cannot simultaneously receive a positive bid. After excluding bids above the valuation, 33 atomic arms and 250 admissible super-actions remain.

The maximum competitor bids have different Beta marginals across the four campaigns. In 30% of rounds, the campaigns share the same quantile, so outcomes are correlated while the marginals remain unchanged. The benchmark is the stochastic clairvoyant, which solves an LP over super-actions using the true means and average cost at most B/T; its expected reward is about 0.548 per round.

## Baseline version

The baseline uses budgeted Combinatorial-UCB. For each atomic arm, it estimates mean reward and cost from semi-bandit feedback, builds a UCB for reward and an LCB for cost. At each round, an LP selects a super-action distribution with estimated cost at most B/T.

The arms are learned successfully, but expenditure control is not: the cost LCB underestimates the true costs and the LP tends to spend too early. The budget is exhausted before the end of the horizon, after which the null super-action is selected. Final pseudo-regret is about 595, compared with about 0.435 per round for the random reference; the budget is never exceeded.

## Improved version

The improved version retains the environment, benchmark, and super-actions, but modifies the learner:

- it estimates each arm's winning probability and obtains expected reward and cost from it;
- it uses KL bounds rather than Hoeffding for the winning probability;
- it uses empirical cost in the LP instead of the cost LCB;
- it updates the spending limit at each round using remaining budget divided by remaining rounds and restricts the LP to super-actions that are still affordable.

Empirical cost prevents the LP constraint from becoming artificially permissive, while dynamic pacing corrects expenditure deviations during initial exploration. KL bounds help rank arms when winning probabilities are small.

## Results

On the same environment realizations, final pseudo-regret falls from about 595 to about 70 and the learner reaches about 96% of the benchmark's expected reward. Expenditure follows the budget through the end of the horizon without violating it. In the ablations, empirical cost reduces pseudo-regret from about 166 to 70 relative to the cost LCB, while the KL bound reduces it from about 100 to 70 relative to Hoeffding.
