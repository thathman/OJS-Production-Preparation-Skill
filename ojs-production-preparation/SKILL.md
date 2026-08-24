---
name: ojs-production-preparation
description: Prepare Open Journal Systems (OJS) articles and issues for production by inspecting uploaded files and journal websites first, detecting journal configuration automatically, extracting only the metadata needed for the selected task, generating only authorised editorial content, and validating publication readiness.
---

# OJS Production Preparation

Use this skill for OJS post-acceptance production work, including journal setup detection, existing-submission article publication preparation, QuickSubmit preparation, article metadata extraction, contributors, declarations, references, issue preparation, issue galleys, identifiers, OJS-safe HTML, DOI metadata preparation, and production QA.

## Prime directive

**Sources first. Detect first. Ask second.**

If the user has already supplied useful files or a journal website, inspect them immediately. Do not ask the user to repeat information that can be determined reliably from those sources.

If no useful source has been supplied, ask for whatever is available, such as the journal website, final article PDFs, complete issue PDFs, accepted manuscripts, submission guidelines, OJS exports/screenshots, production spreadsheets, or policy/setup documents. Do not require every source type.

## Source-first detection

Before asking configuration questions, attempt to detect:

- journal name, abbreviation, publisher, website, ISSN/eISSN and DOI prefix
- publication model, frequency, volume/issue conventions and pagination model
- article sections/types in use
- OJS publication and metadata fields in use
- Prefix/Title/Subtitle convention
- article URL Path convention
- article Galley Label and Galley URL Path conventions
- article Publisher ID usage and pattern
- author/contributor metadata conventions
- article history fields
- rights/licence and copyright policy
- declarations, funding and data-availability requirements
- reference/citation conventions
- issue naming, URL Path, galley and Publisher ID conventions

Track provenance and confidence internally. If sources conflict, flag the conflict instead of silently choosing a value.

## Gap questionnaire

After detection, ask only about missing required configuration, material conflicts, low-confidence production-impacting values, or journal preferences that cannot be inferred.

Publisher ID questions are conditional. Article-level Publisher ID, issue-level Publisher ID and Issue Galley Publisher ID are separate fields. Never assume that because one exists the others exist, and never use a DOI as a Publisher ID.

## Extraction modes

Use `clean` by default unless configured otherwise.

- `strict`: preserve source wording exactly.
- `clean`: repair OCR/layout artefacts without rewriting authored wording.
- `copyedited`: correct language/formatting while preserving meaning; use only when explicitly authorised.
- `hybrid`: return clean extraction and separately flag suggested editorial corrections.

Do not silently turn source-faithful metadata into improved prose.

## Field policies

Each field may use:

- `extract_only`
- `extract_or_blank`
- `extract_or_flag`
- `generate_if_missing`
- `generate_always`
- `ignore`

Do not invent article title, authors, abstract, DOI, ORCID, funding, ethics approval, data availability, affiliation, corresponding-author details, Publisher ID, or references merely to avoid empty fields.

## Required article response order

For **article publication preparation**, the user-facing response must be grouped in this exact order unless the user explicitly asks for another structure:

1. **Title & Abstract**
2. **Contributors**
3. **Metadata**
4. **References**
5. **Galleys**
6. **Issue**

Do not move Issue details earlier, put References at the end, or create a separate top-level Identifiers section in this six-step workflow.

### 1. Title & Abstract

Include, when applicable:

- **Section**
- **Prefix**
- **Title**
- **Subtitle**
- **Abstract**

#### Prefix, Title and Subtitle

When the journal uses a colon to separate title and subtitle:

- split on the first colon
- remove the separating colon from both fields
- everything before the colon is the main title
- everything after the colon is the Subtitle
- if the main title begins with standalone `A`, `An` or `The`, move that word to Prefix
- a leading `A`, `An` or `The` in the Subtitle remains part of the Subtitle
- if there is no colon, leave Subtitle blank
- if there is no main-title Prefix, leave Prefix blank

Example:

`The Effects of X on Y: A Systematic Review`

becomes:

- Prefix: `The`
- Title: `Effects of X on Y`
- Subtitle: `A Systematic Review`

The reconstructed display title should match the published title apart from permitted cleanup.

Preserve structured abstract labels such as Background, Introduction, Methods, Materials and Methods, Results, Discussion and Conclusion.

### 2. Contributors

List every contributor in publication order. Include only source-supported fields such as:

- published/preferred name
- given name
- middle name or initials
- family name
- email
- affiliation
- country
- ORCID
- corresponding-author status

Preserve author order exactly. Map superscript affiliations and corresponding-author markers where possible. Do not guess ambiguous personal-name splits or missing emails/ORCIDs.

### 3. Metadata

Include enabled fields such as:

- **Keywords**
- **Supporting Agencies**
- **Coverage**
- **Rights**
- **Source**
- **Type**
- **Data Availability Statement**
- **Publisher ID**, only when the journal uses an article-level Publisher ID

Publisher ID remains semantically an identifier, but in this required six-step user-facing workflow place it under **Metadata** rather than creating a separate top-level Identifiers group.

#### Keywords

Keywords are typically one- to three-word phrases indicating the main topics of a submission. Preserve author-supplied keywords exactly even when longer. If generation is explicitly authorised because keywords are missing, prefer concise one- to three-word phrases.

#### Supporting Agencies

Use for explicit research funding, sponsors, grant agencies, or other stated institutional support. Do not copy author affiliations into Supporting Agencies merely because the institution appears in the author list.

#### Coverage

Coverage should describe spatial location, temporal period, or jurisdiction. Do not use Coverage as a generic topic/subject field.

#### Rights

