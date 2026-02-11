# Releasor

## Objectif

Créer un skill `releasor` qui automatise le processus de release : création du tag Git, génération du changelog (via `changelogator`), et publication de la release GitHub. Il complète la chaîne : `commitor` → `changelogator` → `releasor`.

## Contexte

Actuellement, le workflow de release est manuel :

1. `commitor` génère des commits bien formatés (`<emoji> <type>(<scope>): <description>`)
2. `changelogator` génère un changelog structuré à partir de ces commits
3. **Il manque** : la création du tag, la mise à jour du `CHANGELOG.md`, et la publication de la release GitHub

Le `releasor` orchestre ces étapes en un seul flux, tout en laissant l'utilisateur valider chaque étape.

## Étapes

### 1. Analyser l'état actuel

- Récupérer le dernier tag : `git tag --sort=-v:refname | head -1`
- Vérifier qu'il y a des commits depuis le dernier tag
- Afficher un résumé : nombre de commits, types présents, suggestion semver

### 2. Générer le changelog

- Utiliser la logique du `changelogator` pour parser et classer les commits
- Proposer le changelog formaté en chat pour validation
- Laisser l'utilisateur ajuster la version et le contenu si nécessaire

### 3. Mettre à jour CHANGELOG.md

- Prépendre la nouvelle entrée en haut du fichier `CHANGELOG.md` (après le titre)
- Créer le fichier s'il n'existe pas, avec un header standard Keep a Changelog
- Ne **pas** écrire sans validation explicite de l'utilisateur

### 4. Créer le tag Git

- Créer un tag annoté : `git tag -a vX.Y.Z -m "Release vX.Y.Z"`
- Le message du tag reprend le changelog généré
- Demander confirmation avant exécution

### 5. Publier la release GitHub (optionnel)

- Utiliser `gh release create vX.Y.Z --title "vX.Y.Z" --notes "<changelog>"`
- Ne proposer cette étape que si `gh` est disponible et que le remote est GitHub
- Demander confirmation explicite avant publication

## Workflow utilisateur attendu

```
User: "prepare release" / "release" / "publish version"
  → Releasor analyse les commits
  → Affiche le changelog + version suggérée
  → User valide ou ajuste
  → Releasor met à jour CHANGELOG.md
  → Crée le tag Git
  → (Optionnel) Publie la release GitHub
```

## Critères de validation

- [ ] Le skill orchestre changelog → tag → release en un flux cohérent
- [ ] Chaque étape destructive demande confirmation explicite
- [ ] Le changelog généré est cohérent avec `changelogator`
- [ ] Le tag respecte le format semver (`vX.Y.Z`)
- [ ] Le CHANGELOG.md est correctement mis à jour (prepend, pas overwrite)
- [ ] La release GitHub est optionnelle et conditionnée à la disponibilité de `gh`
- [ ] Le SKILL.md fait moins de 500 lignes
- [ ] Le skill est référencé dans `marketplace.json` et `README.md`

## Hors-scope

- Release multi-packages / monorepo
- Publication npm / Packagist / autres registres
- Gestion de branches de release (Git Flow)
- Génération de release notes différentes du changelog

---

**Dernière mise à jour** : 2026-02-12
**Généré avec [Claude Code](https://claude.com/claude-code)**
