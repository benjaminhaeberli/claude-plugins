# Checklist Accessibilite Web (WCAG 2.1/2.2 AA)

Based on WCAG 2.1/2.2 level AA success criteria, RGAA 4.1, and Swiss eCH-0059. Focused on what a web developer can verify and fix in code (HTML/CSS/JS/PHP).

---

## 1. Images & medias alternatifs (6 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 1 | Alt text sur toutes les images informatives | CRITIQUE | Chaque `<img>` informative a un `alt` descriptif du contenu. Pas d'alt generique ("image", "photo", "img_001.jpg"). | 1.1.1 |
| 2 | Images decoratives marquees comme telles | HAUTE | Images purement decoratives : `alt=""` ou CSS `background-image`. Pas d'alt sur les images sans valeur informative. | 1.1.1 |
| 3 | SVG accessibles | HAUTE | SVG informatives : `role="img"` + `aria-label` ou `<title>`. SVG decoratives : `aria-hidden="true"`. | 1.1.1 |
| 4 | Videos avec sous-titres | HAUTE | Chaque `<video>` a des sous-titres (`<track kind="captions">`). Sous-titres synchronises et complets. | 1.2.2 |
| 5 | Alternative textuelle pour les medias audio/video | MOYENNE | Transcription textuelle disponible pour les contenus audio et video. | 1.2.1 |
| 6 | Pas de media en lecture automatique | HAUTE | Pas d'`autoplay` avec son. Si autoplay video sans son : controles visibles pour arreter. Duree < 5s ou bouton stop. | 1.4.2 |

## 2. Structure semantique & titres (6 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 7 | Hierarchie de titres logique | CRITIQUE | Un seul `<h1>` par page. Niveaux H1-H6 sans saut (pas de H1 > H3). Structure coherente et descendante. | 1.3.1 |
| 8 | Landmarks ARIA / HTML5 | HAUTE | Utilisation de `<header>`, `<nav>`, `<main>`, `<footer>`, `<aside>`. `<main>` unique par page. Si plusieurs `<nav>`, chacun a un `aria-label` distinct. | 1.3.1 |
| 9 | Listes semantiques | MOYENNE | Listes d'elements en `<ul>`, `<ol>`, ou `<dl>`. Menus de navigation dans un `<ul>` au sein d'un `<nav>`. | 1.3.1 |
| 10 | Tableaux de donnees accessibles | HAUTE | `<th>` avec `scope="col"` ou `scope="row"`. `<caption>` descriptif. Pas de tableau pour la mise en page. | 1.3.1 |
| 11 | Ordre de lecture logique | HAUTE | L'ordre du DOM correspond a l'ordre visuel. Pas de `tabindex` positif. Pas de reorganisation CSS qui casse l'ordre de lecture. | 1.3.2 |
| 12 | Attribut lang sur la page et les changements de langue | HAUTE | `<html lang="fr">` (ou langue appropriee). Changements de langue inline : `<span lang="en">`. | 3.1.1, 3.1.2 |

## 3. Navigation & clavier (7 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 13 | Navigation complete au clavier | CRITIQUE | Tous les elements interactifs accessibles via Tab/Shift+Tab. Actions declenchables avec Enter/Space. Pas d'element accessible seulement a la souris. | 2.1.1 |
| 14 | Focus visible sur tous les elements interactifs | CRITIQUE | Outline visible sur `:focus` ou `:focus-visible`. Pas de `outline: none` sans alternative. Contraste suffisant du focus indicator. | 2.4.7 |
| 15 | Skip link vers le contenu principal | HAUTE | Premier lien focusable : "Aller au contenu principal" (ou equivalent). Visible au focus, cible `<main>` ou `#content`. | 2.4.1 |
| 16 | Pas de piege au clavier | CRITIQUE | L'utilisateur peut toujours sortir d'un composant avec Tab ou Escape. Les modales restituent le focus a l'element declencheur a la fermeture. | 2.1.2 |
| 17 | Ordre de tabulation logique | HAUTE | L'ordre de focus suit l'ordre visuel et logique. Pas de `tabindex > 0`. Les elements masques (`display:none`, `hidden`) ne recoivent pas le focus. | 2.4.3 |
| 18 | Navigation coherente entre les pages | MOYENNE | Menu principal dans le meme ordre sur toutes les pages. Elements recurrents a la meme position. | 3.2.3 |
| 19 | Plusieurs moyens de navigation | MOYENNE | Au moins 2 moyens : menu + recherche, ou menu + plan du site, ou menu + fil d'Ariane. | 2.4.5 |

