---
name: web-design-guidelines
description: Review UI code for Web Interface Guidelines compliance. Use when asked to "review my UI", "check accessibility", "audit design", "review UX", or "check my site against best practices".
metadata:
  author: vercel
  version: "1.0.0"
  argument-hint: <file-or-pattern>
---

# Web Interface Guidelines

Review files for compliance with Web Interface Guidelines.

## How It Works

1. Read the guidelines from `rules.md`, in this skill's folder
2. Read the specified files (or prompt user for files/pattern)
3. Check against all rules in the guidelines
4. Output findings in the terse `file:line` format

## Guidelines Source

Use only the local copy in `rules.md`. Do not fetch guidelines from the network: the user requires every instruction this skill follows to be security-scanned first. `rules.md` is a frozen snapshot of `vercel-labs/web-interface-guidelines` (`command.md`, commit 434b7f9), scanned with NVIDIA SkillSpector (0/100, SAFE).

## Usage

When a user provides a file or pattern argument:
1. Read `rules.md`
2. Read the specified files
3. Apply all rules from the guidelines
4. Output findings using the format specified in the guidelines

If no files specified, ask the user which files to review.
