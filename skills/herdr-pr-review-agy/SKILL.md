---
name: herdr-pr-review-agy
description: Spawns Antigravity CLI (agy) in a Herdr split pane to review an existing PR, post findings directly on the PR, and send the review summary back to the lead agent to fix valid issues. Use when a feature is already implemented and ready for PR review by agy.
---

# Herdr PR Review (`agy`)

Use this workflow when the feature is already implemented and you want `agy` to review the PR.

## Prerequisites
Read `~/.config/herdr-app/AGENTS.md` and run `hc --skill` so you have the exact Herdr CLI commands (`pane.split`, `send`, `keys`) and know your own pane ID.

## Workflow

1. **Locate the PR**
   - Use the PR number or URL provided by the user.
   - If none was provided, detect the current branch's PR with `gh pr view --json url -q .url`. (If no PR exists yet, push the current branch and create one with `gh pr create`.)

2. **Spawn `agy` in a Split Pane**
   - Split a pane to the right using Herdr CLI.
   - Launch `agy -i` in interactive mode (leave it in plan/read-only mode).
   - If `agy` prompts to trust the project folder, confirm it with `hc keys <REVIEWER-PANE-ID> enter`.

3. **Request PR Code Review**
   - Send this prompt to the reviewer `agy` pane (replacing `<PR-NUMBER-OR-URL>` and `<YOUR-PANE-ID>`):
     ```text
     Review PR <PR-NUMBER-OR-URL> using `gh pr diff` and `gh pr view`, and post your review comments directly on the PR using `gh pr review`. Do not edit any local files. When finished, also send your review summary back to my pane using `hc send <YOUR-PANE-ID> "<summary>"`.
     ```

4. **Fix Valid Findings**
   - Wait for the reviewer `agy`'s `hc send` message to arrive in your pane.
   - Fix all valid issues it reported, commit and push the fixes to the PR branch, and summarize what you changed.

