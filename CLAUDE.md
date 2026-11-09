# Règles du dépôt mes_skills

Ce dépôt est la marketplace de plugins personnelle de rosalail. Tout skill, plugin, agent ou commande ajouté ici arrive dans toutes ses sessions Claude.

## Règle obligatoire : scan SkillSpector avant tout ajout

Avant d'ajouter ou de mettre à jour un skill, un plugin, un agent ou une commande dans ce dépôt, tu DOIS :

1. Récupérer la source dans un dossier temporaire, hors de ce dépôt. Ne jamais exécuter ses scripts ni son installeur.
2. Lancer le scan sur chaque dossier de skill ou de plugin à ajouter :

   ```bash
   skillspector scan "<dossier>" --no-llm --format json --output "<rapport>.json"
   ```

   Si `skillspector` est absent, l'installer avec `uv tool install git+https://github.com/NVIDIA/skillspector.git`. S'il ne peut pas tourner, NE PAS ajouter le skill : prévenir l'utilisatrice et attendre sa décision.
3. Lire le code autour de chaque problème HIGH ou CRITICAL, et des MEDIUM qui touchent au réseau, aux clés, aux fichiers, au shell ou à la persistance. Suivre le skill `skill-inspector` pour la revue.
4. Donner à l'utilisatrice le verdict (`APPROVE`, `CAUTION` ou `REJECT`), le score et les points importants, AVANT le commit.
   - `REJECT`, ou un score de 51 ou plus : ne pas ajouter, sauf accord explicite de l'utilisatrice après explication.
   - `CAUTION` : ajouter seulement après son accord explicite.
   - `APPROVE` : ajouter, en indiquant le résultat dans le compte rendu.
5. Mentionner le score et le verdict dans le message de commit.

Un plugin référencé par lien externe dans `.claude-plugin/marketplace.json` (comme `superpowers`) se scanne aussi : cloner sa source dans un dossier temporaire avant de l'ajouter.

## Autres règles

- Ne jamais copier les dossiers de tests ou d'exemples d'un dépôt source.
- Garder la licence d'origine de chaque plugin copié et citer sa source dans le `README.md`.
- Lancer `claude plugin validate .` avant chaque push.
- Les skills `caveman-setup` et `caveman-discover` font passer le trafic API par `gateway.caveman.so` : ne jamais retirer leur avertissement ni leur demande de validation.