## 4. Formulaires (7 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 20 | Label associe a chaque champ | CRITIQUE | Chaque `<input>`, `<select>`, `<textarea>` a un `<label for="id">` ou un `aria-label`/`aria-labelledby`. Pas de placeholder comme seul label. | 1.3.1, 4.1.2 |
| 21 | Messages d'erreur explicites et lies au champ | HAUTE | Erreur identifiee en texte (pas seulement par couleur). `aria-describedby` vers le message d'erreur. `aria-invalid="true"` sur le champ en erreur. | 3.3.1, 3.3.3 |
| 22 | Champs obligatoires identifies | HAUTE | `required` ou `aria-required="true"` sur les champs obligatoires. Indication visuelle + textuelle (pas seulement un asterisque). | 3.3.2 |
| 23 | Autocomplete sur les champs personnels | MOYENNE | `autocomplete` avec les valeurs appropriees (`name`, `email`, `tel`, `address-line1`, etc.) sur les champs de donnees personnelles. | 1.3.5 |
| 24 | Groupement logique des champs | MOYENNE | `<fieldset>` + `<legend>` pour les groupes de radio buttons, checkboxes, ou sections de formulaire liees. | 1.3.1 |
| 25 | Instructions avant le formulaire | MOYENNE | Les formats attendus, contraintes, et etapes sont decrits avant ou au debut du formulaire. Pas uniquement dans le placeholder. | 3.3.2 |
| 26 | Confirmation avant actions irreversibles | MOYENNE | Suppression, envoi definitif : confirmation demandee. Possibilite d'annuler ou corriger avant soumission finale. | 3.3.4 |

## 5. Couleurs & contraste (5 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 27 | Contraste texte/fond suffisant | CRITIQUE | Ratio >= 4.5:1 pour le texte normal, >= 3:1 pour le texte agrandi (>= 18pt ou 14pt bold). Verifier les combinaisons de couleurs dans le CSS. | 1.4.3 |
| 28 | Contraste des composants UI et graphiques | HAUTE | Ratio >= 3:1 pour les bordures de champs, icones, boutons, et elements graphiques porteurs d'information. | 1.4.11 |
| 29 | Information non vehiculee par la couleur seule | CRITIQUE | Erreurs de formulaire, liens, etats : pas seulement par la couleur. Ajouter une icone, un soulignement, ou du texte. | 1.4.1 |
| 30 | Mode sombre / prefers-color-scheme | MOYENNE | Si mode sombre : les contrastes sont aussi respectes. Les images et icones restent visibles. | 1.4.3 |
| 31 | Texte redimensionnable a 200% | HAUTE | Le contenu reste lisible et fonctionnel quand le texte est agrandi a 200%. Pas de troncature, chevauchement, ou perte de contenu. Utiliser des unites relatives (rem, em). | 1.4.4 |

