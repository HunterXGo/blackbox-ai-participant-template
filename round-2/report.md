# round-2 — Investigate

**Team:** BB-031
**Queries used:** 5 / 120

## What we concluded

We investigated whether the effect of one parameter depends on the value of another parameter. A controlled 2x2 experiment using `loan_amount` and `credit_history` showed evidence of an interaction between the two parameters. Increasing `loan_amount` increased the score by 0.0218 when `credit_history` was low, but increased it by 0.0672 when `credit_history` was high. Therefore, the effect of `loan_amount` is stronger at the higher tested `credit_history` value.

## How we got there

We used a 2x2 controlled experiment with two levels of each parameter.

The four configurations produced the following scores:

| Query | loan_amount | credit_history | Score |
|---|---:|---:|---:|
| A | Low | Low | 0.9430 |
| B | High | Low | 0.9648 |
| C | Low | High | 0.8840 |
| D | High | High | 0.9512 |

First, we measured the effect of increasing `loan_amount` when `credit_history` was low:

`0.9648 - 0.9430 = +0.0218`

Then we measured the same change in `loan_amount` when `credit_history` was high:

`0.9512 - 0.8840 = +0.0672`

The effect of `loan_amount` therefore changed by:

`0.0672 - 0.0218 = +0.0454`

Because the effect of changing `loan_amount` was substantially different at the two `credit_history` levels, the results provide evidence that the two parameters interact.

## What we ruled out

We ruled out the explanation that `loan_amount` has exactly the same effect regardless of `credit_history` within the tested configurations. The change in score caused by increasing `loan_amount` was +0.0218 at low `credit_history` but +0.0672 at high `credit_history`.

However, this experiment does not establish the relationship across the entire input space. It only demonstrates the interaction at the tested parameter levels.

## What we are still unsure about

We do not know whether the interaction remains equally strong at intermediate values of `loan_amount` or `credit_history`. We also do not know whether the interaction is caused by a specific threshold or by a smooth relationship between the two parameters. Additional experiments would be required to identify the exact functional form or boundary of the interaction.
