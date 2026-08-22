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

## Metadata field detection

Detect fields from:

- OJS forms/screenshots supplied by the user
- existing article pages
- published metadata blocks
- XML/export files
- submission templates

Do not assume default OJS fields are enabled in every installation.

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

For each article, look for:

- section/article type
- prefix
- title
- subtitle
- author order
- superscript affiliations
- corresponding author marker
- emails
- ORCID
- abstract
- keywords
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

A manuscript contains funding, ethical approval, author contributions, affiliations, and 50 references. If the active QuickSubmit profile requests only Title, Abstract, Keywords, Rights, and Data Availability, only those fields should be returned unless a detected problem needs to be surfaced.