## 6. Composants interactifs & ARIA (6 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 32 | Boutons et liens correctement balises | CRITIQUE | Boutons : `<button>` (pas `<div onclick>`). Liens : `<a href>`. Chaque bouton/lien a un texte accessible (contenu textuel, `aria-label`, ou `sr-only`). | 4.1.2 |
| 33 | Modales/dialogs accessibles | HAUTE | `role="dialog"`, `aria-modal="true"`, `aria-labelledby` vers le titre. Focus piege dans la modale. Fermeture avec Escape. Restitution du focus a la fermeture. | 4.1.2 |
| 34 | Menus deroulants accessibles | HAUTE | Ouverture/fermeture au clavier. `aria-expanded` sur le declencheur. `aria-haspopup` si menu. Focus gere dans le menu ouvert. Escape pour fermer. | 4.1.2 |
| 35 | Onglets (tabs) accessibles | MOYENNE | `role="tablist"`, `role="tab"`, `role="tabpanel"`. Navigation par fleches. `aria-selected` sur l'onglet actif. `aria-controls` / `aria-labelledby` lies. | 4.1.2 |
| 36 | Pas d'ARIA inutile ou incorrect | HAUTE | ARIA utilise seulement quand le HTML natif ne suffit pas. Pas de `role` sur des elements qui ont deja le role natif. Pas d'`aria-label` sur des `<div>` non interactifs. | 4.1.2 |
| 37 | Zones dynamiques (live regions) | MOYENNE | `aria-live="polite"` ou `aria-live="assertive"` pour les messages de statut, notifications, resultats de recherche dynamiques. `role="status"` ou `role="alert"` si applicable. | 4.1.3 |

## 7. Animations & mouvements (3 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 38 | Respect de prefers-reduced-motion | HAUTE | `@media (prefers-reduced-motion: reduce)` desactive ou reduit les animations. Transitions CSS et animations JS conditionnees. | 2.3.3 |
| 39 | Pas de contenu clignotant | CRITIQUE | Pas de clignotement > 3 fois par seconde. Pas de flash intense. Pas de GIF anime rapide. | 2.3.1 |
| 40 | Contenu en mouvement controlable | HAUTE | Carousels, defilements, animations automatiques : bouton pause visible. Auto-stop apres 5 secondes ou controle utilisateur. | 2.2.2 |

## 8. Documents & telechargements (3 pratiques)

| # | Pratique | Priorite | Quoi verifier | WCAG |
|---|----------|----------|---------------|------|
| 41 | Format et poids indiques sur les liens de telechargement | MOYENNE | Liens vers des fichiers : format et taille indiques. Ex: "Rapport annuel (PDF, 2.4 Mo)". Ouverture dans un nouvel onglet signalee. | 2.4.4 |
| 42 | Documents PDF accessibles | MOYENNE | PDFs generes avec une structure balisee (headings, alt text, langue). Ou alternative HTML disponible. | 1.1.1, 1.3.1 |
| 43 | Liens explicites avec contexte | HAUTE | Chaque lien est comprehensible hors contexte ou via `aria-label`. Pas de "cliquez ici" sans contexte. Liens externes signales (`target="_blank"` + indication visuelle + `rel="noopener"`). | 2.4.4 |

---

## Grille de scoring

- **CRITIQUE** = Bloque l'acces pour des utilisateurs en situation de handicap (x5)
- **HAUTE** = Barriere significative (x4)
- **MOYENNE** = Amelioration importante mais impact partiel (x3)

**Calcul du score** : Chaque pratique OK multipliee par son poids. Les pratiques NA sont exclues du total.

```
Score = SUM(pratiques_OK * poids) / SUM(pratiques_applicables * poids) * 100
```

## References

- WCAG 2.1 : https://www.w3.org/TR/WCAG21/
- WCAG 2.2 : https://www.w3.org/TR/WCAG22/
- RGAA 4.1 : Referentiel general d'amelioration de l'accessibilite (France)
- eCH-0059 : Norme suisse d'accessibilite des sites web (WCAG 2.1 AA obligatoire pour les sites publics)
- EAA : European Accessibility Act (directive europeenne, applicable des 2025)
- WAI-ARIA 1.2 : https://www.w3.org/TR/wai-aria-1.2/
