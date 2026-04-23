---
name: refining-skills
description: Standardizes and improves existing project skills, identifying inconsistencies in structure, wording, and quality criteria. Use it when a new skill is created or when a set of SKILL.md files needs normalization.
---

# Refine and standardize skills

## Role

Act as an Agent Skills quality editor.
Your goal is to turn a set of `SKILL.md` files into a coherent, maintainable, and discoverable system.

## Task

Audit and normalize repository skills so they share:
- common structure
- consistent terminology
- comparable writing rules
- uniform verification checklists

Critical rule:
- Do not generate a report.
- Apply only minimal required changes.

## Context

- Project skills at `{Agents_Folder}/skills/`
- Global rules at `{Root_Folder}/AGENTS.md`
- Optional external reference skills (if provided by the user)

## Steps

### 1) Quick inventory

For each target skill:
- locate frontmatter `name` and `description`
- identify main body sections
- detect linked templates or auxiliary files

Outcome: mental table `skill -> structure -> gaps`.

### 2) Homogeneity audit

Evaluate each `SKILL.md` with these criteria:

- **Frontmatter**
  - kebab-case `name`, aligned with folder name
  - third-person `description`
  - `description` includes WHAT it does + WHEN to use it
- **Structure**
  - base order: `Role`, `Task`, `Context` (if applicable), `Steps`, `Verification`
  - section titles in one language per file (avoid unnecessary mixing)
- **Writing**
  - clear imperative verbs
  - avoid ambiguity and repetition
  - concise style, no basic theory
- **Operational consistency**
  - consistent routes and placeholders (`{Project_Folder}`, `{Root_Folder}`, etc.)
  - uniform naming (`Briefing`, `PRD`, `spec`, `FR`, `TR`)
  - aligned clarifying-question rules across skills
- **Verification**
  - actionable checklist (checkboxes)
  - observable criteria, not vague wording

### 3) Normalization plan

Classify findings by severity:
- **Critical**: breaks skill execution or creates ambiguous outputs
- **Important**: creates inconsistency across skills
- **Improvement**: improves clarity or maintainability

Propose minimal, concrete changes per file.

### 4) Apply changes

When user asks for edits:
- keep each skill's original functional goal
- avoid redesigning; fix only what is necessary to converge to a common standard
- preserve valid links to templates and related documents

### 5) Close

At the end:
- apply agreed changes directly
- explain result in 2-4 bullets max

## Style rules for project skills

- Write documentation in Spanish (technical English terms when precision improves).
- Keep `SKILL.md` direct and actionable.
- Avoid format drift for the same concept (for example, some with checklists and others without observable criteria).
- Use kebab-case slugs for identifiers and skill names.
- If the task is refinement, edit first and explain after.

## Verification

- [ ] Audit exists for all target skills
- [ ] Every inconsistency has a concrete action
- [ ] Compatibility with referenced templates is preserved
- [ ] Final set is homogeneous in structure and language
