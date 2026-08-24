# OJS Production Preparation Skill

A journal-agnostic production assistant for Open Journal Systems (OJS).

The skill is designed for post-acceptance production: understand a journal first, prepare issue and article metadata, support QuickSubmit and existing-submission publication preparation, generate only authorised editorial content, and validate the final production package before publication.

## Core principle

**Sources first, questions second.**

The skill inspects available journal files, OJS exports/screenshots and website evidence before asking only about unresolved, conflicting or preference-based items.

## Article publication response order

Existing-submission article preparation now uses this fixed editor-facing order by default:

1. **Title & Abstract**
2. **Contributors**
3. **Metadata**
4. **References**
5. **Galleys**
6. **Issue**

The References step includes the full copy-ready list when source references are available. The Issue step keeps Issue assignment, Pages, Date Published, DOI and the concise article URL Path together. Article Publisher ID, when actually used, is displayed under Metadata rather than as a separate seventh top-level group.

## Important metadata distinctions

- **Section** is the OJS editorial section; **Type** is a Dublin Core-style content type such as `Text`.
- **Source** identifies another work/resource from which the submission is derived; it is not the submission's own DOI.
- **Publisher ID** is an optional external database/deposit identifier and must not be used for DOI.
- **Coverage** is spatial, temporal or jurisdictional metadata rather than a generic subject field.
- **Supporting Agencies** records funding or other explicit research support, not ordinary affiliations.
- Article URL Path, article Galley URL Path, Issue Data URL Path and Issue Galley URL Path are distinct scopes.

## URL Path behaviour

Generated article URL Paths use a concise semantic summary rather than a slugified copy of the complete title. The default aims for roughly 3–6 meaningful terms, preserves the article's distinctive topic/context, checks for collisions and adds only the smallest useful distinguishing term when needed.

Example:

`Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

becomes:

`tbi-management-nigeria`

## What it can prepare

- journal production profile
- OJS issue metadata
- Issue Data, Issue Galley and issue Identifiers
- issue descriptions and summaries
- existing-submission article publication records
- Prefix, Title, Subtitle and Abstract
- contributors and affiliations
- OJS Metadata fields
- complete reference lists
- article galleys
- article Publisher ID when used
- article history and declarations
- QuickSubmit metadata
- rights/licence metadata
- pagination and DOI checks
- OJS-safe HTML
- discrepancy reports and final readiness checks

## Repository structure

- `SKILL.md` — primary skill instructions
- `ojs-production-preparation/` — installable runtime skill package
- `docs/WORKFLOW.md` — end-to-end production workflow
- `docs/DETECTION.md` — automatic detection and confidence rules
- `docs/ARTICLE-PUBLICATION-METADATA.md` — article field semantics, response order and URL Path rules
- `templates/journal-profile.yaml` — reusable journal configuration
- `templates/task-request.yaml` — per-task overrides
- `templates/qa-report.yaml` — validation/reporting structure
- `tests/ACCEPTANCE.md` — core behavioural acceptance scenarios
- `tests/ARTICLE-METADATA-ACCEPTANCE.md` — article-publication acceptance scenarios

## Status

Installable runtime version: **1.0.4**.
