# Checklist Eco-design Web

Curated from GreenIT "115 bonnes pratiques" (v4/v5). Adapted for web developers (HTML/CSS/JS/PHP) in 2026. Outdated, infra-only, and micro-optimization practices removed.

---

## 1. Conception & Fonctionnel (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 1 | Eliminer les fonctionnalites non essentielles | P5 | Chaque fonctionnalite a une utilite validee. Pas de feature inutilisee ou gadget. |
| 2 | Quantifier precisement le besoin | P5 | Les dimensions (pagination, limites, quotas) correspondent au besoin reel, pas plus. |
| 3 | Supprimer les fonctionnalites non utilisees | P4 | Pas de code mort, routes orphelines, ou pages inaccessibles en production. |
| 4 | Mobile first | P5 | Conception responsive partant du mobile. Media queries en `min-width`. |
| 5 | Optimiser le parcours utilisateur | P5 | Parcours courts, sans friction inutile, avec un minimum d'etapes. |
| 6 | Pagination plutot que scroll infini | P4 | Les listes longues utilisent une pagination classique, pas d'infinite scroll. |

## 2. UI & Design (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 7 | Design simple et adapte au web | P4 | Interface epuree, pas de complexite visuelle inutile. |
| 8 | Limiter les carrousels | P5 | Maximum 1 carrousel par page. Preferer du contenu statique. |
| 9 | Eviter les animations JS/CSS superflues | P4 | Maximum 2 animations par page. Pas d'animation purement decorative. Respecter `prefers-reduced-motion`. |
| 10 | Favoriser les polices standards | P4 | Maximum 2 polices custom telechargees. Utiliser `font-display: swap`. Preferer les system fonts. |

## 3. Images & Medias (8 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 11 | Dimensionner les images correctement | P5 | Les images sont servies a la taille d'affichage. Utiliser `srcset` et `sizes`. Pas d'image 4000px affichee en 400px. |
| 12 | Optimiser les images | P4 | Format moderne (WebP/AVIF). Compression appliquee. Pas d'image > 200Ko pour du contenu web standard. |
| 13 | Preferer SVG et glyphes aux images bitmap | P4 | Icones et elements d'interface en SVG, icon font, ou CSS plutot qu'en PNG/JPG. |
| 14 | Optimiser les SVG | P5 | SVG nettoyees (SVGO ou equivalent). Pas de metadonnees inutiles, pas de calques caches. |
| 15 | Lazy loading sur images, iframes et videos | P5 | Attribut `loading="lazy"` sur les elements sous la ligne de flottaison. |
| 16 | Pas de lecture/chargement automatique des medias | P4 | Pas d'attribut `autoplay` sur `<video>` ou `<audio>`. `preload="none"` sur les medias non critiques. |
| 17 | Limiter les GIFs animes | P3 | Pas de GIF anime. Preferer une video courte en WebM/MP4 ou une animation CSS legere. |
| 18 | Adapter la qualite des videos au contexte | P3 | Pas de video 1080p+ sur mobile. Proposer plusieurs resolutions ou utiliser un player adaptatif. |

## 4. CSS (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 19 | Limiter le nombre de fichiers CSS | P5 | Maximum 3-5 fichiers CSS en production (apres bundling). |
| 20 | Externaliser les CSS | P5 | Pas de CSS inline significatif (> 10 lignes) dans le HTML. Le critical CSS inline est acceptable. |
| 21 | Preferer CSS aux images | P4 | Gradients, bordures, ombres, formes simples en CSS plutot qu'en image. |
| 22 | Utiliser les portions necessaires des frameworks CSS | P5 | Tree-shaking ou purge CSS active. Pas de framework CSS complet charge pour quelques classes. |

## 5. JavaScript (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 23 | Utiliser les portions necessaires des libs JS | P5 | Imports specifiques (`import { x } from 'lib'`), pas d'import global. Bundle analyzer pour detecter le bloat. |
| 24 | Externaliser le JavaScript | P5 | Pas de JS inline significatif (> 10 lignes) dans le HTML. |
| 25 | Charger le code uniquement quand necessaire | P4 | Code splitting, dynamic imports, `defer`/`async` sur les scripts. Pas de JS charge inutilement sur des pages ou il n'est pas utilise. |
| 26 | Eviter les temps de blocage JS (TBT) | P4 | Total Blocking Time <= 200ms. Pas de tache JS > 50ms sur le thread principal. Utiliser Web Workers pour le calcul lourd. |
| 27 | Remplacer les widgets sociaux par des liens simples | P5 | Pas de SDK Facebook/Twitter/LinkedIn. Utiliser des liens `<a>` avec `share_url`. |

