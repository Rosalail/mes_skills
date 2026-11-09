# mes_skills

Marketplace de plugins Claude personnelle. Ajoutée une fois au compte Claude, elle rend ses skills disponibles dans toutes les sessions : Claude Code, chat, mobile et Cowork.

## Installation

Sur claude.ai ou dans l'application de bureau :

1. **Customize > Plugins > Add > Add marketplace**, puis saisir `rosalail/mes_skills`.
2. Ajouter le plugin **caveman**.
3. Activer **Settings > Capabilities > Code execution and file creation**.

En ligne de commande, Claude Code :

```bash
/plugin install caveman --marketplace rosalail/mes_skills
```

## Plugin `caveman`

Contient 22 skills, dans `plugins/caveman/skills/`, copiés depuis [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) (licence Apache-2.0, voir `plugins/caveman/LICENSE`). Seuls les skills sont repris, pas les hooks : le mode caveman ne démarre donc pas tout seul. Tapez `/caveman` pour l'activer.

Modification locale : `caveman-setup` et `caveman-discover` font passer le trafic API (requêtes et clés) par le service tiers `gateway.caveman.so`. Leur description et une section en tête de leur `SKILL.md` obligent l'agent à demander une validation explicite avant tout envoi par ce service.

## Ajouter un skill

Placer le dossier du skill (avec son `SKILL.md`) dans `plugins/caveman/skills/`, ou créer un nouveau plugin dans `plugins/` et le déclarer dans `.claude-plugin/marketplace.json`.
