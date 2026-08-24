# OJS Production Preparation Skill v1.0.3

This directory is the installable production skill package.

## Install

Install the `ojs-production-preparation` directory as a skill in any Agent Skills-compatible client. The entry point is `SKILL.md`.

## Runtime behaviour

The skill is journal-agnostic and source-first. It asks for source files and/or the journal website before asking configuration questions, detects as much of the journal setup as possible, asks only about gaps or conflicts, and returns only the fields needed for the selected OJS production task.

Article publication preparation models OJS fields explicitly: Prefix, Title, Subtitle and Abstract; publication details including Pages and article URL Path; Metadata fields such as Keywords, Supporting Agencies, Coverage, Rights, Source, Dublin Core Type and Data Availability Statement; article-level Publisher ID under Identifiers; and Galley Label/URL Path.

Generated article URL Paths now use a concise semantic summary of the title rather than slugifying the complete title. The default aims for roughly 3–6 distinctive terms, checks for collisions, and adds only the smallest useful distinguishing term when necessary. Established journal URL conventions take precedence.

The skill explicitly prevents DOI values from being misused as Publisher IDs or as the submission's own Source. Article publication URL Path and Galley URL Path remain separate scopes.

Issue preparation remains organised into the OJS-facing scopes **Issue Data**, **Issue Galley**, and **Identifiers**. Publisher IDs for articles, issues and issue galleys are treated as separate optional conventions and are detected before the skill asks the editor about them.

## Repository documentation

The repository root contains expanded design references, workflow guides, schemas, templates, examples and acceptance criteria. See `docs/ARTICLE-PUBLICATION-METADATA.md` for the article metadata and URL Path rules.
