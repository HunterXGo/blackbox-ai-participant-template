# round-2 — Investigate

**Team:** BB-031
**Queries used:** 5 / 120

## What we concluded

The system's score responds differently to the tested parameters. `loan_amount` showed a non-monotonic effect: the score increased from 0.9430 at 1 to 0.9707 at 50, then decreased to 0.9648 at 100. `credit_history` showed a decreasing effect in the tested range, with the score falling from 0.9707 at 300.559 to 0.9487 at 811.559. All tests were performed while keeping the other parameters constant.

## How we got there

We first established a baseline using the configuration with `loan_amount=50` and `credit_history=300.559`, which produced a score of 0.9707. We then varied `loan_amount` while keeping the remaining parameters fixed: values of 1, 50, and 100 produced scores of 0.9430, 0.9707, and 0.9648 respectively. This showed that the effect of `loan_amount` was not consistently increasing or decreasing.

Next, we varied only `credit_history` while keeping the other parameters fixed. Values of 300.559, 500.559, and 811.559 produced scores of 0.9707, 0.9676, and 0.9487 respectively. The consistent decrease provided evidence that higher `credit_history` lowers the score in the tested range.

## What we ruled out

We ruled out a simple monotonic relationship between `loan_amount` and the score in the tested range. Increasing `loan_amount` from 1 to 50 increased the score, but increasing it from 50 to 100 decreased the score. We did not observe evidence that the changes were caused by other parameters because they were held constant during these experiments.

## What we are still unsure about

The tested values cover only a limited range, so we cannot claim that these relationships hold across the entire input space. For `loan_amount`, we do not know the exact location of the highest-scoring region or whether the observed non-monotonic pattern continues outside the tested values. For `credit_history`, we have evidence of a decreasing effect only within the tested range of 300.559 to 811.559. Further testing would be required to determine the exact decision boundaries and interactions with other parameters.
