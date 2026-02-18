---
name: accessibility-audit
description: Analyze a web application or codebase for accessibility and WCAG compliance. Use when the user asks to audit, check, review, or analyze accessibility, a11y, WCAG, RGAA, inclusive design, or eCH-0059 compliance of a website, web app, or codebase. Also use when the user mentions "accessibilite", "handicap", "lecteur d'ecran", "screen reader", "ARIA", "contraste", "navigation clavier", "keyboard navigation", "focus", or "WAI". Works with HTML, CSS, JavaScript, TypeScript, PHP, Blade, Kirby, and common web frameworks.
---

# Accessibility Review

Audit a web application's accessibility from the codebase, based on WCAG 2.1/2.2 level AA criteria. Also references RGAA (French) and eCH-0059 (Swiss standard). Focuses on what is verifiable in code.

## How to Conduct the Audit

### 1. Explore the Codebase

Before auditing, understand the project:

- Identify the tech stack (framework, templating engine, component library)
- Locate HTML templates, layouts, and reusable components
- Find form components and validation logic
- Check for existing ARIA usage and patterns
- Identify navigation components (menus, breadcrumbs, skip links)
- Locate media elements (images, videos, audio)
- Check CSS for focus styles, contrast, and responsive behavior

### 2. Load the Checklist

Read `references/checklist.md` in this skill's directory for the full list of practices to check.

### 3. Analyze Each Category

For each category in the checklist, search the codebase for relevant patterns. Use Grep, Glob, and Read tools to find:

- Missing `alt` attributes on images
- Form inputs without associated labels
- Missing ARIA landmarks and roles
- Focus management issues (missing focus styles, focus traps)
- Keyboard navigation gaps (non-interactive elements with click handlers)
- Color contrast issues in CSS (text vs background)
- Missing skip links
- Inaccessible custom components (dropdowns, modals, tabs)
- Missing `prefers-reduced-motion` media query
- Missing lang attribute on HTML element
- Auto-playing media without controls

### 4. Generate the Report

Produce a structured report with these sections:

```
## Audit Accessibilite - [Project Name]

### Score global
X / Y pratiques conformes (Z%)

### Resume
[2-3 sentences summarizing the main findings]

### Resultats par categorie

#### [Category Name]
| # | Pratique | Statut | Commentaire |
|---|----------|--------|-------------|
| 1 | ...      | OK/KO/NA | ...       |

### Risques critiques
[List any issues blocking access for users with disabilities]

### Top priorites
[5-10 most impactful improvements, ordered by severity/effort ratio]

### Recommandations detaillees
[For each KO finding, explain what to fix, how, and relevant WCAG criterion]
```

**Status values:**

- **OK** - Practice is followed
- **KO** - Practice is not followed (include recommendation + WCAG criterion)
- **NA** - Not applicable to this project
- **PARTIAL** - Partially followed (explain what's missing)

### 5. Prioritization

When listing recommendations, prioritize by impact on users:

1. **Critical** - Content inaccessible to entire user groups (missing alt, no keyboard nav, no focus)
2. **High** - Major barriers (form labels, heading hierarchy, ARIA misuse)
3. **Medium** - Significant but partial barriers (contrast, skip links, error messages)
4. **Low** - Enhancements (prefers-reduced-motion, enhanced focus styles)

### 6. Export the Review Report

Once the audit is complete, save the full analysis as a Markdown document at the root of the audited codebase:

```
/docs/YYMMDD_accessibility-audit.md
```

Where `YYMMDD` is the current date (e.g., `260206` for February 6, 2026). Create the `/docs/` directory if it does not exist.

The document must include:

1. The complete audit report (as generated in step 4)
2. An **action plan** section at the end, listing all tasks derived from the findings:
   - Grouped by priority (Critical, High, Medium, Low)
   - Each task broken down into sub-tasks of **2 to 4 hours** (assuming AI-assisted development)
   - Time estimate for each sub-task
   - Total estimated time per priority level

```
### Plan d'action

#### Critique
- [ ] [Tache] — ~Xh (avec IA)
  - [ ] [Sous-tache 1] — ~2h
  - [ ] [Sous-tache 2] — ~3h

#### Haute priorite
...

#### Moyenne priorite
...

#### Basse priorite
...

**Total estime : ~Xh**
```

## Important Notes

- This is a **code audit**, not a full accessibility audit. Manual testing with screen readers and keyboard is still required.
- Focus on what is verifiable in the codebase. Flag what needs manual testing.
- WCAG level AA is the target (legal requirement in Switzerland under eCH-0059 and in the EU under EAA).
- Be specific: point to exact files, lines, and code patterns.
- Adapt to the tech stack: Blade components, Kirby snippets, Livewire components each have their patterns.
- When a criterion cannot be verified from code alone (e.g., cognitive load), mark as "needs manual testing".
