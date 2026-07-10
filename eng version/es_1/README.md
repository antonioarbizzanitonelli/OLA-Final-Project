# Requirement 1 — Single campaign, stochastic environment

## Problem

We consider a single campaign lasting T = 5000 rounds, with budget B = 125 and valuation v = 0.6. At each round, a bid is selected from a discrete grid. The auction is first-price against three competitors with bids uniformly distributed in [0, 1]; the probability of winning with bid b is therefore b^3. If the auction is won, the reward is v - b and the cost is b. Bids above the valuation are excluded because they yield negative utility.

The benchmark is the stochastic clairvoyant, which knows the auction distribution and selects the bid distribution with maximum expected reward while satisfying the average cost constraint B/T. On this instance, its expected reward is about 0.0126 per round.

## Baseline version

The baseline compares two learners:

- **Budget-blind UCB1**, which applies standard UCB1 to bids and stops when the budget is exhausted;
- **Budgeted UCB**, which estimates each bid's mean reward and cost, builds a UCB and an LCB respectively, and uses an LP to select a bid distribution with estimated cost at most B/T.

The two algorithms obtain very similar results: final mean regret is about 40.2 for UCB1 and 39.6 for budgeted UCB. The confidence intervals for cost remain wide and their lower bound is often zero; consequently, the LP constraint is not very informative. Both learners exhaust the budget before the end of the horizon and obtain about 36% of the clairvoyant reward.

## Improved version

The environment, benchmark, and UCB1 remain unchanged. The budgeted learner is modified in three ways:

- it directly estimates the winning probability of each bid, from which it obtains expected reward and cost;
- it uses a KL confidence interval for the winning probability;
- it only considers bids affordable with the remaining budget; the zero bid remains the opt-out action.

Estimating the winning probability is particularly useful because the observation is Bernoulli and the relevant probabilities are small. KL intervals are tighter than Hoeffding intervals in this region and distinguish sooner between bids that rarely win and useful bids.

## Results

The improved version ends with final mean regret of about 11.4, compared with 40.2 for UCB1. It spends about 123 of the available 125 and reaches about 82% of the clairvoyant reward. With the same setup, replacing the KL bound with Hoeffding raises regret to about 35.7 and leaves a substantial part of the budget unused: the wider bound favors low bids that rarely win for too long.
