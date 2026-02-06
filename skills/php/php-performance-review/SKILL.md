---
name: php-performance-review
description: Analyze a PHP web application or codebase for performance issues and optimization opportunities. Use when the user asks to audit, check, review, or analyze the performance, speed, optimization, slow queries, caching, or scalability of a PHP, Laravel, Kirby, Livewire, or Blade application. Also use when the user mentions "performance", "lenteur", "slow", "N+1", "query optimization", "cache", "OPcache", "eager loading", "lazy loading queries", "index SQL", "optimisation", or "benchmark". Specialized for PHP, Laravel, Kirby CMS, Livewire, Blade, Vite, Tailwind CSS, and SQL databases.
---

# PHP Performance Review

Audit a PHP web application's performance from the codebase. Focuses on identifying bottlenecks, inefficient patterns, and missed optimization opportunities in PHP/Laravel/Kirby applications.

## How to Conduct the Audit

### 1. Explore the Codebase

Before auditing, understand the project:

- Identify the framework (Laravel, Kirby, vanilla PHP) and version
- Locate database queries (Eloquent models, Query Builder, raw SQL)
- Find cache configuration and usage
- Check queue/job configuration
- Locate middleware pipeline
- Find eager loading patterns and relationships
- Check frontend build config (Vite, Tailwind, asset pipeline)
- Identify Livewire components (hydration, polling, requests)
- Locate scheduled tasks and artisan commands
- Check database migrations for indexes

### 2. Load the Checklist

Read `references/checklist.md` in this skill's directory for the full list of practices to check.

### 3. Analyze Each Category

For each category in the checklist, search the codebase for relevant patterns. Use Grep, Glob, and Read tools to find:

- N+1 queries (relationships accessed in loops without eager loading)
- Missing database indexes on filtered/sorted/joined columns
- `SELECT *` patterns (Eloquent without `select()`)
- Large collections loaded into memory without chunking
- Missing cache on frequently computed data
- Redundant or duplicated queries per request
- Unoptimized Livewire components (excessive re-renders, polling)
- Missing route/config/view caching in production config
- Large file uploads without streaming
- Synchronous operations that should be queued
- Frontend asset optimization (Vite config, Tailwind purge, code splitting)
- Missing lazy loading on images/iframes
- Oversized API responses (no pagination, no field selection)

### 4. Generate the Report

Produce a structured report with these sections:

```
## Audit Performance PHP - [Project Name]

### Score global
X / Y pratiques conformes (Z%)

### Resume
[2-3 sentences summarizing the main findings]

### Resultats par categorie

#### [Category Name]
| # | Pratique | Statut | Commentaire |
|---|----------|--------|-------------|
| 1 | ...      | OK/KO/NA | ...       |

### Goulots d'etranglement identifies
[List the most impactful performance issues found]

### Top priorites
[5-10 most impactful optimizations, ordered by impact/effort ratio]

### Recommandations detaillees
[For each KO finding, explain the issue, its impact, and the fix with code examples]
```

**Status values:**

- **OK** - Practice is followed
- **KO** - Practice is not followed (include fix with code example)
- **NA** - Not applicable to this project
- **PARTIAL** - Partially followed (explain what's missing)

### 5. Prioritization

When listing recommendations, prioritize by measurable impact:

1. **Critical** - Major bottlenecks (N+1 in loops, missing indexes on large tables, no cache on hot paths)
2. **High impact** - Significant improvements (eager loading, query optimization, queue offloading)
3. **Quick wins** - Low effort gains (config caching, select specific columns, lazy loading images)
4. **Progressive** - Architecture improvements (caching strategy, denormalization, CDN)

### 6. Export the Review Report

Once the audit is complete, save the full analysis as a Markdown document at the root of the audited codebase:

```
/docs/YYMMDD_php-performance-review.md
```

Where `YYMMDD` is the current date (e.g., `260206` for February 6, 2026). Create the `/docs/` directory if it does not exist.

The document must include:

1. The complete audit report (as generated in step 4)
2. An **action plan** section at the end, listing all tasks derived from the findings:
   - Grouped by priority (Critical, High impact, Quick wins, Progressive)
   - Each task broken down into sub-tasks of **2 to 4 hours** (assuming AI-assisted development)
   - Time estimate for each sub-task
   - Total estimated time per priority level

```
### Plan d'action

#### Critique
- [ ] [Tache] — ~Xh (avec IA)
  - [ ] [Sous-tache 1] — ~2h
  - [ ] [Sous-tache 2] — ~3h

#### Impact eleve
...

#### Quick wins
...

#### Progressif
...

**Total estime : ~Xh**
```

## Important Notes

- This is a **static code audit**, not a runtime profiling session. Recommend tools like Laravel Telescope, Debugbar, or Blackfire for runtime analysis.
- Focus on patterns identifiable in code. Flag what needs benchmarking.
- Be specific: point to exact files, lines, models, and queries.
- For Laravel: leverage framework features (eager loading, caching, queues, model scopes).
- For Kirby: check page caching, thumb generation, content locking, panel performance.
- For Livewire: check hydration size, polling frequency, lazy loading components.
- Provide code examples in recommendations (before/after).
