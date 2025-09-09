
# Data Quality Starter Kit (dbt + Great Expectations + Pytest)

This bundle gives you a reproducible lab with **fictitious data** to practice:
- dbt **tests** (schema & custom via dbt-utils) on seeds (DuckDB)
- **Great Expectations** programmatic checks
- **Unit & Integration tests** with pytest
- A **Justfile** to run everything in 1-2 commands

## 🗂 Structure
```
data_quality_starter_kit/
  data/                            # raw CSVs
  dbt_project/                     # dbt project (DuckDB)
    models/
      fct_orders_enriched.sql
      schema.yml                   # dbt tests incl. accepted_values & relationships
    seeds/                         # seeds loaded by dbt seed
    dbt_project.yml
    profiles.yml                   # DuckDB local profile
    packages.yml                   # dbt_utils
  great_expectations_light/
    run_ge_checks.py               # lightweight GE checks with pandas
  tests/
    test_unit_basic.py
    test_integration_join.py
  Justfile
  requirements.txt
  README.md
```

## 🚀 Quickstart
> Requires Python 3.10+

```bash
cd data_quality_starter_kit
just setup
just dbt-seed
just dbt-run-tests
just ge-checks
just pytest
# or everything:
just all
```

## ✅ What gets validated?
- **dbt tests**
  - `not_null`, `unique` on IDs
  - `accepted_values` (countries, statuses)
  - `relationships` orders.customer_id -> customers.customer_id
  - custom rule: `amount_eur >= 0` (via dbt_utils.expression_is_true)
- **Great Expectations**
  - uniqueness, not nulls, accepted values, non-negative amounts
- **pytest**
  - unit checks on columns
  - integration join produces only known customers

## 🧩 Customize
- Add new columns in `data/*.csv` and extend tests in `dbt_project/models/schema.yml`
- Add new GE assertions in `great_expectations_light/run_ge_checks.py`
- Extend Pytest with domain rules (e.g., min basket amount for FR)

## 🛡 Tips
- Mark critical tests with dbt `severity: error` and less critical with `warn`
- Fail CI on test failures and export artifacts (dbt logs & GE run output)
- Add freshness tests if you manage incremental loads
