---
layout: default
title: Maintainer Guide
permalink: /docs/maintainer-guide/
---

# Maintainer Guide

Jekyll Campaign Board is a static publication layer. It is not a private work tracker, a GitHub sync tool, or an editable project-management app.

## Files

- `_includes/campaign-board.html` renders status lanes.
- `_includes/campaign-sections.html` renders campaign detail sections.
- `_includes/campaign-issue-list.html` renders public issue rows.
- `_includes/campaign-dependency-data.html` emits public-only graph JSON.
- `_includes/campaign-dependency-tree.html` renders the optional JavaScript dependency tree.
- `_sass/_campaign-board.scss` contains the board, issue list, graph, and dark-mode styles.
- `_data/campaign_boards/*.yml` contains public board snapshots.

## Publication Rules

Every rendered record must pass both checks:

```text
visibility == "public" && surfaces contains requested_surface
```

Use `surfaces` as a strict allow list. Common surfaces are `blog` and `dependency_tree`.

Do not put secrets, private issue IDs, private URLs, local paths, or sensitive tracker keys in public YAML. Public GitHub Pages repositories expose the YAML source.

## Stable IDs

Items and campaigns can carry multiple `stable_ids`. Each stable ID has its own publication decision:

```yaml
stable_ids:
  - id: "campaign-item:example"
    visibility: "public"
    surfaces:
      - "blog"
      - "dependency_tree"
  - id: "internal:tracker-123"
    visibility: "private"
```

Only public stable IDs allowed on the requested surface are serialized into graph JSON.

## Redacted Placeholders

Use `redacted_dependencies` when a public item depends on work that must not be named in public YAML:

```yaml
redacted_dependencies:
  - reason: "Private planning item."
```

The reason is for maintainers and agents; the rendered public label is `[REDACTED]`.

## Source Refs

`source_refs` are for agents and validation tools. The bundled Liquid views do not render them, and the graph JSON does not serialize them.

```yaml
source_refs:
  - id: "github:owner/repo#123"
    type: "github_issue"
    repo: "owner/repo"
    number: 123
    url: "https://github.com/owner/repo/issues/123"
```

## Agent Maintenance

Agents should maintain the richer graph elsewhere and export only the curated public snapshot.

On each refresh:

1. Read live PR or issue metadata.
2. Read human comments and reviews before interpreting status.
3. Update `status`, `status_note`, and campaign `status_summary`.
4. Keep `summary` stable unless the item scope changed.
5. Add only public aliases to public `stable_ids`.
6. Use `redacted_dependencies` rather than private dependency IDs.
7. Update `last_checked`, `last_checked_display`, and `last_verified`.

Validation and refresh mechanisms should live outside this package. Let GitHub Pages build the static site.
