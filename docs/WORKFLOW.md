# End-to-End Workflow

## 1. Source-first onboarding

The skill begins by asking for available source material unless useful sources have already been provided.

Accepted sources include:

- journal website
- sample article PDFs
- complete issue PDFs
- accepted manuscripts
- OJS exports
- submission guidelines
- editorial policies
- production spreadsheets
- branding assets

The user does not need to supply every type.

## 2. Automatic journal detection

The skill inspects the available sources and attempts to determine:

- journal identity
- publication model
- issue structure
- article sections
- metadata fields in use
- Prefix/Title/Subtitle convention
- article galley label and URL conventions
- article Publisher ID usage
- author metadata expectations
- article history conventions
- copyright/licensing
- contact routing
- reference style
- DOI conventions
- issue and article citation patterns
- issue URL conventions
- issue galley label and URL conventions
- whether Publisher IDs are used for articles, issues, issue galleys, or any combination
- formatting conventions

Detected values are stored with confidence and provenance.

## 3. Conflict analysis

The skill compares source values and identifies conflicts before asking questions.

Examples:

- website says CC BY-NC 4.0, article footer says CC BY 4.0
- issue archive uses `1(1)` while sample PDF says `Vol. 1 No. 2`
- author name differs between title page and citation block
- abstract dose differs from Methods dose
- an article DOI has been entered into Publisher ID
- a submission's own DOI has been entered into Source

Scientific conflicts are never silently corrected.

## 4. Provisional journal profile

The skill creates a profile using detected values and sensible defaults only where safe.

The profile defines:

- which fields matter
- extraction policy per field
- generation policy per field
- missing-value behaviour
- source authority
- output mode
- QA rules

## 5. Gap questionnaire

Only unresolved items are asked.

Typical questions:

- Which of two conflicting licence statements is authoritative?
- Does the journal require a data availability statement in OJS metadata?
- Does the journal use article-level Publisher IDs from an external database/deposit workflow?
- Should issue descriptions be generated automatically?
- Should missing ORCID values be blank or flagged?
- Which contact email is used for manuscript communication?
- Does the journal assign a Publisher ID to the issue itself under Identifiers?
- Does the journal assign a Publisher ID to issue galleys?
- If a Publisher ID is used, what existing pattern should be followed?

Publisher ID questions are conditional. The skill must first try to detect their use and pattern from existing OJS records or journal documentation. Article-level, issue-level and issue-galley Publisher IDs are independent fields and must not be conflated with each other or with DOI.

## 6. Task selection

The skill supports focused tasks rather than always running the entire workflow.

### Article publication preparation

For an existing OJS submission, prepare the publication record in OJS-facing groups:

#### Title and Abstract

- Prefix
- Title
- Subtitle
- Abstract

When the journal uses the configured convention, split the first colon into Title and Subtitle. Move only a leading main-title `A`, `An` or `The` into Prefix. Do not move a leading article from the Subtitle.

#### Metadata

- Keywords
- Supporting Agencies
- Coverage
- Rights
- Source
- Type
- Data Availability Statement

Use the semantics in `docs/ARTICLE-PUBLICATION-METADATA.md`.

Critical distinctions:

- Keywords are normally concise topic phrases; preserve authored keywords exactly.
- Supporting Agencies means funding or other explicit research support, not ordinary author affiliations.
- Coverage means spatial, temporal or jurisdictional coverage, not general subject matter.
- Rights records rights held over the submission.
- Source identifies another work/resource from which the submission is derived; it is not the submission's own DOI.
- Type is a Dublin Core-style content type. Ordinary journal manuscripts are normally `Text`; OJS Section is separate.
- Data Availability must come from author/journal evidence and must not be invented.

#### Identifiers

- Publisher ID, only when the journal uses an article-level external-database/deposit identifier

Never use DOI as Publisher ID.

#### Galleys

- Galley Label
- URL Path

If the final article galley file is already supplied, treat it as the candidate galley and do not request it again. Galley URL Path is distinct from the article's publication URL Path.

### QuickSubmit single article

Returns only configured QuickSubmit fields using the same article metadata semantics above.

### QuickSubmit batch

Processes multiple manuscripts and keeps article order clear.

### Issue preparation

Builds issue metadata using the OJS field groups below.

#### Issue Data

- Date Published
- Identification
  - Volume
  - Number
  - Year
  - Title
- Description
- Cover image
  - Alternate text
- URL Path

#### Issue Galley

