# MultispeQ device-bias validation for Fig. 4 C/D

## Question and conclusion

Device identity was associated with residual variation in both outcomes, but adding device to the published-subset model changed **0 of 14 FDR decisions** and **0 contrast directions**. Formal paired-bootstrap equivalence was established at the strict 0.10-SD margin for **11 contrasts** and at the 0.20-SD margin for the remaining **3 contrasts**; **0 were inconclusive**. The matched-date analysis retained every direction but changed **2 FDR decisions**, so the strongest conclusion is stability of direction and effect scale rather than identical significance calls under every date specification.

A device term is evidence of device-associated residual structure in this experiment. It is not, by itself, proof of a hardware calibration error because device assignment was not fully randomized and two devices were used on only one date.

## Data and identifiability

The analysis uses the **565 measurements** retained by the published Fig. 4 filters: PAR > 0, 0 ≤ re-fitted ECSt ≤ 0.004, 0 ≤ NPQt ≤ 5, genotypes W/p1/p2, and Control/Drought treatments. These observations represent **69 plant clusters** and **4 devices**. A plant cluster is defined as genotype × treatment × replicate plant. **68 of 69 plants** were measured by at least two devices, which permits a mostly within-plant device comparison.

Device allocation was not balanced across dates:

| MultispeQ device | 2022-07-21 | 2022-07-22 | 2022-07-28 |
|---|---:|---:|---:|
| `01:12:37:34` | 70 | 44 | 64 |
| `01:12:52:49` | 0 | 0 | 90 |
| `41:00:08:81` | 34 | 0 | 0 |
| `52:00:08:b1` | 100 | 50 | 113 |

Only `01:12:37:34` and `52:00:08:b1` were present on all three dates. The sensitivity analysis therefore uses those devices only (441 measurements; 69 plant clusters).

## Statistical methods

For each outcome, a within-plant model compared a reduced specification (`plant + date + log(PAR) + time + time²`) with a full specification that also included device. Plant fixed effects absorb stable differences among plants. The device term was assessed with a cluster-robust omnibus F test, with standard errors clustered by plant. Partial R² is the proportional reduction in residual sum of squares after adding device.

Centred device offsets were calculated as equal-device counterfactual predictions from the full within-plant model. They describe model-estimated shifts relative to the mean device, not absolute calibration corrections.

The biological model estimated genotype × treatment contrasts while controlling for date, log(PAR), and a quadratic time-of-day term. The device-aware version additionally included device identity. Seven pre-specified contrasts were evaluated for each of two outcomes (14 total), using plant-clustered standard errors and Benjamini–Hochberg adjustment within each outcome/model family and, secondarily, across all 14 contrasts. A paired 5,000-replicate plant-cluster bootstrap directly estimated the change caused by device adjustment. Matched-date models added genotype × treatment × date, while the stricter device model used plant × date fixed effects.

## Device-associated variation

| Outcome | Omnibus F (df) | p-value | Partial R² | Centred offset range |
|---|---:|---:|---:|---:|
| Re-fitted ECSt | 16.05 (3, 68) | 5.42e-08 | 10.9% | 0.0007992 |
| NPQt | 49.00 (3, 68) | 5.58e-17 | 13.1% | 0.8969 |

The device term explained 10.9% of residual ECSt variation and 13.1% of residual NPQt variation after the within-plant and nuisance controls. Estimated centred offsets were:

| Outcome | Device | Offset | 95% CI |
|---|---|---:|---:|
| Re-fitted ECSt | `01:12:37:34` | -0.000244701 | [-0.000323484, -0.000165919] |
| Re-fitted ECSt | `01:12:52:49` | -0.000188943 | [-0.000297478, -8.04074e-05] |
| Re-fitted ECSt | `41:00:08:81` | 0.000554472 | [0.000375831, 0.000733112] |
| Re-fitted ECSt | `52:00:08:b1` | -0.000120828 | [-0.00020007, -4.15859e-05] |
| NPQt | `01:12:37:34` | -0.0390757 | [-0.115656, 0.037505] |
| NPQt | `01:12:52:49` | -0.150248 | [-0.263149, -0.0373468] |
| NPQt | `41:00:08:81` | 0.543117 | [0.403043, 0.683191] |
| NPQt | `52:00:08:b1` | -0.353793 | [-0.41115, -0.296436] |

## Do the biological conclusions change?

Across all 14 contrasts, **0 changed 5% FDR significance** and **0 changed direction** after adding device identity.

| Outcome | Contrast | Estimate without device | q without device | Estimate with device | q with device | FDR changed? |
|---|---|---:|---:|---:|---:|---|
| Re-fitted ECSt | Dr − Con within W | -0.000374003 | 1.26e-07 | -0.000392566 | 7.6e-09 | no |
| Re-fitted ECSt | Dr − Con within p1 | -0.000278439 | 0.000327 | -0.000282241 | 0.000445 | no |
| Re-fitted ECSt | Dr − Con within p2 | -8.20695e-05 | 0.359 | -0.000110801 | 0.174 | no |
| Re-fitted ECSt | p1 − W within Con | -0.000231089 | 0.000327 | -0.000236982 | 0.00012 | no |
| Re-fitted ECSt | p2 − W within Con | -0.000347062 | 9.31e-06 | -0.000343511 | 1.29e-06 | no |
| Re-fitted ECSt | p1 − W within Dr | -0.000135525 | 0.112 | -0.000126657 | 0.151 | no |
| Re-fitted ECSt | p2 − W within Dr | -5.51286e-05 | 0.446 | -6.17457e-05 | 0.405 | no |
| NPQt | Dr − Con within W | -0.467844 | 1.02e-10 | -0.545758 | 1.08e-10 | no |
| NPQt | Dr − Con within p1 | -0.86878 | 1.96e-29 | -0.918317 | 6.82e-50 | no |
| NPQt | Dr − Con within p2 | -0.678566 | 4.92e-28 | -0.73495 | 1.25e-29 | no |
| NPQt | p1 − W within Con | 0.420951 | 3.13e-07 | 0.406975 | 1.35e-07 | no |
| NPQt | p2 − W within Con | 0.295093 | 0.000159 | 0.28168 | 0.000603 | no |
| NPQt | p1 − W within Dr | 0.0200152 | 0.735 | 0.0344151 | 0.576 | no |
| NPQt | p2 − W within Dr | 0.0843713 | 0.0839 | 0.0924876 | 0.124 | no |

