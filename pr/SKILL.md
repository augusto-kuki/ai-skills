---
name: pr
description: Prepare and publish changes with a develop-first branch strategy, deterministic PR targets, validation steps, commit creation, and PR opening or updating with explicit branch/target reporting. Automatically links related GitHub issues/PRDs so they close on merge, and updates an existing open PR instead of creating a duplicate.
---

# Prepare Application for Commit

Steps:

1. Analyze all uncommitted git changes.

2. Identify related GitHub issues or PRDs before anything else:

   * check the conversation context for any issue or PRD being worked on (e.g. "issue #42", a GitHub issue URL, or a PRD tracked as an issue)
   * check the current branch name for issue references (e.g. `42-feature-name`, `issue-42`)
   * check recent commit messages on the branch for issue references (`#42`, `closes #42`)
   * if candidates are found but linkage is ambiguous, ask the user to confirm which issues this work resolves
   * store the confirmed issue numbers for use in the commit and PR body (steps 10 and 15)
   * if no issue/PRD is related → skip, do not invent references

3. Determine the current branch and apply branch policy before changing anything:

   * preferred working branch is `develop`
   * do not create or checkout `feature/*` branches automatically
   * if current branch is not `develop`, stop and ask the user how to proceed before any checkout
   * only switch branches after explicit user confirmation

4. Update or create the `CHANGELOG.md` file, briefly documenting the changes.

5. Detect project package manager and scripts:

   * identify if the project uses pnpm, yarn, npm or bun
   * read `package.json` scripts (if available)

6. Run lint (if available):

   * find a script related to lint (e.g. lint, lint:fix, check)
   * execute it using the correct package manager
   * if no lint script exists → skip

7. Run tests (if available):

   * find a test script (e.g. test, test:ci, test:unit)
   * execute it
   * if no test script exists → skip

8. Run build (if available):

   * find a build-related script (e.g. build, compile, typecheck)
   * execute it
   * if no build script exists → skip

9. Increment project version using semantic versioning.

10. Stage all changes.

11. Create a conventional commit summarizing the changes:

    * if issues were identified in step 2, reference them in the commit body (e.g. `Refs #42`)

12. Decide PR target branch deterministically from the source branch:

    * if source branch is `develop` → target branch must be `main`
    * if source branch is not `develop` → target branch must be `develop`

13. Before push and PR creation, explicitly report to the user:

    * source branch that will be pushed
    * target branch that will receive the PR
    * related issues/PRDs that will be auto-closed on merge (if any)
    * warning when target is `develop` instead of `main`

14. Push the current branch to remote.

15. Check for an existing open PR from the source branch to the target branch:

    * run `gh pr list --head <source-branch> --base <target-branch> --state open --json number,title,body`
    * if an open PR exists → DO NOT create a new one; update it instead (step 16)
    * if no open PR exists → create a new one (step 17)

16. Update the existing PR (when found in step 15):

    * fetch the current title and body of the existing PR
    * merge the existing description with the new changes: preserve all pre-existing information and append a new section documenting this update (e.g. a `## Update <date>` section summarizing the new changes)
    * update the title only if needed to reflect the broader scope; keep the original intent recognizable
    * ensure all issue-closing keywords are present in the body: keep existing `Closes #N` references and add new ones from step 2 without duplicating
    * apply the update with `gh pr edit <number> --title "..." --body "..."`
    * report to the user that an existing PR was updated (include PR number and URL)

17. Create the pull request (when no open PR exists):

    * use the selected target branch from step 12
    * in the PR body, include a closing keyword line for each issue identified in step 2, one per line: `Closes #42` (use `Closes`/`Fixes`/`Resolves` — these trigger automatic issue closing on merge)
    * note: automatic closing only triggers when the PR targets the repository's default branch (usually `main`); when the target is `develop`, still include the references for traceability, but inform the user that the issues will only auto-close when the changes reach the default branch
    * report the PR number and URL to the user