---
name: markdown-preview
description: Show a markdown draft in a live preview pane to the right of the agent, using backtuick inside Herdr. Use when another skill (issue, pull-request, paper-review) or the user needs a markdown draft written, reviewed or iterated on, for example "montre-moi le brouillon", "preview the description", or whenever a long markdown text is about to be produced for the user to read.
---

# Markdown preview

Show a markdown draft in a `backtuick` pane to the right of the agent, so the user reads rendered markdown while the conversation continues on the left.

`backtuick` reloads the file by itself when it changes.
Never relaunch it after an edit.

## Workflow

### 1. Create the draft file

```bash
mktemp --suffix=.md
```

Write the draft to that file with the Write tool.
Keep the path: all later revisions edit this same file.

### 2. Open the pane

Reuse the preview pane if one was already opened in this session.
Otherwise split to the right of the calling pane, keep the focus on the agent, and read the new pane ID from `.result.pane.pane_id`:

```bash
herdr pane split --current --direction right --cwd "$PWD" --no-focus
herdr pane run <pane-id> "backtuick <file>"
```

Announce the pane in one line (ID and purpose).

### 3. Iterate

Edit the draft file in place.
The preview updates on its own.
Discuss changes in the conversation, not in the pane.

For a second draft in the same session, point the existing pane at the new file: `herdr pane run <pane-id> "backtuick <new-file>"` after quitting the previous viewer with `herdr pane send-keys <pane-id> q`.

### 4. Clean up

Once the draft is consumed (issue created, PR opened, review done):

```bash
herdr pane close <pane-id>
rm <file>
```

Only close the pane created by this workflow.
Keep both if the user wants to keep working on the draft.