## 6. Reseau & Requetes (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 28 | Limiter le nombre de requetes HTTP | P4 | Maximum 40 requetes par page. Combiner les petites ressources. Sprites ou SVG inline pour les icones. |
| 29 | Rechargement partiel des zones de contenu | P4 | Utiliser AJAX/fetch pour mettre a jour des zones sans recharger toute la page. |
| 30 | Utiliser les Service Workers | P4 | Cache offline des assets statiques. Strategie cache-first pour les ressources stables. |
| 31 | Eviter les redirections | P4 | Maximum 1 redirection. Pas de chaine de redirections. |
| 32 | Limiter les domaines servant les ressources | P3 | Maximum 5 domaines differents. Chaque domaine = un DNS lookup supplementaire. |

## 7. Backend & Donnees (7 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 33 | Mettre en cache les donnees calculees souvent utilisees | P4 | Les resultats de calculs couteux sont caches (Redis, APCu, memcache, in-memory). TTL adapte a la volatilite. |
| 34 | Limiter les appels API | P4 | Pas d'appels redondants. Strategie de cache sur les endpoints. Regrouper les requetes quand possible. |
| 35 | Reduire le volume de donnees stockees | P5 | Pas de donnees inutiles en BDD. Politique de purge des donnees obsoletes. |
| 36 | Optimiser les requetes BDD | P3 | Pas de requete N+1. Index sur les colonnes filtrees/triees. `EXPLAIN` sur les requetes lentes. |
| 37 | Format de donnees adapte en BDD | P4 | Types de colonnes dimensionnes au besoin (pas de `TEXT` pour un code postal, pas de `BIGINT` pour un booleen). |
| 38 | Favoriser les pages statiques | P4 | Generer en statique (SSG) ce qui n'a pas besoin d'etre dynamique. Pre-rendu des pages stables. |
| 39 | Architecture modulaire | P3 | Code decouplable. Pas de monolithe charge integralement pour chaque requete. |

## 8. Cache & Compression (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 40 | Cache HTTP (Expires / Cache-Control) | P4 | Headers de cache sur toutes les ressources statiques. `immutable` sur les assets versiones. TTL adapte. |
| 41 | Utiliser un CDN | P4 | Assets statiques servis via CDN. Si pas de CDN, au minimum des headers de cache longs. |
| 42 | Compresser les fichiers texte | P3 | Brotli ou Gzip active sur HTML, CSS, JS, SVG, JSON. Verifiable via header `Content-Encoding`. |
| 43 | Minifier CSS, JS, HTML et SVG | P4 | Tous les fichiers texte sont minifies en production. Build pipeline configure. |
| 44 | Combiner les fichiers CSS et JS | P4 | Bundle les fichiers en production. Maximum 2-3 bundles JS, 2-3 bundles CSS. |

## 9. Contenu & Analytics (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 45 | Limiter les outils d'analytics | P4 | Maximum 1 outil d'analytics. Preferer une solution legere (Plausible, Matomo lite) a Google Analytics. |
| 46 | Optimiser les cookies | P4 | Pas de cookies inutiles. Taille minimale. Pas de cookies sur les domaines servant des ressources statiques. |
| 47 | Adapter les textes au web | P3 | Textes concis et structures. Score de lisibilite Flesch-Kincaid >= 60. |
| 48 | Transcription textuelle des contenus multimedias | P4 | Chaque video/audio a une alternative textuelle (transcription, sous-titres, ou resume). |
| 49 | Politique d'expiration et suppression des donnees | P4 | Les donnees ont une duree de vie definie. Nettoyage automatique des donnees perimees (CRON, TTL). |

## 10. Compatibilite & Durabilite (2 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 50 | Compatibilite avec les anciens appareils | P4 | Le site fonctionne sur des appareils de 5+ ans. Pas de JS/CSS ultra-moderne sans fallback. Performance acceptable sur hardware modeste. |
| 51 | Strategie de fin de vie des contenus | P4 | Les contenus obsoletes sont archives ou supprimes. Pas de pages orphelines indexees. |

---

## Grille de scoring

- **P5** = Impact majeur, a traiter en priorite (x5)
- **P4** = Impact important (x4)
- **P3** = Impact modere (x3)

**Score maximum theorique** = somme des poids de toutes les pratiques applicables.

**Calcul du score**: Chaque pratique OK multipliee par son poids. Les pratiques NA sont exclues du total.

```
Score = SUM(pratiques_OK * poids) / SUM(pratiques_applicables * poids) * 100
```
