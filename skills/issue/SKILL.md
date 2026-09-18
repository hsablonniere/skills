---
name: issue
description: Create an issue (GitHub) or issue (GitLab) focused on the problem, not the solution. Use when the user asks to create an issue, file a bug, report a problem, or says something like "open an issue".
---

# Issue

Create issues that stay focused on the problem.
A good issue describes what is wrong or missing, not how to fix it.

## Workflow

### 1. Detect platform

```bash
git remote get-url origin
```

- Contains `github.com` → GitHub, use `gh`
- Contains `gitlab` → GitLab, use `glab`
- Neither → stop, tell the user the platform isn't supported

### 2. Create or update

Default to creating a new issue.
Only treat it as an update if the user explicitly says so and gives an issue number or URL.

For an update, fetch the existing issue first and use it as the starting point for the interview:

- GitHub: `gh issue view <number> --json title,body`
- GitLab: `glab issue view <number>`

### 3. Interview to isolate the problem

Act like a Product Owner reviewing a request: if the user opens with a solution ("add a retry button", "cache the response"), don't take it at face value.
Ask what breaks or what's missing without it, until the actual problem is clear.

- Ask targeted questions, never generic ones ("why is this an issue?" is useless; "what happens today when X, and what should happen instead?" is useful)
- Keep asking until the problem can be stated in 1-3 sentences
- If the user insists on a solution, accept it but file it under "Solution considered", not as the problem statement
- Skip the interview if the problem is already clear and specific from the conversation

### 4. Gather context

Think like the maintainer who will triage this issue: what would they need to reproduce or locate the problem?
Pull only what's relevant and available, don't force a fixed template.
Examples: OS/browser for a UI bug, a command name for a CLI, an environment or config for infra, a file/module for a code-level issue.
Skip anything not applicable.

### 5. Generate title and description

#### Title

- Short and descriptive (< 85 characters)
- English, lowercase (except proper nouns)
- States the problem, not the fix (`"login fails with expired token"`, not `"add token refresh"`)

#### Description structure

Use only the relevant sections, skip empty ones:

```markdown
## TL;DR
<!-- 200-250 characters, summarizing context, problem and impact -->

## Context
<!-- Only what's relevant: environment, versions, where it happens -->

## Problem
What is wrong or missing. The core of the issue.

### How to reproduce
<!-- Only for reproducible bugs -->
1. Step 1
2. Step 2

### Impact
<!-- Only if severity/scope isn't obvious from the problem statement -->

## Solution considered
<!-- Optional, open-ended, not prescriptive -->

## Solution ruled out
<!-- Optional, what won't work and why -->

## Open questions
<!-- Optional, last section, what stays undecided -->
```

#### Rules

- `## TL;DR` is present by default, as the first section, unless the user declines it
- `## TL;DR` is a 200-250 character summary of the context, the problem and its impact, written as plain prose
- `## TL;DR` never mentions the considered or ruled out solutions, it describes the situation only
- `## Problem` is always present
- `## Context` only if it helps a maintainer locate or reproduce the issue
- `### How to reproduce` is a subsection of `## Problem`, only for bugs with clear repro steps
- `### Impact` only if scope/severity isn't already obvious
- `## Solution considered` / `## Solution ruled out` only if the interview surfaced actual candidate solutions, never invented
- `## Open questions` optional, always the last section, for what the interview left undecided
- `## Open questions` stays at H2 even when some questions are about a considered solution, because others question the problem framing itself
- No labels, no assignees, no milestone unless the user explicitly asks

### 6. Create or update the issue

**Create (new issue):**

- GitHub: `gh issue create --title "<title>" --body "<body>"`
- GitLab: `glab issue create --title "<title>" --description "<body>"`

**Update (existing issue):**

- GitHub: `gh issue edit <number> --title "<title>" --body "<body>"`
- GitLab: `glab issue update <number> --title "<title>" --description "<body>"`

Pass `<body>` via a heredoc, never a literal `"..."` string.
Write each paragraph as a single unwrapped line, don't hard-wrap prose at ~80/110 characters like a text editor would; let Markdown soft-wrap it.

Show the issue URL when done.
