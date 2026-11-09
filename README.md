# mes_skills

Skills Claude Code, rangés dans `.claude/skills/`. Ils se chargent automatiquement dans toute session Claude Code ouverte sur ce dépôt.

## Caveman

22 skills copiés depuis [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) (licence Apache-2.0, voir `.claude/skills/CAVEMAN-LICENSE`).

Modification locale : `caveman-setup` et `caveman-discover` font passer le trafic API (requêtes et clés) par le service tiers `gateway.caveman.so`. Leur description et une section en tête de leur `SKILL.md` obligent l'agent à demander une validation explicite avant tout envoi par ce service.
