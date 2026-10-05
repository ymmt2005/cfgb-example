# Using this corpus from CFGB

Pin the example repository to a reviewed commit. Copy the positive tree into a
temporary directory; never mutate the source checkout in place for negative tests.
`fixtures/validation/cases.json` contains independent mutations of this baseline.
Each mutation is applied alone, with the fixed clock `2026-10-03T12:00:00+09:00`.
Supported fixture operations: set/remove a frontmatter key, replace body, remove
an asset, copy a variant. Each operation is declarative input to the future test harness. Assert the specified
`expectedError` or `expectedWarning` and `expectedExit`; warning-only authoring
cases must succeed. `default` means no mode flag. Summary mode checks use the
Go-decoded string: YAML `42` becomes `"42"`, `false` becomes `"false"`, and `null`
becomes empty text. Numeric/boolean values are not raw-type errors; empty/null
values follow the same missing-summary rule in each mode. Short summaries may
also warn, without changing a successful exit. `expectedDecodedSummary` records
the value to check at the typed loader boundary. Alias uniqueness is semantic
validation on decoded slices; emit `E_ALIAS_DUPLICATE` in every mode, including
duplicates across variants. The OG path-escape mutation resolves outside the
article directory; path containment is checked before asset existence. A
cross-locale alias emits `E_URL_COLLISION`. Future publication dates warn with
`W_FUTURE_DATE` and exit 0 in default/authoring mode; publish mode rejects them.

`fixtures/validation/schema-cases.json` contains optional standalone Schema
scenarios. Apply those operations to a positive tree and check the article
Schema's result against `expectedValid`, only when that separate check is
explicitly requested. They are not `cfgb validate`/`build` rejection requirements
and have no CLI exit-code/validation-mode contract. In particular, the existing
slug/alias patterns and raw summary types are not typed-loader policies.

