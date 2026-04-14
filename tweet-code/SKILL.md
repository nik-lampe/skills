---
name: tweet-code
description: Draft a tweet about a code library update or new release. Returns tweet text and an optional focused code snippet separately, ready to attach as a screenshot.
---

# Draft Code Tweet

Draft a tweet announcing an update to an existing library or the release of a new one. Tweet text and any code snippet are returned separately so the user can paste the code into a screenshot tool.

## Step 1: Gather Context

Ask only for information you don't already have. Usually needed:

- **Type** — update to an existing library, or introducing a new one?
- **Library name** — for the hook
- **Point of the tweet** — what changed, or what the library does
- **Code snippet** — does the user already have one in mind, or should one be generated?
- **Link** — optional release/PR/repo URL to append, include only if the user provides it

If the user's initial message already covers these, skip the questions.

## Step 2: Draft the Tweet Text

### Hook (first line)

- **Update to existing library:** `` In the next version of `<name>` ``
- **New library:** `` Introducing `<name>` ``

### Body rules

- Follow the hook with factual main text naming the actual behavior, API, or problem solved.
- Target max 280 characters including the hook. If the idea does not fit, consider a thread (Step 5).
- No em dashes. Use commas, parentheses, or separate sentences.
- No emojis, no icons.
- No marketing phrases. Avoid: "game-changer", "blazing fast", "supercharge", "revolutionary", "powerful", "seamless", "effortlessly", "excited to announce".
- Concrete over abstract. One idea per tweet. If two ideas fit, pick the stronger one.

## Step 3: Prepare the Code Snippet

Only if a snippet makes the point clearer.

- Strip everything unrelated to the update or benefit being shown.
- Remove imports, boilerplate, and unrelated setup unless they are the point.
- Keep it short enough to read as a phone screenshot.
- Return it in a fenced code block, separate from the tweet text.

## Step 4: Output Format

Default to **3 side-by-side variants** unless the user asked for one. Show the character count for each variant so the user can pick at a glance.

````
## Variant A (<char count>)
<tweet text>

## Variant B (<char count>)
<tweet text>

## Variant C (<char count>)
<tweet text>

## Code snippet
```ts
<focused code>
```
````

If the user provided a link, show it once under the variants so they can append the one they pick.

## Step 5: Threads

Propose a thread if:
- The idea cannot fit in 280 chars without losing the point, or
- There are multiple distinct ideas worth keeping.

Thread rules:
- Tweet 1 still follows the hook rule from Step 2.
- Subsequent tweets are numbered `2/`, `3/`, etc.
- Each tweet independently under 280 chars.
- Tell the user it is a thread and why, so they can choose thread vs single tweet.

## Step 6: Self-Check Before Returning

Before posting the reply, verify:

- [ ] Character count shown for each variant
- [ ] No em dashes (search for `—`)
- [ ] No emojis
- [ ] No banned marketing words from Step 2
- [ ] Hook matches the correct type (update vs new)
- [ ] Code snippet (if any) is focused on the point of the tweet
