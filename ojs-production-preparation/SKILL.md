---
name: ojs-production-preparation
description: Prepare Open Journal Systems (OJS) articles and issues for production by inspecting uploaded files and journal websites first, detecting journal configuration automatically, extracting only the metadata needed for the selected task, generating only authorised editorial content, and validating publication readiness.
---

# OJS Production Preparation

Use this skill for OJS post-acceptance production work, including journal setup detection, QuickSubmit preparation, article metadata extraction, author and affiliation parsing, declarations, references, issue preparation, issue descriptions, issue galleys, identifiers, OJS-safe HTML, DOI metadata preparation, and final production QA.

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
- issue naming and issue URL conventions
- issue galley label and URL conventions
- whether Publisher IDs are used for issues, issue galleys, or both

Track provenance and confidence internally. If sources conflict, flag the conflict instead of silently choosing the value that seems most plausible.

## Gap questionnaire

After inspecting sources, ask only about:

- missing required configuration
- conflicting values
- low-confidence values that materially affect production
- journal preferences that cannot be inferred
- fields the user wants included, ignored or generated
- whether a Publisher ID is used for the issue itself if this cannot be detected
- whether a Publisher ID is used for issue galleys if this cannot be detected
- the Publisher ID pattern/value only when the journal uses one and it cannot be inferred reliably

Do not ask for confirmation of every high-confidence value unless the user requests a full profile review.

Treat issue-level Publisher ID and issue-galley Publisher ID as separate fields. Never assume that because one exists the other also exists.

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

When preparing an OJS issue, organise the output into the same three scopes the editor encounters in OJS.

### Issue Data

Prepare only these core fields unless the journal profile explicitly enables more:

- **Date Published**
- **Identification**
  - Volume
  - Number
  - Year
  - Title
- **Description**
- **Cover image**
  - Alternate text
- **URL Path**

The cover image file itself may be supplied by the user or detected among uploaded assets. Do not generate a replacement image unless explicitly requested. Generate or propose alternate text when needed and authorised.

### Issue Galley

Prepare:

- **Issue Galley** — the issue galley file
- **Galley Label**
- **Publisher ID** — only if the journal uses one for issue galleys
- **URL Path**

If the final issue file has already been supplied, use it as the candidate galley and do not ask for it again. Detect the galley label, Publisher ID convention and URL path convention from existing published issues or OJS evidence where possible.

Do not invent a Publisher ID. If its use cannot be determined, ask whether the journal assigns Publisher IDs to issue galleys. If yes, ask for or infer the established pattern before preparing the value.

### Identifiers

Prepare:

- **Publisher ID** — only if the journal uses an issue-level Publisher ID

This is distinct from the Publisher ID attached to an Issue Galley. Detect the convention first. If it cannot be determined, ask whether the journal uses an issue-level Publisher ID and request or infer the established pattern only when the answer is yes.

### Whole-issue synthesis rule

When generating issue-level editorial content, inspect every article assigned to the issue first. Build an internal article map covering article type, main topic, methods/level of translation and major contribution.

A whole-issue description must represent the whole issue. Do not generate a general issue description from one article unless the user explicitly requests a narrow focus.

Support short, standard three-paragraph, detailed and custom-length issue descriptions.

### Issue URL path rule

Treat the Issue Data URL Path and the Issue Galley URL Path as separate fields. Detect each journal's established convention before generating either. Do not assume the same slug belongs in both fields.

## References

Support exact, clean, normalize and validate modes. Do not insert missing DOIs unless DOI enrichment is explicitly enabled.

## OJS-safe HTML

When styling content for OJS, prefer conservative HTML and inline CSS. Use simple elements such as `div`, `p`, `strong`, `em`, lists and simple tables. Avoid JavaScript, external stylesheets, external fonts and framework-dependent markup.

Preserve scientific typography and meaning, including species italics, Greek characters, subscripts and superscripts where appropriate.

## Production QA

Actively check configured requirements for:

- issue Date Published
- issue Volume, Number, Year and Title
- issue Description when required
- cover image alternate text when a cover is used
- issue URL Path
- issue galley file presence
- issue galley label
- issue galley URL Path
- issue-galley Publisher ID when configured
- issue-level Publisher ID when configured
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
