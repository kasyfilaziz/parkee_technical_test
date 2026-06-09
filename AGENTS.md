# AGENTS.md

## What this repo is

A data engineering technical test for Parkee. Single flat project — no `beginner/intermediate/advanced` split.

## Directory layout

- `dags/` — Airflow DAGs and pipeline code (Python)
- `data_ingestion/` — Custom ingestion scripts (Python, Go)
- `data_transformation/` — dbt Core models, sources, tests
- `data_visualization/` — FastAPI webserver, Go HTTP API, Jupyter notebooks
- `docker-compose.yaml` — spins up Airflow + Postgres + FastAPI + Go API

## Commands

```bash
# Start all services
docker-compose up -d

# Airflow webserver is at http://localhost:8080 (admin/admin)
# FastAPI is at http://localhost:8000
# Go API is at http://localhost:9090
# PostgreSQL is at localhost:5432

# dbt (run inside airflow container or locally)
cd data_transformation
dbt deps
dbt run
dbt test

# Great Expectations (when configured)
great_expectations checkpoint run <checkpoint_name>
```

## Gotchas

- DAGs go in `dags/`, not `dags.py`. Airflow v2 requires a folder.
- dbt profiles must point to the correct warehouse. Check `~/.dbt/profiles.yml` or a local `profiles.yml` before running `dbt run`.
- Great Expectations checkpoints are per data context — there is no global default.
- The repo uses both PostgreSQL (local) and BigQuery (cloud). Do not mix credentials.
- Airflow container has golang 1.21 installed — use it to compile Go ingestion scripts before DAGs run.
- FastAPI and Go containers both mount to the same Postgres. FastAPI runs on port 8000, Go on 9090 (host) → 8080 (container).
