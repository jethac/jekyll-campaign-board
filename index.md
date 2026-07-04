---
layout: default
title: Jekyll Campaign Board
description: A GitHub Pages-compatible campaign board powered by YAML, Liquid, Sass, and small optional JavaScript views.
---

<p class="site-eyebrow">Classic GitHub Pages compatible</p>

# Jekyll Campaign Board

<p class="site-lede">A read-only campaign tracker for public project pages. Keep the real work graph in your agent, notes, or issue tracker, then publish a curated YAML snapshot to Jekyll.</p>

<div class="usage-grid">
  <section>
    <h2>Strict publication boundary</h2>
    <p>Records render only when they are explicitly public and allowed on the requested surface.</p>
  </section>
  <section>
    <h2>Jira-style presentation</h2>
    <p>Campaign lanes, status pills, issue rows, dependencies, and redacted placeholders are static HTML/CSS.</p>
  </section>
  <section>
    <h2>Optional graph surface</h2>
    <p>The dependency-tree view is generated from a public-only JSON payload emitted by Liquid.</p>
  </section>
</div>

{% assign demo_board = site.data.campaign_boards.demo %}

## Demo Board

{% include campaign-board.html board=demo_board surface="blog" %}
{% include campaign-dependency-tree.html board=demo_board surface="dependency_tree" id="demo-dependencies" %}
{% include campaign-sections.html board=demo_board surface="blog" %}

## Usage

Copy `_includes/campaign-*.html`, `_sass/_campaign-board.scss`, and a board YAML file into a Jekyll site. Import the Sass partial from your main stylesheet:

```scss
@import "campaign-board";
```

See [the maintainer guide]({{ '/docs/maintainer-guide/' | relative_url }}) for the YAML contract and agent maintenance rules.
