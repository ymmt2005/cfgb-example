# CFGB Example

An example blog you can explore and use as a starting point for your own site
with [**CFGB — Git-based Blog on Cloudflare**](https://github.com/ymmt2005/cfgb).
You keep articles, images and settings in Git; CFGB builds the site.

**[Visit the live blog](https://ymmt2005.github.io/cfgb-example/)** ·
[English](https://ymmt2005.github.io/cfgb-example/en/) ·
[日本語](https://ymmt2005.github.io/cfgb-example/ja/) ·
[简体中文](https://ymmt2005.github.io/cfgb-example/zh-Hans/) ·
[한국어](https://ymmt2005.github.io/cfgb-example/ko/)

Try the language switcher, search, topic and monthly archive pages, and light
and dark themes. The [Markdown showcase](src/content/posts/2026/2026-09-20-markdown-showcase/en.md)
demonstrates highlighted code, Mermaid diagrams, tables, footnotes, images and
internal links. The site also generates RSS feeds and a sitemap.

The sample articles are fictional and reusable. This repository contains blog
content and settings; CFGB supplies the renderer and its dependencies.

## Build and preview locally

Install the following:

- Git to clone the repository.
- The binary for your OS and architecture from the
  [CFGB v0.2.1 release](https://github.com/ymmt2005/cfgb/releases/tag/v0.2.1).
  Rename it to `cfgb` (`cfgb.exe` on Windows), make it executable where needed,
  and place it on your PATH. See [CFGB's installation guidance](https://github.com/ymmt2005/cfgb#build-a-site)
  for release verification.
- Node.js and npm or pnpm compatible with the release's
  [toolchain requirements](https://github.com/ymmt2005/cfgb/releases/download/v0.2.1/toolchain-requirements.json).
  Its `testedNodeVersion`, `testedNpmVersion` and `testedPnpmVersion` identify
  a tested combination. CFGB uses npm by default; set `CFGB_PACKAGE_MANAGER=pnpm`
  to use pnpm.

Then clone and build:

```sh
git clone https://github.com/ymmt2005/cfgb-example.git
cd cfgb-example
cfgb build --static --base-url http://localhost:8000/ --out dist
```

Serve `dist/site/` with a local HTTP server. For example, if you have Python:

```sh
python3 -m http.server 8000 --directory dist/site
```

Open [http://localhost:8000/](http://localhost:8000/), or go directly to
[/en/](http://localhost:8000/en/), [/ja/](http://localhost:8000/ja/),
[/zh-Hans/](http://localhost:8000/zh-Hans/) or [/ko/](http://localhost:8000/ko/).
Re-run the build after editing content.

`--static` creates entry and alias pages and direct language links for hosting
without a Cloudflare Worker. `--base-url` overrides the public URL for this build.
The checked-in configuration uses the placeholder `https://example.invalid`.

CFGB installs its embedded build dependencies in a temporary workspace. You
do not need a `package.json` or framework source in your blog repository.
Building requires internet access to download dependencies; serving the
generated site requires only a static host.

## Make it your blog

Fork this repository, or copy its content and settings into your own repository.
Start with these files:

| File or directory                                      | What to change                                                      |
| ------------------------------------------------------ | ------------------------------------------------------------------- |
| [`cfgb.yaml`](cfgb.yaml)                               | Site title, public base URL, default language and enabled languages |
| [`src/content/posts/`](src/content/posts/)             | Articles and their images                                           |
| [`src/content/home/`](src/content/home/)               | Introduction on each language's home page                           |
| [`src/content/pages/about/`](src/content/pages/about/) | About pages                                                         |
| [`src/content/aside/`](src/content/aside/)             | Optional sidebar Markdown                                           |
| [`src/data/topics.yaml`](src/data/topics.yaml)         | Topic IDs and labels for each enabled language                      |
| [`src/data/linkcards/`](src/data/linkcards/)           | Cached metadata for link cards                                      |

The example enables Japanese (`ja`), English (`en`), Simplified Chinese
(`zh-Hans`) and Korean (`ko`). Keep translated versions of an article in the same
folder as `ja.md`, `en.md`, `zh-Hans.md` and `ko.md`; an article can also exist
in only one language. The Protocol Buffers guide and Markdown showcase have
translations in each enabled language, while other articles demonstrate
missing-translation fallback. Each version has its own title, slug,
summary and publication date. If you remove a language from the configuration,
remove its Markdown variants too.

To add an article, create a folder such as
`src/content/posts/2026/2026-10-05-hello/` and write an `en.md` file:

```markdown
---
title: Hello, CFGB
slug: hello-cfgb
publishedAt: "2026-10-05T09:00:00Z"
topics:
  - writing
summary: My first article on a Git-based blog.
---

## Hello

Write your article here.
```

Use topic IDs defined in `src/data/topics.yaml`. Put article images in that
article's `assets/` directory and reference them with Markdown, for example
`![A description](./assets/photo.png)`. See the existing articles for more
formatting and links between articles.

`site.timezone` in `cfgb.yaml` determines monthly archive membership from the
publication timestamp, independently of the source folder name. Article times
are displayed in the reader's browser timezone; the fallback without JavaScript
is UTC.

CFGB v0.2.1 provides `build` and `version`. Edit Markdown directly for authoring;
the planned authoring, migration and Cloudflare upload commands are not yet
available.

## Publish to GitHub Pages

The included [Pages workflow](.github/workflows/pages.yml) installs CFGB with
[cfgb-action](https://github.com/ymmt2005/cfgb-action), builds the site, checks
generated links and publishes it when `main` changes. Pull requests build and
check links without publishing.

To publish your own copy:

1. Set `site.baseUrl` in `cfgb.yaml` to your public site URL.
2. Set `SITE_URL` in `.github/workflows/pages.yml` to the same URL. For a project
   site, include the repository path: `https://YOUR-NAME.github.io/YOUR-REPO/`.
3. If you keep the separate [Links workflow](.github/workflows/links.yml), replace
   its `https://example.invalid` link-check arguments with your public site URL.
4. In **Settings → Pages**, select **GitHub Actions** as the source.
5. Push to `main`, or select **Actions → Pages → Run workflow**.

The workflow uses `--static --base-url "$SITE_URL"` and uploads `dist/site/`.
CFGB applies the hosting path to navigation, images, search, feeds and canonical
URLs. The workflow uses GitHub's job token; no PAT or Cloudflare API token is
needed.

For another static host, build with that host's complete public URL and
`--static`, then publish `dist/site/`.

## Further reading

- [CFGB](https://github.com/ymmt2005/cfgb) — CLI releases and documentation.
- [cfgb-action usage](https://github.com/ymmt2005/cfgb-action/blob/main/docs/usage.md)
  — installing CFGB and its matching build toolchain in GitHub Actions.
- [Test fixtures](tests/README.md) — developer reference for the acceptance data
  in this repository. These files are separate from the published blog content.

CFGB is an independent open-source project, not affiliated with Cloudflare, Inc.

## License

The documentation, sample articles, original assets and test fixtures are
licensed under the [Apache License, Version 2.0](LICENSE).
