---
name: nyx-git-commit-quality
description: Use when preparing, reviewing, or creating commits in the Nyx repository and the change scope, staged files, working-tree cleanliness, Gitmoji, or Conventional Commit message is uncertain.
---

# Nyx Git Commit Quality

## Overview

A commit is a small, reviewable record of one intentional change. Its files, checks, and message must describe the same user request. “Clean” means no accidental files, secrets, generated output, whitespace errors, or unrelated changes—not that pre-existing work may be deleted.

## Required workflow

1. **Capture the baseline before staging.** Run:

   ```bash
   git status --short
   git diff --stat
   git diff --name-only
   ```

   Record pre-existing changes. Never hide, reset, stash, or overwrite them to make the tree look clean.

2. **Map the request to files.** Read the relevant diff and confirm every changed path belongs to the requested outcome:

   ```bash
   git diff
   git diff --cached
   git diff --check
   git diff --cached --check
   ```

   Stage explicit paths (`git add path/to/file`). Do not use `git add .` or `git add -A` when unrelated changes are present.

3. **Inspect the staged boundary again.** The staged diff must contain one logical change and no `.env*`, secrets, private keys, local certificates, `node_modules`, `.next`, logs, temporary files, or accidental deletions. Check for conflict markers and suspicious generated files. If the request contains multiple independent outcomes, split them or ask before proceeding.

4. **Run proportionate checks.** Read the repository scripts first. For this Next.js project, use `pnpm lint` and `pnpm build` when the change affects application code; run `pnpm test` only when a test script exists. Do not claim a check passed without fresh output from the complete command.

5. **Choose one Gitmoji and message.** Use this format:

   ```text
   <gitmoji> <type>(optional-scope): <imperative summary>
   ```

   The first token is the Gitmoji, the type is lowercase, and the summary states the user-visible intent rather than implementation trivia.

6. **Commit only with authority.** If the user asked for a commit, create it after the staged diff and checks pass. Otherwise, prepare the verified staged set and proposed message, then stop. Never amend, rebase, reset, checkout, or clean files without explicit authorization.

7. **Verify the result.** After committing, run:

   ```bash
   git show --check --stat --oneline HEAD
   git status --short
   ```

   Report any unrelated pre-existing changes that remain. Do not call the working tree clean when those changes are still present.

## Gitmoji quick reference

| Gitmoji | Type | Use for |
| --- | --- | --- |
| ✨ | `feat` | New user-facing capability |
| 🐛 | `fix` | Bug fix |
| ♻️ | `refactor` | Behavior-preserving restructuring |
| 🎨 | `style` | Formatting or visual styling only |
| 📝 | `docs` | Documentation |
| ✅ | `test` | Tests or test coverage |
| 🔧 | `chore` | Tooling or project maintenance |
| 📦 | `deps` | Dependency changes |
| ⚡️ | `perf` | Performance improvement |
| 🔒 | `security` | Security hardening or remediation |
| 🚀 | `deploy` | Deployment or release configuration |

Select the dominant intent. Do not use `chore` to conceal a feature or bug fix.

## One good example

For a reusable Nyx sidebar changed in `src/components/nyx/` and its tests:

```text
✨ feat(ui): add reusable Nyx sidebar
```

The staged diff contains only that sidebar, its direct consumers, and tests; `git diff --cached --check`, the relevant project checks, and the post-commit inspection all pass.

## Stop conditions and common mistakes

- **Unrelated dirty files:** keep them out of the commit and report them.
- **A secret or private key is staged:** unstage it, investigate why, and rotate it if exposed; do not commit it.
- **Whitespace or conflict errors:** fix the source and rerun the checks.
- **Several unrelated intents:** split the changes or ask which one is in scope.
- **A vague message such as `update stuff`:** rewrite it with Gitmoji, type, and intent.
- **“Clean” achieved with `git reset --hard` or `git clean`:** stop; those commands can destroy user work.

**Red flag:** a commit cannot be explained as “this request changed these files for this reason.” Stop and inspect the scope again.
