# Changelog

All notable changes to this repo are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

The standard's own changelog lives in
`plugins/doc-filing/skills/doc-filing/SKILL.md` — see the `**Changelog:**`
block in that file. This file tracks the **repo package** (manifests, CI,
governance files, worked examples) — not the standard itself.

## [Unreleased]

### Added

- Initial public release of the Document Filing System v4.4 as a Claude Code
  marketplace plugin + standalone IS repo.
- Vendored `doc-filing` skill (`plugins/doc-filing/skills/doc-filing/SKILL.md`)
  and reference doc (`references/000-DR-STND-document-filing-system.md`).
- Top-level `.claude-plugin/marketplace.json` so the package installs via
  `claude plugin marketplace add intent-solutions-io/intent-doc-filing-solution`.
- CI: marketplace JSON validate + skill frontmatter lint + markdownlint + gitleaks.

### Notes

- The repo applies doc-filing v4.4 to itself: `000-docs/000-DR-STND-intent-doc-filing-system.md`
  is the canonical "this standard in this repo" worked example.
