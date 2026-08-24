# Automatic Detection Rules

## Goal

The skill should learn the journal from available source material before asking the editor to configure it manually.

Detection is broad. Output remains narrow.

## Detection sources

The skill may inspect:

- journal website pages
- uploaded PDFs and DOCX manuscripts
- full issue PDFs
- OJS exports
- OJS screenshots/forms
- journal policies
- submission guidelines
- production spreadsheets
- branding material

When a website is supplied, useful pages commonly include:

- About
- Submissions
- Author Guidelines
- Editorial Policies
- Open Access
- Copyright / Licensing
- Contact
- Editorial Team
- Current Issue
- Archives

Do not assume these paths exist. Discover from the site when possible.

## Confidence levels

### High

Use when multiple authoritative sources agree or one highly authoritative source explicitly states the value.

Examples:

- ISSN printed consistently on the website and published issue
- journal title identical across masthead and issue PDF
- section explicitly printed above article title

### Medium

Use when the value is explicit but appears in only one source or in a source that may be outdated.

Examples:

- licence listed on only one policy page
- publication frequency stated only in About text

### Low

Use when inferred from patterns rather than directly stated.

Examples:

- assumed continuous publication because article numbers are used
- inferred article section from manuscript structure

Low-confidence values that affect production should be confirmed.

## Conflict rules

A conflict exists when two relevant sources provide materially different values.

Examples:

- different licence versions
- different publisher names
- different article dates
- different issue numbers
- different author spellings
- different DOI strings
- different issue URL Path patterns
- different Publisher ID patterns
- submission DOI placed in the Source field
- DOI placed in Publisher ID

Conflicts must record:

- value A
- source A
- value B
- source B
- likely authority if profile rules permit
- whether editorial confirmation is required

Never hide a scientific-content conflict.

## Journal identity detection

Look for:

- full title in masthead/header
- abbreviated title in citations
- ISSN/eISSN labels
- publisher statement
- footer copyright
- DOI prefix in article citations

Do not derive an ISSN from unrelated numeric identifiers.

## Publication model detection

Look for:

- `Volume`, `Vol.`
- `Issue`, `No.`
- year-only archive structures
- article numbers/e-locations
- continuous pagination
- issue-specific pagination
- stated publication frequency

Do not infer publication frequency from a single issue.

## Section detection

Prefer explicit labels printed in the article or OJS table of contents.

Record observed section names exactly before optional normalization.

If a source uses both `Original Article` and `Original Research`, keep them separate until the user confirms whether they are distinct sections.

Section is not the same as Dublin Core Type. A submission may be in the OJS section `Editorial` while its metadata Type is `Text`.

## Metadata field detection

Detect fields from:

- OJS forms/screenshots supplied by the user
- existing article pages
- published metadata blocks
- XML/export files
- submission templates

Do not assume default OJS fields are enabled in every installation.

## Article publication detection

For each article, detect OJS publication fields by scope.

### Prefix, Title and Subtitle

Look for:

- whether OJS has Prefix enabled
- whether published titles use a colon as the Title/Subtitle separator
- whether existing records move leading `A`, `An` or `The` into Prefix

When the configured convention uses the first colon:

- text before the first colon is main title
- text after it is Subtitle
- the colon itself is not stored in either field
- only a leading article on the main title can become Prefix
- a leading article in the Subtitle stays in the Subtitle

Validate by reconstructing the display title and comparing it to the source.

### Keywords

Detect author-supplied keywords from manuscript/PDF/OJS metadata. Preserve them as supplied in source-faithful modes.

Do not reject an authored keyword merely because it contains more than three words. The one- to three-word guideline applies primarily to generated keywords.

### Supporting Agencies

Look for explicit evidence of:

- funders
- sponsors
- grant agencies
- named institutional support that facilitated the research

Do not infer Supporting Agencies from author affiliation alone.

### Coverage

Look for explicit or safely inferable:

- spatial location
- geographic coordinates
- temporal period/date/date range
- jurisdiction

Do not classify ordinary topical terms as Coverage.

### Rights

Capture evidence for:

- copyright holder
- copyright year
- licence
- licence URL
- other intellectual/property rights where stated

Use profile source precedence when article and journal rights evidence differ.

### Source

Look for an explicitly identified work/resource from which the submission is derived, translated, reproduced, adapted or otherwise sourced.

A Source value may be the identifier/DOI of that external source work.

Never infer the submission's own DOI as Source. Never automatically copy the submission citation into Source.

### Type

Detect the nature of the main content using Dublin Core-style resource types.

For conventional journal articles, editorials, reviews and commentaries whose main content is textual, `Text` is normally a safe inference when the Type field is enabled. Preserve a different explicit type such as Dataset or Image when supported.

