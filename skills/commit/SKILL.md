---
name: commit
description: Create a git commit with a well-crafted conventional commit message. Use when the user asks to commit, create a commit, or says something like "commit this".
---

# Commit

Create conventional commit messages that explain the **why**, not the what.

The commit message helps:
- The **PR reviewer** understand the context quickly
- The **devs in 6 months** who will `git blame` this code

## Workflow

### 1. Get the staged diff

```bash
git diff --cached
```

If nothing is staged, stop and tell the user to stage changes first.

### 2. Get existing scopes

Extract scopes already used in the project to stay consistent:

```bash
git log --oneline --no-merges | sed -n 's/^[^ ]* [a-z]*(\([^)]*\)).*/\1/p' | sort | uniq -c | sort -rn
```

Use one of these scopes if it fits.
Only invent a new scope if none matches.

### 3. Analyze the diff

Identify:
- Which files are touched
- Which functions/classes/components are modified
- The pattern of the change (addition, deletion, refactoring, fix...)

If needed, use `Read` to explore touched files for better architectural understanding.

### 4. Decide: ask or skip

**Skip questions if the why is obvious:**
- Bug fix with explicit error message
- Typo / formatting
- Adding tests for existing code
- Rename with clear reason in the code
- The conversation already provides full context (e.g. you just helped implement the change)

**Ask 1-2 questions if you need to clarify:**
- Refactoring without a visible problem
- New feature without business context
- Non-obvious technical choice
- Behavior change without an apparent bug

Questions must be **specific to the diff** — mention what you see and ask why:
- "You're migrating from bash to Node.js — was it for performance, maintainability, something else?"
- "You're adding retry with backoff on API X — what was the issue in prod?"
- "You're replacing moment.js with date-fns — bundle size problem?"

Never ask vague questions like "Why this change?" or "What was the problem?".

### 5. Generate the commit message

#### Format

```
type(scope): short description

One paragraph stating the problem and how this change resolves it.
```

#### Rules

- Title < 72 characters, english, lowercase
- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`
- Body: one paragraph of 150 to 250 characters by default
- A second paragraph only when there is a genuinely separate point to make, such as a caveat or a migration note

#### The body explains the WHY

Open with the problem, in the past tense.
State what was wrong or missing before this change, and its consequence.

Close with the resolution only when that clause carries something the diff does not already show: a cross-reference to an existing pattern, a decision, a scope.
Drop it when it would merely restate the diff.

> The features file was read again on every check, and the read was async, which forced all callers up to the command tree construction to await.
> It's now loaded once at startup and reloaded after a write, just like the config file.

The closing clause earns its place here for "just like the config file", not for "loaded once at startup" which the diff shows anyway.

Be concrete.
List the actual limitations rather than alluding to them.

Bad: "The JS git implementation has some limitations."
Good: "The pure JS git implementation is HTTP-only, slow on repositories with rewritten history, times out on large pushes and can't deploy from a linked worktree."

Point at existing conventions in the codebase.
When the change follows a pattern already used elsewhere, name it: it tells the reviewer the approach was not invented for this commit.
Verify it first, "just like the config file" is only worth writing after reading how the config file is actually handled.

Naming symbols is encouraged.
`isFeatureEnabled()` or `clever features disable system-git` give the reader an entry point.
What is forbidden is narrating the edit, not mentioning code.

**Forbidden patterns** (the diff already shows the what):
- "This change/commit introduces/adds/creates..."
- "This refactors/updates/modifies..."
- Any form of "add X, create X, update X, remove X, rename X, refactor X"

### 6. Wrap the body

Do not rely on the LLM to respect line length.
Wrap the body (not the title) at 80 characters using `fmt`:

```bash
echo "<body>" | fmt -w 80
```

### 7. Commit

Always use a HEREDOC to avoid escaping issues:

```bash
git commit -F - << 'EOF'
type(scope): description

Wrapped body paragraph.
EOF
```
