---
name: pr
description: Create a GitHub Pull Request against the default base branch, summarizing all changes on the current branch.
---

# Create Pull Request

Create a GitHub PR against the default base branch with a clear summary of all changes.

## Repository Context

- Default base branch: !`git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo main`
- Current branch: !`git branch --show-current`
- Working tree status: !`git status`
- Commits ahead of base: !`git log $(git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo main)..HEAD --format='%h %s'`
- Full diff vs base: !`git diff $(git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@' || echo main)...HEAD`
- Upstream tracking: !`git rev-parse --abbrev-ref @{upstream} 2>/dev/null || echo none`

## Step 1: Validate State

Using the context above:
- If current branch equals the base branch: stop and tell the user to create a feature branch first.
- If there are uncommitted changes in the status: warn the user and ask whether to proceed without them or stop.
- If there are no commits ahead of base: stop and tell the user there's nothing to PR.

## Step 2: Identify Unrelated Changes

Read the full diff in the context. Identify any files that appear unrelated to the branch's purpose — e.g. lockfile-only changes from unrelated installs, accidental edits, or changes from a different task. If such files are detected, ask the user:
> "These files appear unrelated to the PR — should they be included in the description?
> - `path/to/file1`
> - `path/to/file2`"

Only use the confirmed-relevant changes when drafting the PR below.

## Step 3: Draft PR Title and Body

**Title:** Short conventional-commit-style title (under 70 chars). Use the dominant change type (`feat:`, `fix:`, `chore:`, etc.).

**Body:** Use this exact format:

```
## Summary
<2-5 bullet points describing the key changes>

## Test plan
<Bulleted checklist of how to verify these changes>
```

## Step 4: Push and Create PR

1. If upstream tracking is `none` in the context, push with: `git push -u origin HEAD`
2. Otherwise, push with: `git push`
3. Create the PR: `gh pr create --base <base> --title "<title>" --body "<body>"` using a HEREDOC for the body
4. Return the PR URL to the user
