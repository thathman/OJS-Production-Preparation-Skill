# OJS Production Preparation Skill v1.0.4

This directory is the installable production skill package.

## Install

Install the `ojs-production-preparation` directory as a skill in any Agent Skills-compatible client. The entry point is `SKILL.md`.

## Runtime behaviour

The skill is journal-agnostic and source-first. It asks for source files and/or the journal website before asking configuration questions, detects as much of the journal setup as possible, asks only about gaps or conflicts, and returns only the fields needed for the selected OJS production task.

For article publication preparation, the user-facing response now follows one fixed six-step workflow unless the editor asks for a different structure:

1. **Title & Abstract**
2. **Contributors**
3. **Metadata**
4. **References**
5. **Galleys**
6. **Issue**

References are pasted in full when available from the source. Pages, DOI, Date Published, Issue assignment and the concise article URL Path are kept together under **Issue**. Article Publisher ID, when actually used, is displayed under **Metadata** rather than creating an extra top-level Identifiers step.

Generated article URL Paths use a concise semantic summary of the title rather than slugifying the complete title. The default aims for roughly 3–6 distinctive terms, checks for collisions, and adds only the smallest useful distinguishing term when necessary. Established journal URL conventions take precedence.

The skill explicitly prevents DOI values from being misused as Publisher IDs or as the submission's own Source. Article publication URL Path and Galley URL Path remain separate scopes.

Issue preparation remains organised into the OJS-facing scopes **Issue Data**, **Issue Galley**, and **Identifiers**. Publisher IDs for articles, issues and issue galleys are treated as separate optional conventions and are detected before the skill asks the editor about them.

## Repository documentation

The repository root contains expanded design references, workflow guides, schemas, templates, examples and acceptance criteria. See `docs/ARTICLE-PUBLICATION-METADATA.md` for the article metadata, response-order and URL Path rules.
