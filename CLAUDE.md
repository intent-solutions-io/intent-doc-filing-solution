# CLAUDE.md

Agent guidance for the **intent-doc-filing-solution** repo.

## What this repo is

The public, canonical home of the **Document Filing System v4.4** standard. It ships the
`doc-filing` Claude Code skill and a working example of the standard applied to a real repo
(this one).

## Repo layout (apply doc-filing v4.4 to itself)

- `000-docs/` — flat-by-default chronological + categorized docs. Anything durable lives here.
- `plugins/doc-filing/` — the plugin source (skill, manifest, references).
- `.claude-plugin/` — top-level Claude Code marketplace manifest.
- `.github/workflows/` — CI gates.

## When working on this repo

- **Do not** edit files under `plugins/doc-filing/skills/doc-filing/` without reading the
  source-of-truth note below — those files are vendored from `~/.claude/skills/doc-filing/`
  in dev and the canonical version lives in the author machine until the next standard bump.
- **Do** add new entry points under `plugins/doc-filing/` (additional skills, commands,
  agents) if they support the standard — keep them declarative and small.
- **Do** add worked-example docs to `000-docs/` (e.g. `NNN-OD-...md`) when a decision or
  adoption pattern deserves to be captured.
- **Do not** commit secrets; this is a public repo.
- **License**: MIT. Keep `LICENSE` intact; do not add proprietary submodules.

## Skill vendoring

The `plugins/doc-filing/skills/doc-filing/SKILL.md` is the canonical home for the published
skill. When the standard bumps:

1. Edit `SKILL.md` in this repo (this is what the world sees).
2. Mirror to `~/.claude/skills/doc-filing/SKILL.md` locally if you want it live in your dev box.

## CI gates (`.github/workflows/ci.yml`)

- `validate-marketplace` — JSON-schema valid `.claude-plugin/marketplace.json` + plugin manifests.
- `validate-skill-md` — frontmatter lint (8 required fields: `name`, `description`,
  `allowed-tools`, `version`, `author`, `license`, `compatibility`, `tags`).
- `lint-md` — markdownlint against `000-docs/` and `README.md` / `CLAUDE.md`.
- `gitleaks` — secret scan (always on).

## Linked systems (off-repo)

- Authoritative skill source: `~/.claude/skills/doc-filing/SKILL.md` (private machine-local).
- Companion marketplace: `jeremylongshore/claude-code-plugins-plus-skills` (multi-skill catalog).
- Organization: `intent-solutions-io` (GitHub).
- Standard codename: **doc-filing v4.4**.

## When this repo is wrong

If a downstream user finds the standard ambiguous, open an issue with the failing pattern as
a worked example. Do not "fix" the skill until the issue has an adopted solution — doc-filing
is a standard, not a feature, and breaking changes need a v4.5 release and migration note.
