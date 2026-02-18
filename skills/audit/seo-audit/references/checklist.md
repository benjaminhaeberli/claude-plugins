# Checklist SEO Technique

Best practices for technical SEO auditing from the codebase. Focused on what a web developer can verify and fix in code (HTML/CSS/JS/PHP). Adapted for 2026.

---

## 1. Balises meta essentielles (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 1 | Title unique et optimise par page | CRITIQUE | Chaque page a un `<title>` unique, 50-60 caracteres, mot-cle principal en debut. Pas de title duplique entre pages. |
| 2 | Meta description unique par page | HAUTE | Chaque page a une `<meta name="description">` unique, 150-160 caracteres, incitative. Pas de description dupliquee. |
| 3 | Balise canonical correcte | CRITIQUE | Chaque page a un `<link rel="canonical">` pointant vers l'URL preferee. Auto-referent sur les pages sans variantes. Pas de canonical vers une page 404 ou redirigee. |
| 4 | Viewport meta tag | HAUTE | `<meta name="viewport" content="width=device-width, initial-scale=1">` present dans le layout principal. |
| 5 | Langue de la page declaree | HAUTE | Attribut `lang` sur la balise `<html>`. Correspond a la langue reelle du contenu. |
| 6 | Charset declare | MOYENNE | `<meta charset="UTF-8">` present en premier dans le `<head>`. |

## 2. Hierarchie des titres & structure semantique (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 7 | Un seul H1 par page | CRITIQUE | Chaque page/template a exactement un `<h1>`. Pas de H1 dans le header/nav global. |
| 8 | Hierarchie H1-H6 logique | HAUTE | Pas de saut de niveau (H1 > H3 sans H2). Structure coherente et descendante. |
| 9 | HTML semantique | HAUTE | Utilisation de `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`. Pas de `<div>` pour tout. |
| 10 | Fil d'Ariane (breadcrumb) | MOYENNE | Breadcrumb present sur les pages internes. Balisage schema.org/BreadcrumbList en JSON-LD. |
| 11 | Liens internes avec ancres descriptives | MOYENNE | Pas de "cliquez ici" ou "en savoir plus" comme seul texte d'ancre. Ancres descriptives du contenu cible. |

## 3. Donnees structurees / Schema.org (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 12 | JSON-LD present sur les pages cles | HAUTE | Au minimum : Organization/LocalBusiness sur la home, BreadcrumbList sur les pages internes, Article sur les contenus editoriaux. |
| 13 | Schema Organization / LocalBusiness | HAUTE | Nom, logo, URL, coordonnees, reseaux sociaux. Present sur la page d'accueil au minimum. |
| 14 | Schema Article / BlogPosting | MOYENNE | Sur les articles/blog : headline, datePublished, dateModified, author, image. |
| 15 | Schema BreadcrumbList | MOYENNE | Coherent avec le fil d'Ariane visible. Chaque item a name et URL. |
| 16 | Schema FAQPage / HowTo si applicable | MOYENNE | Si la page contient une FAQ ou un tutoriel, le schema correspondant est present. |

## 4. Sitemap & indexation (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 17 | Sitemap XML genere et a jour | CRITIQUE | `/sitemap.xml` existe, est accessible, et contient toutes les pages indexables. Genere automatiquement (pas a la main). |
| 18 | Robots.txt correctement configure | CRITIQUE | `/robots.txt` existe. Ne bloque pas de pages importantes. Contient la reference au sitemap. Pas de `Disallow: /` en production. |
| 19 | Meta robots coherents | CRITIQUE | Pas de `noindex` accidentel sur des pages importantes. Les pages admin/login/duplicat ont `noindex`. Coherence entre meta robots et robots.txt. |
| 20 | Pages 404 personnalisees | MOYENNE | Page 404 custom avec navigation, recherche, et liens vers les pages principales. Status HTTP 404 correct (pas un 200). |
| 21 | Gestion des redirections 301 | HAUTE | Les anciennes URLs redirigent en 301 vers les nouvelles. Pas de chaines de redirections (max 1 hop). Pas de redirections 302 pour des deplacements permanents. |
| 22 | Pagination SEO-friendly | MOYENNE | Les pages paginees ont des canonicals corrects. Pas de contenu duplique entre pages. URLs propres (`?page=2` ou `/page/2`). |

## 5. URLs & routing (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 23 | URLs propres et lisibles | HAUTE | Slugs en minuscules, mots separes par des tirets. Pas de parametres inutiles, pas d'IDs cryptiques. Ex: `/blog/mon-article` et non `/post?id=42`. |
| 24 | Coherence des trailing slashes | MOYENNE | Toutes les URLs avec ou sans trailing slash, mais pas les deux. Redirection de l'une vers l'autre. |
| 25 | HTTPS partout | CRITIQUE | Toutes les pages en HTTPS. Redirection HTTP -> HTTPS. Pas de mixed content. |
| 26 | Pas de contenu duplique par URL | HAUTE | Une seule URL par contenu. Pas de version www et non-www accessible. Pas de variantes avec parametres (`?ref=`, `?utm_`) indexees. |
| 27 | URLs multilingues avec hreflang | HAUTE | Si site multilingue : `<link rel="alternate" hreflang="x">` sur chaque page. Bidirectionnel. Inclut `x-default`. Structure d'URL claire (`/fr/`, `/de/` ou sous-domaines). |

