# forge-docs

Documentation **content** for [`phpdevkits/forge-sdk`](https://github.com/phpdevkits/forge-sdk). This repo holds markdown only — it is consumed by the phpdevkits docs platform and published at **forge.phpdevkits.com**.

## Structure

```
docs/
├── config.json        # display name, theme, ordered sidebar nav
├── introduction.md
├── installation.md
├── authentication.md
├── servers.md
├── sites.md
├── deployments.md
├── ssh-keys.md
└── daemons.md
```

Each markdown file has frontmatter (`title`) and is referenced by its slug (filename without `.md`) in `config.json`'s `nav`.

## Versioning — one branch per version

Versions are **branches**, not folders. The default branch is the latest version line (mirrors `laravel/docs`):

| Branch | Documents |
|--------|-----------|
| `0.x`  | Forge SDK `0.x` (current) |
| `1.x`  | created when the SDK ships a `1.x` line |

To start a new version line, branch from the current one (e.g. `git checkout -b 1.x`) and edit `docs/` there. The docs platform tracks each version branch and renders a version switcher.

## Editing

Edit any file under `docs/` on the relevant version branch and push. A webhook resyncs **forge.phpdevkits.com** within seconds (a nightly job is the fallback).

## `config.json`

```json
{
    "name": "Forge SDK",
    "theme": { "primary": "#e0532f" },
    "nav": [
        { "group": "Getting Started", "items": [ { "title": "Introduction", "page": "introduction" } ] }
    ]
}
```
