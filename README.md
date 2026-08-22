# OJS Production Preparation Skill

A journal-agnostic production assistant for Open Journal Systems (OJS).

The skill is designed for the post-acceptance production workflow: understand a journal first, prepare issue and article metadata, support QuickSubmit, generate only authorised editorial content, and validate the final production package before publication.

## Core principle

**Sources first, questions second.**

The skill should never start by dumping a long questionnaire on the editor. It first asks for the available journal materials, inspects them, detects as much configuration as it can, builds a provisional journal profile, and then asks only about unresolved, conflicting, or preference-based items.

Typical source material includes:

- journal website URL
- sample published article PDFs
- a complete published issue
- final accepted manuscripts
- submission guidelines
- editorial and publication policy documents
- journal setup/onboarding documents
- production spreadsheets
- branding files and logos

## What it can prepare

- journal production profile
- OJS issue metadata
- issue descriptions and summaries
- issue cover briefs
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
- `docs/WORKFLOW.md` — end-to-end production workflow
- `docs/DETECTION.md` — automatic detection and confidence rules
- `templates/journal-profile.yaml` — reusable journal configuration
- `templates/task-request.yaml` — per-task overrides
- `templates/qa-report.yaml` — validation/reporting structure

## Design goals

- General OJS support, not tied to one journal
- Minimal questioning
- No unnecessary metadata
- No silent invention of article metadata
- Explicit source provenance
- Strong discrepancy detection
- Conservative OJS-compatible HTML
- Reusable journal profiles
- Per-task overrides

## Status

Initial skill architecture and production workflow are being implemented on the `feature/initial-ojs-production-skill` branch.
