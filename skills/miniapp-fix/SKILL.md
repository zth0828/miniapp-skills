---
name: miniapp-fix
description: Fix WeChat Mini Program build, preview, or DevTools problems. Use when the user reports compile errors, preview failures, DevTools import issues, template residue, stale compile conditions, TypeScript recognition drift, or any repo-scoped problem surfaced by the official DevTools CLI.
tools:
  - Bash
  - Read
  - Edit
  - Glob
  - Grep
  - Write
slash_command: /miniapp-fix
---

# Miniapp Fix

## Overview

Use this skill when a miniapp fails to compile, preview, or behave correctly inside WeChat DevTools. Treat the official DevTools CLI as the primary evidence source, but do not stop at CLI-only failures when the repo itself is polluted or misconfigured.

## Quick Start

1. Read `references/fix-playbook.md`.
2. Inspect `git status`, repository root, `project.config.json`, `project.private.config.json`, and `app.json`.
3. Confirm the DevTools CLI is available and can print help.
4. Try `open` to establish IDE connectivity and the live service port.
5. Try `preview` as the primary compile check.
6. Classify the result:
   - CLI-visible and repo-scoped → fix the repo
   - CLI-visible but host/account blocker → tell the user what to change on the host
   - repo polluted by DevTools → restore tracked files, then delete residue
   - GUI/runtime only → stop the CLI loop and ask for runtime evidence

## Core Rules

- Prefer the official CLI over screenshots for first-pass diagnosis.
- Auto-fix only repo-scoped problems: config drift, page-path mismatch, generated residue, small syntax issues with exact locations.
- Restore tracked files from version control before deleting DevTools-generated clutter.
- Treat `project.private.config.json` as local-only state, not shared truth.
- If `preview` is green but runtime still fails, stop grinding CLI and ask for GUI/runtime evidence.
- Do not auto-fix changes that alter app strategy, publish state, backend settings, or broad syntax without explicit direction.

## Output Format

Keep the answer operational:

1. what was observed (CLI output, repo state, DevTools changes)
2. which class the problem falls into
3. what was fixed or what further evidence is needed
4. the next command or user action

## Resources

- `references/fix-playbook.md`: CLI command ladder, timeout handling, pollution cleanup, and fix boundaries
- `references/example-prompts.md`: reusable trigger examples and evaluation notes
