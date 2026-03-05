# *audit

Audit every context file in this repo for bloat, redundancy, and stale instructions.
Execute the full audit skill defined in `skills/claude-context-doctor/SKILL.md`.

## Behavior

1. Discover all context files (CLAUDE.md, AGENTS.md, .claude/rules/*.md, and any file described as instructions or rules for Claude).
2. 2. Audit every instruction in every file using the classification tags in the skill.
   3. 3. Produce the audit report (findings table per file, token budget summary).
      4. 4. Output lean replacements for each file — only [KEEP] items, with [CONDITIONAL] items moved to suggested `.claude/rules/` files.
         5. 5. End with a plain-language summary of what was removed, relocated, and the net context reduction.
           
            6. ## Important
           
            7. - Do not preserve an instruction just because it exists.
               - - Do not modify any files until the user reviews and approves the audit output.
                 - - If no context files are found, report that and stop.
