# CFGB Example

A fictional multilingual blog and acceptance corpus for
[CFGB — Git-based Blog on Cloudflare](https://github.com/ymmt2005/cfgb).
This repository supplies the sample content, assets and settings; CFGB builds
the site, which is published on GitHub Pages.

**[Visit the live blog](https://ymmt2005.github.io/cfgb-example/)** ·
[English](https://ymmt2005.github.io/cfgb-example/en/) ·
[日本語](https://ymmt2005.github.io/cfgb-example/ja/) ·
[简体中文](https://ymmt2005.github.io/cfgb-example/zh-Hans/) ·
[한국어](https://ymmt2005.github.io/cfgb-example/ko/)

## Explore the example

The [Markdown showcase](src/content/posts/2026/2026-09-20-markdown-showcase/en.md)
contains code, diagrams, tables, footnotes, images and internal links. Its
[large diagrams](https://ymmt2005.github.io/cfgb-example/en/posts/markdown-rendering-showcase/#large-mermaid-diagrams)
provide a place to try the expanded viewer, zoom, fit and full screen.

The showcase and Protocol Buffers guide have Japanese, English, Simplified
Chinese and Korean versions. Other articles demonstrate missing translations.
Browse the topic and monthly archives, try search, and compare light and dark
views. The sample articles are fictional and reusable.

## Repository contents

| Path | Purpose |
| --- | --- |
| [`cfgb.yaml`](cfgb.yaml) | Example site settings |
| [`src/content/`](src/content/) | Articles, home/about pages and sidebar content |
| [`src/assets/site.svg`](src/assets/site.svg) | Original branding image used by this site |
| [`src/data/`](src/data/) | Topic definitions and cached link cards |
| [`examples/ymmt2005.dev.yaml`](examples/ymmt2005.dev.yaml) | An alternative site configuration example |
| [`tests/`](tests/README.md) | Acceptance data and developer notes, separate from published content |

The example defaults to Japanese, uses `Asia/Tokyo` for archives, and enables
site branding in `cfgb.yaml`. Its checked-in public URL is the placeholder
`https://example.invalid`; local and Pages builds override it.

## Run this example locally

Install CFGB and its build prerequisites using the
[CFGB installation guidance](https://github.com/ymmt2005/cfgb#build-a-site), then:

```sh
git clone https://github.com/ymmt2005/cfgb-example.git
cd cfgb-example
cfgb build --static --base-url http://localhost:8000/ --out dist
python3 -m http.server 8000 --directory dist/site
```

The last command uses Python's HTTP server; another static server works too.
Open `http://localhost:8000/` to browse the generated example. Edit the sample
content and rebuild to see changes.

## How this example is published

The [Pages workflow](.github/workflows/pages.yml) installs a pinned CFGB release
through [cfgb-action](https://github.com/ymmt2005/cfgb-action), builds with
`--force --static --base-url "$SITE_URL"`, checks generated links, and publishes
`dist/site/` when `main` changes. Pull requests build and check links without
publishing. The live example uses GitHub Pages.

The [Links workflow](.github/workflows/links.yml) also builds the corpus and
checks generated links. Scheduled and manual runs add advisory external-link
reports. Both workflows obtain link-check tooling from the matching CFGB tag.

For a fork, update `cfgb.yaml`, `SITE_URL` in the Pages workflow, and the matching
origin in the Links workflow. Select **GitHub Actions** as the Pages source in
repository settings.

See [CFGB's documentation](https://github.com/ymmt2005/cfgb) for content syntax,
CLI options and other hosting choices, and the
[setup Action documentation](https://github.com/ymmt2005/cfgb-action/blob/main/docs/usage.md)
for installation in workflows.

## License

The documentation, sample articles, original assets and test fixtures are
licensed under the [Apache License, Version 2.0](LICENSE).
