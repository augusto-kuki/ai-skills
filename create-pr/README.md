# create-pr

Validate, commit and publish code safely using AI agents.

## What it does

- maps uncommitted changes
- runs lint, tests and build
- updates changelog
- applies semantic versioning
- creates conventional commit
- pushes and opens PR

## Usage

This skill is automatically triggered when the user asks to:

- commit changes
- open a PR
- publish code

## Installation

```bash
npx skills add github:augusto-kuki/ai-skills --skill create-pr
``` 

## Test installation

After uploading to GitHub:

```bash
npx skills@latest add github:augusto-kuki/ai-skills --skill create-pr