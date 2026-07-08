# WP1b / M1 — NuSMV corroboration on the two decisive benchmarks

Extends the multi-engine corroboration from the controlled probe to the actual
programs where the disagreements occur (review M1). Faithful SMV models of the two
decisive benchmarks, checked with NuSMV (BDD symbolic, unbounded):

| Benchmark | Property | NuSMV | matches |
| --- | --- | --- | --- |
| `stairs_light` | `light → pir` (P1, buttons idle) | **false** (unsafe) | K-ESBMC unsafe, vs ESBMC skip |
| `traffic_light` | `!(NS_Green & EW_Green)` | **true** (safe, unbounded) | K-ESBMC safe, vs ESBMC havoc |

`stairs.smv` is a direct TOF model (preset reduced to 3; the P1 violation is
preset-independent, per the paper). `traffic.smv` is a faithful behavioral model of
the four-phase one-hot sequencing. Both are independent in *decision procedure* from
K-ESBMC/ESBMC; per the honest caveat in §4.3 they encode the same faithful timer
semantics, so they rule out a BMC/K-ESBMC-modeling artifact rather than furnishing a
second independent account of the standard.

Reproduce: `NUSMV=/path/to/NuSMV; for m in stairs traffic; do $NUSMV $m.smv; done`
