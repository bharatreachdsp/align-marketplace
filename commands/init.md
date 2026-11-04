---
description: Set up ALIGN in this repository (sign in, opt in, start capturing decisions)
---

Set up ALIGN for the repository this session runs in. The person asked for it by running this command; do these
steps in order and show them each command's output as it is.

1. Run this with the Bash tool from the repository's root (it can wait up to 10 minutes while the person signs in
   in their browser, so give it a timeout of 600000 ms):

   ```sh
   "${CLAUDE_PLUGIN_ROOT}/scripts/align" init --harness claude-code
   ```

2. Only the person can opt a repository in, and they do it in their browser: the command prints a line starting
   `Confirm` with a link and waits. Tell the person to open that link (signed in as themselves), read what ALIGN
   captures and never captures, and confirm there; for a beta organisation, confirming there also accepts the pilot
   data terms it shows. Never add `--yes` on your own, nor `--accept-pilot-terms`: neither opts in or accepts anything. Do not ask
   them to type anything into this session's terminal: Claude Code's interface holds that terminal, so the command
   never asks there from your shell.

3. If it asked the person to sign in, tell them to open the link it printed and enter the code it shows, then wait
   for the command to finish.

4. When it finishes, tell the person in one or two sentences what it set up. If it said that reading past sessions
   (backfill) was skipped because it ran inside an agent's shell, tell them they can run `align backfill` in their
   own terminal whenever they like. The plugin already provides the hooks, the MCP server and `/align:align`, so
   nothing else needs installing.

If the command is missing or fails to start, tell the person to follow the install steps at
https://align.bharatreachdsp.com/install#claude-code-plugin instead.
