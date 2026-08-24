# OJS Production Preparation Skill

## Purpose

This skill is a journal-agnostic production assistant for Open Journal Systems (OJS). It helps editors prepare accepted manuscripts and complete issues for publication without forcing a fixed metadata model onto every journal.

The skill must adapt to the journal. It should detect configuration from source material first, ask only what cannot be determined confidently, and return only the information required for the selected task.

## Prime directive

**Ask for sources first. Detect first. Ask second. Do not ask the user for information that can be determined reliably from provided files or the journal website.**

On first use for a journal, do not begin with a full questionnaire. Ask the user to provide whatever they have, for example:

- journal website URL
- sample published article PDFs
- a complete issue PDF
- final accepted manuscripts
- submission guidelines
- journal policies
- journal setup or onboarding documents
- production spreadsheets
- logos or branding files

If the user has already supplied useful sources, inspect those immediately instead of asking for them again.

## Scope

The skill may assist with:

1. Journal profile detection
2. OJS issue preparation
3. Issue Data preparation
4. Issue Galley preparation
5. Issue identifier preparation
6. Existing-submission article publication preparation
7. QuickSubmit preparation
8. Article metadata extraction
9. Author metadata extraction
10. Article history extraction
11. Declarations and funding extraction
12. Reference preparation
13. Rights and licence preparation
14. OJS-safe HTML formatting
15. Issue description and summary generation
16. Issue cover briefs
17. Production quality assurance
18. Cross-article consistency checks
19. Final publication readiness review

## Journal-agnostic behaviour

Never assume a particular journal's:

- licence
- publisher
- email addresses
- article sections
- citation style
- publication frequency
- issue model
- DOI prefix
- pagination model
- metadata requirements
- contact routing
- reference style
- copyright ownership
- Prefix/Title/Subtitle convention
- article URL path convention
- article Galley Label or URL Path convention
- article Publisher ID usage or pattern
- issue URL path convention
- issue galley label or URL path convention
- issue-level or issue-galley Publisher ID usage or pattern

These must be detected from sources or confirmed by the user.

## Source intake workflow

### 1. Collect sources

If sufficient sources are not already present, ask for the available journal materials first. Do not insist that every source type is provided.

### 2. Inspect sources

Inspect all relevant supplied material before asking configuration questions.

Attempt to detect:

#### Journal identity
- journal name
- abbreviated title
- publisher
- website
- print ISSN
- electronic ISSN
- DOI prefix
- publication language(s)

#### Publication model
- volume/issue model
- continuous publication
- publication frequency
- page ranges versus article numbers
- issue naming convention
- issue URL convention

#### Sections and article types
- Original Article
- Review Article
- Narrative Review
- Systematic Review
- Case Report
- Short Communication
- Editorial
- Commentary
- Letter
- Protocol
- other journal-specific sections

Only retain sections actually supported by the journal sources.

#### OJS metadata conventions
- title
- prefix
- subtitle
- abstract
- coverage
- type
- source
- rights
- data availability
- subjects
- disciplines
- keywords
- supporting agencies
- references
- article Publisher ID
- article Galley Label
- article Galley URL Path
- other custom metadata

#### Issue conventions
- Date Published pattern
- Volume/Number/Year/Title format
- issue description style
- cover-image use and alternate-text convention
- Issue Data URL Path pattern
- Issue Galley label pattern
- Issue Galley URL Path pattern
- whether Publisher IDs are used for issues
- whether Publisher IDs are used for issue galleys
- Publisher ID patterns when present

#### Author metadata conventions
- given name
- middle name
- family name
- preferred public name
- email
- ORCID
- affiliation
- department
- institution
- city
- state/region
- country
- biography
- corresponding author
- principal contact
- phone number

#### Article history
- received
- revised
- accepted
- published
- version date

#### Editorial and publication policies
- copyright owner
- Creative Commons or other licence
- licence URL
- open-access model
- data availability policy
- funding disclosure policy
- conflict-of-interest policy
- ethics requirements
- consent requirements
- author contribution policy
- acknowledgement policy

#### Contact routing
- editorial email
- submission email
- production email
- support email
- general contact email
- copyright/licensing email
- technical support email

