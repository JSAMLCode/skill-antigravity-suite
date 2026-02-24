---
name: skill-creator
description: A skill designed to guide the agent in properly creating, formatting, and documenting new Antigravity skills.
---

# Skill Creator

You are tasked with creating a new Antigravity skill based on a user's request. To do this correctly, you **must** follow the official structure and formatting guidelines for skills.

## 1. Skill Folder Structure
Skills are stored as folders inside the workspace's `.agents/skills/` (or `.agent/skills/`) directory. 
When creating a new skill, create a folder named exactly as the skill (kebab-case preferred, e.g., `my-new-skill`).

Inside this folder, you **must** create a `SKILL.md` file.

Optional subdirectories you can create if the skill's complexity requires them:
- `scripts/`: Helper scripts or utilities that extend capabilities.
- `examples/`: Reference implementations and usage patterns.
- `resources/`: Additional files, templates, or assets the skill may reference.

## 2. SKILL.md Format
The `SKILL.md` file is the core instruction file for the skill. It **must** start with a YAML frontmatter block containing the `name` and `description` of the skill, followed by detailed markdown instructions.

### Example Template:
```markdown
---
name: [skill-name]
description: [A short, clear description of what the skill does and when to use it]
---

# [Skill Title]

## Overview
Provide a high-level summary of what this skill helps the agent achieve.

## Instructions
List detailed, step-by-step markdown instructions for the agent to follow when this skill is invoked.
- Be highly specific.
- Provide examples of how to format tool calls or expected outputs.
- Outline constraints or rules the agent must not break.

## Resources (Optional)
If the skill uses files in `scripts/`, `examples/`, or `resources/`, explain how and when to read or reference them.
```

## 3. Creating the Skill
1. Analyze the user's request for the new skill.
2. Determine the optimal instructions and constraints the new skill will need.
3. Use the `write_to_file` tool to create `<workspace-root>/.agents/skills/<skill-name>/SKILL.md` and populate it rigorously using the format above.
4. If applicable, create any necessary supporting files in the `scripts/`, `examples/`, or `resources/` directories using `write_to_file`.

## 4. Best Practices for Skill Instructions
- **Command specific tools**: If the skill requires specific MCP servers or tools, explicitly tell the agent *which* tools to use and *how*.
- **Set constraints**: Clearly define what the agent should *not* do.
- **Agent persona**: Remember that the reader of the `SKILL.md` is an AI agent (like you). Write the instructions addressing the agent directly, using clear imperatives (e.g., "You MUST do X", "Before proceeding, check Y").
