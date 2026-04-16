---

name: create-pr
description: Prepare and publish changes by analyzing git diff, discovering available project scripts, running validations when available, updating changelog, versioning, committing and opening a PR.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Prepare Application for Commit

Steps:

1. Analyze all uncommitted git changes

2. Update `CHANGELOG.md` documenting the changes (if the file exists)

3. Detect project package manager and scripts:

   * identify if the project uses pnpm, yarn, npm or bun
   * read `package.json` scripts (if available)

4. Run lint (if available):

   * find a script related to lint (e.g. lint, lint:fix, check)
   * execute it using the correct package manager
   * if no lint script exists → skip

5. Run tests (if available):

   * find a test script (e.g. test, test:ci, test:unit)
   * execute it
   * if no test script exists → skip

6. Run build (if available):

   * find a build-related script (e.g. build, compile, typecheck)
   * execute it
   * if no build script exists → skip

7. Increment project version using semantic versioning (if applicable)

8. Stage all changes

9. Create a conventional commit summarizing the changes

10. Push the current branch to remote

11. Create a pull request to the main branch
