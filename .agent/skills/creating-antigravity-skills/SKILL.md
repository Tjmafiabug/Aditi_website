---
name: creating-antigravity-skills
description: Generates well-structured Antigravity agent skills. Use when the user asks to create a skill, build an agent skill, add a new skill to the .agent directory, or scaffold a skills folder for Antigravity.
---

# Creating Antigravity Skills

## When to Use This Skill
- User asks to "create a skill", "add a skill", or "scaffold a skill"
- User wants to extend agent capabilities with a new `.agent/skills/` entry
- User describes a repeatable workflow they want the agent to always follow

## Checklist
- [ ] Clarify the skill's purpose and triggers if not specified
- [ ] Determine optional directories needed (`scripts/`, `examples/`, `resources/`)
- [ ] Write `SKILL.md` with valid YAML frontmatter
- [ ] Write any supporting scripts or templates
- [ ] Verify output against the validation rules below

---

## Folder Structure

Every skill lives under `.agent/skills/<skill-name>/`:

```
<skill-name>/
├── SKILL.md          # Required — main logic and instructions
├── scripts/          # Optional — helper scripts (bash, python, etc.)
├── examples/         # Optional — reference implementations
└── resources/        # Optional — templates or static assets
```

Place the skill directory at:
```
<workspace-root>/.agent/skills/<skill-name>/
```

---

## YAML Frontmatter Rules

```yaml
---
name: <gerund-form-name>
description: <third-person description with explicit triggers>
---
```

| Field | Rules |
|---|---|
| `name` | Gerund form (`creating-x`, `managing-y`). Lowercase, hyphens, numbers only. Max 64 chars. No "claude" or "anthropic". |
| `description` | Third person. State what the skill does AND list trigger keywords. Max 1024 chars. |

**Good example:**
```yaml
name: generating-pdf-reports
description: Generates formatted PDF reports from structured data. Use when the user mentions PDF generation, report exports, or document creation.
```

---

## Writing the SKILL.md Body

### Formatting by Freedom Level

| Task Type | Format |
|---|---|
| High freedom (heuristics, judgment calls) | Bullet points |
| Medium freedom (reusable patterns) | Code block templates |
| Low freedom (fragile, exact commands) | Specific bash commands |

### Structural Sections (use as needed)

```markdown
## When to Use This Skill
- [Trigger 1]
- [Trigger 2]

## Checklist
- [ ] Step 1
- [ ] Step 2

## Workflow
[Plan → Validate → Execute pattern for complex tasks]

## Instructions
[Core logic, snippets, or rules]

## Resources
- [scripts/helper.sh](scripts/helper.sh)
```

### Key Principles
- Keep `SKILL.md` under **500 lines**
- If more detail is needed, link to a secondary file (`[See ADVANCED.md](ADVANCED.md)`) — max one level deep
- Always use `/` for paths, never `\`
- Do **not** over-explain basic concepts — assume a capable agent
- For scripts, tell the agent to run `--help` if usage is unclear

---

## Validation Rules

Before finalizing, verify:
- [ ] `name` is in gerund form and passes the character rules
- [ ] `description` is third-person and mentions specific triggers
- [ ] All paths use forward slashes
- [ ] `SKILL.md` is under 500 lines
- [ ] Any referenced `scripts/` files actually exist in the output
- [ ] No placeholder content left in the file

---

## Output Template

When generating a skill, produce output in this structure:

```
.agent/skills/<skill-name>/
├── SKILL.md
├── scripts/          (if needed)
│   └── <helper>.sh
└── resources/        (if needed)
    └── <template>
```

Then write each file's content. See [examples/sample-skill/](examples/sample-skill/) for a complete worked reference.
