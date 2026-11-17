<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/duckwake/duckwake.github.io/main/brand/svg/duckwake-horizontal-reversed.svg">
    <img alt="DuckWake" src="https://raw.githubusercontent.com/duckwake/duckwake.github.io/main/brand/svg/duckwake-horizontal.svg" width="360">
  </picture>
</p>

<h3 align="center">Real-time DuckLake tables, exactly once.</h3>

<p align="center">
  <a href="https://duckwake.github.io">Website</a> ·
  <a href="https://duckwake.github.io/docs/start/quick-start/">Quick start</a> ·
  <a href="https://github.com/duckwake/duckwake/blob/main/docs/sql.md">SQL reference</a> ·
  <a href="https://github.com/duckwake/duckwake/blob/main/docs/roadmap.md">Roadmap</a>
</p>

DuckWake is an open-source (MIT) streaming SQL engine for DuckDB and DuckLake. You write a view in SQL or as a dbt
model, and DuckWake keeps its table in your lake current as events arrive from Kafka: every few seconds it
commits the new rows, the view's state and the Kafka offsets in one DuckLake transaction, so a crash never loses
or repeats an event.

- **Your lake is the result.** Each view is a DuckLake table that dashboards, notebooks and dbt read with plain
  SQL. Views can also read lake tables and other views through their change feed.
- **Exactly once, and you can check it.** `duckwake_verify` compares a view's table with its query run as one
  batch. On every pull request, CI kills workers mid-commit, then checks every view.
- **Nothing new to run.** It runs next to DuckDB, as an extension, a CLI or a small server. Workers coordinate
  through the lake's catalog: no coordinator service, no separate state store.

```sql
CALL duckwake_create_view('analytics.clicks_per_min',
  'SELECT window_start, game_id, count(*) AS n, sum(amount) AS revenue
   FROM TUMBLE(clicks, ts, INTERVAL 1 MINUTE) GROUP BY ALL');
CALL duckwake_start('clicks_per_min');
FROM lake.analytics.clicks_per_min;   -- an ordinary DuckLake table
```

## Repositories

| Repository | What it is |
| --- | --- |
| [duckwake](https://github.com/duckwake/duckwake) | The engine: the `duckwaked` server, the `duckwake` extension and CLI, the docs and the design documents |
| [dbt-duckwake](https://github.com/duckwake/dbt-duckwake) | Sources and views as dbt models, on dbt-duckdb (mirrored from `dbt/` in duckwake) |
| [duckwake.github.io](https://github.com/duckwake/duckwake.github.io) | The website, the brand kit and the extension repository |

## Status

Version 0.2, on its [roadmap](https://github.com/duckwake/duckwake/blob/main/docs/roadmap.md) to production.
Kafka with JSON today; windows, aggregates, lookup joins and views over views. Stream–stream joins and CDC
sources are next.

## Get involved

Questions, ideas and bug reports are welcome as
[issues](https://github.com/duckwake/duckwake/issues). If you keep data in DuckDB or DuckLake and have events in
Kafka, we would like to hear what you do today when someone needs numbers fresher than the nightly batch.
See [CONTRIBUTING.md](https://github.com/duckwake/duckwake/blob/main/CONTRIBUTING.md) to build and test.
