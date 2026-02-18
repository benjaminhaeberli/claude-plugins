---
name: gdpr-audit
description: Analyze a web application or codebase for GDPR/RGPD and nLPD (Swiss) compliance. Use when the user asks to audit, check, review, or analyze privacy compliance, data protection, GDPR, RGPD, nLPD, cookie consent, personal data handling, or user rights implementation. Also use when the user mentions "privacy by design", "données personnelles", "CNIL", "protection des données", "consentement", or "droit à l'effacement". Works with HTML, CSS, JavaScript, TypeScript, PHP, and common web frameworks.
---

# GDPR Review

Audit a web application's GDPR/RGPD compliance from a developer's perspective, based on the CNIL developer guide (18 fiches) and GDPR articles. Also covers Swiss nLPD principles.

## How to Conduct the Audit

### 1. Explore the Codebase

Before auditing, understand the project:

- Identify the tech stack, framework, and backend language
- Locate forms, user registration, login, and profile pages
- Find cookie/consent management code
- Identify database schemas and data models (what personal data is stored)
- Locate privacy policy, terms of service, and legal pages
- Find third-party integrations (analytics, ads, social SDKs, payment)
- Check for logging, monitoring, and error tracking code

### 2. Load the Checklist

Read `references/checklist.md` in this skill's directory for the full list of practices to check.

### 3. Analyze Each Category

For each category, search the codebase for relevant patterns using Grep, Glob, and Read tools:

- **Data collection**: forms, input fields, registration flows — what personal data is collected?
- **Consent**: cookie banners, consent management, opt-in mechanisms
- **User rights**: account deletion, data export, profile editing, opt-out
- **Data storage**: database schemas, encryption at rest, hashing of passwords
- **Security**: HTTPS, CSRF protection, XSS prevention, SQL injection protection, auth mechanisms
- **Third parties**: analytics scripts, ad SDKs, social widgets, external APIs
- **Cookies & trackers**: cookie usage, localStorage, sessionStorage, fingerprinting
- **Data retention**: TTL, purge mechanisms, archival logic
- **Logging**: what personal data appears in logs, log retention policies
- **Transfers**: data sent to external services, cross-border considerations

### 4. Generate the Report

```
## Audit GDPR/RGPD - [Project Name]

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
[List any high-risk findings that could lead to sanctions: missing consent, data leaks, etc.]

### Top priorites
[5-10 most impactful improvements, ordered by risk/effort ratio]

### Recommandations detaillees
[For each KO finding, explain what to fix, how, and relevant GDPR article]
```

**Status values:**
- **OK** - Practice is followed
- **KO** - Practice is not followed (include recommendation + relevant GDPR article)
- **NA** - Not applicable to this project
- **PARTIAL** - Partially followed (explain what's missing)

### 5. Prioritization

When listing recommendations, prioritize by risk:

1. **Critical** - Data breaches, missing consent, sensitive data exposed (sanctions risk)
2. **High** - Missing user rights, no data retention policy, insecure auth
3. **Medium** - Incomplete information notices, cookies without proper management
4. **Low** - Documentation gaps, minor configuration improvements

### 6. Export the Review Report

Once the audit is complete, save the full analysis as a Markdown document at the root of the audited codebase:

```
/docs/YYMMDD_gdpr-audit.md
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

- This is a **technical code audit**, not a legal audit. Recommend consulting a DPO/lawyer for full compliance.
- Focus on what is verifiable in the codebase. Flag what needs human/legal review.
- The same principles apply to GDPR (EU), RGPD (FR), and nLPD (CH) — note any differences when relevant.
- Be specific: point to exact files, lines, and code patterns.
- When a practice cannot be verified from code alone (e.g., DPO appointment), mark as "needs verification" rather than OK/KO.
