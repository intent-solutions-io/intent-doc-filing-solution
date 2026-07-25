---
name: doc-filing
description: |
  Document Filing System v4.4 - Universal document organization with chronological sequencing, category codes, and a flat-by-default 000-docs structure that allows disciplined numbered nesting at scale. Use when organizing loose project documents, cleaning up scattered files, or converting to standardized naming. Trigger with phrases like "organize docs", "file documents", "doc filing", "/doc-filing".
allowed-tools: "Read,Write,Edit,Glob,Grep,Bash(ls:*),Bash(mkdir:*),Bash(mv:*),Bash(find:*),Bash(cp:*)"
model: sonnet
version: 4.4.0
author: Jeremy Longshore <jeremy@intentsolutions.io>
license: MIT
---

# Document Filing System v4.4 (LLM/AI-ASSISTANT FRIENDLY)

**Purpose:** Universal, deterministic naming + filing standard for project docs with canonical cross-repo "000-*" standards series
**Status:** Production Standard (v3-compatible, v4.0-compatible, v4.2-compatible, v4.3-compatible)
**Changelog:**
- **v4.4** adds **disciplined numbered nesting at scale** — `000-docs/` stays flat by default, but may nest **one level** into coded `NNN-CC-cluster-name/` folders once it exceeds ~50 files and a cluster has ~8+ docs; `NNN` is one **global** chronological sequence across the whole tree; canonical `000-*` always stays at the flat root. **Non-breaking** — flat dirs stay valid.
- **v4.3** migrates from 6767 prefix to 000-* prefix for canonical standards.

## Overview

This skill automatically organizes loose project documents into a **flat-by-default** `000-docs/`
directory (allowing disciplined numbered nesting at scale — see "000-docs Layout" below) using:

1. **Chronological Sequencing** - NNN prefixes (001-999), one global sequence across the whole tree
2. **Category Codes** - 2-letter codes (PP, AT, DR, etc.)
3. **Document Types** - 4-letter codes (PROD, ARCH, STND, etc.)
4. **Kebab-case Descriptions** - 1-4 words lowercase

## Prerequisites

- **File System Access**: Read/write permissions to project directory
- **No Dependencies**: Uses only standard bash commands (ls, mkdir, mv, find, cp)
- **Supported Formats**: .md, .pdf, .doc, .docx, .txt, .xlsx, .xls, .csv, .ppt, .pptx

This skill works in any project directory. No installation or configuration required.

## ONE-SCREEN RULES (MEMORIZE THESE)

1. **Two filename families only:**
   - **Project docs:** `NNN-CC-ABCD-short-description.ext` (001-999)
   - **Canonical standards:** `000-CC-ABCD-short-description.ext`
2. **NNN is chronological** (001-999), **one global sequence repo-wide** (flat root + every subfolder). **000-* is reserved for canonical cross-repo standards.**
3. **All codes are mandatory:** `CC` (category) + `ABCD` (type).
4. **Description is short:** 1-4 words (project), 1-5 words (000-*), **kebab-case**, lowercase.
5. **Subdocs:** either `005a` letter suffix or `006-1` numeric suffix.
6. **000-* = canonical cross-repo standards.** Keep one authoritative copy (this `/doc-filing` skill); other repos mirror/link it — no byte-for-byte sync required.
7. **Flat by default; disciplined numbered nesting at scale.** Nest **one level** into `NNN-CC-cluster-name/` only when `000-docs/` exceeds ~50 files **and** a cluster has ~8+ related docs. Canonical `000-*` always stays at the flat root.
8. **Next number = recursive scan of the whole tree** (`find 000-docs -type f`), never `ls` of one dir — the `NNN` sequence is global, never restarted per folder.

## Instructions

### Phase 1: Setup & Scan

**Step 1: Display Current Location**
```bash
echo "=== DOC-FILING: DOCUMENT ORGANIZATION ==="
echo ""
echo "Current directory: $(pwd)"
echo "Project: $(basename $(pwd))"
echo ""
```

**Step 2: Ensure the 000-docs Directory (flat by default)**
```bash
mkdir -p 000-docs
echo "Ready: 000-docs/ (flat by default; disciplined numbered nesting at scale — see '000-docs Layout' below)"
```

