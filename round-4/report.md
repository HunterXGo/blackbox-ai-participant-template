# round-4 — Reconstruct

**Team:** BB-031
**Queries used:** 70 / 80 budget

## What we concluded

The system is a synthetic lending eligibility model that produces a score and an APPROVE/DECLINE decision. In our Round 4 investigation, we found that the model is strongly affected by age, loan amount, credit history, and region.

Age has a strong positive effect in the tested Region C configuration: increasing age from 20 to 72 increased the score from 0.7995 to 0.9655.

Loan amount has a non-monotonic effect. In the tested Region C configuration, the score increased as loan amount approached 50, reaching 0.9673 at loan_amount=50, followed by small fluctuations.

Credit history showed different behavior depending on the controlled configuration. In Region C at age 60, increasing credit history from 300 to 900 generally decreased the score. However, in Region A at age 40, we discovered a sharp decision boundary: credit_history values up to 452.0 produced a score of 0.0200 and DECLINE, while 452.5 and above produced scores around 0.89 and APPROVE.

This indicates that the system contains strong conditional and threshold-like behavior rather than following simple monotonic rules for every feature.

## How we got there

We first performed a controlled age sweep while keeping the other parameters fixed in Region C. Increasing age from 20 through 72 consistently increased the score, establishing age as a strong positive feature.

We then performed a controlled loan_amount sweep. The score increased from 0.9438 at loan_amount=5 to 0.9673 around loan_amount=50, after which the values fluctuated slightly. This ruled out a simple monotonic relationship and suggested a local optimum around 50.

Next, we swept credit_history from 300 to 900 under a controlled Region C configuration at age 60. The score generally decreased from 0.9498 to 0.9208, showing that credit_history does not behave as a universally positive feature.

We then investigated Region A, where several low-credit configurations produced a score of 0.0200 and DECLINE. This led us to focus on the credit_history boundary.

Finally, we performed a fine-grained sweep around the suspected boundary using credit_history values from 350 through 600 while holding the remaining features fixed at age=40 and Region A. Values through 452.0 produced DECLINE, while 452.5 and above produced APPROVE. This narrowed the observed transition to the interval (452.0, 452.5].

## What we ruled out

We ruled out a simple monotonic effect of loan_amount. The score peaked around loan_amount=50 and then showed small fluctuations rather than continuously increasing or decreasing.

We also ruled out the assumption that higher credit_history always improves the score. In the controlled Region C sweep at age 60, increasing credit_history generally reduced the score.

We ruled out the idea that the Region A failures were caused by region alone. Region A produced both DECLINE and APPROVE outcomes depending on the other feature values. In particular, the detailed credit_history sweep at age 40 showed a sharp transition from DECLINE to APPROVE.

We also ruled out the hypothesis that the Region A boundary could be described using only a broad low-credit cutoff. The fine-grained experiments showed that the important transition occurs very close to credit_history=452.5 under the tested configuration.

## What we are still unsure about

The observed credit_history threshold is conditional on the specific configuration used in the experiment. We cannot claim that 452.5 is a universal threshold for every age, region, loan amount, or combination of features.

The exact mathematical form of the underlying model is still unknown. The experiments reveal behavioral rules and boundaries, but they do not prove the internal implementation.

We also have not exhaustively tested interactions between all features. Some effects may change substantially when other variables are changed.

The discovered Region A threshold is therefore best treated as a high-confidence local decision boundary rather than a universal rule for the entire system.