- Issue Galley file
- Galley Label
- Publisher ID, only when the journal uses one for issue galleys
- URL Path

#### Identifiers

- Publisher ID, only when the journal uses an issue-level Publisher ID

The Issue Data URL Path and Issue Galley URL Path are separate values. The issue-level Publisher ID and issue-galley Publisher ID are also separate values.

If the complete issue PDF has already been supplied, treat it as the candidate Issue Galley file and do not ask the user to upload it again.

### Validation

Compares prepared metadata against source material and reports discrepancies.

### Full production workflow

Runs issue setup, article extraction/publication preparation, QA and readiness review.

## 7. Article processing

For each article:

1. Identify the authoritative manuscript/version.
2. Determine the OJS Section separately from Dublin Core Type.
3. Parse Prefix, Title and Subtitle according to the journal's configured rule.
4. Extract configured metadata using the OJS field meanings.
5. Prepare article Identifiers without using DOI as Publisher ID.
6. Prepare Galley Label and URL Path using the journal's established convention.
7. Clean only layout/OCR artefacts according to extraction mode.
8. Track provenance.
9. Flag conflicts.
10. Return only requested/configured fields.

Recommended default field policy:

- Title: `extract_only`
- Authors: `extract_only`
- Abstract: `extract_only`
- Keywords: `extract_only`
- ORCID: `extract_or_blank`
- Supporting Agencies: `extract_or_blank`
- Coverage: `extract_or_blank`
- Rights: `extract_or_flag`
- Source: `extract_or_blank`
- Type: `extract_or_flag`
- Data availability: `extract_or_flag`
- Publisher ID: conditional, `extract_or_flag`
- References: `extract_only`

## 8. Whole-issue synthesis

Before generating issue-level editorial copy:

1. Inspect all articles assigned to the issue.
2. Build an internal article map.
3. Identify genuine cross-issue themes.
4. Ensure every article is represented.
5. Generate only the configured output lengths.

Issue-level content may be editorially generated. Article-level source metadata should remain source-faithful.

## 9. Issue preparation sequence

For an issue-preparation task, use this order:

1. Inspect the complete issue file, cover, journal website and prior published issues.
2. Detect the issue naming convention, issue URL Path pattern, galley label pattern, galley URL Path pattern and Publisher ID usage.
3. Prepare **Issue Data**.
4. Prepare **Issue Galley**.
5. Prepare **Identifiers**.
6. Ask only unresolved conditional questions, especially Publisher ID usage or patterns.
7. Validate the issue record against the source files before publication.

Do not invent Publisher IDs. If a Publisher ID is configured as unused, omit it rather than showing an empty field as though it requires completion.

## 10. OJS formatting

Where the journal wants formatted content, the skill may output conservative OJS-safe HTML using inline CSS.

Formatting should enhance readability without changing wording.

## 11. Final QA

The validation stage checks configured blocking and warning rules.

Article-level checks should include, when enabled:

- Prefix/Title/Subtitle reconstruction
- submission DOI not used as Source
- DOI not used as Publisher ID
- Section not substituted for Dublin Core Type
- Supporting Agencies supported by actual funding/support evidence
- Coverage limited to spatial/temporal/jurisdictional meaning
- Data Availability wording supported by sources
- Galley Label
- Galley URL Path
- article Publisher ID pattern when used

Issue-level checks should include, when enabled:

- Date Published
- Volume
- Number
- Year
- Title
- Description
- cover image alternate text
- Issue Data URL Path
- Issue Galley file
- Galley Label
- Issue Galley Publisher ID
- Issue Galley URL Path
- issue-level Publisher ID under Identifiers

Possible blocking issues:

- missing required title
- missing required author
- DOI used as Publisher ID
- missing required article Galley Label
- unresolved page overlap
- missing required licence
- issue volume/number mismatch
- missing required issue galley
- missing required issue galley label
- missing Publisher ID when the journal profile explicitly requires one

Possible warnings:

- no ORCID
- submission DOI entered in Source
- Section used as Type
- unsupported Supporting Agency
- invalid Coverage semantics
- invented Data Availability wording
- inconsistent spelling
- reference formatting anomaly
- publication date variance
- article, issue or galley URL Path does not match the detected convention
- Publisher ID does not match the configured pattern

Final state:

- `ready`
- `ready_with_warnings`
- `not_ready`

## 12. Reuse

Once confirmed, the journal profile should be reused for future issues. New source evidence can update the provisional profile, but conflicting changes should be surfaced for review rather than silently replacing existing rules.
