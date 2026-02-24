# Releasor

## Objectif

Créer un skill `releasor` qui automatise le processus de release : création du tag Git, génération du changelog (via `changelogator`), et publication de la release GitHub. Il complète la chaîne : `commitor` → `changelogator` → `releasor`.

## Contexte

Actuellement, le workflow de release est manuel :

1. `commitor` génère des commits bien formatés (`<emoji> <type>(<scope>): <description>`)
2. `changelogator` génère un changelog structuré à partir de ces commits
3. **Il manque** : la création du tag, la mise à jour du `CHANGELOG.md`, et la publication de la release GitHub

Le `releasor` orchestre ces étapes en un seul flux, tout en laissant l'utilisateur valider chaque étape.

IMPORTANT : Par défaut, ne pas push et créer le tag seulement en local, et demander confirmation explicite à l'utilisateur pour qu'il valide le tout avant de passer à l'étape 6 (push + publication).

## Étapes

### 1. Analyser l'état actuel

- Récupérer le dernier tag : `git tag --sort=-v:refname | head -1`
- Vérifier qu'il y a des commits depuis le dernier tag
- Afficher un résumé : nombre de commits, types présents, suggestion semver

### 2. Générer le changelog

- Utiliser la logique du `changelogator` pour parser et classer les commits
- **Enrichissement optionnel (si `gh` disponible)** : récupérer les PRs associées aux commits via `gh pr list --search "SHA"` ou l'API, et ajouter les numéros de PR (`#123`) + liens contributeurs dans le changelog
- Proposer le changelog formaté en chat pour validation
- Laisser l'utilisateur ajuster la version et le contenu si nécessaire

### 3. Mettre à jour CHANGELOG.md

- Prépendre la nouvelle entrée en haut du fichier `CHANGELOG.md` (après le titre)
- Créer le fichier s'il n'existe pas, avec un header standard Keep a Changelog
- Ne **pas** écrire sans validation explicite de l'utilisateur

### 4. Committer et créer le tag Git

- Committer le `CHANGELOG.md` avec `commitor` (le commit doit précéder le tag)
- Créer un tag annoté sur ce commit : `git tag -a vX.Y.Z -m "Release vX.Y.Z"`
- Le message du tag reprend le changelog généré
- Demander confirmation avant exécution

### 5. Afficher les release notes

- Toujours générer et afficher les release notes complètes en chat (changelog + contributeurs si `gh` disponible)
- Les notes sont prêtes à copier-coller, quel que soit le contexte
- L'utilisateur valide le tout avant le push

### 6. Push et publication

- Push le commit et le tag
- Si `gh` est disponible et que le remote est GitHub : proposer la publication automatique via `gh release create vX.Y.Z --title "vX.Y.Z" --notes "<release notes>"`
- Demander confirmation explicite avant chaque action

## Workflow utilisateur attendu

```
User: "prepare release" / "release" / "publish version"
  → Releasor analyse les commits
  → Affiche le changelog + version suggérée
  → User valide ou ajuste
  → Releasor met à jour CHANGELOG.md
  → Commit (via commitor) + crée le tag Git
  → Affiche les release notes complètes (prêtes à copier-coller)
  → User valide avant push
  → Push commit + tag
  → (Optionnel, si gh) Publie la release GitHub
```

## Enrichissement GitHub (optionnel, pas de dépendance dure)

Le `releasor` génère **toujours** des release notes complètes, prêtes à être utilisées — que `gh` soit disponible ou non. La différence :

| | Sans `gh` | Avec `gh` |
|---|---|---|
| **Changelog** | Basé sur les commits Git | Enrichi avec numéros de PR (`#123`) |
| **Contributeurs** | Non inclus | Section `## Contributors` avec `@username` |
| **Release notes** | Affichées en chat, prêtes à copier-coller | Affichées en chat + publication automatique possible |
| **Publication** | Manuelle (copier dans GitHub UI) | Via `gh release create` |

- **PRs associées** : résolution commit → PR via `gh pr list --search "SHA"` ou `gh api`, ajout du `(#123)` à côté de chaque entrée
- **Contributeurs** : section `## Contributors` en bas des release notes, avec `@username` et lien profil — utile surtout pour les projets avec contributions externes
- **Sans `gh`** : les release notes sont quand même générées et affichées, l'utilisateur peut les copier-coller dans l'UI GitHub ou tout autre outil

> **Note** : l'enrichissement contributeurs n'a pas vocation à remonter dans le `changelogator` lui-même. Le changelog pur reste basé sur les commits Git. C'est le `releasor`, au moment de préparer les release notes, qui ajoute cette couche sociale.

## Critères de validation

- [ ] Le skill orchestre changelog → tag → release en un flux cohérent
- [ ] Chaque étape destructive demande confirmation explicite
- [ ] Le changelog généré est cohérent avec `changelogator`
- [ ] Le tag respecte le format semver (`vX.Y.Z`)
- [ ] Le CHANGELOG.md est correctement mis à jour (prepend, pas overwrite)
- [ ] La release GitHub est optionnelle et conditionnée à la disponibilité de `gh`
- [ ] Si `gh` est disponible, les PRs et contributeurs sont inclus dans les release notes
- [ ] Sans `gh`, les release notes sont générées et affichées (prêtes à copier-coller)
- [ ] Avec `gh`, les notes sont enrichies (PRs, contributeurs) et publication proposée
- [ ] Le SKILL.md fait moins de 500 lignes
- [ ] Le skill est référencé dans `marketplace.json` et `README.md`

## Hors-scope

- Release multi-packages / monorepo
- Publication npm / Packagist / autres registres
- Gestion de branches de release (Git Flow)
- Génération de release notes avec un format/contenu totalement distinct du changelog (les release notes sont le changelog + enrichissement optionnel)

---

**Dernière mise à jour** : 2026-02-17
**Généré avec [Claude Code](https://claude.com/claude-code)**
