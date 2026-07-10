# Online Learning and Advertising

This submission contains four online learning exercises applied to budgeted bidding problems. Each folder contains two notebooks: a baseline version and an improved version.

## Method

For each exercise, we started with a solution as close as possible to the algorithms presented in the course. The baseline version shows the direct application of the method and evaluates its behavior on the chosen instance.

We then analyzed the baseline results, focusing on regret, budget usage, selected actions, and their evolution over time. Whenever a concrete limitation emerged, the improved version addressed only that point, without changing the problem or the benchmark. This makes the comparison useful both to understand what works in the initial solution and where the improvement comes from.

The changes differ across exercises because they depend on the observed behavior. In some cases, the main difficulty is estimating small winning probabilities; in others, it is spreading expenditure over the whole horizon or adapting to environmental changes. The README in each exercise folder describes both versions and reports the main results.

## Structure

- `es_1`: single campaign in a stochastic environment;
- `es_2`: multiple campaigns in a stochastic environment;
- `es_3`: a primal-dual method in stochastic and non-stationary environments;
- `es_4`: limited-memory and change-detection methods in a mildly non-stationary environment.