**Step 2b: Scan the EXISTING 000-docs tree recursively (it may contain coded subfolders)**

At scale, `000-docs/` may hold one level of `NNN-CC-cluster-name/` folders. Always walk the **whole
tree**, not just the top level — both to read current state and to compute the next global `NNN`.
```bash
# Every filed doc, across the flat root AND every subfolder, in true chronological order:
find 000-docs -type f -name '*.md' 2>/dev/null | sort
# Highest existing global NNN (shared across root + all subfolders):
find 000-docs -type f -printf '%f\n' 2>/dev/null | grep -oE '^[0-9]{3}' | sort -n | tail -1
```

**Step 3: Scan for Loose Documents**

Find all document files in project root and first-level directories.
Exclude: node_modules, .git, dist, build, vendor, 000-docs itself.

```bash
find . -maxdepth 2 -type f \
  \( -iname "*.md" -o -iname "*.pdf" -o -iname "*.doc" -o -iname "*.docx" \
  -o -iname "*.txt" -o -iname "*.xlsx" -o -iname "*.xls" -o -iname "*.csv" \
  -o -iname "*.ppt" -o -iname "*.pptx" \) \
  ! -path "*/node_modules/*" \
  ! -path "*/.git/*" \
  ! -path "*/dist/*" \
  ! -path "*/build/*" \
  ! -path "*/vendor/*" \
  ! -path "*/000-docs/*" \
  ! -path "*/.next/*" \
  ! -path "*/coverage/*" \
  ! -name "README.md" \
  ! -name "CLAUDE.md" \
  ! -name "LICENSE.md" \
  ! -name "CONTRIBUTING.md" \
  ! -name "CHANGELOG.md" \
  -print | sort
```

### Phase 2: Interactive Categorization

For each document found:

1. **Display** the filename and ask for categorization
2. **Suggest** likely category and document type based on filename
3. **Wait** for confirmation or correction
4. **Rename** according to NNN-CC-ABCD-description.ext format
5. **Move** to 000-docs/

### Phase 3: Renaming & Organization

**Renaming Algorithm:**

1. **Get next sequence number** - Highest existing NNN across the **whole** `000-docs/` tree
   (recursive — the sequence is global, shared by the flat root and every subfolder), then +1.
2. **Determine category code** - Based on content analysis or user input (CC)
3. **Determine document type** - Based on content analysis or user input (ABCD)
4. **Extract description** - Clean filename to 1-4 word kebab-case description
5. **Preserve extension** - Keep original file extension
6. **Generate new name** - Combine as `NNN-CC-ABCD-description.ext`
7. **Choose placement** - flat root by default; a coded `NNN-CC-cluster-name/` folder **only at
   scale** (see "000-docs Layout" below). A `000-*` canonical standard **always** goes to the flat
   root.

```bash
# Get next sequence number — RECURSIVE scan of the whole tree (global NNN), NOT `ls` of one dir.
# (Replaces the v4.3 `ls 000-docs/ | grep` one-liner, which only saw the top level and would
#  collide once subfolders exist.)
NEXT_NUM=$(printf "%03d" $(($(find 000-docs -type f -printf '%f\n' 2>/dev/null \
  | grep -oE '^[0-9]{3}' | sort -n | tail -1) + 1)))

# Generate new name
NEW_NAME="${NEXT_NUM}-${CATEGORY}-${DOC_TYPE}-${DESCRIPTION}.${EXTENSION}"

# Choose destination:
#   - default (flat):       DEST="000-docs"
#   - at scale (nested):    DEST="000-docs/<NNN-CC-cluster-name>"   # e.g. 000-docs/040-UC-kobiton
#   - canonical 000-* file: DEST="000-docs"                        # ALWAYS the flat root
DEST="000-docs"   # flat by default; point at a coded NNN-CC-cluster-name/ folder only at scale

# Move and rename
mv "$ORIGINAL_FILE" "${DEST}/$NEW_NAME"
```

### 000-docs Layout (flat by default, disciplined numbered nesting at scale)

`000-docs/` is **flat by default** — the filename is the index, so an agent can `ls` one directory
and see the whole history. **Nest only at scale:** when `000-docs/` exceeds **~50 files** *and* a
topical cluster has **~8+ related docs**, group that cluster into **one level** of a coded folder.

