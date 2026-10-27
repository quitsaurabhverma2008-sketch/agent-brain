---
name: learnings
description: "Continuous learning system. BEFORE any task, search the global knowledge base for relevant prior lessons; AFTER solving a non-trivial problem, append a concise entry. Use when starting a project, debugging, setting up tools/MCPs, deploying, or when the user says 'learn from this' / 'yaad rakho' / 'seekh lo'. Knowledge lives in ~/.agents/knowledge/LEARNINGS.md so it persists across sessions and projects."
license: MIT
---

# Learnings — Continuous Learning System

**Purpose:** Every non-trivial problem solved becomes reusable knowledge. Never repeat the same mistake twice.

## Storage

| What | Where |
|---|---|
| Global lessons (cross-project) | `~/.agents/knowledge/LEARNINGS.md` |
| Project state (not lessons) | `<project>/BRAIN.md` |
| This skill | `~/.agents/skills/learnings/SKILL.md` |

**Never store secrets, tokens, passwords, API keys, or personal data in the knowledge base.**

## BEFORE a task (READ — cheap, do not load the whole file)

1. Search the index by tag/topic:
   ```bash
   grep -in -E "mcp|termux|apk" ~/.agents/knowledge/LEARNINGS.md
   ```
2. Read the Quick Index row first; open only matching entries using line numbers:
   ```bash
   sed -n '120,140p' ~/.agents/knowledge/LEARNINGS.md
   ```
3. Apply the lesson. If current reality contradicts an entry, trust current reality and update the entry.

## AFTER a task (WRITE — only if reusable)

Add an entry when **any** is true:
- A bug took more than ~10 minutes or more than 2 attempts.
- A tool/config quirk was discovered (version-specific behavior, env var, flag).
- A command sequence worked and will be reused.
- A user preference or decision was established.
- A safer or faster approach replaced an older one.

Do **not** add: trivial one-liners, secrets, or project-specific state (that goes to `BRAIN.md`).

### Entry format (max ~12 lines)

```md
## L-XXX — <short title>
- Date: YYYY-MM-DD
- Tags: tag1, tag2
- Problem: ...
- Cause: ...
- Fix: ...
- Verify: ...
- Reuse: when to apply
```

Also add one row to the **Quick Index** table at the top of the file.

## Consolidation

- If the file exceeds ~400 lines, or a tag has 5+ entries: merge duplicates into a `## Playbook — <topic>` section and delete redundant entries.
- Keep IDs stable and the Quick Index in sync.

## Project-specific lessons

- Generalizable → global `LEARNINGS.md`.
- Only-this-project → append under `## Lessons` in that project's `BRAIN.md`.

## Session end

1. Update `BRAIN.md` (status + next steps).
2. Append at most 3 best lessons to `LEARNINGS.md`.
3. If a lesson invalidates an older one, mark the old entry `⛔ Superseded by L-XXX` (do not silently delete history).

## Quality bar

A good entry lets a future session fix the problem in under a minute. Include the exact command, the error text, and the verification step.
