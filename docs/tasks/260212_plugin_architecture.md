# Architecture des plugins

## Objectif

Revoir l'architecture du package `claude-plugins` pour passer de 2 plugins (general-skills, php-skills) à 3 plugins thématiques, permettant aux utilisateurs d'installer uniquement les skills qui les intéressent.

## Contexte

L'architecture actuelle regroupe tous les skills non-PHP dans un seul plugin `general-skills` (8 skills). Ce plugin mélange des skills très différents :

- **Audits / Reviews** : ecodesign, gdpr, seo, accessibility
- **Git / Workflow** : commitor, changelogator, (futur releasor)
- **Outils** : documentator, skill-creator

Un utilisateur qui veut uniquement les audits doit installer tous les skills git et docs aussi. Inversement, quelqu'un qui veut juste `commitor` récupère 4 skills d'audit qu'il n'utilisera pas.

## Proposition : 3 plugins

### Option A : reviews / git / php

| Plugin          | Skills                                                          | Description                          |
| --------------- | --------------------------------------------------------------- | ------------------------------------ |
| **review-skills** | ecodesign-review, gdpr-review, seo-review, accessibility-review | Audits qualité web                   |
| **git-skills**    | commitor, changelogator, releasor, documentator, skill-creator  | Workflow git et documentation        |
| **php-skills**    | php-security-review, php-performance-review                     | Audits PHP / Laravel / Kirby         |

**Avantage** : séparation claire entre "auditer" et "produire".
**Inconvénient** : `documentator` et `skill-creator` ne sont pas vraiment liés à Git.

### Option B : reviews / docs / php

| Plugin          | Skills                                                          | Description                          |
| --------------- | --------------------------------------------------------------- | ------------------------------------ |
| **review-skills** | ecodesign-review, gdpr-review, seo-review, accessibility-review | Audits qualité web                   |
| **docs-skills**   | commitor, changelogator, releasor, documentator, skill-creator  | Documentation et workflow            |
| **php-skills**    | php-security-review, php-performance-review                     | Audits PHP / Laravel / Kirby         |

**Avantage** : le nom "docs" englobe mieux documentator et skill-creator.
**Inconvénient** : commitor/changelogator/releasor sont plus "git" que "docs".

### Option C : reviews / workflow / php

| Plugin            | Skills                                                          | Description                          |
| ----------------- | --------------------------------------------------------------- | ------------------------------------ |
| **review-skills** | ecodesign-review, gdpr-review, seo-review, accessibility-review | Audits qualité web                   |
| **workflow-skills**| commitor, changelogator, releasor, documentator, skill-creator  | Workflow de développement            |
| **php-skills**    | php-security-review, php-performance-review                     | Audits PHP / Laravel / Kirby         |

**Avantage** : "workflow" est assez large pour englober git, docs et création de skills.
**Inconvénient** : nom plus vague.

## Impact technique

### Fichiers à modifier

- `.claude-plugin/marketplace.json` : éclater `general-skills` en 2 plugins distincts
- `README.md` : restructurer les sections (table, installation, structure)
- `CLAUDE.md` : mettre à jour la structure décrite
- Potentiellement déplacer les dossiers de skills si on change l'arborescence (`skills/general/` → `skills/review/` + `skills/workflow/`)

### Questions ouvertes

1. **Nommage du 2e plugin** : git-skills, docs-skills, ou workflow-skills ?
2. **Arborescence physique** : garder tout dans `skills/general/` avec deux plugins pointant dessus, ou séparer physiquement ?
3. **Rétrocompatibilité** : les utilisateurs existants de `general-skills@benjaminhaeberli` devront-ils réinstaller ?
4. **php-skills avec reviews** : faut-il fusionner les audits PHP dans `review-skills` (avec un tag "php") ou garder le plugin PHP séparé ?

## Critères de validation

- [ ] Les utilisateurs peuvent installer chaque plugin indépendamment
- [ ] L'installation de tous les plugins donne le même résultat qu'avant
- [ ] Le marketplace.json est valide et chaque plugin a sa propre entrée
- [ ] Le README reflète la nouvelle structure
- [ ] Le CLAUDE.md est à jour

## Hors-scope

- Migration automatique depuis l'ancienne architecture
- Système de dépendances entre plugins (ex: releasor dépend de changelogator)
- Versioning indépendant par plugin

---

**Dernière mise à jour** : 2026-02-12
**Généré avec [Claude Code](https://claude.com/claude-code)**