## 6. Open Graph & reseaux sociaux (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 28 | Open Graph tags complets | HAUTE | `og:title`, `og:description`, `og:image`, `og:url`, `og:type` sur chaque page. Image OG >= 1200x630px. |
| 29 | Twitter Card tags | MOYENNE | `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`. Type `summary_large_image` pour les articles. |
| 30 | Image de partage optimisee | MOYENNE | Image par defaut si aucune image specifique. Dimensions correctes. Alt text sur l'image OG. |
| 31 | Favicon et icones | MOYENNE | Favicon present (`.ico` + `.svg`). Apple touch icon. `manifest.json` avec icones pour PWA. |

## 7. Performance & Core Web Vitals (6 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 32 | Lazy loading sur images/iframes | HAUTE | `loading="lazy"` sur les images sous la ligne de flottaison. Pas de lazy loading sur les images above-the-fold (LCP). |
| 33 | Images optimisees (format, taille, srcset) | HAUTE | Formats modernes (WebP/AVIF). `srcset` et `sizes` pour le responsive. Pas d'image surdimensionnee. |
| 34 | Fonts optimisees | HAUTE | `font-display: swap` ou `optional`. Preload des polices critiques. Maximum 2-3 polices custom. Formats WOFF2. |
| 35 | Critical CSS / above-the-fold | MOYENNE | CSS critique inline dans le `<head>`. Le reste charge en async. Pas de render-blocking CSS non critique. |
| 36 | JavaScript non bloquant | HAUTE | `defer` ou `async` sur les scripts. Pas de JS bloquant le rendu dans le `<head>`. Code splitting si SPA. |
| 37 | Preconnect / prefetch strategiques | MOYENNE | `<link rel="preconnect">` vers les origines tierces critiques (fonts, CDN, API). `<link rel="prefetch">` sur les pages probables suivantes. |

## 8. Contenu & accessibilite SEO (5 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 38 | Attribut alt sur toutes les images | HAUTE | Chaque `<img>` a un attribut `alt` descriptif. Images decoratives : `alt=""`. Pas d'alt generique ("image", "photo"). |
| 39 | Liens avec textes descriptifs | MOYENNE | Pas de "cliquez ici", "lire la suite" sans contexte. `aria-label` si le texte visible ne suffit pas. |
| 40 | Contenu textuel accessible aux moteurs | HAUTE | Le contenu principal est dans le HTML (pas uniquement dans du JS client-side). SSR ou pre-rendering si SPA. |
| 41 | Temps de chargement du contenu principal | HAUTE | Le contenu visible ne depend pas d'appels API lents. SSR ou cache pour le contenu principal. |
| 42 | Pas de contenu cache ou cloake | MOYENNE | Le contenu visible par les moteurs est le meme que celui visible par les utilisateurs. Pas de `display:none` sur du contenu textuel important. |

## 9. Specificites techniques (4 pratiques)

| # | Pratique | Priorite | Quoi verifier |
|---|----------|----------|---------------|
| 43 | Rendu cote serveur (SSR) pour le contenu principal | HAUTE | Si SPA : le contenu est pre-rendu ou rendu cote serveur. Le HTML source contient le contenu (pas juste un `<div id="app">`). |
| 44 | Erreurs de crawl minimisees | MOYENNE | Pas de liens internes cassés (404). Pas de ressources manquantes (images, CSS, JS). |
| 45 | Internationalisation correcte | HAUTE | Si multilingue : chaque langue a ses propres meta tags, son propre sitemap, et des hreflang bidirectionnels. Pas de contenu mixte FR/EN sur une meme page. |
| 46 | Mobile-first responsive | HAUTE | Design responsive (pas de site mobile separe). Media queries en `min-width`. Touch targets >= 48px. Texte lisible sans zoom. |

---

## Grille de scoring

- **CRITIQUE** = Bloque l'indexation ou cause une perte de ranking majeure (x5)
- **HAUTE** = Facteur de ranking significatif (x4)
- **MOYENNE** = Amelioration utile mais impact modere (x3)

**Calcul du score** : Chaque pratique OK multipliee par son poids. Les pratiques NA sont exclues du total.

```
Score = SUM(pratiques_OK * poids) / SUM(pratiques_applicables * poids) * 100
```

## References

- Google Search Central documentation (2026)
- Schema.org vocabulary
- Core Web Vitals (LCP, INP, CLS)
- RFC 6596 (canonical)
- RFC 8288 (web linking)