Rights may include copyright, intellectual-property rights, licence rights, or other property rights. Prefer authoritative article/journal rights evidence according to source precedence. Do not put DOI metadata in Rights.

#### Source

Source identifies another work/resource from which the submission is derived. It may contain the DOI or identifier of that **source work**. Do not put the submission's own DOI in Source and do not automatically copy the article citation there.

#### Type

Type is the Dublin Core-style nature/genre of the main content. For conventional articles, reviews, editorials and commentaries, `Text` is normally appropriate. Keep OJS **Section** separate from **Type**: `Editorial` or `Original Articles` may be sections while the Type remains `Text`.

#### Data Availability Statement

Extract the authors' statement when present. Never invent a repository, accession number, availability status, or data-sharing claim. Do not convert absence into `Data not available` unless journal policy explicitly authorises a standard statement.

#### Publisher ID

Publisher ID may record an external database/deposit identifier. It is **not a DOI field**. Detect journal usage/pattern first; if unused, omit it.

### 4. References

When references are available in the supplied source, **paste the complete copy-ready reference list**. Do not merely report the reference count or say that references should be entered.

Support:

- `exact`
- `clean`
- `normalize`
- `validate`

In `clean` mode, repair PDF line-wrap/layout artefacts without changing substantive bibliographic content. Do not search for or insert missing DOIs unless explicitly enabled.

Flag numbering anomalies, unnumbered references, duplicates, malformed entries, or conflicting bibliographic details. Do not silently renumber or rewrite the source unless the active mode authorises it.

### 5. Galleys

Include:

- **Galley file/candidate file** when known
- **Galley Label**
- **URL Path**

If the final article PDF/HTML/XML has already been supplied, treat it as the candidate galley and do not ask for the same file again.

Detect the journal's label/path convention before generating values. `PDF` is common but must not override an established convention.

Keep Galley URL Path distinct from the article publication URL Path. The concise semantic-title rule below applies to the article URL Path, not automatically to the Galley URL Path.

### 6. Issue

Include, when applicable:

- **Issue assignment**
- **Pages**
- **Date Published**
- **DOI**
- **Article URL Path**

Do not forget Pages or the article URL Path when those fields are part of the OJS publication screen.

#### Article URL Path

If the journal already has an established article URL Path convention, follow it.

Otherwise generate a **concise semantic summary of the title rather than a slugified copy of the full title**.

Default behaviour:

- aim for roughly 3–6 meaningful terms
- lowercase, hyphen-separated
- preserve the most distinctive topic, intervention/exposure, population, condition, or geographic context
- omit filler words and generic scaffolding when the remaining terms still identify the article
- use a familiar abbreviation when clearly established and more readable
- do not automatically include the subtitle
- do not reproduce every meaningful word from the title
- do not add DOI/pages/volume/issue merely for uniqueness unless the journal convention requires it
- check for collisions and add only the smallest useful distinguishing term

Example:

`Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

Preferred generated URL Path:

`tbi-management-nigeria`

Do not generate the full-title slug.

## QuickSubmit

For QuickSubmit, preserve the field order used by the user's OJS installation when known. Potential fields include Section, Prefix, Title, Subtitle, Abstract, Coverage, Type, Source, Rights, Data Availability Statement, Subjects, Disciplines, Keywords, Supporting Agencies and References.

Do not include disabled/ignored fields. Apply the semantic rules above: Section is not Type, the submission DOI is not Source, and DOI is not Publisher ID.

## Issue preparation

For `prepare_issue`, organise issue-level output into the OJS-facing scopes below.

### Issue Data

- Date Published
- Identification: Volume, Number, Year, Title
- Description
- Cover image alternate text
- URL Path

### Issue Galley

- Issue Galley file
- Galley Label
- Publisher ID only if the journal uses an Issue Galley Publisher ID
- URL Path

### Identifiers

- Publisher ID only if the journal uses an issue-level Publisher ID

Treat Issue Data URL Path and Issue Galley URL Path as separate fields. Treat issue-level Publisher ID and Issue Galley Publisher ID as separate fields. Do not invent either.

When a complete final issue PDF is already supplied, use it as the candidate Issue Galley.

Before generating an issue description, inspect every article assigned to the issue and build an internal article map so the description represents the whole issue.

## OJS-safe HTML

When styling content for OJS, use conservative HTML and inline CSS. Prefer simple `div`, `p`, `strong`, `em`, lists and simple tables. Avoid JavaScript, external stylesheets/frameworks/fonts. Preserve scientific typography, species italics, Greek characters, subscripts and superscripts.

## Production QA

Check configured requirements and flag at minimum:

- wrong article response-group order
- missing Pages or article URL Path when required
- Prefix/Title/Subtitle reconstruction mismatch
- full-title article URL slug instead of a concise semantic path
- vague or colliding article URL Paths
- missing full reference list when source references are available
- reference numbering anomalies or malformed entries
- submission DOI used as Source
- DOI used as Publisher ID
- Section used as Dublin Core Type
- unsupported Supporting Agency values
- invalid Coverage semantics
- invented Data Availability statements
- missing/incorrect Galley Label or Galley URL Path
- title/author/order/corresponding-author mismatches
- abstract mismatch
- scientific value, dose, unit or organism conflicts
- licence text/URL mismatch
- volume/issue/year or pagination conflicts
- DOI-format and publication-date conflicts

Do not automatically correct scientific discrepancies. Report them for editorial review.

For issue QA, return `ready`, `ready_with_warnings`, or `not_ready`. Do not mark an issue ready while a configured blocking requirement remains unresolved.

## Supported tasks

Use the smallest task that satisfies the request:

- prepare issue
- prepare article publication record
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