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

## Plugin `impeccable`

Copié depuis [pbakaus/impeccable](https://github.com/pbakaus/impeccable), version 4.8.1, licence Apache-2.0. Ce plugin sert au design frontend : `/impeccable shape`, `audit`, `critique`, `polish`, `animate`, etc. Il inclut les agents et les hooks d'origine, qui lancent un contrôle de design au démarrage, après chaque `Edit`/`Write` et à la fin d'une réponse. Au premier usage, son lanceur télécharge un binaire et vérifie son empreinte avec `engine-checksums.txt`.

## Plugin `taste-skill`

Copié depuis [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill), version 2.5.1, licence MIT. Le skill `design-taste-frontend` sert à construire des sites web professionnels, vérifiés dans un vrai navigateur. Il pèse environ 40 Mo, à cause des images d'exemple.

## Plugin `animation-design`

Rassemble 14 skills d'animation et de design web : `60fps-animation`, `accessible-animation`, `ascii-animation`, `awwwards`, `frontend-design`, `glassmorphism`, `gsap-web`, `lottie-animation`, `micro-interaction`, `page-transition-animation`, `particle-system`, `shader-glsl`, `svg-animation` et `threejs-animation`. Les sources et licences sont dans `plugins/animation-design/SOURCES.md`.

## Plugin `core-3d-animation`

Copié depuis [freshtechbro/claudedesignskills](https://github.com/freshtechbro/claudedesignskills), licence MIT. Couvre Three.js, GSAP ScrollTrigger, React Three Fiber, Framer Motion et Babylon.js, avec des commandes et des agents.

## Plugin `superpowers`

Pas de copie ici : la marketplace pointe directement vers [obra/superpowers](https://github.com/obra/superpowers), qui reste ainsi toujours à jour. Couvre la planification, le débogage, le TDD et la revue de code.

## Ajouter un skill

Placer le dossier du skill (avec son `SKILL.md`) dans `plugins/caveman/skills/`, ou créer un nouveau plugin dans `plugins/` et le déclarer dans `.claude-plugin/marketplace.json`.
