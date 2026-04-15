---
name: commit
description: Create a git commit with an auto-generated conventional commit message summarizing the changes. Optionally push to remote.
---

# Create Commit

Create a commit with an auto-generated message summarizing the relevant changes for the current task.

## Repository Context

- Working tree status: !`git status`
- Unstaged changes: !`git diff`
- Staged changes: !`git diff --cached`
- Recent commits (style reference): !`git log -5 --format='%h %s'`

## Step 1: Check for Changes

If there are no changes in the context above (nothing staged, unstaged, or untracked): stop and tell the user there's nothing to commit.

## Step 2: Read Untracked Files

For untracked files shown in the status above, read them with the Read tool to understand their contents. Diffs already cover modified files.

## Step 3: Filter to Relevant Changes

Evaluate each changed/untracked file against the apparent intent of the current task. Classify each as **relevant** or **unrelated**.

Mark a file as unrelated if it:
- Belongs to a different feature or fix unrelated to the current task
- Is an auto-generated artifact (lockfiles, build output, `.cache/`) not caused by this task
- Appears accidentally modified (e.g. whitespace-only changes in unrelated files)

If unsure about any file, ask the user before proceeding:
> "These files are also changed — are they part of this commit?
> - `path/to/file1`
> - `path/to/file2`"

Only carry the relevant files forward into Step 4.

## Step 4: Draft Commit Message

Write a terse conventional commit message. Why over what — the diff says what.

**Subject line:** `<type>(<scope>): <imperative summary>` — scope optional
- Types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci`, `style`, `revert`
- Imperative mood: "add", "fix", "remove" — not "added", "adds", "adding"
- ≤50 chars preferred, hard cap 72; no trailing period

**Body** — skip entirely when subject is self-explanatory. Add body only for:
- Non-obvious *why*, breaking changes, migration notes, linked issues
- Wrap at 72 chars; bullets use `-` not `*`; issue refs at end: `Closes #42`

**Never include:** "This commit does X", "I/we/now/currently", AI attribution, emoji (unless project convention requires it)

**Always include body for:** breaking changes, security fixes, data migrations, reverts — future debuggers need the context.

If multiple unrelated changes exist, suggest the user split them into separate commits.

## Step 5: Stage and Commit

1. Stage only the relevant files identified in Step 3 with `git add <specific files>` — never use `git add -A`
2. Create the commit using a HEREDOC:

```
git commit -m "$(cat <<'EOF'
<type>(<scope>): <subject>

<optional body>
EOF
)"
```

3. Run `git status` to verify the commit succeeded.

## Step 6: Print Summary

Print a summary of the commit to the user:
- The commit message (subject and body)
- A bulleted list of files included in the commit (from `git show --name-only --format= HEAD`)

## Step 7: Ask About Push

After a successful commit, ask the user: **"Push to remote?"**

- If yes: check for upstream with `git rev-parse --abbrev-ref @{upstream} 2>/dev/null`, then `git push -u origin HEAD` (no upstream) or `git push` (has upstream). After a successful push, print the remote commit URL (from `gh browse $(git rev-parse HEAD) --no-browser`).
- If no: done
