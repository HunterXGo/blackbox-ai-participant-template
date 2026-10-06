# round-1 — Observe

**Team:** BB-031  
**Queries used:** 36 / 150

## What we concluded

The system GK-01 is a deterministic black-box lending eligibility system that returns a score between 0 and 1 and an APPROVE/DECLINE decision.

Our observations show that most tested inputs produce high approval scores, generally around 0.87–0.97. Several individual features produced little or no observable change in the tested configurations, while `employment_years` showed a small but measurable effect.

The most important observation was a sharp change in behaviour: query #25 returned approximately `0.9209 APPROVE`, while query #26 returned `0.0200 DECLINE`. This indicates that the system can have a strong nonlinear or interaction-based response.

We therefore conclude that the model should not be interpreted using normal real-world lending assumptions. The observed outputs suggest that some features may have weak effects individually while specific feature values or interactions can cause large changes in the final decision.

## How we got there

We first established a high-scoring baseline around:

- age = 75
- credit_history ≈ 300.559
- debt_ratio = 0.7–1
- dependents = 5–6
- employment_years ≈ 1
- income = 10
- loan_amount = 50
- num_accounts = 17
- recent_defaults = 5
- region = C

The baseline produced scores around `0.9673 APPROVE`.

We then varied individual features to observe local effects.

### Dependents

Changing dependents between tested values such as 5, 0 and 6 produced approximately the same score (`0.9673`) in the relevant observations.

This suggested that dependents had little observable influence in this tested region.

### Debt ratio

Changing debt_ratio between tested values including 0, 0.7 and 1 produced little or no observable change around the same high approval score.

This suggested that debt_ratio was relatively insensitive in the tested configuration.

### Employment years

Employment years produced a more noticeable effect.

Examples from the observations included:

- employment_years = 3.0 → approximately `0.9701`
- employment_years = 3.5 → `0.9707`
- employment_years = 3.9 → `0.9699`
- employment_years = 4.0 → `0.9699`

The value 3.5 was also reproduced with the same result, supporting deterministic behaviour.

The results suggest a small, non-monotonic local effect rather than a simple rule where increasing employment years always increases the score.

### Income

Income was also varied. In one tested configuration, changing income from 0 to 100 resulted in the same observed score of approximately `0.9661`.

This provided evidence that income had little observable effect in that particular region of the input space.

### Extreme output change

A particularly important observation was the transition between query #25 and query #26:

- Query #25 → `0.9209 APPROVE`
- Query #26 → `0.0200 DECLINE`

The magnitude of this change is much larger than the small changes observed in most other experiments.

This indicates that the system may contain threshold behaviour, nonlinearities, or interactions between features.

## What we ruled out

### "Every feature has a strong independent effect"

Our experiments did not support this. Changes to dependents and debt_ratio produced little or no observable change in the tested configurations.

### "Higher employment years always means a higher score"

This was not supported. The scores around 3–4 employment years increased slightly and then decreased, suggesting that the effect is not simply monotonic.

### "The system follows normal lending intuition"

We rejected this as a reliable assumption. The system explicitly describes its feature names as synthetic, and our observations showed that apparently meaningful changes sometimes had almost no effect.

### "The model is random"

Repeated identical inputs produced the same output, including the repeated employment_years = 3.5 observation. This provides evidence that the system is deterministic rather than randomly changing its output.

### "The decline at query #26 is definitely caused by age alone"

We did not accept this hypothesis because the available observations do not isolate age sufficiently to prove that age was the only changed variable responsible for the decline. We therefore treat the cause of the sharp decline as unresolved.

## What we are still unsure about

We have not identified the exact decision boundary responsible for the `0.0200 DECLINE` result.

We also have not established whether the weak effects observed for dependents, debt_ratio and income remain true across the entire allowed input ranges. Our conclusions for these features are limited to the configurations that were actually tested.

The interaction between age, recent_defaults, region and the other features remains uncertain. In particular, we cannot conclude from the current evidence that any single feature alone determines the APPROVE/DECLINE decision.

The remaining uncertainty is therefore whether the system is primarily driven by specific thresholds in individual features or by interactions between multiple features.
