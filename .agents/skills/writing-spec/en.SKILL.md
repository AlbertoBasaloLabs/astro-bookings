---
name: writing-spec
description: Writes a functional specification for a specific requirement from a PRD. Use it when a requirement must be detailed into model, API contract, acceptance criteria, and out-of-scope.
---

# Create a functional specification from a PRD

## Role

Act as a business analyst specialized in decomposing requirements.
Turn one functional requirement (FR) or technical requirement (TR) from the PRD into an implementable specification for product and engineering.

## Task

Create a functional specification for a single PRD requirement, using a standard structure of problem, solution, and verification.

## Context

- Briefing in `{Project_Folder}/briefing.md`
- PRD available at `{Project_Folder}/PRD.md`.
- Explicit target FR/TR (`FR1`, `FR2`, `TR1`, `TR2`, etc.) or equivalent description.

If the PRD is missing, create it first with [writing-prd](../writing-prd/SKILL.md).

## Steps

### 1. Clarifying questions

Ask questions only when they block document decisions:
- clear FR/TR target identification
- functional scope (what the system must do)
- ambiguous business rules
- lifecycle states
- field validations
- expected contract errors

If uncertainty is minor, document explicit assumptions.

When possible, propose initial options and ask for validation instead of open-ended questions.

Do not mix multiple FR/TRs in the same spec unless the user explicitly requests it.

### 2. Writing the specification

Generate the spec using the template that best fits the context:
- [Specification template in English](./en.spec.template.md)
- [Plantilla de especificación en español](./es.spec.template.md)

You can use either template even when the project mixes languages or needs a base template in another language.

#### 2.1 Identifier and slug

- Assign a unique spec identifier based on its requirement (for example, `FR1`, `TR2`)
- Add a descriptive kebab-case slug (for example, `rocket-management`).

The file name must combine both values: `{id-slug}.spec.md`. Example: `FR1-rocket-management.spec.md`.
- `{Project_Folder}/specs/{id-slug}.spec.md`

#### 2.2 Content rules

- Problem: describe context, user stories, entities, and business rules.
- Solution: without deep technical detail, describe what must be done in each physical layer (UI, API, DB).
- Verification: acceptance criteria using EARS (Easy Approach to Requirements Syntax).

## Verification

- [ ] Complete specification at `{Project_Folder}/specs/{id-slug}.spec.md`
- [ ] Based on a single PRD requirement
- [ ] Clear structure of problem, solution, and verification
