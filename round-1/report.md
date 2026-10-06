# round-1 — Observe

**Team:** BB-027  
**Queries used:** 34 / budget

## What we concluded

The system's clearance score is strongly affected by `discrepancy_ratio`, `port`, and `declared_value`. Higher `discrepancy_ratio` produced higher scores in our tests, while `port C` consistently appeared worse than the other tested ports. `container_count` and `shipper_years` showed comparatively weak effects in the tested ranges.

## How we got there

1. **Discrepancy ratio:** Compared values around `0`, `0.5`, and `1`. Increasing it to `1` produced substantially higher scores and APPROVE decisions.
2. **Container count:** Tested `50` vs `100`. The score changed only slightly, suggesting weak influence.
3. **Prior shipments:** Increasing `prior_shipments` from `10` to `20` improved the observed score, suggesting positive influence.
4. **Port:** Tested ports A, B, C and D while keeping other inputs similar. Port C produced a much lower score (`0.6430`) and DECLINE, while B/D produced scores around `0.87–0.89`.
5. **Shipper years:** Changing `shipper_years` produced only a tiny score change (`+0.0003`), suggesting weak influence.
6. **Declared value:** Changing `declared_value` from `100` to `71` increased the score from `0.8972` to `0.9430`, revealing a non-intuitive relationship.

## What we ruled out

- **Higher container count automatically lowers clearance** — rejected; increasing it to 100 caused only a small score change.
- **Lower discrepancy ratio improves clearance** — rejected; lower values produced lower scores in our experiments.
- **Higher declared value always improves clearance** — rejected; reducing `100 → 71` increased the score.
- **All ports behave similarly** — rejected; port C showed a significant negative effect compared with B/D.
- **Shipper years is a major driver** — rejected for the tested region; its observed effect was extremely small.

## What we are still unsure about

- Whether the effect of each feature is **linear or non-linear** across its full range.
- Whether `port C` is always worse or only worse under the configurations tested.
- Whether `declared_value` has an optimal range rather than simply benefiting from lower values.
- The independent effect of `prior_shipments`, since it was not tested as extensively as port and discrepancy ratio.
- Possible **interactions between features**, where one feature changes the effect of another.
