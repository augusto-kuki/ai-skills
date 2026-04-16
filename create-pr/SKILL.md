---
name: create-pr
description: Safely prepare and publish local changes with full validation. Use when the user asks to review uncommitted changes, run lint/tests/build, update changelog, apply semantic versioning, create a conventional commit, push, and open a PR. Adapt dynamically to the project stack and available scripts without hardcoding commands.
---

# Create PR

## Purpose

Standardize the process of finalizing and publishing code changes with high quality and safety.

This skill ensures the agent:

* fully understands all uncommitted changes;
* validates code through lint, tests, and build;
* maintains proper versioning and changelog;
* creates clear, traceable commits;
* safely publishes changes via push and pull request.

---

## Core Responsibilities (Strict)

The agent MUST:

1. **Map all uncommitted changes**

   * Identify every modified, added, and deleted file
   * Understand the intent and impact of the changes

2. **Validate implementation quality**

   * Run lint, tests, and build (in strict order)
   * Stop immediately on critical failure

3. **Understand repository context**

   * Detect stack, tooling, scripts, and workflows
   * Avoid assumptions or hardcoded commands

4. **Maintain versioning and changelog**

   * Update `CHANGELOG.md` (if present)
   * Increment version using semantic versioning
   * Sync version in `package.json` (if applicable)

5. **Publish changes**

   * Create a conventional commit
   * Push branch safely
   * Open a PR targeting the correct base branch

---

## Why This Matters

This workflow reduces regressions and ensures consistency by enforcing:

* zero-assumption execution across different stacks;
* mandatory validation before publishing;
* high-quality commit history;
* predictable and reproducible delivery for teams and agents.

---

## Operational Principles

* **Safety First** → Never skip validation without explicit instruction
* **Fail Fast** → Stop on the first critical failure
* **Adaptability** → Detect tools dynamically, never hardcode
* **Transparency** → Always report what was executed and results
* **Non-destructive Behavior** → Never force push, version bump, or PR without justification

---

## Mandatory Workflow

### 1. Inspect Repository State

Run:

* `git rev-parse --is-inside-work-tree`
* `git status --short`
* `git diff --name-only`

Goals:

* confirm this is a git repository;
* list all changed files;
* understand the full scope before taking action.

---

### 2. Detect Stack and Tooling

Dynamically detect package manager and commands.

#### JS/TS priority order:

1. `pnpm-lock.yaml` → use `pnpm`
2. `yarn.lock` → use `yarn`
3. `bun.lockb` or `bun.lock` → use `bun`
4. `package.json` → use `npm`

If `package.json` exists:

* read available `scripts`;
* ONLY execute scripts that actually exist.

#### Fallback strategies:

* If `Makefile` exists → use targets like `lint`, `test`, `build`
* Otherwise detect ecosystem:

  * Python → `pytest`
  * Go → `go test`
  * Rust → `cargo test`

---

### 3. Run Quality Gates (Strict Order)

Execute in this exact order:

1. **Lint**
2. **Tests**
3. **Build**

#### Rules:

* Stop immediately on failure
* Do NOT continue if any critical step fails
* Do NOT mask or ignore failures

#### Script priority (if `package.json` exists):

**Lint**

1. `lint:fix`
2. `lint`
3. `check`

**Tests**

1. `test`
2. `test:ci`
3. `test:unit`

**Build**

1. `build`
2. `compile`
3. `typecheck`

---

### 4. Update Changelog

If `CHANGELOG.md` exists:

* Add a new entry describing the change
* Keep it concise and meaningful

Include:

* what changed;
* why it changed;
* technical or functional impact;
* breaking changes (if any).

---

### 5. Apply Semantic Versioning

Always update version when publishing changes.

#### Rules:

* `patch` → bug fixes (no breaking changes)
* `minor` → new backward-compatible features
* `major` → breaking changes

#### Required actions:

* Update version in:

  * `package.json` (if present)
  * any other version source if applicable

If unsure → ask for confirmation before proceeding.

---

### 6. Prepare Commit

Run:

* `git add -A`

Create a **conventional commit**:

Format:

```
type(scope): summary
```

Common types:

* `feat`, `fix`, `chore`, `refactor`, `test`, `docs`, `perf`, `ci`

#### Rules:

* message must reflect actual changes;
* be concise but specific;
* align with modified files.

---

### 7. Publish Changes

#### Push

* Push current branch to `origin`
* Set upstream if needed

#### Pull Request

* Open PR to correct base branch (`main`, `develop`, etc.)
* Title = commit summary
* Description must include:

  * summary of changes
  * validation results (lint/tests/build)
  * potential risks

If PR creation is not possible (e.g., no CLI or remote):

* clearly report limitation;
* DO NOT fake success.

---

## Failure Handling

On any failure, ALWAYS report:

1. command executed;
2. main error message;
3. impact (what is blocked);
4. recommended next step.

Never hide or soften failures.

---

## Expected Agent Output

At the end, the agent MUST report:

* changed files scope;
* commands executed;
* lint/test/build results;
* changelog update status;
* version bump details;
* commit message and hash;
* push status;
* PR status.

---

## Completion Checklist

Before finishing, ensure:

* all changes reviewed;
* lint passed (or explicitly skipped);
* tests passed (or explicitly skipped);
* build passed (or explicitly skipped);
* changelog updated (if applicable);
* version updated correctly;
* commit created;
* push executed (or justified);
* PR created (or justified).
