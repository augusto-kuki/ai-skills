---

name: create-pr
description: Analyze repository changes, discover available scripts (lint, test, build), validate if possible, then commit, push and open a PR. Use when the user wants to safely publish code.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Create PR

## Quick Start

Understand changes → discover available validations → run what exists → commit → push → open PR.

---

## When to Use

Use when the user asks to:

* commit changes
* open a PR
* publish code
* validate before committing

---

## Execution Rules

* Always discover available tools before executing anything
* Never assume scripts or commands
* Only run what actually exists
* Missing scripts are NOT errors
* Stop only on real execution failures
* Prefer incremental execution if the environment is unstable

---

## Workflow

### 1. Inspect Changes

* Analyze current git state
* Identify all modified, added and deleted files
* Understand the scope before acting

---

### 2. Discover Project Capabilities

If `package.json` exists:

* Read `scripts`
* Detect if there are scripts related to:

  * lint (e.g. lint, lint:fix, check)
  * tests (e.g. test, test:ci, test:unit)
  * build (e.g. build, compile, typecheck)

If no `package.json`:

* Try to infer tooling from project structure (Makefile, language, etc.)

---

### 3. Run Available Validations

* If lint script exists → run it
* If test script exists → run it
* If build script exists → run it

Rules:

* Execute only what was discovered
* Skip silently if not available
* Stop on real failures
* Never fail due to missing scripts

---

### 4. Update Changelog (if present)

* Detect if `CHANGELOG.md` exists
* If yes, add a concise entry describing the change

---

### 5. Versioning (if applicable)

* If project uses versioning (e.g. `package.json`):

  * determine correct semver bump (patch, minor, major)
* If uncertain → ask before applying

---

### 6. Commit

* Stage all changes
* Generate a conventional commit message based on actual changes

Format:

```id="x8k1qz"
type(scope): summary
```

---

### 7. Push and Open PR

* Push current branch
* Open PR to default branch

Include in PR:

* summary of changes
* which validations were executed (and which were skipped)

---

## Failure Handling

* If a command fails → report and stop
* If something is missing → skip, do not fail
* Never hide errors

---

## Output

Always report:

* files changed
* detected scripts
* validations executed vs skipped
* commit message
* PR status