#### Formatting conventions
- reference style
- citation pattern
- article history display
- issue citation format
- abstract structure
- keyword style
- scientific naming conventions
- HTML style preferences if inferable

### 3. Build a provisional profile

Create an internal provisional journal profile based on detected information.

Each detected value should have:

- value
- source
- confidence: `high`, `medium`, or `low`
- conflict status

### 4. Ask only the gap questionnaire

After detection, ask only about:

- missing required configuration
- conflicting values
- low-confidence values that materially affect production
- user preferences that cannot be inferred
- fields to include or ignore
- fields that may be editorially generated
- whether the journal uses an article-level Publisher ID if this cannot be detected
- whether the journal uses an issue-level Publisher ID if this cannot be detected
- whether the journal uses an Issue Galley Publisher ID if this cannot be detected
- the Publisher ID value or pattern only where its use is confirmed and cannot be inferred reliably

Do not ask the user to confirm every high-confidence value unless they request a full profile review.

Publisher ID questions are conditional. Article-level Publisher ID, issue-level Publisher ID and Issue Galley Publisher ID are separate fields; never infer one from the existence of another, and never substitute DOI for Publisher ID.

## Source authority and conflicts

A journal profile may define a source hierarchy. If none exists, use a provisional default such as:

1. Current official journal website/policy page
2. Current journal setup or production document
3. Final published issue/article
4. Current submission guidelines
5. Final accepted manuscript
6. Submission metadata/export
7. User preference

This order is not universal. If conflicting evidence exists, report the conflict rather than silently reconciling it.

Do not silently change scientific content because one source appears more plausible.

## Extraction modes

The profile must support these modes:

### `strict`
Extract exactly what appears in the source. Preserve grammar, spelling, capitalization and wording. Do not normalize content.

### `clean`
Default production mode. Remove OCR duplication, broken line wraps, repeated headers/footers and obvious layout artefacts without changing authored wording.

### `copyedited`
Correct language and formatting while preserving meaning. Use only when the user or journal profile explicitly allows it.

### `hybrid`
Return clean extracted content and separately flag suggested editorial corrections.

## Field behaviour policies

Each metadata field must use one of these policies:

### `extract_only`
Never generate or rewrite. If absent, report as missing or leave blank according to profile settings.

### `extract_or_blank`
Extract when present; otherwise leave blank.

### `extract_or_flag`
Extract when present; otherwise flag for review.

### `generate_if_missing`
Use the source value if present. Generate only when absent and generation is authorised.

### `generate_always`
Editorially generated content.

### `ignore`
Do not extract, request or display.

The user may override field behaviour for a single task.

## Task selection

Once the journal profile is sufficiently complete, ask what production task to perform if the user has not already specified it.

Supported tasks include:

- `prepare_issue`
- `prepare_article_publication`
- `quicksubmit_single`
- `quicksubmit_batch`
- `extract_article_metadata`
- `extract_authors`
- `extract_declarations`
- `prepare_references`
- `validate_metadata`
- `generate_issue_description`
- `generate_issue_cover_brief`
- `style_ojs_html`
- `production_qa`
- `prepare_doi_metadata`
- `full_production_workflow`

Return only the fields required by the journal profile and task.

## Existing-submission article publication workflow

When preparing an existing OJS submission for publication, preserve the field grouping and semantics used by the user's OJS interface.

### Title and Abstract

Prepare:

- Prefix
- Title
- Subtitle
- Abstract

#### Prefix, Title and Subtitle convention

When the journal uses the colon convention, split the first colon into Title and Subtitle:

- anything before the first colon is the main title
- anything after the first colon is the Subtitle
- the separating colon is not stored in either field
- if the main title begins with standalone `A`, `An` or `The`, move that word to Prefix and remove it from Title
- a leading `A`, `An` or `The` in the Subtitle remains part of the Subtitle
- if there is no colon, leave Subtitle blank
- if there is no main-title Prefix, leave Prefix blank

Example:

`The Effects of X on Y: A Systematic Review`

becomes:

- Prefix: `The`
- Title: `Effects of X on Y`
- Subtitle: `A Systematic Review`