- **Folder grammar:** `NNN-CC-cluster-name/` — `NNN` from the global sequence (when the cluster
  opened), `CC` the cluster's dominant 2-letter category, `cluster-name` kebab-case 1–3 words.
  Files inside keep the full `NNN-CC-ABCD-desc.ext`.
- **`NNN` is one global chronological sequence** across the flat root and every subfolder — never
  per-folder, never reused. The folder is a topical *view*; the number is the *timeline*.
- **One level only.** No folders within folders. No uncoded folders (`docs/`, `misc/`, `archive/`).
- **Canonical `000-*` standards always live at the flat root**, never nested.
- **`000-INDEX.md` is mandatory** at the flat root and groups entries by folder.

Full rule + worked examples: `{baseDir}/references/000-DR-STND-document-filing-system.md` §3.1.

### Phase 4: Generate Inventory

Create/regenerate `000-docs/000-INDEX.md` **at the flat root** (mandatory — it is the nav layer
once folders exist) with:
- A recursive listing of the whole tree (walk subfolders), in global chronological order
- **Grouped by folder** when `NNN-CC-cluster-name/` subfolders exist (each group lists its files
  with their subpaths); a flat repo just lists files
- Quick reference for category/type codes

```bash
# Enumerate the whole tree for the index (root + every subfolder), chronological by global NNN:
find 000-docs -type f -name '*.md' ! -name '000-INDEX.md' 2>/dev/null | sort
```

### Phase 5: Summary Report

Display counts by category and total documents organized.

## Category Codes (CC) - 2 Letters

| Code | Category |
|------|----------|
| PP | Product & Planning |
| AT | Architecture & Technical |
| DC | Development & Code |
| TQ | Testing & Quality |
| OD | Operations & Deployment |
| LS | Logs & Status |
| RA | Reports & Analysis |
| MC | Meetings & Communication |
| PM | Project Management |
| DR | Documentation & Reference |
| UC | User & Customer |
| BL | Business & Legal |
| RL | Research & Learning |
| AA | After Action & Review |
| WA | Workflows & Automation |
| DD | Data & Datasets |
| MS | Miscellaneous |

## Document Types (ABCD) - 4 Letters

### PP - Product & Planning
PROD, PLAN, RMAP, BREQ, FREQ, SOWK, KPIS, OKRS

### AT - Architecture & Technical
ADEC, ARCH, DSGN, APIS, SDKS, INTG, DIAG

### DC - Development & Code
DEVN, CODE, LIBR, MODL, COMP, UTIL

### TQ - Testing & Quality
TEST, CASE, QAPL, BUGR, PERF, SECU, PENT

### OD - Operations & Deployment
OPNS, DEPL, INFR, CONF, ENVR, RELS, CHNG, INCD, POST

### LS - Logs & Status
LOGS, WORK, PROG, STAT, CHKP

### RA - Reports & Analysis
REPT, ANLY, AUDT, REVW, RCAS, DATA, METR, BNCH

### MC - Meetings & Communication
MEET, AGND, ACTN, SUMM, MEMO, PRES, WKSP

### PM - Project Management
TASK, BKLG, SPRT, RETR, STND, RISK, ISSU, STAT

### DR - Documentation & Reference
REFF, GUID, MANL, FAQS, GLOS, SOPS, TMPL, CHKL, STND, INDEX

### UC - User & Customer
USER, ONBD, TRNG, FDBK, SURV, INTV, PERS

### BL - Business & Legal
CNTR, NDAS, LICN, CMPL, POLI, TERM, PRIV

### RL - Research & Learning
RSRC, LERN, EXPR, PROP, WHIT, CSES

### AA - After Action & Review
AACR, LESN, PMRT

### WA - Workflows & Automation
WFLW, N8NS, AUTO, HOOK

### DD - Data & Datasets
DSET, CSVS, SQLS, EXPT

### MS - Miscellaneous
MISC, DRFT, ARCH, OLDV, WIPS, INDX

## 000-* Canonical Standards (NOT HANDLED BY THIS SKILL)

**What is 000-* Series?**
The 000-*-series represents **canonical, cross-repo reusable standards** (SOPs). These are global standards applied across multiple projects.

