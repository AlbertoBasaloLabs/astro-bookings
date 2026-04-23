---
name: organizing-backlog
description: Organizes a software project backlog, prioritizing specifications by dependency and status. Use it when a new specification is created or an existing one changes status.
---

# Organize backlog

## Role

Act as a Product Owner or Scrum Master responsible for organizing a software project backlog.

## Task

Organize the project backlog by prioritizing specifications according to dependencies and current status.

## Context

- PRD available at `{Project_Folder}/PRD.md`.
- Specifications in `{Project_Folder}/specs/`
- Backlog (if it already exists) at `{Project_Folder}/BACKLOG.md`.

## Steps

### 1. Clarifying questions

Ask questions only if they block the document decisions:
- missing PRD or specifications
- unclear status or dependencies for a specification

### 2. Backlog writing

Generate the backlog document with the template that best fits the context:

- [In English](en.backlog.template.md)
- [En español (Spanish)](es.backlog.template.md)

You can use either template even when the project mixes languages or needs a base template in another language.

Rules:
- Use the composite identifier `{id-slug}` as the unique key.
- In the dependency column, use short IDs `[FR1, FR2]`.
- Specifications move through these states:
  - `Pending`: work has not started yet.
  - `In Progress`: work is actively ongoing.
  - `Completed`: work is finished.
  - `Blocked`: waiting for unresolved dependencies.
  - `Failed`: work could not be completed.

## Verification

- [ ] Backlog updated at `{Project_Folder}/BACKLOG.md`
- [ ] Includes all specifications
- [ ] Each one includes up-to-date dependencies and status
