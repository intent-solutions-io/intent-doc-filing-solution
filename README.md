# intent-doc-filing-solution

**Document Filing System v4.4** — universal, deterministic naming + filing standard for project docs.

Published as a [Claude Code](https://code.claude.com) marketplace plugin and as a standalone
Intent Solutions repository. The plugin installs the `doc-filing` skill into any Claude Code
session; the repo itself applies the standard to its own `000-docs/` directory as a working
example (dogfooding).

- **Source of truth**: this repo at `intent-solutions-io/intent-doc-filing-solution`.
- **Skill location**: `plugins/doc-filing/skills/doc-filing/SKILL.md`.
- **Standard reference**: `plugins/doc-filing/skills/doc-filing/references/000-DR-STND-document-filing-system.md`.
- **License**: MIT. Author: Jeremy Longshore `<jeremy@intentsolutions.io>`.

## Install

```bash
# Inside Claude Code:
/plugin marketplace add intent-solutions-io/intent-doc-filing-solution
/plugin install doc-filing
```

Or copy `plugins/doc-filing/skills/doc-filing/SKILL.md` directly into
`~/.claude/skills/doc-filing/SKILL.md`.

## What it does

The `doc-filing` skill teaches an LLM agent to organize loose project documents into a
**flat-by-default** `000-docs/` directory using:

1. **Chronological sequencing** — `NNN` numeric prefixes (001–999), one global sequence
   across the whole tree.
2. **Category codes** — `CC` two-letter codes that classify doc type (e.g. `OD` objective
   document, `ST` standard, `RR` research record, `AT` architecture, `DECR` decision record).
3. **Cluster names** — short kebab-case topic slugs (`pr-prescreen-system`).
4. **Disciplined numbered nesting at scale** — flat by default; one level of
   `NNN-CC-cluster-name/` subfolders allowed once the root exceeds ~50 files AND a cluster
   has ~8+ docs. Canonical `000-*` standards always stay at the flat root.

See `plugins/doc-filing/skills/doc-filing/SKILL.md` for the complete v4.4 spec.

## Repo structure

```
.
├── .claude-plugin/
│   └── marketplace.json         Claude Code marketplace manifest
├── plugins/
│   └── doc-filing/
│       ├── .claude-plugin/
│       │   └── plugin.json      Plugin manifest
│       └── skills/
│           └── doc-filing/
│               ├── SKILL.md     The skill (copy of ~/.claude/skills/doc-filing/SKILL.md)
│               └── references/
│                   └── 000-DR-STND-document-filing-system.md
├── 000-docs/                    Working example (this repo applying v4.4 to itself)
├── .github/
│   └── workflows/
│       └── ci.yml               Validate the marketplace + lint skill frontmatter
├── CLAUDE.md                    Repo agent guidance
├── LICENSE                      MIT
└── README.md
```

## Use as a reference standard

If you want to **adopt** the standard in your own repo:

1. Copy `plugins/doc-filing/skills/doc-filing/SKILL.md` into your project's
   `.claude/skills/doc-filing/SKILL.md` (or expose via your Claude Code marketplace).
2. Trigger `/doc-filing` in a Claude Code session to have an agent organize your loose docs
   into a flat `000-docs/` tree.

## Compatibility

- **Skill version**: 4.4.0 (v3-, v4.0-, v4.2-, v4.3-compatible — non-breaking upgrades).
- **Claude Code**: any agent supporting AgentSkills.io spec (`name`, `description`,
  `allowed-tools`, `version`, `author`, `license`, `compatibility`, `tags`).
- **Model**: tested on Sonnet; should work on Haiku and Opus with no changes.

## Contributing

See `CONTRIBUTING.md`. PRs welcome for: typo fixes in the standard doc, additional
worked examples, and adoption notes from teams using the standard at scale.

## License

MIT — see `LICENSE`.