The reconstructed display title must match the published title apart from allowed extraction cleanup.

### Metadata

Use these OJS field meanings.

#### Keywords

Keywords are typically one- to three-word phrases indicating the main topics of a submission.

Preserve author-supplied keywords exactly in strict/clean extraction, even when an author's phrase is longer than three words. If generation is explicitly authorised because keywords are missing, prefer concise one- to three-word phrases.

#### Supporting Agencies

Supporting Agencies indicate the source of research funding or other institutional support that facilitated the research.

Extract explicit funders, sponsors, grant agencies or stated institutional support. Do not copy author affiliations into Supporting Agencies solely because the institution appears in the author list.

#### Coverage

Coverage typically indicates:

- spatial location, such as a place name or geographic coordinates
- temporal period, such as a period label, date or date range
- jurisdiction, such as a named administrative entity

Do not use Coverage as a generic subject/topic field.

#### Rights

Rights record rights held over the submission, including Intellectual Property Rights, copyright, licence rights and other property rights.

Prefer explicit article rights statements and the journal's authoritative rights/licence policy according to source precedence. Keep copyright holder, year, licence name and licence URL conceptually distinct even if OJS presents a single Rights field.

#### Source

Source identifies another work or resource from which the submission is derived. It may be an identifier such as the DOI of that source work.

Do not put the submission's own DOI in Source. Do not automatically copy the submission's journal citation into Source. If the submission is not derived from another identifiable work/resource and the journal has no special Source convention, follow the configured missing-value policy.

#### Type

Type describes the nature or genre of the main content using Dublin Core-style resource types.

For a conventional journal article, editorial, review, commentary or other textual manuscript, `Text` is normally the appropriate Type. Other types may include `Dataset`, `Image` or another Dublin Core type where supported.

OJS Section and Dublin Core Type are separate concepts. Do not use `Editorial`, `Original Article` or `Review Article` as Type merely because that is the submission's Section.

#### Data Availability Statement

This is a short statement describing whether the authors made research data available and, if so, where readers may access it.

Extract the authors' statement when present. Do not invent an availability status, repository, accession number or data-sharing claim. Do not turn absence into `Data not available` unless the journal explicitly authorises a standard statement.

### Identifiers

Prepare:

- **Publisher ID** — only if the journal uses an article-level Publisher ID

Publisher ID may record an external database or deposit-workflow identifier, such as one associated with PubMed export/deposit.

**Publisher ID must not be used for DOI.** DOI and Publisher ID are separate identifiers.

Detect usage and the established pattern before asking. If article-level Publisher IDs are not used, omit the field.

### Galleys

The core article Galley information is:

- **Galley Label**
- **URL Path**

If the final article galley file is already supplied, treat it as the candidate galley and do not ask for the same file again.

Detect the journal's Galley Label and URL Path convention. `PDF` is a common label but should not replace an established journal-specific convention.

Article Galley URL Path is distinct from the article's publication URL Path.

See `docs/ARTICLE-PUBLICATION-METADATA.md` for the expanded field semantics and acceptance rules.

## QuickSubmit workflow

For QuickSubmit, preserve the field order used by the user's OJS installation when known.

Potential fields include:

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

If the profile is not configured, infer which fields appear in the user's request or OJS screenshots/forms, then ask only about genuinely ambiguous fields.

Apply the article-publication semantics above. In particular, do not confuse Section with Type, the submission DOI with Source, or DOI with Publisher ID.

## Article metadata rules

### Titles
Use the source title exactly according to the active extraction mode. Do not editorially improve article titles during metadata extraction. When Prefix/Subtitle are enabled, split the source title according to the configured title convention while preserving the published wording.

### Abstracts
Preserve section labels such as Background, Methods, Results and Conclusion. In clean mode remove duplicated OCR lines, layout-caused hyphenation and running headers/footers while preserving authored wording.

### Keywords
Extract exact keywords and preserve spelling. Output in the user's configured separator format. Apply the typical one- to three-word preference only to generated keywords, not as a reason to rewrite author-supplied keywords.

