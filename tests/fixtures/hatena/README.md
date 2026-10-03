# Synthetic Hatena migration corpus

All identifiers, origins and source entries are fictional. Reserved `.invalid`
hosts must be served by a fake transport; do not make real DNS/HTTP requests.
`input/responses.json` maps paginated Atom request URLs to responses.

The first Japanese article links forward to the later Japanese article. The
English article is an explicitly approved translation pair and shares the same
asset bytes. Another entry uses raw HTML, another Hatena syntax, and a synthetic
draft must be excluded. The latter three have no publishable expected output.

`decisions/` contains reviewed pair/category/ownership decisions. `expected/site/`
contains three expected Markdown variants, per-variant provenance, one shared
asset and a complete manifest. The summaries are supplied fixture values, not
actual AI responses. Paths are outside normal `src/content` discovery.

`cases.json` supplies restart/conflict scenarios for the future importer harness.
No importer implementation or live credentials are included.

The manifest records successful applications only and has no `status` field.
Its source/target hashes are a pair from the last successful apply. Conflicts
never update either hash or create a new ownership entry. Separate reports in
`expected/conflicts/` carry observed/proposed hashes and optional last-applied
pairs; `input/conflict-inputs.json` provides their exact input bytes. Live reports
belong under `.cfgb-work/hatena/`, outside discoverable content. Preserve local
targets and provenance sidecars on conflict.