The largest percentage shifts occurred for contrasts whose unadjusted estimates were close to zero, so a small absolute change produces a large relative percentage.

## Formal equivalence of device-adjusted contrasts

All 5,000 paired plant-cluster bootstrap replicates succeeded. The complete 90% interval for the device-induced change was inside ±0.10 pooled SD for **11/14 contrasts**. The other **3/14**—the three NPQt drought-within-genotype contrasts—were equivalent at ±0.20 SD. **0/14** were inconclusive at the secondary margin.

| Outcome | Contrast | Change (SD) | 90% bootstrap interval | Equivalence |
|---|---|---:|---:|---|
| Re-fitted ECSt | Dr − Con within W | -0.032 | [-0.076, +0.015] SD | equivalent at 0.10 SD |
| Re-fitted ECSt | Dr − Con within p1 | -0.006 | [-0.048, +0.039] SD | equivalent at 0.10 SD |
| Re-fitted ECSt | Dr − Con within p2 | -0.049 | [-0.089, -0.002] SD | equivalent at 0.10 SD |
| Re-fitted ECSt | p1 − W within Con | -0.010 | [-0.058, +0.034] SD | equivalent at 0.10 SD |
| Re-fitted ECSt | p2 − W within Con | +0.006 | [-0.038, +0.048] SD | equivalent at 0.10 SD |
| Re-fitted ECSt | p1 − W within Dr | +0.015 | [-0.018, +0.049] SD | equivalent at 0.10 SD |
| Re-fitted ECSt | p2 − W within Dr | -0.011 | [-0.044, +0.025] SD | equivalent at 0.10 SD |
| NPQt | Dr − Con within W | -0.092 | [-0.161, -0.031] SD | equivalent at 0.20 SD only |
| NPQt | Dr − Con within p1 | -0.059 | [-0.128, +0.008] SD | equivalent at 0.20 SD only |
| NPQt | Dr − Con within p2 | -0.067 | [-0.145, +0.008] SD | equivalent at 0.20 SD only |
| NPQt | p1 − W within Con | -0.017 | [-0.077, +0.041] SD | equivalent at 0.10 SD |
| NPQt | p2 − W within Con | -0.016 | [-0.082, +0.048] SD | equivalent at 0.10 SD |
| NPQt | p1 − W within Dr | +0.017 | [-0.042, +0.078] SD | equivalent at 0.10 SD |
| NPQt | p2 − W within Dr | +0.010 | [-0.049, +0.067] SD | equivalent at 0.10 SD |

## Date and device sensitivity analyses

Restricting the reference analysis to the two devices observed on every date produced **0 device-adjustment FDR changes**. Restricting Control and Drought to their concurrent dates (21 and 28 July; 471 observations) retained all contrast directions but produced **2 FDR changes** in the device-aware model: p2 ECSt Dr − Con became significant, and p2 − W NPQt within Drought became significant. The device-aware genotype × treatment × date interaction was significant for NPQt (p=0.0254) but not ECSt.

The stricter within-plant-date analysis retained 496 observations from 147 sessions. Device remained associated with both outcomes:

| Outcome | Device omnibus p | Partial R² | Bridge device × date p |
|---|---:|---:|---:|
| Re-fitted ECSt | 3.45e-07 | 12.1% | 0.0184 |
| NPQt | 4.57e-11 | 16.1% | 0.034 |

The bridge-device interaction was significant for both ECSt and NPQt, indicating that the relative offset between the two devices used on every date was not constant across dates. A single additive correction is therefore an approximation; device should remain in the inferential model rather than treating corrected point values as error-free.

## Interpretation and limitations

The reference analysis is robust to adding device: directions and FDR calls are unchanged, and every contrast change is bounded within 0.20 SD. However, only 11 contrasts meet the stricter 0.10-SD criterion, two borderline FDR decisions change in the matched-date specification, NPQt shows date-dependent genotype/treatment structure, and bridge-device offsets vary by date. These results support the broad biological pattern while arguing against a stronger claim that instrument adjustment or date specification is immaterial.

This is an observational robustness analysis rather than a calibration experiment. Device assignment was unbalanced across dates, environmental covariates may not remove all session effects, and model-estimated offsets are conditional on this experiment and protocol. A dedicated calibration study would randomize devices across the same leaves and reference standards.

## Reproducibility

The complete analysis is in [`notebooks/multispeq_device_bias_validation.ipynb`](../notebooks/multispeq_device_bias_validation.ipynb). The main diagnostic is [`device_bias_validation.png`](device_bias_validation.png); machine-readable model summaries, equivalence intervals, matched-date contrasts, offsets and sensitivity results are in the adjacent `device_bias_*.csv` files. The notebook also writes a git-ignored row-level table to `data/processed/fig4_device_adjusted.parquet`.
