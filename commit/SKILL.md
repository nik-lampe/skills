---
name: commit
description: Create a git commit with an auto-generated conventional commit message summarizing the changes. Optionally push to remote.
---

# Create Commit

Create a commit with an auto-generated message summarizing all staged and unstaged changes.

## Step 1: Gather Changes

Run these commands in parallel:
- `git status` — see untracked and modified files
- `git diff` — unstaged changes
- `git diff --cached` — staged changes
- `git log -5 --format='%h %s'` — recent commits for message style reference

If there are no changes (nothing staged, unstaged, or untracked): stop and tell the user there's nothing to commit.

## Step 2: Read Changed Files

For untracked files shown in `git status`, read them to understand their contents.
For modified files, the diff output is sufficient.

## Step 3: Draft Commit Message

Write a conventional commit message:
- **Type:** `feat:`, `fix:`, `chore:`, `refactor:`, `test:`, `docs:`, etc.
- **Scope (optional):** package name if changes are scoped to one package, e.g. `feat(api):`
- **Subject:** concise summary of what changed and why (under 72 chars)
- **Body (if needed):** additional context for complex changes, separated by a blank line

If multiple unrelated changes exist, suggest the user split them into separate commits.

## Step 4: Stage and Commit

1. Stage relevant files with `git add <specific files>` — prefer specific files over `git add -A`
2. Create the commit using a HEREDOC:

```
git commit -m "$(cat <<'EOF'
<type>(<scope>): <subject>

<optional body>
EOF
)"
```

3. Run `git status` to verify the commit succeeded.

## Step 5: Ask About Push

After a successful commit, ask the user: **"Push to remote?"**

- If yes: check for upstream with `git rev-parse --abbrev-ref @{upstream} 2>/dev/null`, then `git push -u origin HEAD` (no upstream) or `git push` (has upstream)
- If no: done