### Article type/section
Prefer an explicitly stated OJS Section. Keep Section separate from Dublin Core Type. When the main content is unambiguously a conventional textual journal submission and Type is enabled, `Text` may be returned as an inferred Type.

### Source/citation metadata
Extract journal title, abbreviation, year, volume, issue, pages/article number and DOI where present for citation/QA purposes. Do not invent missing DOI values, and do not put the submission's own DOI into OJS Source.

## Author metadata rules

Parse author order exactly as published or supplied.

Attempt to map:

- superscript affiliations
- corresponding author markers
- ORCID identifiers
- email addresses
- phone numbers

Do not guess how a multi-part personal name should be split if the source structure is ambiguous. Flag it for review.

Corresponding-author metadata should be matched back to the author list and affiliation where possible.

## Article history rules

Extract only configured dates:

- Received
- Revised
- Accepted
- Published
- Versioned

Preserve display wording when extracting. If database-ready values are needed, additionally provide ISO `YYYY-MM-DD` dates without replacing the source form.

## Declarations

Search for common labels and equivalents:

- Data availability
- Funding
- Supporting agencies
- Competing interests
- Conflict of interest
- Ethical approval
- Consent for publication
- Author contributions
- Acknowledgements
- Trial registration

Map them to configured OJS fields without paraphrasing unless copyediting is enabled.

Do not treat ordinary affiliation as Supporting Agencies unless the source explicitly indicates funding, sponsorship or other support.

## References

Support:

- `exact`
- `clean`
- `normalize`
- `validate`

Do not search for or insert missing DOIs unless explicitly enabled for the task.

## Issue preparation

For `prepare_issue`, mirror the OJS editor interface and organise output into exactly these scopes unless the user asks for something else.

### Issue Data

Core fields:

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

Do not add unrelated issue fields simply because OJS or another installation may support them.

The cover image file may be supplied or detected among source assets. Do not generate a replacement cover unless explicitly requested. Alternate text may be generated when authorised.

### Issue Galley

Core fields:

- **Issue Galley** — file
- **Galley Label**
- **Publisher ID** — only if the journal uses an Issue Galley Publisher ID
- **URL Path**

If a complete final issue PDF has already been supplied, treat it as the candidate Issue Galley and do not ask for it again.

Detect Galley Label, Publisher ID usage/pattern and URL Path convention from existing published issues, OJS exports/screenshots or journal documentation where possible.

Do not invent a Publisher ID.

### Identifiers

Core field:

- **Publisher ID** — only if the journal uses an issue-level Publisher ID

This field is independent of both article-level and Issue Galley Publisher IDs. Never copy one scope's Publisher ID into another without explicit evidence that the journal intentionally uses the same value.

### Issue URL path rules

Treat the Issue Data URL Path and Issue Galley URL Path as separate fields. Detect the journal's established pattern for each. Do not assume the same slug is used in both scopes.

### Publisher ID questionnaire logic

Before asking about Publisher IDs:

1. Inspect current and archived OJS article/issue records where available.
2. Inspect OJS exports, screenshots or setup documentation supplied by the user.
3. Determine separately whether Publisher IDs are used for article Identifiers, issue Identifiers and Issue Galley.
4. Detect any stable pattern.

Only if use remains unresolved, ask targeted questions for the relevant scope.

Only if the answer is yes and the pattern/value remains unknown should the skill ask for the expected Publisher ID or convention.

### Whole-issue synthesis rule

Before generating an issue title, description, summary or cover concept:

1. Inspect every article assigned to the issue.
2. Build an internal article map containing article type, main topic, methods/level of translation and major contribution.
3. Generate the issue-level content from the whole issue, not from one article.
4. Do not claim themes unsupported by the included articles.

### Issue description levels

When configured, support:

- `short`: approximately 75-150 words
- `standard`: approximately 3 paragraphs
- `detailed`: approximately 5-7 paragraphs
- custom word count

Generated issue descriptions are editorial synthesis, not manuscript text.

## Cover brief generation

When requested, derive a cover concept from journal branding, issue title/theme, article mix, scientific subject matter and volume/issue/year. Do not generate or replace a supplied cover unless explicitly requested.

## OJS-safe HTML

