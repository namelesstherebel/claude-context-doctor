# claude-context-doctor

Audits your `CLAUDE.md`, `AGENTS.md`, and `.claude/rules/` files for bloat, redundancy, and stale instructions. Produces a tagged audit report and lean replacements.

## Quick Install

**1. Install the plugin**

```
claude plugin marketplace add namelesstherebel/claude-context-doctor
claude plugin install claude-context-doctor
```

**2. Confirm it is installed**

```
claude plugin list
```

You should see `claude-context-doctor` with status `installed`. If it is not listed, re-run the install command above.

**3. Run the audit**

Open Claude Code in the repo you want to audit and type:

```
*audit
```

If `*audit` doesn't fire immediately, use this fallback prompt:

```
Run the claude-context-doctor audit. Read the plugin's commands/audit.md and execute the full 5-step audit process for this repository.
```

## What It Does

**Context audit** — a 5-step process that discovers every context file in your repo, classifies each instruction with a tag (`[NATIVE]`, `[LINTER]`, `[STALE]`, `[REDUNDANT]`, `[OVER-SPECIFIED]`, `[CONDITIONAL]`, `[CORRECTION]`, etc.), produces a findings table with a token budget summary, and outputs lean replacements ready to copy in.

**Progressive disclosure** — conditional instructions get moved to `.claude/rules/` files with glob patterns so they only load when relevant, costing zero tokens the rest of the time.

**No auto-apply** — the audit output is a report and proposal only. Nothing in your repo changes until you review and apply the replacements yourself.

## Commands

| Command | What it does |
|---------|-------------|
| `*audit` | Discover all context files, classify every instruction, produce audit report and lean replacements |

## Audit Tags

### REMOVE — High confidence bloat

| Tag | Meaning |
|-----|---------|
| `[NATIVE]` | Claude already does this correctly without being told |
| `[LINTER]` | Formatting or style rule — belongs in a linter, not a context file |
| `[STALE]` | References files, patterns, or versions no longer in this repo |
| `[REDUNDANT]` | Duplicates another instruction in the same or different file |
| `[OVER-SPECIFIED]` | Explains why at length — a one-liner would do the same job |

### RELOCATE — Keep but move

| Tag | Meaning |
|-----|---------|
| `[CONDITIONAL]` | Only relevant to a specific file type or workflow — move to `.claude/rules/` |
| `[REFERENCE]` | Points Claude to where to find info — verify the path still exists |

### KEEP — Genuinely useful

| Tag | Meaning |
|-----|---------|
| `[UNIVERSAL]` | Applies to nearly every task in this repo |
| `[CORRECTION]` | Fixes a pattern Claude gets wrong in this specific codebase |
| `[CRITICAL-PATH]` | Build commands, deploy steps, env setup Claude can't infer |

## Example Output

```
## Audit Report

### Files Scanned
- CLAUDE.md — 84 lines, ~620 tokens

### Total Context Budget
- Current: ~620 tokens loaded per session
- After audit: ~180 tokens (71% reduction)

### Findings by File

#### CLAUDE.md
| Instruction | Tag | Recommendation |
|-------------|-----|----------------|
| "always write clean readable code" | [NATIVE] | Remove |
| "follow PSR-12 formatting" | [LINTER] | Remove — add phpcs to workflow |
| "use wp_remote_get not file_get_contents" | [CORRECTION] | Keep |
| "for database work, see db-rules.md" | [CONDITIONAL] | Move to .claude/rules/database.md |
```

## Setup Guide

### Install the plugin

```
claude plugin marketplace add namelesstherebel/claude-context-doctor
claude plugin install claude-context-doctor
```

To reinstall after an update:

```
claude plugin marketplace remove claude-context-doctor
claude plugin marketplace add namelesstherebel/claude-context-doctor
claude plugin install claude-context-doctor
```

### Verify installation

Run `claude plugin list` in your terminal and confirm `claude-context-doctor` appears with status `installed`.

### Run the audit

Open Claude Code in the repo you want to audit and type `*audit`. The plugin will:

1. Discover all context files
2. 2. Classify every instruction with a tag
   3. 3. Produce a findings table with a before/after token budget
      4. 4. Output lean replacement files ready to copy in
        
         5. Nothing changes in your repo until you copy in the replacement files yourself.
        
         6. ## Repo Structure
        
         7. ```
            claude-context-doctor/
            ├── .claude-plugin/
            │   ├── plugin.json          ← manifest
            │   └── marketplace.json     ← marketplace listing
            ├── commands/
            │   └── audit.md             ← *audit command entrypoint
            ├── skills/
            │   └── claude-context-doctor/
            │       └── SKILL.md         ← full 5-step audit workflow
            ├── CLAUDE.md                ← registers *audit as a star command
            └── README.md
            ```

            ## License

            MIT — Copyright (c) 2026 Stefan Kuczynski
