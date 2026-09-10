# KEA3 tomato drought trial: ECS re-fit

Analysis code behind **Figure 4** of

> von Bismarck T., Nguyen Q. H., Ruß J., Strandberg L., Velazquez Ayala D. C., Krämer M., Damen J., Kunz H.-H.,
> Jahns P., Köhl K., Matuszynska A., Armbruster U. *Experimental and computational analysis of KEA3 function in the
> vegetable crop tomato compared with Arabidopsis.* In preparation (2026).
> **DOI: to be added on publication.**

## The experiment in two sentences

Tomato plants (Moneymaker wild type and two CRISPR *slkea3* knock-out lines, plus two *SlKEA3* overexpression lines)
were grown for four weeks under randomly fluctuating light, then half of them were watered only once a week.
In weeks 5 and 6 (21 and 28 July 2022), and on a recovery day after re-watering (22 July), leaves were measured with
MultispeQ handhelds running the Photosynthesis RIDES 2.0 protocol, giving NPQt (heat dissipation of excess light) and
ECSt (a proxy for the proton motive force across the thylakoid membrane) for 1,380 measurements.

## Data on openJII

The measurements live on the openJII platform, where they can be browsed, filtered, charted and downloaded:

- Experiment: [KEA3 tomato drought trial (MultispeQ, RL3)](https://openjii.org/en-US/platform/experiments/e0acb807-5a1a-4f35-8902-a1d8629d4cc9)
- Dashboards on the experiment page: *Paper companion: KEA3 drought (Fig. 4)*, *Data quality (QC)*, and
  [*Fig. 4 from re-fitted ECSt*](https://openjii.org/en-US/platform/experiments/e0acb807-5a1a-4f35-8902-a1d8629d4cc9/dashboards/fdb0e3cd-fd7a-4b63-8322-3e62edff2c86),
  which is built on the table this notebook produces
- The re-fitted values are on the experiment as the data table `ecs_refit` (1,380 rows, one per measurement)

Nothing in this repository reads Databricks or any internal system; everything goes through the public openJII REST API.

## Why a re-fit

The PhotosynQ macro fits a single exponential to the 520 nm ECS decay inside the device pipeline. Within the plotted
ranges its values agree with the re-fit (Pearson r about 0.96 for ECSt and 0.94 for gH+, see
`outputs/refit_vs_macro.png`), but on some traces it returns extreme or negative values (ECSt down to −11.5,
gH+ to −3×10⁵). The re-fit below applies one documented procedure to every trace, and the paper uses its values.
The notebook re-fits every trace from `ECS_averaged_trace`:

1. linear regression on points 0 to 17 gives the baseline, which is subtracted from the whole trace;
2. the trace is shifted so that point 19 is zero;
3. `a * exp(-t / tau) + c` is fitted to points 19 to 40 with `t = 0, 1.5, 3.0, ...`;
4. `ECSt_calc = a`, `tau_refit = tau`, `gH+_refit = 1000 / tau`.

Figure filters: `PAR != 0`, `0 <= ECSt_calc <= 0.004`, `0 <= NPQt <= 5`, lines W, p1, p2.

## Run it

You need an openJII account with access to the experiment and a personal API key (openjii.org → Profile menu → Account →
API keys). The experiment is public; until its embargo date only signed-in users can read it.

```bash
git clone https://github.com/Jan-IngenHousz-Institute/kea3-tomato-ecs-refit.git
cd kea3-tomato-ecs-refit
cp .env.example .env            # paste your key into .env
uv sync
uv run jupyter notebook notebooks/kea3_tomato_ecs_refit.ipynb
```

Or headless, which re-executes the notebook in place and refreshes `outputs/`:

```bash
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/kea3_tomato_ecs_refit.ipynb
```

The first run downloads the 1,380 measurements into `data/raw/` (about 12 MB, git-ignored) and writes the re-fitted
table to `data/processed/ecs_refit.parquet` (git-ignored). Later runs use the local copy; delete `data/raw/` to refresh.
Runtime is about one minute.

## What is in here

| Path | What |
|---|---|
| `notebooks/kea3_tomato_ecs_refit.ipynb` | the reproducible notebook, committed with its outputs |
| `notebooks/original/` | the original analysis notebook by Thekla von Bismarck, unchanged, for provenance |
| `outputs/fig4_control_drought.png` | Fig. 4 C and D redrawn from the re-fit |
| `outputs/fig4_recovery.png` | the same plot for the recovery day (not in the paper) |
| `outputs/refit_vs_macro.png` | device macro values against the re-fit |
| `outputs/example_fits.png` | eight example traces with their fits |
| `outputs/legend_counts.md` | the per-line replicate counts, checked against the figure legend |

## Codes used in the data

| `which_plant` | line |
|---|---|
| W | Moneymaker wild type |
| p1 | *slkea3-1* |
| p2 | *slkea3-2* |
| L7, L12 | the two *SlKEA3* overexpression lines (measured, not shown in Fig. 4) |

| `condition` | treatment |
|---|---|
| Con | watered every 2 to 3 days |
| Dr | watered once a week from week 4 |
| Rec | drought plants after re-watering, 22 July only |

`rep` is the replicate plant (1 to 12 per line and condition).

## Licence and credit

Code: MIT (see `LICENSE`). The analysis method and the original notebook are by Thekla von Bismarck. Data: see the
openJII experiment page. Please cite the paper once the DOI is available.
