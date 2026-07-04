# Jekyll Campaign Board

A Classic GitHub Pages-compatible campaign board for Jekyll sites.

It is intentionally just YAML, Liquid, Sass, and a small optional JavaScript dependency-tree view. The package presents public campaign state; it does not sync GitHub, mutate YAML, run builds, or store private project data.

## What It Provides

- Jira-style campaign lanes rendered from YAML.
- Campaign detail sections with status summaries.
- Issue/PR rows with status pills and dependencies.
- Strict `visibility: public` plus `surfaces` allow-list checks.
- Redacted dependency placeholders that do not expose private IDs.
- Multiple stable IDs per item or campaign.
- Light and dark mode CSS variables.
- Optional public-only JSON graph and dependency-tree renderer.

## Quick Start

Copy these files into a Jekyll site:

```text
_includes/campaign-board.html
_includes/campaign-sections.html
_includes/campaign-issue-list.html
_includes/campaign-dependency-data.html
_includes/campaign-dependency-tree.html
_sass/_campaign-board.scss
```

Add a board under `_data/campaign_boards/example.yml`, then render it:

```liquid
{% assign board = site.data.campaign_boards.example %}
{% include campaign-board.html board=board surface="blog" %}
{% include campaign-dependency-tree.html board=board surface="dependency_tree" id="example-dependencies" %}
{% include campaign-sections.html board=board surface="blog" %}
```

Import the stylesheet:

```scss
@import "campaign-board";
```

## Publication Model

The YAML in this repository is a public snapshot. Keep private planning data in Hermes, OpenClaw, GitHub Projects, local notes, or another source of truth, then export only the curated public subset.

Records render only when both checks pass:

```text
visibility == "public" && surfaces contains requested_surface
```

If a public item depends on private work, use `redacted_dependencies` instead of putting the private dependency ID in public YAML.

## Demo

The repository is also a GitHub Pages demo site:

<http://jethachan.net/jekyll-campaign-board/>

The sample board lives at `_data/campaign_boards/demo.yml` and points at example issues in this repository.

## License

MIT License. See [LICENSE](LICENSE).
