# Contributing

Thanks for your interest in the Document Filing System v4.4 standard. This repo is a
**standards package** — changes to `plugins/doc-filing/skills/doc-filing/SKILL.md` are
**breaking changes** that ship under a new minor or major version of the standard (see
the `**Changelog:**` block at the top of that file).

## What we accept

- **Typo / wording fixes** in the standard doc or skill body → small PR, fast review.
- **Worked examples** in `000-docs/` → PRs welcome; tag with `example` label.
- **Adoption notes** from teams using the standard at scale → PR to `000-docs/NNN-RR-...md`.
- **Companion skills / commands / agents** under `plugins/doc-filing/` that operate **on**
  the standard (not change it) → PRs welcome.

## What requires a new standard version

- Changes to the canonical category codes (`CC`) list.
- Changes to the chronological sequencing rules (e.g. multi-root sequences).
- Changes to nesting rules (when a flat dir may become nested).
- Changes to the canonical `000-*` standards series at the flat root.

These ship under a new `v4.x` standard bump + migration note. Open an issue first.

## What we don't accept

- Substantive edits to the skill without an open issue + adopted solution.
- Commits that mix standard changes with repo-governance changes — split them.
- Secrets, proprietary content, or anything that would block MIT redistribution.

## Workflow

1. Branch from `origin/main`.
2. Make your change.
3. Run CI locally (see `.github/workflows/ci.yml` for the gates).
4. Open a PR. The PR title should follow Conventional Commits (`docs:`, `feat:`, `fix:`
   for the standard itself; `chore(repo):` for repo-governance changes).
5. Wait for CI + review. Merge gate: required contexts green (see `ci.yml`).

## Reporting issues

Open a GitHub issue with:

- The failing pattern as a worked example (real filenames if possible).
- The expected vs actual file layout.
- Whether you think it's a standard bug, a worked-example bug, or unclear documentation.

## License

By contributing, you agree your contributions are licensed under the MIT License (see `LICENSE`).
