# Using this corpus from CFGB

Pin the example repository to a reviewed commit. Copy the positive tree into a
temporary directory; never mutate the source checkout in place for negative tests.
`fixtures/validation/cases.json` contains independent mutations of this baseline.
Each mutation is applied alone, with the fixed clock `2026-10-03T12:00:00+09:00`.
Supported fixture operations: set/remove a frontmatter key, replace body, remove
an asset, copy a variant. Each operation is declarative input to the future test harness. Assert the specified
`expectedError` or `expectedWarning` and `expectedExit`; warning-only authoring
cases must succeed. `default` means no mode flag. Summary structural types are
checked before semantic missing/empty/Unicode White_Space checks.

`fixtures/configuration/cases.json` uses `setConfig`/`removeConfig` operations
with JSON Pointer paths on a copy of `cfgb.yaml`. Omitted preview-access settings
must be populated by the configuration loader as true; Schema defaults alone do
not mutate data. Invalid configuration exits 2.

AI lifecycle records carry complete title/body/summary and sidecar hashes; a fake
provider should be invoked only for `generate` cases, and never for protected/no-op
cases. Migration cases describe sequential runs with source and local edits.
Implement those against a fake Atom/asset server before using live credentials.
Hatena `input/conflict-inputs.json` supplies exact observed/proposed bytes and a
prior successful entry where present. Expected conflict reports are validated
against CFGB's separate conflict Schema. `proposedTarget` is synthetic plan output,
not proof of importer conversion. Conflicts preserve target, sidecars and the
last-applied manifest pair, and never create ownership of an unowned target.

Expected routes are origin-relative. `expected/static-routes.json` lists emitted
public routes; `expected/worker-routes.json` lists runtime endpoints and locale
redirect cases; `expected/fallbacks.json` lists 404 assets and navigation/fetch/HEAD
cases. Error assets and Worker routes are not canonical article routes. The locale
cookie name is `cfgb_locale`. Missing non-navigation requests delegate to Static
Assets if they reach the Worker, without an independent fallback algorithm.
`build-delivery/cases.json` assumes all unspecified prerequisites are valid: a
sealed matching artifact, clean checkout, configured target and trusted access.
Successful upload cases assert no rebuild. These are declarative acceptance cases,
not executed Cloudflare tests. Cases may provide simulated CI variables/Git
metadata, release-runtime mismatches, retained-session failures and Worker-level
Access policy conditions. Unspecified requirements remain satisfied; provenance
SHA values are synthetic, not actual repository commits. Bootstrap acceptance
checks pinned executable reuse across separate Workers Builds command shells.
Build IDs are synthetic opaque strings, not required RFC UUIDs. Preserve their
exact values in the manifest; `toolchainSessionId` and the workspace component
are lowercase SHA-256 of the exact UTF-8 build-ID bytes. Path-separator cases
assert only the hashed component is used and no raw-value path is created.
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
status remain CLI expectations here. See the [setup Action contract](https://github.com/ymmt2005/cfgb/blob/main/docs/spec/09-github-action.md).
No live Action acceptance has been run at this documentation-only stage.
