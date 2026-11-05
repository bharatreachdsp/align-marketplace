# ALIGN for Claude Code

ALIGN is the decision ledger for teams that build software with AI agents. This plugin connects Claude Code to it:
it captures what was decided, and why, in your Claude Code sessions in the repositories you opt in; it opens each
new session with the decisions your team has confirmed for the code you are working on; and it gives Claude MCP
tools to look decisions up, record a proposal or ask the owner of a decision. Proposals stay visible only to you
until you confirm them; only a person confirms a decision.

## Install

```text
/plugin marketplace add bharatreachdsp/align-marketplace
/plugin install align@align-marketplace
```

Then, in a repository your team works on, run `/align:init`. It signs you in through your browser, shows what is
captured and what never is, and asks you to opt the repository in. Nothing is captured anywhere until you do.
`/align:align` shows the status and the next step. You need Node.js 22.14 or later and git.

## What the plugin runs

- Four hooks (session start, prompt, stop, session end) call `scripts/align`, which runs the ALIGN CLI installed in
  `~/.align` (or, before the first `/align:init`, the copy of the CLI in `lib/align.mjs`, the same file as the npm
  package `@align-agent/cli`). In a repository you have not opted in, every hook exits at once without reading,
  writing or sending anything.
- An MCP server (`align mcp`) with seven tools: get the context pack, get, search and record decisions, list open
  questions, read a contract, and ask a decision's owner.
- Two commands: `/align:init` and `/align:align`.

## What is captured, and what never is

In an opted-in repository, ALIGN captures your prompts and Claude's replies (with secrets removed and code blocks
replaced, on your machine), the file paths Claude touched in that repository, your commits and pull requests, and
the decisions you confirm. It never captures file contents, tool outputs, the commands your tools run, diffs from
your machine, `.env` files, other repositories, hostnames or usernames: tool calls become placeholders such as
`‹tool: Edit, 1200 bytes›`. Redaction runs again on the server. Paths you list in `.aligndeny` are never read.
Proposals from your sessions are visible only to you until you confirm or share them; your raw transcripts only
ever to you. There are no per-developer metrics anywhere in ALIGN.

## Network destinations

- The ALIGN server for your account, `https://align.bharatreachdsp.com` (or the server you signed in to with
  `--server`): sign-in, uploads of redacted session events, the context pack and the MCP tools.
- Your browser opens the same server's sign-in page during `/align:init`.
- `https://registry.npmjs.org` only when you install the CLI from npm yourself (the plugin itself updates through
  Claude Code's plugin manager).

Nothing else. The plugin sends no telemetry of its own.

## Turn it off or uninstall

- Stop capture in one repository: `align off` (or run `align on` to resume).
- Delete a session you already sent: `align sessions rm <id>`.
- Remove the plugin: `/plugin uninstall align@align-marketplace`.
- Remove everything ALIGN added on this machine: `align uninstall --purge --yes` (it deletes `~/.align`).

Source: the CLI's source is public under the MIT licence at https://github.com/bharatreachdsp/align-cli. Security
reports: see SECURITY.md there.
