# AI Skills

This repository stores reusable agent skills that I use across different projects.

The goal is to keep each skill documented, easy to install, and easy to evolve over time as new needs appear.

## Repository Purpose

- Centralize personal skills in one place
- Reuse the same automation patterns across projects
- Keep skill documentation and behavior consistent
- Grow a maintainable skills library over time

## Current Skills

- `create-pr` - Safely validates, commits, and publishes local changes with a pull request flow.
- `backend` - Enforces Clean Architecture with DDD-inspired modeling for NestJS backends.

## Repository Structure

Each skill should live in its own folder, for example:

```text
<skill-name>/
  SKILL.md      # Main skill definition and behavior
  README.md     # Human-readable usage and installation notes
```

## How to Install a Skill

Example:

```bash
npx skills add github:augusto-kuki/ai-skills --skill create-pr
```

## Adding a New Skill

When adding a new skill, follow this checklist:

1. Create a new folder named after the skill (kebab-case recommended).
2. Add `SKILL.md` with clear purpose, constraints, and workflow.
3. Add a local `README.md` with usage and installation instructions.
4. Add the new skill to the **Current Skills** section in this root `README.md`.

## Maintenance Notes

- Keep descriptions short and outcome-oriented.
- Prefer explicit workflows and failure handling in `SKILL.md`.
- Update this file whenever a skill is added, removed, or significantly changed.

## License

Add a license section/file when you decide how this repository will be shared.
