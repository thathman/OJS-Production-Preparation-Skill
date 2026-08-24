# OJS Production Preparation Skill

A journal-agnostic production assistant for Open Journal Systems (OJS).

The skill is designed for the post-acceptance production workflow: understand a journal first, prepare issue and article metadata, support QuickSubmit and existing-submission publication preparation, generate only authorised editorial content, and validate the final production package before publication.

## Core principle

**Sources first, questions second.**

The skill should never start by dumping a long questionnaire on the editor. It first asks for the available journal materials, inspects them, detects as much configuration as it can, builds a provisional journal profile, and then asks only about unresolved, conflicting, or preference-based items.

Typical source material includes:

- journal website URL
- sample published article PDFs
- a complete published issue
- final accepted manuscripts
- OJS exports or screenshots
- submission guidelines
- editorial and publication policy documents
- journal setup/onboarding documents
- production spreadsheets
- branding files and logos

## What it can prepare

- journal production profile
- OJS issue metadata
- Issue Data, Issue Galley and issue Identifiers
- issue descriptions and summaries
- issue cover briefs
- existing-submission article publication records
- Prefix, Title, Subtitle and Abstract
- OJS metadata: Keywords, Supporting Agencies, Coverage, Rights, Source, Type and Data Availability Statement
- article Publisher ID when the journal uses an external-database/deposit identifier
- article Galley Label and URL Path
- QuickSubmit article metadata
- author and affiliation metadata
- article history dates
- declarations and funding metadata
- references
- rights and licence metadata
- pagination/article-number checks
- OJS-safe HTML
- metadata discrepancy reports
- final issue readiness checks

## Important metadata distinctions

The skill keeps semantically different OJS fields separate:

- **Section** is the OJS editorial section; **Type** is a Dublin Core-style content type such as `Text`.
- **Source** identifies another work/resource from which the submission is derived; it is not the submission's own DOI.
- **Publisher ID** is an optional external database/deposit identifier and must not be used for DOI.
- **Coverage** is spatial, temporal or jurisdictional metadata rather than a generic subject field.
- **Supporting Agencies** records funding or other explicit research support, not ordinary affiliations.
- Article URL Path, article Galley URL Path, Issue Data URL Path and Issue Galley URL Path are distinct scopes.

## Behaviour model

The skill separates fields by policy:

- `extract_only` — never invent or rewrite
- `extract_or_blank` — extract if present, otherwise leave blank
- `extract_or_flag` — extract if present, otherwise flag for review
- `generate_if_missing` — generate only if no source value exists and generation is allowed
- `generate_always` — editorially generated field
- `ignore` — do not extract or display

It also separates extraction from editorial generation. Article metadata is normally source-faithful. Issue-level presentation can be generated when authorised.

## First-run flow

1. Ask for available files and/or the journal website.
2. Inspect the sources.
3. Detect journal configuration and production conventions.
4. Identify conflicts and confidence levels.
5. Build a provisional journal profile.
6. Ask only the gap questionnaire.
7. Confirm the profile.
8. Ask which production task to perform.
9. Return only the configured fields for that task.
10. Validate against the sources before publication.

## Repository structure

- `SKILL.md` — primary skill instructions
- `ojs-production-preparation/` — installable runtime skill package
- `docs/WORKFLOW.md` — end-to-end production workflow
- `docs/DETECTION.md` — automatic detection and confidence rules
- `docs/ARTICLE-PUBLICATION-METADATA.md` — OJS article metadata, Identifiers and Galley semantics
- `templates/journal-profile.yaml` — reusable journal configuration
- `templates/task-request.yaml` — per-task overrides
- `templates/qa-report.yaml` — validation/reporting structure
- `tests/ACCEPTANCE.md` — core behavioural acceptance scenarios
- `tests/ARTICLE-METADATA-ACCEPTANCE.md` — article-publication metadata acceptance scenarios

## Design goals

- General OJS support, not tied to one journal
- Minimal questioning
- No unnecessary metadata
- No silent invention of article metadata
- Explicit source provenance
- Strong discrepancy detection
- Correct OJS field semantics
- Conservative OJS-compatible HTML
- Reusable journal profiles
- Per-task overrides

## Status

Installable runtime version: **1.0.2**.

The current runtime includes issue preparation and explicit existing-submission article publication preparation, including title/prefix/subtitle parsing, OJS metadata semantics, article-level Publisher ID handling and article Galley Label/URL Path preparation.
