---

name: create-pr
description: Prepare and publish changes with a develop-first branch strategy, deterministic PR targets, validation steps, commit creation, and PR opening with explicit branch/target reporting.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Prepare Application for Commit

Steps:

1. Analyze all uncommitted git changes.

2. Determine the current branch and apply branch policy before changing anything:

   * preferred working branch is `develop`
   * do not create or checkout `feature/*` branches automatically
   * if current branch is not `develop`, stop and ask the user how to proceed before any checkout
   * only switch branches after explicit user confirmation

3. Update or create `CHANGELOG.md` documenting the changes.

4. Detect project package manager and scripts:

   * identify if the project uses pnpm, yarn, npm or bun
   * read `package.json` scripts (if available)

5. Run lint (if available):

   * find a script related to lint (e.g. lint, lint:fix, check)
   * execute it using the correct package manager
   * if no lint script exists → skip

6. Run tests (if available):

   * find a test script (e.g. test, test:ci, test:unit)
   * execute it
   * if no test script exists → skip

7. Run build (if available):

   * find a build-related script (e.g. build, compile, typecheck)
   * execute it
   * if no build script exists → skip

8. Increment project version using semantic versioning.

9. Stage all changes.

10. Create a conventional commit summarizing the changes.

11. Decide PR target branch deterministically from the source branch:

   * if source branch is `develop` → target branch must be `main`
   * if source branch is not `develop` → target branch must be `develop`

12. Before push and PR creation, explicitly report to the user:

   * source branch that will be pushed
   * target branch that will receive the PR
   * warning when target is `develop` instead of `main`

13. Push the current branch to remote.

14. Create the pull request using the selected target branch from step 11.
