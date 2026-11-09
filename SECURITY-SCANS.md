# Scans de sécurité

Scan NVIDIA SkillSpector v2.12.0 (`--no-llm`) du 9 novembre 2026, complété par une lecture du code autour de chaque résultat HIGH ou CRITICAL. Verdict final : `APPROVE` (sûr), `CAUTION` (prudence, comportement sensible mais documenté et limité) ou `REJECT` (à refuser).

Le score du scanner seul ne fait pas le verdict : les scores élevés ci-dessous viennent surtout de faux positifs, détaillés pour chaque cas.

## Résumé

| Plugin | Pire score scanner | Verdict après revue | Raison principale |
|---|---:|---|---|
| `caveman` | 100 (`caveman-compress`) | `CAUTION` | `caveman-compress` envoie le texte du fichier au fournisseur de modèle (documenté). `caveman-setup` et `caveman-discover` passent par `gateway.caveman.so` (validation obligatoire ajoutée). Les 19 autres : 0 à 20. |
| `impeccable` | 100 | `CAUTION` | Hooks automatiques qui lancent un binaire téléchargé, vérifié par empreinte, non isolé, après chaque modification. |
| `taste-skill` | 100 | `CAUTION` | Ses scripts lancent un Chrome sans fenêtre et des scripts Node en local, pour les captures et vérifications. |
| `core-3d-animation` | 74 (`gsap-scrolltrigger`) | `APPROVE` | Faux positifs. Bug sans risque : `react-three-fiber/scripts/component_generator.py` a une erreur de syntaxe et ne s'exécute pas. |
| `animation-design` | 13 | `APPROVE` | Rien de notable. |
| `superpowers` (obra/superpowers @ `22f0b46`) | 100 (`writing-skills`) | `APPROVE` | Faux positifs : exemples de documentation. |
| `skill-inspector` | 0 | `APPROVE` | Consignes de revue en lecture seule. |
| `ponytail` (@ `96608e0`) | 62 (100 sur le dépôt entier) | `CAUTION`, accord donné par l'utilisatrice | Hooks Node à chaque message ; journal local des coûts activé par défaut ; webhook optionnel. |
| `grill-me` | 8 | `APPROVE` | Consignes d'interrogatoire, rien de notable. |

## Détail des résultats élevés

### `caveman-compress` (100, CAUTION)
- `TT3` `scripts/compress.py:746` : appel réseau. C'est la fonction annoncée du skill (envoi au modèle pour compression), et c'est écrit en tête de son `SKILL.md`.
- `PE3` (`.env`, `.netrc`, `~/.aws/credentials`…) : liste de fichiers sensibles que le script **refuse** de compresser. C'est une protection.
- `E2` `compress.py:586` : transmet l'environnement au processus `claude` qu'il lance. Normal pour appeler la CLI.

### `impeccable` (100, CAUTION)
- `AS1` `reference/hooks.md` : la documentation cite `~/.claude/settings.json`, où le hook peut s'installer. Le skill indique lui-même que le hook tourne sans isolation, avec le réseau et l'environnement de l'agent.
- `AR2`, `MP3` : expressions banales (« do not judge », « new identity ») dans des guides de design.
- `TM1` `scripts/live-browser.js:8361` : commentaire sur la touche Suppr.
- `AE1` : fichiers trop longs pour l'analyse complète.

### `taste-skill` (100, CAUTION)
- `P2` (19 fois) : commentaires HTML des gabarits (`assets/starter.html`), pas d'instructions cachées.
- `YR4`, `P6`, `AE1` : texte de maquette et fichiers trop longs pour l'analyse.
- Scripts : `child_process` sert à lancer Chrome sans fenêtre et ses propres scripts. Réseau : polices Google, jsDelivr, images d'exemple.

### `core-3d-animation` (74, APPROVE)
- `AE1` « texte obfusqué » dans `gsap-scrolltrigger/references/easing_guide.md` : courbes d'easing dessinées avec des caractères `╱╲─`. Aucun caractère invisible ni contenu encodé.
- Les scripts ne font qu'écrire le code généré dans un fichier.

### `superpowers` (100, APPROVE)
- `writing-skills` : `RA1` sur l'expression « Write skill » ; `AS1` sur des exemples `ls ~/.claude/skills/`.
- `systematic-debugging` : `PE3` sur un exemple de diagnostic de signature macOS (`security list-keychains`) ; `E4` sur « Capture context » dans un guide de débogage.
- `brainstorming` : `TM1` sur `rm -f` de son propre fichier d'état de serveur local.
- `using-superpowers` : `AS1` sur la documentation de `~/.codex/config.toml`.

### `ponytail` (62 sur la partie installée, CAUTION)
- Le scan du dépôt entier donne 100, presque uniquement à cause de `benchmarks/` et `tests/`, qui ne sont pas copiés.
- `PE3` `plugin.json:31` : le jeton du webhook est rangé dans le trousseau système, jamais dans un fichier. C'est une protection.
- `OH1` `hooks/band/metrics.ts:93` : `re.exec(output)` est une recherche de texte, pas une exécution.
- À connaître : hooks Node au démarrage, à chaque message et pour chaque sous-agent ; journal local des coûts dans `~/.config/ponytail/band/` activé par défaut ; `/ponytail band import` lit les anciennes conversations Claude pour en tirer les chiffres ; webhook https optionnel, vide par défaut. Aucun envoi caché trouvé.

### Note sur `superpowers`
Ce plugin pointe vers le dépôt de son auteur : une nouvelle version n'est pas scannée automatiquement. Relancer le scan en cas de doute.
