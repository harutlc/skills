---
name: herdr-pr-review
description: >-
  Spawns Claude Code in a Herdr split pane to run /code-review xhigh on an existing PR, post findings directly on the PR, and send the review summary back to the lead agent to fix valid issues. Use when a feature is already implemented and ready for PR review.
pattern: reviewer
category: verification
---
# Herdr PR Review (Claude Code `/code-review xhigh`)

Use this workflow when the feature is already implemented and you want Claude Code to review the PR.

## Prerequisites
Read `~/.config/herdr-app/AGENTS.md` and run `hc --skill` so you have the exact Herdr CLI commands (`pane.split`, `send`, `keys`) and know your own pane ID.

## Workflow

1. **Locate the PR**
   - Use the PR number or URL provided by the user.
   - If none was provided, detect the current branch's PR with `gh pr view --json url -q .url`. (If no PR exists yet, push the current branch and create one with `gh pr create`.)

2. **Spawn Claude Code in a Split Pane**
   - Split a pane to the right using Herdr CLI.
   - Launch Claude Code in read-only plan mode:
     ```bash
     claude --permission-mode plan --allowedTools "Bash(gh pr:*),Bash(git diff:*),Bash(git status:*),Bash(hc send:*)"
     ```

3. **Run `/code-review xhigh`**
   - Send this prompt to the Claude Code pane (replacing `<PR-NUMBER-OR-URL>` and `<YOUR-PANE-ID>`):
     ```text
     /code-review xhigh <PR-NUMBER-OR-URL> Review this PR and post your review comments directly on the PR. Do not edit any local files. When finished, also send your review summary back to my pane using `hc send <YOUR-PANE-ID> "<summary>"`.
     ```

4. **Fix Valid Findings**
   - Wait for Claude Code's `hc send` message to arrive in your pane.
   - Fix all valid issues it reported, commit and push the fixes to the PR branch, and summarize what you changed.
