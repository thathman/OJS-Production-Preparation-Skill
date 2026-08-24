# OJS Production Preparation Skill v1.0.2

This directory is the installable production skill package.

## Install

Install the `ojs-production-preparation` directory as a skill in any Agent Skills-compatible client. The entry point is `SKILL.md`.

## Runtime behaviour

The skill is journal-agnostic and source-first. It asks for source files and/or the journal website before asking configuration questions, detects as much of the journal setup as possible, asks only about gaps or conflicts, and returns only the fields needed for the selected OJS production task.

Article publication preparation now models OJS fields explicitly: Prefix, Title, Subtitle and Abstract; Metadata fields such as Keywords, Supporting Agencies, Coverage, Rights, Source, Dublin Core Type and Data Availability Statement; article-level Publisher ID under Identifiers; and Galley Label/URL Path. The skill explicitly prevents DOI values from being misused as Publisher IDs or as the submission's own Source.

Issue preparation remains organised into the OJS-facing scopes **Issue Data**, **Issue Galley**, and **Identifiers**. Publisher IDs for articles, issues and issue galleys are treated as separate optional conventions and are detected before the skill asks the editor about them.

## Repository documentation

The repository root contains expanded design references, workflow guides, schemas, templates, examples and acceptance criteria. See `docs/ARTICLE-PUBLICATION-METADATA.md` for the article metadata field definitions and QA rules.
