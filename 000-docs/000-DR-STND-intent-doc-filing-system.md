# Intent Doc-Filing Solution — Repo's own standard

**Codename:** `000-DR-STND-intent-doc-filing-system`
**Status:** Active (dogfooding v4.4)
**Owner:** Jeremy Longshore
**Applies to:** this repository only

This document records **how this repo applies** the Document Filing System v4.4 to itself.
It is a worked example — not the canonical standard. The canonical standard lives at
[`plugins/doc-filing/skills/doc-filing/SKILL.md`](../../plugins/doc-filing/skills/doc-filing/SKILL.md)
and its [`references/000-DR-STND-document-filing-system.md`](../../plugins/doc-filing/skills/doc-filing/references/000-DR-STND-document-filing-system.md).

## Layout

- `000-docs/` is the flat-by-default root.
- Canonical `000-*` standards series stays at the flat root (this file is one of them).
- We do not currently nest any sub-folders (the tree is small).
- Category codes used in this repo:
  - `STND` — Standard (this repo's *own* standards + the vendored standard).
  - `RR` — Research Record (adoption notes, comparisons).
  - `OD` — Objective Document (charters, mission statements).

## Filename pattern

`NNN-CC-cluster-name.md`

| File | Pattern |
|---|---|
| `000-docs/000-DR-STND-intent-doc-filing-system.md` | canonical standard, flat root |
| `plugins/doc-filing/skills/doc-filing/SKILL.md` | vendored skill (not renamed) |
| `plugins/doc-filing/skills/doc-filing/references/000-DR-STND-document-filing-system.md` | vendored reference (not renamed) |

## Why this file exists

To prove the standard is **usable**, not just specified. A standard that its own spec
repo can't follow is a paper standard. This repo follows it.

## Bumping the standard

When v4.5 ships:

1. Bump `version` in `plugins/doc-filing/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`.
2. Bump `**Version:**` in `plugins/doc-filing/skills/doc-filing/SKILL.md`.
3. Add a new entry under the standard's `**Changelog:**` block.
4. If this repo needs to change layout, file a new `000-docs/NNN-OD-...md` decision record
   and link it from this file.

## Refs

- Canonical standard: `plugins/doc-filing/skills/doc-filing/SKILL.md`
- v4.4 reference: `plugins/doc-filing/skills/doc-filing/references/000-DR-STND-document-filing-system.md`
