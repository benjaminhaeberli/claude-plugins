# Changelogator

## Objectif

Créer un skill `changelogator` qui génère automatiquement un CHANGELOG.md structuré à partir des commits Git, en exploitant le format de commit défini par le skill `commitor`.

## Contexte

Le skill `commitor` produit des messages de commit au format `<emoji> <type>(<scope>): <description>` avec 12 types standardisés. Chaque type est mappé vers une catégorie [Keep a Changelog](https://keepachangelog.com/) :

| Catégorie Changelog | Types de commit                  |
| ------------------- | -------------------------------- |
| Added               | feat                             |
| Changed             | patch, style, perf, data         |
| Fixed               | fix                              |
| Removed             | remove                           |
| Technical           | docs, refactor, test, ai, config |

Le skill `changelogator` doit lire les commits depuis le dernier tag Git, les parser, les classer selon ce mapping, et proposer une entrée de changelog formatée.

## Étapes

### 1. Analyser les commits

- Lire les commits depuis le dernier tag Git (`git log <last-tag>..HEAD`)
- Parser chaque message pour extraire : emoji, type, scope, description, numéro de tâche
- Gérer les commits qui ne suivent pas le format (les regrouper dans une section "Other")

### 2. Classer par catégorie changelog

- Appliquer le mapping type → catégorie Keep a Changelog
- Regrouper les commits par catégorie
- Trier les catégories dans l'ordre standard : Added, Changed, Fixed, Removed, Technical

### 3. Suggérer la version semver

- Analyser les types présents :
  - `feat` présent → bump **minor**
  - Uniquement `fix`, `patch`, `style`, `perf`, `docs`, etc. → bump **patch**
  - Breaking change détecté (via `!` après le type ou mention dans le body) → bump **major**
- Afficher la suggestion de version mais laisser l'utilisateur décider

### 4. Générer la sortie

- Formater l'entrée changelog en Markdown
- Inclure la date du jour et le numéro de version suggéré
- Ne **pas** écrire directement dans le fichier CHANGELOG.md — afficher en chat pour validation

### 5. Format de sortie attendu

```markdown
## v[X.Y.Z] - YYYY-MM-DD

### ✨ Added

- Description du feat (TASK-XXX)

## 🛠 Changed

- Description du patch

## 🐛 Fixed

- Description du fix

### 🔥 Removed

- Description du remove

## 🏗️ Technical

- Description du technical
```

La section "Technical" est incluse uniquement si l'utilisateur le demande explicitement (pas dans un changelog public par défaut).

## Critères de validation

- [ ] Le skill parse correctement les commits au format `commitor`
- [ ] Les commits sont classés dans les bonnes catégories changelog
- [ ] La suggestion de version semver est cohérente
- [ ] Le skill gère les commits hors-format sans planter
- [ ] La sortie est un Markdown valide et bien formaté
- [ ] Le SKILL.md fait moins de 500 lignes
- [ ] Le skill est référencé dans `marketplace.json` et `README.md`

## Hors-scope

- Écriture automatique dans CHANGELOG.md (on affiche seulement)
- Gestion multi-tags (on ne traite que depuis le dernier tag)
- Intégration Notion (sera un skill séparé)

---

**Dernière mise à jour** : 2026-02-12
**Généré avec [Claude Code](https://claude.com/claude-code)**
