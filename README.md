# Observability

The common observability vocabulary -- the monitors and checks that watch a system, the alerts they raise, the logs, metrics and traces they read, the dashboards that show them and the SLOs that judge them -- distilled from the APIs that already exist. The incidents people respond to belong to the incident-management context. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Observability, Monitoring, APM, Application Performance Monitoring, Logging, Log Management, Metrics, Tracing, Uptime Monitoring, Infrastructure Monitoring in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| monitor | 44 | 44 | id 73%, name 70%, status 57%, type 55% | monitors 31, checks 8, monitor 6, healthchecks 1 |
| alert | 14 | 14 | id 86%, status 64%, name 50% | alerts 12, alert-rules 2, alert 1, triggers 1 |
| dashboard | 14 | 13 | name 85%, description 69%, id 69%, created_at 62% | dashboards 13, dashboard 2, boards 1 |
| log | 47 | 37 |  | logs 39, log 10, log_entries 1 |
| metric | 10 | 9 | metric 56%, type 56% | metrics 8, timeseries 3, series 2 |
| trace | 50 | 42 | trace_id 62%, name 52%, metadata 50% | traces 38, spans 12, trace 11 |
| slo | 7 | 7 | created_at 100%, name 86%, description 71%, sli 71%, tags 71%, updated_at 71%, id 57% | slos 5, slo 2 |

Cohort: 279 providers with harvested OpenAPI, 104 crosswalks. Of the providers exposing a monitor: 80% create, 96% list, 84% fetch, 66% update, 77% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A monitor agrees on name, id, status and type: uptime checks, synthetic tests and metric monitors share the identity of a watcher even where their thresholds differ.
- An SLO agrees almost completely -- name, description, its indicator, tags and dates -- the newest concept here and the most standardised.
- Traces agree on a trace id, a name and metadata; logs agree on nothing as a resource. Several are OpenTelemetry payloads whose shape lives in the OTLP standard, and others LLM-observability traces with a different shape again. OTLP deserves its own pass, as Open Banking did.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Incidents belong to incident management; observability ends at the alert.
- A monitor covers checks, uptime checks, synthetics and probes; singular `/check` is an action and does not count.
- Alert rules, alert policies and triggers are alerts (the rule and its firing are not yet separated -- too few providers expose both).
- Metrics count only beneath a query, series or versioned API path; a bare `/metrics` is a Prometheus scrape endpoint or a usage counter almost everywhere.
- Duplicates merged: Grafana, Honeycomb, Catchpoint.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
