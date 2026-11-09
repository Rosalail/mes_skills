---
name: install-skill-gate
description: Mandatory gate before installing, adding or updating any skill, plugin, agent or marketplace, in any session or repository (including rosalail/mes_skills). Scan with NVIDIA SkillSpector, give the verdict, and wait for approval before installing or committing.
---

# Install skill gate

The user requires that every skill, plugin, agent or command is scanned with NVIDIA SkillSpector before it is installed or added anywhere. This rule has no exception.

## Steps

1. Get the source into a new, empty temporary directory, outside any repository. Never run its scripts, its installer, or any one-line install command it suggests.
2. Make sure the scanner exists:

   ```bash
   command -v skillspector || uv tool install git+https://github.com/NVIDIA/skillspector.git
   ```

   If it still cannot run, stop. Tell the user, and do not install anything until they decide.
3. Scan every skill or plugin folder you plan to install:

   ```bash
   skillspector scan "<folder>" --no-llm --format json --output "<report>.json"
   ```

   Never scan test fixtures as if they were the product, and never install them.
4. Review the result with the `skill-inspector` skill: read the code around every HIGH or CRITICAL finding, and around MEDIUM findings that touch network, credentials, files, shell or persistence.
5. Report to the user, in their language, before installing: verdict (`APPROVE`, `CAUTION` or `REJECT`), score, and key findings.
   - `REJECT`, or score 51 or more: do not install. Install only if the user explicitly insists after the explanation.
   - `CAUTION`: install only after explicit user approval.
   - `APPROVE`: install, and include the result in your report.
6. When adding to `rosalail/mes_skills`, also follow that repository's `CLAUDE.md`, and put the score and verdict in the commit message.
