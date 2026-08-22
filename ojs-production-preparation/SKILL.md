---
name: ojs-production-preparation
description: Prepare Open Journal Systems (OJS) articles and issues for production by inspecting uploaded files and journal websites first, detecting journal configuration automatically, extracting only the metadata needed for the selected task, generating only authorised editorial content, and validating publication readiness.
---

# OJS Production Preparation

Use this skill for OJS post-acceptance production work, including journal setup detection, QuickSubmit preparation, article metadata extraction, author and affiliation parsing, declarations, references, issue preparation, issue descriptions, OJS-safe HTML, DOI metadata preparation, and final production QA.

## Prime directive

Ask for sources first. Detect first. Ask second.

If the user has already supplied useful files or a journal website, inspect them immediately. Do not ask the user to repeat information that can be determined reliably from those sources.

If no useful source has been supplied, ask the user to provide whatever is available, such as:

- journal website URL
- sample published article PDFs
- complete issue PDFs
- final accepted manuscripts
- submission guidelines
- policy or setup documents
- production spreadsheets
- logos or branding files

Do not require every source type.

## Source-first journal detection

Before asking configuration questions, attempt to detect:

- journal name, abbreviation, publisher and website
- ISSN/eISSN and DOI prefix when present
- publication model, frequency, volume/issue conventions, pagination or article-number model
- article sections/types in actual use
- OJS metadata fields in use
- author metadata conventions
- received/revised/accepted/published history fields
- licence and copyright policy
- contact emails by function
- declarations and ethics requirements
- citation and reference conventions
- issue naming and URL conventions

Track provenance and confidence internally. If sources conflict, flag the conflict instead of silently choosing the value that seems most plausible.

## Gap questionnaire

After inspecting sources, ask only about:

- missing required configuration
- conflicting values
- low-confidence values that materially affect production
- journal preferences that cannot be inferred
- fields the user wants included, ignored or generated

Do not ask for confirmation of every high-confidence value unless the user requests a full profile review.

## Extraction modes

Use `clean` by default unless the user or journal profile specifies otherwise.

- `strict`: preserve source wording exactly, including grammar, spelling and capitalization.
- `clean`: repair OCR/layout artefacts such as duplicated lines, page headers, broken line wraps and obvious line-break hyphenation without rewriting authored wording.
- `copyedited`: correct language and formatting while preserving meaning. Use only when explicitly allowed.
- `hybrid`: return clean extraction and separately flag copyediting suggestions.

Do not silently turn authored wording into improved prose during metadata extraction.

## Field policies

Treat each configured field as one of:

- `extract_only`: never generate or rewrite.
- `extract_or_blank`: extract if present, otherwise leave blank.
- `extract_or_flag`: extract if present, otherwise flag for review.
- `generate_if_missing`: generate only when absent and generation is authorised.
- `generate_always`: editorially generated content.
- `ignore`: do not extract, request or display.

Article title, author list, abstract and references should normally be source-faithful. Issue descriptions, summaries, cover concepts and URL slugs may be generated only when authorised.

## Output discipline

Return only the information required for the active task and journal profile. Do not dump every value detected simply because it is available.

For routine QuickSubmit work, default to compact field/value output plus important warnings. Preserve the field order used by the user's OJS installation when known.

Potential QuickSubmit fields include:

- Section
- Prefix
- Title
- Subtitle
- Abstract
- Coverage Information
- Type
- Source
- Rights
- Data Availability Statement
- Subjects
- Disciplines
- Keywords
- Supporting Agencies
- References

Do not include disabled or ignored fields.

## Article metadata rules

Titles, abstracts, keywords, references and declarations must come from the supplied source unless the user explicitly requests copyediting or generation.

Preserve structured abstract labels such as Background, Methods, Results and Conclusion.

Do not invent article type, DOI, ORCID, funding, ethics approval, data availability, affiliation or corresponding-author details when they are absent.

If an article section/type is inferred rather than explicitly stated, mark it as inferred.

## Author and affiliation rules

Preserve author order exactly. Map superscript affiliations, corresponding-author markers, ORCIDs, emails and phone numbers where possible.

Do not guess how ambiguous multi-part names should be split. Flag ambiguous name parsing for review.

## Issue preparation

When generating issue-level content, inspect every article assigned to the issue first. Build an internal article map covering article type, main topic, methods/level of translation and major contribution.

A whole-issue description must represent the whole issue. Do not generate a general issue description from one article unless the user explicitly requests a narrow focus.

Support short, standard three-paragraph, detailed and custom-length issue descriptions.

## References

Support exact, clean, normalize and validate modes. Do not insert missing DOIs unless DOI enrichment is explicitly enabled.

## OJS-safe HTML

When styling content for OJS, prefer conservative HTML and inline CSS. Use simple elements such as `div`, `p`, `strong`, `em`, lists and simple tables. Avoid JavaScript, external stylesheets, external fonts and framework-dependent markup.

Preserve scientific typography and meaning, including species italics, Greek characters, subscripts and superscripts where appropriate.

## Production QA

Actively check configured requirements for:

- title mismatches
- author order or spelling mismatches
- affiliation/corresponding-author mismatches
- abstract mismatches
- inconsistent doses, units, organisms or abbreviations
- licence text versus licence URL mismatch
- journal URL/name mismatch
- volume/issue/year mismatch
- pagination overlap or duplicate article numbers
- malformed references
- missing required declarations
- DOI formatting conflicts
- conflicting publication dates

Do not automatically correct scientific discrepancies. Report them for editorial review.

For issue QA, return one of:

- `ready`
- `ready_with_warnings`
- `not_ready`

Do not mark an issue ready while a configured blocking requirement is unresolved.

## Supported tasks

Use the smallest task that satisfies the request:

- prepare issue
- prepare one QuickSubmit article
- prepare batch QuickSubmit metadata
- extract article metadata
- extract authors/affiliations
- extract declarations
- prepare or validate references
- validate existing OJS metadata
- generate issue description
- generate cover brief
- style OJS HTML
- prepare DOI metadata
- run production QA
- run full production workflow

User instructions for the current task override profile defaults for that run.
