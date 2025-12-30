# Lemmas
lemmas powered by sporrer

## One-liner
Lemmas is an objective-driven market mapping engine for France: define a business objective, generate the relevant company universe, rank it with an explainable score, export it in minutes.

## The deep problem
Building a reliable company universe for a given objective is still slow, manual, and non reproducible.
Teams waste days translating messy local signals into business criteria, then defending opaque lists that cannot be audited or rerun.

## What is actually unsolved
- Objective to universe: turning a business intent into a concrete set of companies at scale.
- Evidence: being able to justify why each company is included and why it ranks higher.
- Iteration: changing the objective and getting a new list fast, without rebuilding everything.
- Industrialization: reproducible runs, versioned logic, programmatic access, exports.

## Why SIRENE matters but is not the product
SIRENE is a baseline enrichment layer for France:
- identity (SIREN, name, address)
- activity codes (NAF)
- size proxies (effectifs ranges)
- establishments structure
It is not sufficient to capture real business intent by itself.
Lemmas is built to plug additional signals later, without rewriting the core.

## North Star
North Star metric:
- Produce an actionable list of 1,000 companies (filtered + scored + exported) in under 10 minutes on a standard laptop.

Secondary metrics:
- Time to first export
- Explain score usage rate
- Profile iteration rate (profile edits followed by rerun)
- Export quality (low manual cleanup needed)

## Product
### Core workflow
1) Define objective via a Profile
2) Reduce universe via Filters
3) Rank via Explainable Scoring
4) Export and reuse

### Product surfaces
- Web Explorer (premium UI)
- API (programmatic access)
- CLI (batch runs)

## Functions (v1)
### Profiles
- YAML profiles with strict schema validation
- Versioning (profile hash + schema version)
- Simulation on sample (score distribution + top reasons)

### Explorer
- Fast search (name, SIREN, city, postal code, NAF)
- Filters and facets
- Table with pagination and sorting
- On-demand scoring for segment or selection
- Explain score (component breakdown + evidence)
- Configurable export (CSV, Excel)

### Company view
- France canonical identity snapshot
- Establishments overview
- Score and explanation for selected profile
- Add to export selection

### Jobs
- Run tracking (inputs, versions, parameters)
- Status and metrics (time, rows processed, memory)
- Artifacts (exports, logs, reports)
- Deterministic rerun support

## Non goals (v1)
- Global coverage
- Full legal documents platform
- CRM features
- People and executives data

## PRD (v1)
### Users
- Growth, Sales Ops, RevOps, consultants
- Data teams needing a reliable engine and API

### Top use cases
- Market mapping: "show me all companies matching objective X"
- Target list building: "give me top 1,000 with reasons"
- File enrichment: "enrich my CSV by SIREN and score it"

### Requirements
- Deterministic results: same dataset snapshot + same profile version => same output
- Explainability: each score component has traceable evidence
- Performance: smooth exploration, segment scoring in minutes
- Reliability: job history and artifacts, rerun capability

## Architecture
### Principles
- Core engine independent from API/UI
- Canonical store for serving, not only exports
- Precompute stable features, score fast on demand
- Version everything: dataset, profile, engine

### Modules
- core/
  - pipeline (ingest, clean, normalize, feature build)
  - scoring (components, explain output)
  - profiles (schema, validation, versioning)
- connectors/
  - sirene (baseline enrichment)
  - future signals (optional)
- store/
  - parquet partitions (companies, establishments, features)
  - query engine (fast filters and facets)
- api/
  - search, company, score, profiles, jobs, exports
- ui/
  - explorer, company view, profiles, jobs
- worker/
  - async execution for heavy jobs

### Data contracts
- Company (canonical, keyed by SIREN)
- Establishment (keyed by SIRET)
- Features (stable, reusable)
- Score (profile_version scoped)
- ScoreExplanation (evidence list)
- Job (inputs, parameters, artifacts, metrics)