**000-* Filename Pattern (v4.2 Rule):**
```
000-*-{a|b|c|...}-[TOPIC-]CC-ABCD-short-description.ext
```

**Fields:**
- `000-*`: fixed canonical prefix (used ONCE)
- `{a|b|c|...}`: **mandatory letter suffix** for chronological ordering
- `[TOPIC-]`: optional uppercase grouping prefix (e.g., INLINE, LAZY, SLKDEV)
- `CC`: 2-letter category code
- `ABCD`: 4-letter document type
- `short-description`: 1-5 words, kebab-case

**Examples:**
- `000-*-a-DR-STND-document-filing-system-standard-v4.md`
- `000-*-b-DR-INDEX-standards-catalog.md`
- `000-*-c-INLINE-DR-STND-inline-source-deployment.md`
- `000-*-DR-STND-...` (WRONG - missing letter suffix)
- `000-*-000-DR-INDEX-...` (WRONG - numeric ID instead of letter)

## Pattern Matching Rules

| Pattern Keywords | Category | Type |
|------------------|----------|------|
| requirement, product, feature, spec | PP | PROD |
| plan, roadmap, strategy | PP | PLAN |
| architecture, design, technical | AT | ARCH |
| decision, adr, choice | AT | ADEC |
| api, endpoint, integration | AT | APIS |
| code, module, component | DC | CODE |
| test, testing, qa | TQ | TEST |
| bug, issue, defect | TQ | BUGR |
| security, audit, pentest | TQ | SECU |
| deploy, deployment, release | OD | DEPL |
| infrastructure, devops, config | OD | INFR |
| log, journal, daily | LS | LOGS |
| status, progress, update | LS | STAT |
| report, analysis, findings | RA | REPT |
| meeting, notes, minutes | MC | MEET |
| task, backlog, sprint | PM | TASK |
| implementation status, epic tracker, milestone | PM | STAT |
| guide, manual, handbook | DR | GUID |
| reference, docs, documentation | DR | REFF |
| sop, procedure, process | DR | SOPS |
| standard | DR | STND |
| template | DR | TMPL |
| research, study, experiment | RL | RSRC |
| proposal, pitch, whitepaper | RL | PROP |
| postmortem, lessons | AA | PMRT |
| after-action, aar | AA | AACR |
| workflow, automation | WA | WFLW |
| data, dataset, csv, sql | DD | DSET |
| No pattern matches | MS | MISC |

## Safety Features

- Never modifies files in 000-docs/
- Never touches project root files (README, CLAUDE.md, etc.)
- Preserves original file extensions
- Skips system directories (.git, node_modules)
- Generates audit trail (000-INDEX.md)
- Prompts for confirmation on categorization
- Safe to run multiple times

## Examples

**Before:**
```
./project-requirements-draft.md
./docs/api-integration-guide.pdf
./Meeting Notes - Sprint Planning.docx
```

**After (in 000-docs/):**
```
000-docs/001-PP-PROD-project-requirements-draft.md
000-docs/002-AT-APIS-api-integration-guide.pdf
000-docs/003-MC-MEET-sprint-planning.docx
```

## Output

Upon completion, this skill produces:

1. **000-docs/ directory** - Flat-by-default folder containing all organized documents (one level
   of coded `NNN-CC-cluster-name/` subfolders only at scale)
2. **000-INDEX.md** - Comprehensive inventory (mandatory, at the flat root) with:
   - A recursive, global-chronological listing of the whole tree
   - Entries grouped by folder when subfolders exist
   - Quick reference for category and type codes
3. **Summary report** - Console output showing:
   - Total documents organized
   - Count per category
   - Any skipped files

## Error Handling

| Situation | Behavior |
|-----------|----------|
| No loose documents found | Reports "0 documents found", creates empty 000-docs/ |
| File already exists in 000-docs/ | Skips file, reports in summary |
| Permission denied | Reports error, continues with remaining files |
| Invalid filename characters | Sanitizes to kebab-case automatically |
| Duplicate sequence number | Increments to next available NNN |
| User cancels categorization | Skips file, no partial moves |

The skill is idempotent - safe to run multiple times without duplicating files.

## Resources

- `{baseDir}/references/000-DR-STND-document-filing-system.md` - Full canonical standard (v4.4)
