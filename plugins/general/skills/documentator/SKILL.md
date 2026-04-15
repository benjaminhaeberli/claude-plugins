---
name: documentator
description: >
  Generate or update structured documentation in Markdown.
  Use when the user asks to document, write docs, create a PRD, draft a task description,
  write technical documentation, explain a feature, create a specification,
  or any documentation-related request.
  Supports: technical docs, PRDs, task descriptions, architecture decisions, guides, notes.
---

## How to Document

### 1. Understand the Request

- Identify the **type** of document: general documentation, task, PRD, guide, decision record, etc.
- Identify the **subject**: feature, system, process, concept, etc.
- Ask clarifying questions if the scope is ambiguous

### 2. Determine the Output Location

- **General documentation**: `/docs/YYMMDD_topic_name.md`
- **Tasks and PRDs**: `/docs/tasks/YYMMDD_topic_name.md`
- Respect any explicit path provided by the user (overrides defaults)
- Use today's date for the `YYMMDD` prefix (e.g., `260206` for February 6, 2026)
- Use lowercase snake_case for `topic_name` (e.g., `260206_scaling_serveur.md`)

### 3. Write the Document

- **Language**: French by default, unless the user specifies otherwise
- **Tone**: Clear, concise, professional
- Structure the content with appropriate headings (`##`, `###`)
- Use lists, tables, and code blocks where they improve clarity
- Adapt the structure to the document type:
  - **General doc**: Context, content sections, references
  - **PRD**: Objectif, contexte, exigences fonctionnelles, exigences techniques, critères d'acceptation, hors-scope
  - **Task**: Objectif, contexte, étapes, critères de validation
  - **Guide**: Introduction, prérequis, étapes, troubleshooting

### 4. Add Footer Metadata

Every document **must** end with the following footer, separated by a horizontal rule:

```markdown
---

**Dernière mise à jour** : YYYY-MM-DD
**Généré avec [Claude Code](https://claude.com/claude-code)**
```

Use today's full date (e.g., `2026-02-06`) for the update field.

### 5. Updating Existing Documents

When updating an existing document:

- **Never** change the filename date prefix — it represents the creation date
- **Only** update the `Dernière mise à jour` date in the footer to today's date
- Preserve the existing structure and modify only the relevant sections

## Important Notes

- This skill generates documentation files, not code. If the user needs code changes alongside documentation, handle them separately.
- Create the `/docs/` or `/docs/tasks/` directory if it doesn't exist yet.
- For large topics, prefer splitting into multiple focused documents over one monolithic file.
