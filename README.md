# CFGB example and implementation contract

Reference content for the v1 implementation specifications for **CFGB — Git-based Blog on Cloudflare**,
an independent open-source project not affiliated with
Cloudflare, Inc.

This repository is a **content/specification corpus**, not an implemented Astro
site or the Go CLI. Nothing here deploys a website or calls an AI provider.
The command examples describe the CLI to be implemented. CFGB owns the reusable
renderer, Worker and pinned dependencies, with sources embedded in its Go release
binary. This corpus and real content repositories remain free of Astro, Worker
and package configuration; Node.js is a CFGB build prerequisite.

## Start here

- [Architecture and decisions](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/00-architecture.md)
- [CLI commands and diagnostics](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/01-cli.md)
- [Configuration contract](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/02-configuration.md)
- [Content, rendering and URL contract](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/03-content.md)
- [GitHub and Cloudflare delivery](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/04-delivery.md)
- [AI summaries and ownership](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/05-ai.md)
- [Hatena migration](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/06-migration.md)
- [Acceptance matrix and fixture guide](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/07-acceptance.md)
- [Primary-source references](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/08-references.md)
- [Unified GitHub Action contract](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/09-github-action.md)

`cfgb.yaml` deliberately uses `https://example.invalid`: the example must not
claim the canonical URLs of the real blog. The separate production site uses
`https://ymmt2005.dev`; see [its configuration overlay](examples/ymmt2005.dev.yaml).
No account IDs, API keys, personal drafts, or actual Hatena exports are included.
All sample articles and migration records are synthetic, written for this corpus.
They are not statements or imported publications by the repository owner.

## Repository layout

| Path | Purpose |
| --- | --- |
| `src/content/posts/<year>/<article-key>/{ja,en}.md` | Publishable positive examples |
| `src/content/home/`, `src/content/pages/about/` | Locale-specific standalone pages |
| `src/data/topics.yaml` | Shared topic IDs and localized labels |
| `src/data/linkcards/` | Committed metadata; no build-time fetch |
| [CFGB schemas](https://github.com/ymmt2005/cfgb/tree/main/schemas) | Canonical JSON Schema 2020-12 contracts |
| `tests/search/queries.yaml` | Search acceptance queries; renderer still required |
| `tests/build-delivery/` | Artifact, command boundary and deployment gate cases |
| `tests/ai/` | Summary evaluation and ownership transition cases |
| `tests/fixtures/` | Negative validation and synthetic migration inputs |
| `tests/expected/` | Static routes, Worker routes, 404 fallbacks, translation groups and aliases |

## Using the corpus

See `tests/README.md` for fixture semantics. No CLI, renderer, CI workflow, or
validator is implemented here. Expected results are acceptance data for the future
CFGB implementation. They do not imply that browser rendering, Pagefind quality,
Cloudflare Access, Hatena migration or LLM generation have already been tested.

Pin a Git commit when consuming this repository. Review schema changes in the
[tool repository](https://github.com/ymmt2005/cfgb) together with example changes.
Tool version, schema version, and site version remain independent. The
[cfgb-action repository](https://github.com/ymmt2005/cfgb-action) owns one integrated
Action for setup/prepare/validate/summarize/build and optional uploads. It uses
this corpus for CLI parity checks; Action and CLI releases are separately pinned.
No Action implementation or active workflow is added to this corpus.

## License

This repository, including its documentation, sample articles, original assets
and test fixtures, is licensed under the [Apache License, Version 2.0](LICENSE).
