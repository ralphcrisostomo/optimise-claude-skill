---
name: optimise-claude
description: Use when auditing, trimming, or restructuring AI skill files to reduce context-window consumption.
---

# Optimise Claude

Use this skill when auditing AI skill files for size, structure, duplication, or bloated AGENTS.md inline blocks.

## When to Use

- Skill files exceed ~120 lines.
- Frontmatter or section order is non-canonical.
- Multiple skills contain duplicated content.
- An AGENTS.md file has inline instruction blocks >30 lines that should be skills.

## Workflow

Run phases sequentially. Skip any phase that does not apply.

### Phase 1 — Inventory & Triage

1. List every `SKILL.md` under `.agents/skills/`.
2. For each file, record: name, line count, has frontmatter (y/n), has canonical sections (y/n).
3. Flag violations: >120 lines, missing/incorrect frontmatter, wrong section order.
4. Output a triage table sorted by line count descending.

```
| Skill | Lines | FM | Sections | Violations |
|-------|------:|----|----------|------------|
```

### Phase 2 — Reduce Token Usage

For each flagged skill:

- Trim prose to imperative bullets.
- Collapse verbose examples to minimal code fences.
- Remove redundant explanations already covered by parent AGENTS.md.
- Remove blank lines between list items.
- Target <=120 lines. If still over, split into focused sub-skills.

### Phase 3 — Fix Structure & Frontmatter

For each skill:

- Ensure YAML frontmatter has `name` matching directory and `description` starting with "Use when".
- Enforce canonical section order: H1 title, scope line, When to Use, Rules/Instructions, Quick Reference, Validation.
- Remove empty or placeholder sections.
- Use imperative voice throughout.

### Phase 4 — Cross-Skill Deduplication

1. Identify repeated content blocks across skills (>5 similar lines).
2. Move shared content to the nearest AGENTS.md or a dedicated shared skill.
3. Replace duplicates with a one-line pointer: "See `<skill-name>` for …".
4. Reword overlapping `description` fields so each skill has a unique trigger.

### Phase 5 — Extract Bloated AGENTS.md Blocks

1. Scan every `AGENTS.md` for inline instruction blocks >30 lines.
2. For each block, create a new skill in `.agents/skills/<name>/SKILL.md`.
3. Replace the original block with a slim pointer + Quick Reference (see `puretec-app-skill` for pattern).
4. Run `bun skills` to sync new skills to `.claude/skills/`.

## Output Format

After all phases, produce a summary report:

```
## Optimisation Report

| Skill | Before | After | Delta |
|-------|-------:|------:|------:|
| ...   |    250 |   110 |  -140 |

Total skills: N
Total lines saved: N
New skills created: N
```

## Quick Reference

- Source of truth: `.agents/skills/` — never edit `.claude/skills/` directly.
- Canonical section order: H1, scope, When to Use, Rules, Quick Reference, Validation.
- Target: <=120 lines per skill.
- Sync after changes: `bun skills` -> "Sync skills (.agents -> .claude)".

## Validation

- `wc -l .agents/skills/*/SKILL.md` — no file exceeds 120 lines.
- Every `SKILL.md` has valid YAML frontmatter with `name` and `description`.
- No two skills share >5 identical lines.
- No `AGENTS.md` has inline instruction blocks >30 lines without a skill pointer.
