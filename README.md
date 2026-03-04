# optimise-claude

AI skill that audits and optimises skill files to reduce context-window consumption.

## What It Does

- **Inventory & Triage** — lists all skills, flags line count / frontmatter / structure violations
- **Reduce Token Usage** — trims prose, collapses examples, targets <=120 lines per skill
- **Fix Structure** — enforces canonical section order, corrects frontmatter
- **Cross-Skill Dedup** — finds repeated content, consolidates, rewords overlapping triggers
- **Extract AGENTS.md Blocks** — converts inline blocks >30 lines into standalone skills

## Usage

### In a new project

Copy or clone into your project's `.agents/skills/` directory:

```bash
# Clone directly
git clone https://github.com/ralphcrisostomo/optimise-claude-skill .agents/skills/optimise-claude

# Or copy from a local checkout
cp -r /path/to/optimise-claude .agents/skills/optimise-claude
```

Then invoke the skill in Claude Code or any compatible AI agent to audit and optimise skill files.

### Wire into AGENTS.md

Add a pointer to your project's root `AGENTS.md`:

```markdown
## Skill Optimisation

Load the **`optimise-claude`** skill when auditing, trimming, or restructuring AI skill files to reduce context-window consumption.
```

### Sync to .claude/skills/

If your project uses a skills sync script:

```bash
bun skills
```

This copies `.agents/skills/optimise-claude/SKILL.md` to `.claude/skills/optimise-claude/SKILL.md`.
