# Using this corpus from CFGB

Pin the example repository to a reviewed commit. Copy the positive tree into a
temporary directory; never mutate the source checkout in place for negative tests.
`fixtures/validation/cases.json` contains independent mutations of this baseline.
Each mutation is applied alone, with the fixed clock `2026-10-03T12:00:00+09:00`.
Supported fixture operations: set/remove a frontmatter key, replace body, remove
an asset, copy a variant. Each operation is declarative input to the future test harness.

AI lifecycle records carry complete title/body/summary and sidecar hashes; a fake
provider should be invoked only for `generate` cases, and never for protected/no-op
cases. Migration cases describe sequential runs with source and local edits.
Implement those against a fake Atom/asset server before using live credentials.

Expected routes are origin-relative. Rendering checks substitute `site.baseUrl`
for absolute canonical metadata. Preview origin must never replace canonical
origin. The fixture clock is for tests only; production validation uses real time.

Search expectations must run against actual Pagefind output in a browser.
Initial data validation only checks that query targets exist, not actual ranking.
No production validator or test harness is implemented in this repository.
