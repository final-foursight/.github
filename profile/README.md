# Final Foursight

A college basketball bracketology project. This directory is the parent workspace for a set of independent repos, each responsible for one layer of the pipeline: raw data in, bracket projections out.

## Services

| Repo | Layer | Responsibility |
|---|---|---|
| [`final-foursight-data`](./final-foursight-data) | Data engineering | Ingestion, validation, cleaning, feature engineering, and storage of all NCAA basketball data. Single source of truth for the rest of the project. |
| [`final-foursight-modeling`](./final-foursight-modeling) | Statistical modeling | Trains and evaluates predictive models (team strength, win probability, tournament selection, seeding, free throw models, etc.) using data from `final-foursight-data`. |
| [`final-foursight-sim`](./final-foursight-sim) | Simulation engine | Runs Monte Carlo simulations of games, seasons, conference tournaments, Selection Sunday, and the NCAA Tournament using the trained models. |
| [`final-foursight-api`](./final-foursight-api) | Backend API | Exposes data, model predictions, and simulation results via a REST API for the web app and other consumers. |
| [`final-foursight-web`](./final-foursight-web) | Frontend | User-facing interface: bracket projections, team pages, simulations, visualizations, and interactive tools. |
| [`final-foursight-infrastructure`](./final-foursight-infrastructure) | Platform engineering | Shared CI/CD, Docker, Terraform, deployment scripts, and reusable project templates. No business logic. |
| [`final-foursight-brand`](./final-foursight-brand) | Brand/design | Logo, color, typography, and brand guidelines. Source of truth for design assets; consumers copy in production-ready exports rather than referencing it live. |

## Data flow

```
final-foursight-data  →  final-foursight-modeling  →  final-foursight-sim  →  final-foursight-api  →  final-foursight-web
                                                                                        ↑
                                                                    final-foursight-infrastructure (supports all)
```

Each repo is deployed and versioned independently.

## Open questions

These are decisions worth making early, before the repos fill in enough that they get expensive to change:

- **Shared contracts** — `data`, `modeling`, `sim`, and `api` all need to agree on schemas and payload shapes. Undecided whether this lives as a versioned package published from `data`, a standalone `final-foursight-schemas` repo, or is just documented and hand-kept in sync.
- **Orchestration/scheduling** — something needs to decide when ingestion runs, when models retrain, and when sims kick off (especially time-sensitive around Selection Sunday). Not yet decided whether this lives in `infrastructure` (e.g. scheduled GitHub Actions) or as its own orchestration layer.
- **Data storage ownership** — `data` owns "storage," but it's not yet decided whether that means a database/warehouse `data` provisions and owns directly, or infra-provisioned storage that `data` just writes to.
- **Cross-repo versioning** — since each repo deploys independently, we need a policy for how `api` pins to specific `modeling`/`sim` versions (and vice versa) so a breaking change in one doesn't silently break another.

## Conventions

- Each repo manages its own dependencies (`pyproject.toml`) and Python version (`.python-version`).
- Business logic stays out of `final-foursight-infrastructure`; it exists to support the other repos, not implement product behavior.
