---
name: ecodesign-review
description: Analyze a web application or codebase for eco-design and digital sobriety. Use when the user asks to audit, check, review, or analyze the ecological impact, green IT compliance, eco-design, energy efficiency, or digital sobriety of a website, web app, or codebase. Also use when the user mentions "ecodesign", "green IT", "sobriete numerique", "eco-conception", "RGESN", or "115 pratiques". Works with HTML, CSS, JavaScript, TypeScript, PHP, and common web frameworks.
---

# Ecodesign Analyze

Audit a web application's eco-design practices based on a curated subset of the GreenIT "115 bonnes pratiques" (v4/v5), filtered for relevance in 2026.

## How to Conduct the Audit

### 1. Explore the Codebase

Before auditing, understand the project:

- Identify the tech stack (framework, build tools, CSS methodology, backend language)
- Locate entry points: HTML templates, main JS/CSS bundles, API routes
- Check for a build pipeline (bundler config, minification, compression)
- Identify static assets: images, fonts, videos, documents

### 2. Load the Checklist

Read `references/checklist.md` in this skill's directory for the full list of practices to check.

### 3. Analyze Each Category

For each category in the checklist, search the codebase for relevant patterns. Use Grep, Glob, and Read tools to find:

- Unused dependencies or bloated imports
- Unoptimized images (large files, wrong formats)
- Missing lazy loading on images/iframes/videos
- Inline CSS/JS that should be externalized
- Autoplay on media elements
- Third-party scripts (analytics, social widgets, trackers)
- Missing cache headers or CDN setup
- Oversized API responses or redundant API calls
- Database queries that could be optimized (N+1, missing indexes)

### 4. Generate the Report

Produce a structured report with these sections:

```
## Audit Eco-design - [Project Name]

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

1. **Quick wins** - Low effort, high impact (missing lazy loading, unoptimized images)
2. **High impact** - More effort but significant gains (removing unused deps, static pages)
3. **Progressive** - Long-term improvements (architecture, caching strategy)

### 6. Export the Review Report

Once the audit is complete, save the full analysis as a Markdown document at the root of the audited codebase:

```
/docs/YYMMDD_ecodesign-review.md
```

Where `YYMMDD` is the current date (e.g., `260206` for February 6, 2026). Create the `/docs/` directory if it does not exist.

The document must include:

1. The complete audit report (as generated in step 4)
2. An **action plan** section at the end, listing all tasks derived from the findings:
   - Grouped by priority (Quick wins, High impact, Progressive)
   - Each task broken down into sub-tasks of **2 to 4 hours** (assuming AI-assisted development)
   - Time estimate for each sub-task
   - Total estimated time per priority level

```
### Plan d'action

#### Quick wins
- [ ] [Tache] — ~Xh (avec IA)
  - [ ] [Sous-tache 1] — ~2h
  - [ ] [Sous-tache 2] — ~3h

#### Impact eleve
...

#### Progressif
...

**Total estime : ~Xh**
```

## Important Notes

- Do NOT check for practices that are handled by default in modern tooling (HTTP/2, HSTS, etc.)
- Focus on what the developer can actually change in their code
- Adapt the analysis to the project's tech stack - a static site has different concerns than a SPA
- When unsure if a practice applies, mark as NA with explanation
- Be specific in recommendations: point to exact files and lines
