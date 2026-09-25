# HANDOVER

## What This Project Does

BayesTransitEquity models transit accessibility and equity with Bayesian spatial methods and presents outputs through a Next.js map app.

## Current Direction

The project is moving from a San Diego proof-of-concept through California pilots
toward a U.S. uncertainty-aware transit equity application. The 2026-09-23 initial
refresh/product plan is in `context/PLATFORM_PLAN.md` (D014). It proposes dated
regional releases, annual baseline reviews, lightweight quarterly source checks,
and recomputation when relevant inputs change. Detailed California scope remains draft.

The central premise is that transit equity planning should not rely only on deterministic accessibility scores. The useful policy object is a probability statement: how confident are we that a tract falls below a policy-relevant accessibility threshold, and how likely is an intervention to move that tract across the boundary?

Near-term direction:

First platform milestone: a reproducible San Diego release with explicit input
versions and export QA. `scripts/run_pipeline.py` and `scripts/run_model.py` are
empty; the current exporter still uses fixed San Diego inputs and summary values.
Complete this foundation before scheduling refreshes. Existing expansion work:

1. Finalize California expansion scope.
2. Run statewide data inventory and EDA before modeling.
3. Build deterministic accessibility baselines.
4. Benchmark scalable probabilistic methods against MCMC validation slices.
5. Export statewide risk/ambiguity/intervention layers for a scale-aware app.
6. Write paper claims around uncertainty, threshold governance, and intervention responsiveness.

## How To Run

Current San Diego baseline:

```bash
conda env create -f environment.yml
conda activate bayestransit
python scripts/download_data.py --config configs/san_diego.yaml
python scripts/export_frontend.py
```

Frontend:

```bash
cd app
npm install
npm run dev
```

California commands are not defined yet. Do not start statewide data pulls until `context/SCOPE.md` and `context/TASKS.md` are approved.

## Validation Commands

Current known commands:

```bash
git status --short --branch
python -m pytest
cd app
npm run build
```

Notes:

- Tests currently appear scaffolded but mostly empty.
- `npm run build` is the relevant frontend smoke gate once app work begins.
- Future California tasks should add deterministic validation scripts and run-bundle checks.

## Done

- 2026-09-23 inference design exploration in `context/MODELING_NOTES.md`: candidates,
  current sampling behavior, spatial bottlenecks, and proposed uncertainty benchmark.
- Initial platform and refresh plan recorded on 2026-09-23; linked from agent entry,
  scope, status, decisions, and repo index. Planning only, no pipeline or UI changes.
- Local workflow control files created under `context/`.
- Draft California expansion scope and task plan written locally.

## Not Done

- Sardor has not yet approved the California scope as final.
- No California data inventory has been run.
- No California config exists.
- No statewide deterministic accessibility outputs exist.
- No scalable inference benchmark exists.
- No California app layer exists.

## Important Local Workflow Files

- `AGENTS.md`
- `context/PROJECT.md`
- `context/SCOPE.md`
- `context/TASKS.md`
- `context/AGENTS.md`
- `context/HANDOVER.md`
- `graphify-out/graph.json`
- `graphify-out/GRAPH_REPORT.md`

The `context/` files are local workflow state unless Sardor explicitly asks to publish them. `AGENTS.md` and `graphify-out/` are tracked so future agents can navigate the repo cheaply.

Graphify is enabled for bounded repository retrieval. Use `graphify query "<question>"`, `graphify path "<A>" "<B>"`, or `graphify explain "<concept>"` before broad source browsing when `graphify-out/graph.json` exists. After meaningful code changes, run `graphify update .`.

## Important Existing Context

- `context/PROJECT_BRIEF.md`: original project premise.
- `context/STATUS.md`: detailed San Diego pipeline status.
- `context/DATASETS.md`: current data source notes.
- `context/RISKS.md`: modeling, data, engineering, and paper risks.
- `context/INTERFACES.md`: pipeline-to-frontend data contracts.
- `context/DASHBOARD_VISUAL_RESEARCH.md`: visual/interaction research for upcoming dashboard work.
- `context/PLATFORM_PLAN.md`: product shape, proposed refresh policy, release QA, and phased U.S. expansion.
- `context/MODELING_NOTES.md`: inference alternatives, prior-equivalence checks, sparse spatial design, and benchmark proposal.
- `context/structure.md`: artifact index and repo map.
- `context/FINDINGS.md`: empirical findings log.

## Caveats

- San Diego results do not prove statewide behavior.
- Statewide data quality will vary substantially by agency and region.
- Full-state MCMC should not be assumed feasible or necessary.
- Approximate inference must be calibrated against MCMC slices before it supports paper claims.
- The public app should expose uncertainty and data-quality caveats rather than hiding them.

## Continue From Here

- For the modeling workstream, read `context/MODELING_NOTES.md`; define an equivalent
  model target and probability/interval tolerances before benchmarking. TASK-007/008
  remain pending; this exploration did not complete their statewide prerequisites.
- Read `context/PLATFORM_PLAN.md`; scope the explicit-input San Diego release as
  the first implementation task. Benchmark regional runs before cloud sizing.
- Review `context/SCOPE.md`.
- Review `context/TASKS.md`, especially TASK-001 through TASK-004.
- Decide the leading deliverable priority: paper, map, reusable pipeline, or sequenced combination.
- Once approved, move TASK-001 to `in-review`, QA it, then start TASK-002.
