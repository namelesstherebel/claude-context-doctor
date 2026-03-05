# Skill: Claude Context Doctor

Audit all context files in a repo and produce lean replacements.

---

## STEP 1 — Discover All Context Files

Find and read every context file:
- `CLAUDE.md` (root and any subdirectories)
- - `AGENTS.md`
  - - `.claude/rules/*.md`
    - - Any file named or described as instructions, rules, findings, or guidelines for Claude
     
      - List every file found with its line count and estimated token count before proceeding.
     
      - ---

      ## STEP 2 — Audit Each File

      For every instruction or rule in every file, classify it using the tags below. Be ruthless.

      ### REMOVE — High confidence bloat

      | Tag | Meaning |
      |-----|---------|
      | [NATIVE] | Claude already does this correctly without being told |
      | [LINTER] | Formatting or style rule — use a linter/formatter instead |
      | [STALE] | References files, patterns, libraries, or versions no longer in this repo |
      | [REDUNDANT] | Duplicates another instruction in the same or different file |
      | [OVER-SPECIFIED] | Explains why at length — a one-line directive would do the same job |

      ### RELOCATE — Keep but move

      | Tag | Meaning |
      |-----|---------|
      | [CONDITIONAL] | Only relevant to a specific file type, task, or workflow — should be in a scoped `.claude/rules/` file |
      | [REFERENCE] | Points Claude to where to find info — verify the path still exists |

      ### KEEP — Genuinely useful

      | Tag | Meaning |
      |-----|---------|
      | [UNIVERSAL] | Applies to nearly every task in this repo |
      | [CORRECTION] | Fixes a pattern Claude gets wrong in this specific codebase |
      | [CRITICAL-PATH] | Build commands, deploy steps, env setup Claude can't infer |

      ---

      ## STEP 3 — Produce the Audit Report

      Output in this exact format:

      ```
      ## Audit Report

      ### Files Scanned
      - [filename] — [line count] lines, ~[token estimate] tokens

      ### Total Context Budget
      - Current: ~[X] tokens loaded per session
      - After audit: ~[Y] tokens ([Z]% reduction)

      ### Findings by File

      #### [filename]
      | Instruction | Tag | Recommendation |
      |-------------|-----|----------------|
      | "exact instruction text" | [TAG] | Keep / Remove / Move to .claude/rules/X.md |
      ```

      ---

      ## STEP 4 — Produce Lean Replacements

      After the report, output a revised version of each file containing only [KEEP] items, with [CONDITIONAL] items moved to suggested `.claude/rules/` files.

      Format each output as a fenced code block labeled with its intended path:

      ```
      <!-- REVISED: CLAUDE.md -->
      [lean content here]

      <!-- NEW: .claude/rules/[context].md -->
      ---
      globs: ["**/*.ext"]
      ---
      [scoped rules here]
      ```

      ---

      ## STEP 5 — Final Summary

      End with a one-paragraph plain-language summary covering:
      - What was removed and why
      - - What was relocated
        - - The net context reduction
          - - Any instructions that should be converted to hooks, linters, or slash commands instead
           
            - ---

            ## Audit Principles

            - Do not preserve an instruction just because it exists. Err toward removal.
            - - If Claude already follows a rule in this codebase without being told, it's noise.
              - - Length is not quality. A 5-line CLAUDE.md that's all [CORRECTION] beats a 200-line one with 180 [NATIVE] entries.
                - - Style rules belong in tooling, not context files.
                  - - Progressive disclosure over front-loading. If an instruction only matters sometimes, it should only load sometimes.
