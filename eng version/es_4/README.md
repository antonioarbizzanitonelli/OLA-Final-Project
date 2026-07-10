# Requirement 4 — Mildly non-stationary environments with multiple campaigns

## Problem

The horizon T = 3000 is divided into three segments of 1000 rounds. In each segment, maximum competitor bids follow fixed Beta distributions, but their parameters change from one segment to the next; consequently, the attractive campaigns also change. The instance has four campaigns, a chain conflict graph, budget B = 560, and a bid grid denser at low values.

Combinatorial SW-UCB, combinatorial CUSUM-UCB, and a primal-dual method with full feedback are compared. The main benchmark is an oracle that solves a separate LP in each segment while maintaining the same average budget per round.

## Baseline version

SW-UCB uses a window of about 219 rounds, while CUSUM-UCB uses 7 reference samples per arm, a threshold of about 14.6, and additional exploration of about 0.07. Both estimate reward with UCB and cost with LCB, and select a super-action distribution through an LP with limit B/T. The primal-dual method updates Hedge and the dual multiplier with full feedback.

The cumulative gap to the local oracle is about 562 for SW-UCB, 533 for CUSUM-UCB, and 716 for the primal-dual method. The two UCB methods consume the entire budget too early because the cost LCB makes the constraint insufficiently restrictive. The primal-dual method is instead conservative and leaves about 167 of the available 560 unused. CUSUM detects few changes because each arm receives a limited number of observations between two change points.

## Improved version

For SW-UCB, the window is increased to 500 rounds, i.e., half a segment. This preserves sufficient data for each arm without including too many rounds from the previous segment. For SW-UCB and CUSUM-UCB, LP cost becomes empirical cost and the spending limit is updated at every round as remaining budget divided by remaining rounds.

The primal-dual method uses learning rates computed from the bounds of its updates and applies budget control directly to the sampled super-action. The CUSUM change detector remains unchanged.

## Results

The gap to the local oracle falls from 562 to 417 for SW-UCB, from 533 to 345 for CUSUM-UCB, and from 716 to 660 for the primal-dual method. In the improved version, mean gap per round is 0.139 for SW-UCB, 0.115 for CUSUM-UCB, and 0.220 for the primal-dual method. CUSUM-UCB is the best method on this instance: it adapts to changes and, with more regular budget management, preserves room for exploration in later segments. All algorithms respect the budget.