When styling content for OJS, prefer conservative HTML and inline CSS.

Use simple elements such as `div`, `p`, `strong`, `em`, lists and simple tables. Avoid JavaScript, external stylesheets, unsupported layout frameworks and external fonts.

Preserve scientific typography and meaning, including species italics, Greek characters, subscripts and superscripts.

## Automatic discrepancy detection

Actively flag production inconsistencies, including:

- title mismatch across sources
- Prefix/Title/Subtitle reconstruction mismatch
- author order/name mismatch
- corresponding author mismatch
- abstract mismatch
- submission DOI incorrectly entered as Source
- DOI incorrectly entered as Publisher ID
- OJS Section incorrectly entered as Dublin Core Type
- Supporting Agencies populated from affiliation without support evidence
- Coverage populated with a general topic rather than spatial/temporal/jurisdictional metadata
- unsupported or invented Data Availability Statement
- article Galley Label missing when required
- article Galley URL Path conflict
- article Publisher ID pattern conflict
- conflicting doses or units
- organism inconsistency
- abbreviation inconsistency
- licence text versus licence URL mismatch
- journal URL mismatch
- volume/issue mismatch
- page-range overlap
- duplicate pagination
- article type mismatch
- malformed references
- missing declarations required by profile
- inconsistent DOI formatting
- conflicting publication dates
- Issue Data URL Path conflicts
- Issue Galley URL Path conflicts
- issue Publisher ID pattern conflicts

Do not automatically correct scientific discrepancies. Report them for editorial review.

## Pagination and issue consistency

For issue-level QA, check:

- article order
- first and last page
- overlapping page ranges
- gaps when the journal expects continuous pagination
- duplicate article numbers
- volume/issue/year consistency
- issue citation consistency

## Provenance

Internally track the source for every extracted field.

At minimum:

- source file or webpage
- source location/page when available
- confidence
- whether value is exact, cleaned, inferred or generated

Display provenance only when the profile output mode requests it.

## Missing information behaviour

The journal profile should define one of:

- `blank`
- `not_stated`
- `flag`
- `ask`
- `generate_if_allowed`

Do not invent article metadata merely to avoid an empty field.

## Output modes

### `compact`
Only field/value pairs required for the task.

### `standard`
Field/value pairs plus important warnings.

### `detailed`
Field/value pairs, warnings, provenance and notes.

### `audit`
Full extraction provenance, conflicts, confidence, missing fields and validation results.

Default to `compact` for routine QuickSubmit/article-publication work unless the journal profile says otherwise.

## Per-task overrides

Task-specific instructions take precedence over profile defaults for that run and must not permanently mutate the saved profile unless the user asks to save the override.

## Final production QA

When asked to validate an issue or article publication record, check configured items under:

### Article Title and Abstract
- Prefix
- Title
- Subtitle
- Abstract
- reconstructed display title

### Article Metadata
- Keywords
- Supporting Agencies
- Coverage
- Rights
- Source
- Type
- Data Availability Statement

### Article Identifiers
- Publisher ID when configured
- confirm Publisher ID is not DOI

### Article Galleys
- Galley Label
- URL Path
- galley presence if information is available

### Issue Data
- Date Published
- Volume
- Number
- Year
- Title
- Description when required
- Cover image alternate text when a cover is used
- URL Path

### Issue Galley
- Issue Galley file
- Galley Label
- Publisher ID when configured
- URL Path

### Issue Identifiers
- Publisher ID when configured

### Other Article Metadata
- authors
- affiliations
- corresponding author
- section
- pagination/article number
- publication date
- DOI
- rights/licence
- funding
- ethics/consent where required
- references

### Cross-article checks
- volume/issue consistency
- page range consistency
- article numbering
- publication-date consistency
- journal name/abbreviation consistency
- licence consistency

Return one of:

- `ready`
- `ready_with_warnings`
- `not_ready`

Do not mark an issue ready when a configured blocking requirement is unresolved.

## Safety against unnecessary work

The skill must not extract or generate everything simply because it can.

The sequence is:

1. Detect broadly.
2. Configure narrowly.
3. Output only what is needed.

This is a core design requirement.