Configuration and article front matter decode directly into Go structs; topics
decode into a typed map. Loading uses the YAML decoder's behavior and preserves
valid UTF-8 article body bytes. The user explicitly requested malformed UTF-8
checks and file-start BOM removal; preserve interior U+FEFF and line endings.
Do not apply the optional JSON Schemas or additional YAML tag/document rules as
implicit loading/build gates. The validation
fixtures above describe separate validation scenarios, not loader prerequisites.
Before implementing a new correctness/input-rejection rule, obtain explicit
human approval as required by [CFGB's AGENTS.md](https://github.com/ymmt2005/cfgb/blob/main/AGENTS.md).
Existing unverified policies are listed in the [PR #3 audit](https://github.com/ymmt2005/cfgb/blob/cursor/phase1-renderer-build-47ed/docs/reviews/pr-3-policy-audit.md).

`fixtures/configuration/cases.json` uses `setConfig`/`removeConfig` operations
with JSON Pointer paths on a copy of `cfgb.yaml`. `stage: load` exercises direct
Go struct decoding and defaults; it does not run JSON Schema validation. Omitted
preview-access settings default to true, while explicit false is preserved.
`deploy.productionBranch` defaults to `main` when the field or section is absent.
`stage: command` cases identify the later CLI command that checks a decoded
setting: deploy rejects an invalid branch with `E_DEPLOY_TARGET`, and summarize
rejects missing model settings with `E_PROVIDER_CONFIG`. Those command checks
are not configuration-decoder constraints. Decode/configuration-use failures
exit 2. The configuration schema is an optional standalone/editor aid.

AI lifecycle records carry complete title/body/summary and sidecar hashes; a fake
provider should be invoked only for `generate` cases, and never for protected/no-op
cases. Migration cases describe sequential runs with source and local edits.
Implement those against a fake Atom/asset server before using live credentials.
Hatena `input/conflict-inputs.json` supplies exact observed/proposed bytes and a
prior successful entry where present. Expected conflict reports are validated
against CFGB's separate conflict Schema. `proposedTarget` is synthetic plan output,
not proof of importer conversion. Conflicts preserve target, sidecars and the
last-applied manifest pair, and never create ownership of an unowned target.
Migration cases require zero model calls even with all AI credentials absent.
Pair candidates use deterministic snapshot heuristics; only approved explicit
decisions form translation groups. Summary generation is a later authoring step.

Expected routes are origin-relative. `expected/static-routes.json` lists emitted
public routes; `expected/worker-routes.json` lists runtime endpoints and locale
redirect cases; `expected/fallbacks.json` lists 404 assets and navigation/fetch/HEAD
cases. Error assets and Worker routes are not canonical article routes. The locale
cookie name is `cfgb_locale`. Missing non-navigation requests delegate to Static
Assets if they reach the Worker, without an independent fallback algorithm.
`build-delivery/cases.json` assumes unspecified prerequisites are valid: a
complete artifact with its required files, a publication snapshot, completed
checks, a configured target, and trusted access. Recorded commit, branch, build
ID, and provenance provider are optional diagnostics. A dirty checkout, an edit
to site bytes after the build, and a difference between those diagnostics and
the current invocation stay successful. The manifest carries no configuration,
input, or output hashes.
A case's optional `productionBranch` sets `deploy.productionBranch`. Omission
means `main`. `branch` is the current invocation's branch. It may differ from a
branch recorded in the artifact. Custom-branch cases verify deploy accepts only
the configured branch and preview accepts other branches, including `main` when
production is `master`. Successful upload cases assert no rebuild. These are
declarative acceptance cases, not executed Cloudflare tests. Cases may provide
simulated CI variables and Git metadata, release-runtime mismatches,
retained-session failures, and Worker-level Access policy conditions.
Unspecified requirements remain satisfied; diagnostic SHA values are synthetic,
not actual repository commits. `expectedManifest` lists diagnostics that are
present. An empty build identifier is omitted. Runtime-config cases use a
synthetic release compatibility date: both upload commands must emit that exact
top-level `compatibility_date`, `workers_dev: false`, `preview_urls: true`,
`previews: {}`, and top-level assets, independently of execution date. No real
release compatibility test is claimed. Live integration acceptance also checks
the public canonical custom domain, disabled production workers.dev route, and
enabled Preview/Version URLs with Access protection, for fresh and previously
configured Workers. Bootstrap acceptance checks pinned executable reuse across
separate Workers Builds command shells. Build IDs are synthetic opaque strings,
not required RFC UUIDs. Preserve a non-empty value in the manifest as
`buildUUID`. `toolchainSessionId` is the basename of a fresh workspace under the
shared temporary directory (`os.TempDir()`), matching `cfgb-build-*`. The
manifest ID equals that basename. Do not require a particular random suffix.
The raw build identifier does not choose the workspace path, including when it
contains path separators or differs from the identifier recorded in the
artifact. Two builds get distinct workspaces even when their build identifiers
are equal.

`expectedUploadArtifact: supplied-artifact` means CFGB uploads the bytes it was
given, including an artifact edited after the build, and does not rebuild or
change those bytes. Upload cases cover required files, runtime compatibility,
publication timestamps, the current branch, and Access. Transferred-artifact
cases leave the supplied bytes unchanged and recreate only the pinned upload
toolchain. See the [artifact checks](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/10-build-runtime.md#artifact-checks).
CFGB executable release-attestation verification is a separate security requirement.

`expected/sitemap.json` fixes the corpus index, numbered sitemap file, canonical
HTML URL set and article alternates. Parse real `@astrojs/sitemap` output in
renderer acceptance; the index must reference every emitted file and their URL
union must equal `canonicalPaths`, excluding the listed resources. Substitute
`site.baseUrl` for origin-relative fixture paths; compare URL sets rather than
XML byte layout. Paired articles with different slugs retain their actual
alternates; unpaired articles do not gain invented alternates.
Rendering checks substitute `site.baseUrl`
for absolute canonical metadata. Preview origin must never replace canonical
origin. The fixture clock is for tests only; production validation uses real time.

Search expectations must run against actual Pagefind output in a browser.
Initial data validation only checks that query targets exist, not actual ranking.
No production validator or test harness is implemented in this repository.

`ymmt2005/cfgb-action` requires an exact version, selects the runner asset and
verifies its immutable release and GitHub release attestation before installing
the CLI and registering it on PATH. Workflow
`run` steps execute the selected CLI release against this corpus directly.
Release-attestation/platform-selection/cache/PATH tests belong in `cfgb-action`; domain diagnostics and exit
status remain CLI expectations here. See the [setup Action documentation](https://github.com/ymmt2005/cfgb-action#readme).
The setup Action's CI exercises release verification and installation on native
runners. This repository consumes that tested Action with an exact release pin.

## Executed link checks

The repository's [Links workflow](../.github/workflows/links.yml) builds the
current positive corpus using the setup Action and an exact CFGB release, then runs checksum-pinned
lychee on the generated HTML. Internal link checks run on push/PR. Manual and
weekly runs add advisory external-link availability reports with caching and
retries. These checks do not execute the negative acceptance fixtures described
above, and do not imply that later migration/AI/Cloudflare deployment gates are implemented.
The [Pages workflow](../.github/workflows/pages.yml) additionally checks the
static build at the GitHub Pages project path before publishing it.

## Archive and reader dates

`site.timezone` controls monthly archive membership only. `expected/date-display.json`
uses the September UTC / October Tokyo boundary article: its archive route stays
fixed while browser-local date text changes. Generated `datetime`, JSON-LD and
Pagefind timestamps remain UTC; RSS keeps the same UTC instant. Without JavaScript,
display the static UTC date. `new` uses UTC for creation timestamps/group names.
The renderer/browser never receives the configured archive timezone. The current
CFGB tests cover this contract through Go metadata, generated HTML/RSS and real
browser contexts; this fixture also exposes the expected values to corpus consumers.

## Large Mermaid diagrams

The translated Markdown showcases contain a wide flowchart, a tall flowchart
with review loops, and a detailed sequence diagram. They share diagram source
while keeping the surrounding instructions localized. Use the generated pages
to check label readability, scrolling in both directions, zoom, fit, actual
size, full screen, themes, and returning to the article. CFGB v0.4.0 supplies
the expanded viewer; JavaScript-disabled pages retain the diagram source.
