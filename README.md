# CFGB example and implementation contract

Reference content for the v1 implementation specifications for **CFGB — Git-based Blog on Cloudflare**,
an independent open-source project not affiliated with
Cloudflare, Inc.

This repository owns the **content/specification corpus** and publishes its
positive examples through GitHub Pages using CFGB v0.1.0. It does not call AI
providers. CFGB owns the reusable renderer, Worker and pinned dependencies,
with sources embedded in its Go release binary. This corpus and real content
repositories remain free of Astro, Worker and package configuration; Node.js
is a CFGB build prerequisite. Some command examples describe later CLI milestones
that remain under implementation.

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
- [GitHub setup Action usage](https://github.com/ymmt2005/cfgb-action/blob/main/docs/usage.md)
- [Build runtime and bootstrap](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/10-build-runtime.md)

`cfgb.yaml` deliberately uses `https://example.invalid`: the example must not
claim the canonical URLs of the real blog. The separate production site uses
`https://ymmt2005.dev`; see [its complete configuration example](examples/ymmt2005.dev.yaml).
Copy that example to the personal repository root as `cfgb.yaml`; it is not an
overlay or implicit merge, and relative paths resolve from that root.
No account IDs, API keys, personal drafts, or actual Hatena exports are included.
All sample articles and migration records are synthetic, written for this corpus.
They are not statements or imported publications by the repository owner.

## Repository layout

| Path | Purpose |
| --- | --- |
| `src/content/posts/<year>/<article-key>/{ja,en}.md` | Publishable positive examples |
| `src/content/home/`, `src/content/pages/about/` | Locale-specific standalone pages |
| `src/content/aside/` | Optional locale Markdown for the right-hand column. Not a page |
| `src/data/topics.yaml` | Shared topic IDs and localized labels |
| `src/data/linkcards/` | Committed metadata; no build-time fetch |
| [CFGB schemas](https://github.com/ymmt2005/cfgb/tree/main/schemas) | Canonical JSON Schema 2020-12 contracts |
| `tests/search/queries.yaml` | Search acceptance queries; renderer still required |
| `tests/build-delivery/` | Artifact, command boundary and deployment gate cases |
| `tests/ai/` | Summary evaluation and ownership transition cases |
| `tests/fixtures/` | Negative validation and synthetic migration inputs |
| `tests/expected/` | Static routes, Worker routes, 404 fallbacks, sitemap expectations, translation groups and aliases |

## Using the corpus

See `tests/README.md` for fixture semantics. The CLI and renderer live in CFGB.
The Links CI workflow installs an exact immutable CFGB release with the
separately commit-pinned setup Action and checks the generated HTML. Other
expected results remain acceptance data for the ongoing CFGB implementation. They do not imply that browser rendering, Pagefind quality,
Cloudflare Access, Hatena migration or LLM generation have already been tested.

Pin a Git commit when consuming this repository. Review schema changes in the
[tool repository](https://github.com/ymmt2005/cfgb) together with example changes.
Tool version, schema version, and site version remain independent. The
[cfgb-action repository](https://github.com/ymmt2005/cfgb-action) installs a verified
CLI and registers it on PATH. Workflows run the CLI directly against this corpus;
Action and CLI releases are separately pinned. Future GitHub-specific capabilities
stay in that single Action repository and entry point when justified. The setup Action implementation stays in its own repository.

## GitHub Pages publication

The [Pages workflow](.github/workflows/pages.yml) builds the site with
`cfgb build --static --base-url https://ymmt2005.github.io/cfgb-example/ --out dist`,
checks the generated links, and publishes `dist/site/` on main updates. Pull
requests run the build/link checks without deploying. The project path is applied
to navigation, images, bundles, search, feeds and canonical metadata. Static
entry/alias pages and direct language links work without a Cloudflare Worker.
The corpus's `cfgb.yaml` and expected URLs keep their reserved test origin;
publication overrides the URL for that invocation.

For initial setup, open this repository's **Settings → Pages** and select
**GitHub Actions** as the source. Then run **Actions → Pages → Run workflow**
on main, or push a main update. Publication uses the job's GitHub token with
`pages: write` and `id-token: write`; no PAT or Cloudflare token is required.
The public address is <https://ymmt2005.github.io/cfgb-example/>.

## License

This repository, including its documentation, sample articles, original assets
and test fixtures, is licensed under the [Apache License, Version 2.0](LICENSE).

## Link checks

[Links CI](.github/workflows/links.yml) builds the current content and checks all
generated HTML on pushes and pull requests. Internal files, images, and fragment
references must resolve; self-origin absolute URLs are mapped to the generated
site, and aliases resolve through `_redirects`. Worker endpoints are covered by
CFGB's Worker tests. Negative validation/migration fixtures are not scanned.

lychee is installed with [aqua.yaml](aqua.yaml) and
[aqua-checksums.json](aqua-checksums.json): the tool/registry versions and their
checksums are committed, and the aqua-installer Action uses a full commit SHA.
CI enforces checksum verification and never regenerates the lock.

Scheduled runs (Monday 06:17 Japan time) and manual runs additionally check the
external links authored in the articles. The checker uses a one-day cache,
limited concurrency, retries, and timeouts. Results appear in the Actions job
summary and the `external-links` artifact. External availability is advisory;
installation, build, and internal-link errors still fail the workflow. The
Links workflow does not deploy the site, and no AI credentials are required.

The temporary `cfgb-tool/` checkout supplies only the link-check script from the
same CLI release tag. Framework sources and dependencies come from the installed
binary's temporary build workspace. Review the Action commit and CLI release
pins independently when updating CFGB.

To update lychee or the registry, edit the exact pins, run
`aqua update-checksum -prune`, and review/commit the new checksums with the config.
