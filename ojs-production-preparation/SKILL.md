---
name: ojs-production-preparation
description: Prepare Open Journal Systems (OJS) articles and issues for production by inspecting uploaded files and journal websites first, detecting journal configuration automatically, extracting only the metadata needed for the selected task, generating only authorised editorial content, and validating publication readiness.
---

# OJS Production Preparation

Use this skill for OJS post-acceptance production work, including journal setup detection, article publication preparation, QuickSubmit preparation, article metadata extraction, author and affiliation parsing, declarations, references, issue preparation, issue descriptions, issue galleys, identifiers, OJS-safe HTML, DOI metadata preparation, and final production QA.

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
- OJS publication and metadata fields in use
- title prefix/subtitle conventions
- article URL Path convention
- article galley label and URL Path conventions
- article Publisher ID usage and pattern
- author metadata conventions
- received/revised/accepted/published history fields
- licence and copyright policy
- contact emails by function
- declarations and ethics requirements
- citation and reference conventions
- issue naming and issue URL conventions
- issue galley label and URL conventions
- whether Publisher IDs are used for issues, issue galleys, articles, or any combination

Track provenance and confidence internally. If sources conflict, flag the conflict instead of silently choosing the value that seems most plausible.

## Gap questionnaire

After inspecting sources, ask only about:

- missing required configuration
- conflicting values
- low-confidence values that materially affect production
- journal preferences that cannot be inferred
- fields the user wants included, ignored or generated
- whether a Publisher ID is used for the article if this cannot be detected
- whether a Publisher ID is used for the issue itself if this cannot be detected
- whether a Publisher ID is used for issue galleys if this cannot be detected
- the Publisher ID pattern/value only when the relevant scope uses one and it cannot be inferred reliably

Do not ask for confirmation of every high-confidence value unless the user requests a full profile review.

Treat article-level Publisher ID, issue-level Publisher ID and issue-galley Publisher ID as separate fields. Never assume that because one exists the others also exist.

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

For routine article publication or QuickSubmit work, preserve the field order used by the user's OJS installation when known and return compact field/value output plus important warnings.

## Article publication preparation

When preparing an existing OJS submission for publication, organise the relevant values into the OJS-facing scopes below. Do not mix the meaning of fields merely because their values look similar.

### Title and abstract

Prepare:

- **Prefix**
- **Title**
- **Subtitle**
- **Abstract**

#### Prefix, Title and Subtitle rule

Use the first colon in the published title as the Title/Subtitle separator when the journal follows this convention.

- Anything before the first colon belongs to the main title.
- Anything after the first colon belongs to the Subtitle.
- Do not include the separating colon in either field.
- If the main title begins with the standalone article `A`, `An` or `The`, move that leading article into **Prefix** and remove it from **Title**.
- A leading `A`, `An` or `The` inside the Subtitle stays in the Subtitle and is not a Prefix.
- If there is no subtitle colon, leave Subtitle blank.
- If there is no leading main-title article, leave Prefix blank.

Example:

`The Effects of X on Y: A Systematic Review`

becomes:

- Prefix: `The`
- Title: `Effects of X on Y`
- Subtitle: `A Systematic Review`

Reconstructing `Prefix + Title + ": " + Subtitle` should reproduce the published title, aside from permitted extraction cleanup.

### Publication details

When requested, prepare article publication details including:

- **Pages**
- **URL Path**
- **Date Published**
- **DOI**
- **Issue assignment**

#### Article URL Path rule

If the journal already has an established article URL Path convention, follow it.

Otherwise, when generating a URL Path, create a **concise semantic summary of the title rather than a slugified copy of the full title**.

Default behaviour:

- aim for roughly 3–6 meaningful terms
- use lowercase terms separated by hyphens
- preserve the most distinctive topic, intervention/exposure, population, condition or geographic context
- omit filler words and leading articles when they add no meaning
- omit generic title scaffolding when the remaining terms still identify the article clearly
- use a familiar abbreviation when it is well established by the title/article context and improves readability
- do not automatically include the subtitle merely because it exists
- do not reproduce every meaningful word from the title
- do not add DOI, pages, volume or issue merely for uniqueness unless the journal convention requires it
- check for collisions; when a collision occurs, add the smallest useful distinguishing term rather than expanding to the full title

Example:

`Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

Preferred generated URL Path:

`tbi-management-nigeria`

Do not generate the full-title slug.

### Metadata

Prepare only enabled metadata fields. Use these OJS meanings:

#### Keywords

Keywords are typically one- to three-word phrases indicating the main topics of a submission.

- Preserve author-supplied keywords exactly in source-faithful extraction modes, even if an author's keyword is longer than three words.
- If keyword generation is explicitly authorised because the source has none, prefer concise one- to three-word phrases.
- Do not turn the abstract into a long list of phrases merely to fill the field.

#### Supporting Agencies

Supporting Agencies indicate research funding or other institutional support that facilitated the research.

- Extract explicit funders, sponsors, grant agencies or other stated institutional support.
- Do not copy author affiliations into Supporting Agencies merely because an institution appears in the author list.
- If no support is stated, follow the journal's missing-value policy.

#### Coverage

Coverage typically describes one or more of:

- spatial location, such as a place name or geographic coordinates
- temporal period, such as a period label, date or date range
- jurisdiction, such as a named administrative entity

Do not use Coverage as a general subject/topic field. Extract or infer geographic, temporal or jurisdictional coverage only when supported and allowed by the profile.

#### Rights

Rights record rights held over the submission, including copyright, intellectual-property rights, licence rights or other property rights.

- Prefer explicit article rights statements and the journal's authoritative rights/licence policy according to source precedence.
- Keep copyright holder, copyright year, licence name and licence URL conceptually distinct even if OJS presents a single Rights field.
- Do not put a DOI in Rights.

#### Source

Source identifies another work or resource from which the submission is derived. It may be an identifier, including a DOI, for that source work.

- Do not put the submission's own DOI in Source.
- Do not automatically put the journal citation in Source.
- Populate Source only when the submission is explicitly derived from, based on, translated from, reproduced from, or otherwise linked as a derivative of another identifiable work/resource, or when the journal has an established Source convention.
- If no such source exists, leave the field blank according to the profile.

#### Type

Type describes the nature or genre of the submission's main content using Dublin Core-style resource types.

- For a conventional journal article, editorial, review, commentary or other textual manuscript, `Text` is normally the appropriate Type.
- Other valid types may include `Dataset`, `Image` or another Dublin Core type when the main content actually has that nature.
- Do not use the OJS editorial section name such as `Editorial`, `Original Article` or `Review Article` as Type merely because that is the submission's section.
- Section and Type are separate concepts.

#### Data Availability Statement

This is a short statement describing whether the authors made the research data available and, if so, where readers can access it.

- Extract the authors' statement when present.
- Do not invent a data-sharing claim, repository, accession number or availability status.
- Do not convert the absence of a statement into `Data not available` unless the journal explicitly authorises that wording.

### Identifiers

Prepare:

- **Publisher ID** — only if the journal uses an article-level Publisher ID

The Publisher ID may record an identifier from an external database or deposit workflow, for example an identifier associated with PubMed export/deposit.

**Never use Publisher ID for the submission DOI.** DOI and Publisher ID are separate identifiers.

Detect article-level Publisher ID usage and pattern from OJS exports, existing submissions, screenshots or journal documentation. If use cannot be determined, ask whether the journal uses it. If not used, omit it.

### Galleys

For an article galley, the core OJS information to prepare is:

- **Galley Label** — required when OJS requires it
- **URL Path**

The galley file itself may already be supplied as part of production. If so, treat that PDF/HTML/XML file as the candidate galley without asking for it again.

Detect the journal's established Galley Label and URL Path conventions before generating or recommending values. A normal PDF galley commonly uses `PDF` as the label, but preserve the journal's actual convention.

Do not assume an article Galley URL Path should equal the article's own publication URL Path. The concise-title-summary rule applies to generated article publication URL Paths, not automatically to Galley URL Paths.

## QuickSubmit workflow

For QuickSubmit, potential fields include:

- Section
- Prefix
- Title
- Subtitle
- Abstract
- Coverage
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

Apply the article-publication metadata definitions above. In particular, do not confuse Section with Type, the submission DOI with Source, or DOI with Publisher ID.

## Article metadata rules

Titles, abstracts, keywords, references and declarations must come from the supplied source unless the user explicitly requests copyediting or generation.

Preserve structured abstract labels such as Background, Methods, Results and Conclusion.

Do not invent article DOI, ORCID, funding, ethics approval, data availability, affiliation, corresponding-author details or Publisher ID when they are absent.

If an article section/type is inferred rather than explicitly stated, mark it as inferred. A Dublin Core Type of `Text` may be marked inferred when the supplied main content is unambiguously a textual journal submission.

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

This is distinct from both the article-level Publisher ID and the Publisher ID attached to an Issue Galley. Detect the convention first. If it cannot be determined, ask whether the journal uses an issue-level Publisher ID and request or infer the established pattern only when the answer is yes.

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

- Prefix/Title/Subtitle parsing and reconstruction
- generated article URL Path that simply reproduces the full title
- generated article URL Path that is too vague to identify the article
- duplicate/colliding article URL Paths
- own DOI incorrectly entered as Source
- DOI incorrectly entered as Publisher ID
- Section incorrectly substituted for Dublin Core Type
- Supporting Agencies incorrectly populated from affiliations without support evidence
- Coverage used as a topic rather than spatial/temporal/jurisdictional metadata
- unsupported or invented Data Availability statements
- article galley label
- article galley URL Path when configured
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