Do not copy the OJS Section value into Type.

### Data Availability Statement

Search for labels and equivalents such as:

- Data Availability
- Availability of Data
- Data Sharing
- Data and Materials Availability

Extract the statement rather than inventing one. Do not infer `Data not available` solely from absence.

### Article Publisher ID

Detect whether article-level Publisher ID is used from:

1. OJS exports
2. OJS publication/identifier screenshots
3. existing submissions
4. journal production documentation
5. external deposit/export conventions

Publisher ID may represent an external database/deposit identifier. It is not a DOI field.

Never copy DOI into Publisher ID. Ask whether article-level Publisher IDs are used only when detection remains unresolved.

### Article Galleys

Detect:

- Galley Label convention
- Galley URL Path convention
- candidate final galley file among supplied PDFs/HTML/XML files

The article Galley URL Path is separate from the article publication URL Path. Detect them independently.

## Issue preparation detection

For issue preparation, detect the three OJS scopes separately.

### Issue Data

Attempt to detect:

- Date Published
- Volume
- Number
- Year
- Title
- Description convention
- cover image use
- cover image alternate-text convention
- Issue Data URL Path pattern

### Issue Galley

Attempt to detect:

- whether a complete issue galley is normally published
- file format used for the issue galley
- Galley Label convention
- whether an Issue Galley Publisher ID is used
- Issue Galley Publisher ID pattern when present
- Issue Galley URL Path pattern

A supplied complete final issue PDF should be treated as the candidate issue galley unless the user indicates otherwise.

### Identifiers

Attempt to detect:

- whether the issue itself uses a Publisher ID under Identifiers
- the issue-level Publisher ID pattern when present

Article-level Publisher ID, issue-level Publisher ID and Issue Galley Publisher ID are independent. Never infer one solely from the presence of another.

### Publisher ID detection order

Before asking the user about Publisher IDs:

1. Inspect OJS exports or screenshots if supplied.
2. Inspect existing article and issue records.
3. Inspect current and archived issue records.
4. Inspect journal setup/production documentation.
5. Compare multiple records for stable identifier patterns.

If Publisher ID use remains unresolved, ask separately whether the journal uses:

- an article-level Publisher ID under article Identifiers
- an issue-level Publisher ID under issue Identifiers
- an Issue Galley Publisher ID

Ask for the value or pattern only when use is confirmed and the convention is still unknown.

### URL Path detection rule

Detect independently:

- article publication URL Path
- article Galley URL Path
- Issue Data URL Path
- Issue Galley URL Path

Do not assume the same path or slug applies across these scopes.

## Licence detection

Capture separately:

- human-readable licence name
- short identifier
- URL
- copyright holder
- year

Validate whether the textual licence and URL agree.

Example conflict:

- text: `CC BY-NC-SA 4.0`
- URL: `/licenses/by/4.0/`

This should be flagged as a mismatch rather than silently resolved unless the journal profile explicitly defines an override.

## Contact detection

Classify email addresses by role using surrounding context.

Possible roles:

- editorial
- submissions
- production
- technical support
- general support
- copyright/licensing
- advertising

Do not use one detected address for all purposes.

## Article metadata detection

For each article, also look for:

- author order
- superscript affiliations
- corresponding author marker
- emails
- ORCID
- abstract
- article history
- citation block
- DOI
- page range/article number
- declarations
- references

## OCR/layout cleanup

In clean mode, detection may remove:

- duplicated lines
- repeated headers/footers
- page numbers
- line-break hyphenation caused by typesetting
- repeated journal mastheads

It must not silently correct authored grammar or scientific wording.

## Whole-issue detection

When a full issue or all issue articles are available, detect:

- article order
- section grouping
- page sequence
- issue citation pattern
- issue theme if explicitly stated
- cover style
- volume/number/year
- candidate Issue Galley file
- Issue Data and Issue Galley URL conventions
- issue-level and galley-level Publisher ID usage

For generated issue summaries, create an internal article map first so every article is represented.

## Provenance record

Recommended internal record:

```yaml
field: licence.short_name
value: CC BY-NC 4.0
status: detected
confidence: medium
source:
  type: webpage
  location: Open Access Statement
conflict: false
transformation: exact
```

Possible `transformation` values:

- `exact`
- `cleaned`
- `normalized`
- `inferred`
- `generated`

## Detection does not equal output

The skill may detect information that is useful for profile building or QA without showing it in routine task output.

Example:

A manuscript contains funding, ethical approval, author contributions, affiliations and 50 references. If the active article-publication profile requests only Title, Abstract, Keywords, Rights and Data Availability, only those fields should be returned unless a detected problem needs to be surfaced.
