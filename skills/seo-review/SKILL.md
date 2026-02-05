---
name: seo-review
description: Analyze a web application or codebase for technical SEO best practices. Use when the user asks to audit, check, review, or analyze the SEO, search engine optimization, technical SEO, on-page SEO, or indexability of a website, web app, or codebase. Also use when the user mentions "SEO", "referencement", "referencement naturel", "meta tags", "sitemap", "schema.org", "structured data", "donnees structurees", "canonical", "indexation", "SERP", "rich snippets", or "Core Web Vitals". Works with HTML, CSS, JavaScript, TypeScript, PHP, Blade, Kirby, and common web frameworks.
---

# SEO Review

Audit a web application's technical SEO from the codebase, based on current best practices (2026). Focuses on what is verifiable in code — not analytics data or external crawl tools.

## How to Conduct the Audit

### 1. Explore the Codebase

Before auditing, understand the project:

- Identify the tech stack (framework, templating engine, SSR/CSR/SSG)
- Locate HTML templates, layouts, and page components
- Find routing configuration and URL generation logic
- Check for sitemap/robots.txt generation
- Identify meta tag management (global vs per-page)
- Locate structured data (JSON-LD, microdata, RDFa)

### 2. Load the Checklist

Read `references/checklist.md` in this skill's directory for the full list of practices to check.

### 3. Analyze Each Category

For each category in the checklist, search the codebase for relevant patterns. Use Grep, Glob, and Read tools to find:

- Missing or duplicate `<title>` and `<meta name="description">`
- Missing canonical URLs or self-referencing canonicals
- Heading hierarchy issues (multiple H1, skipped levels)
- Missing `alt` attributes on images
- Missing Open Graph and Twitter Card meta tags
- Structured data (JSON-LD) presence and completeness
- Sitemap.xml and robots.txt generation/configuration
- URL structure (clean slugs, trailing slashes consistency)
- Hreflang tags for multilingual sites
- Rendering strategy (SSR vs CSR impact on crawlability)
- Internal linking patterns and orphan pages
- Performance indicators (lazy loading, image optimization, font loading)
- Mobile responsiveness (viewport meta, responsive design)

### 4. Generate the Report

Produce a structured report with these sections:

```
## Audit SEO Technique - [Project Name]

### Score global
X / Y pratiques conformes (Z%)

### Resume
[2-3 sentences summarizing the main findings]

### Resultats par categorie

#### [Category Name]
| # | Pratique | Statut | Commentaire |
|---|----------|--------|-------------|
| 1 | ...      | OK/KO/NA | ...       |

### Top priorites
[List the 5-10 most impactful improvements, ordered by effort/impact ratio]

### Recommandations detaillees
[For each KO finding, explain what to fix and how]
```

**Status values:**

- **OK** - Practice is followed
- **KO** - Practice is not followed (include recommendation)
- **NA** - Not applicable to this project
- **PARTIAL** - Partially followed (explain what's missing)

### 5. Prioritization

When listing recommendations, prioritize by:

1. **Critical** - Blocks indexing or causes major ranking loss (missing titles, noindex errors, broken canonicals)
2. **High impact** - Significant ranking factors (structured data, heading hierarchy, meta descriptions)
3. **Quick wins** - Low effort improvements (alt texts, Open Graph tags, sitemap tweaks)
4. **Progressive** - Long-term improvements (URL restructuring, rendering strategy)

## Important Notes

- This is a **technical code audit**, not a content or backlink audit.
- Focus on what the developer can change in the codebase.
- Adapt to the tech stack: a Kirby site has different SEO patterns than a Laravel SPA.
- For Laravel: check Blade templates, route names, and packages like `spatie/laravel-sitemap` or `artesaos/seotools`.
- For Kirby: check meta snippets, SEO plugins, routing, and content structure.
- Be specific in recommendations: point to exact files and lines.
