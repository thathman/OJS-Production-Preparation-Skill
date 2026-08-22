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
- author metadata expectations
- article history conventions
- copyright/licensing
- contact routing
- reference style
- DOI conventions
- issue and article citation patterns
- formatting conventions

Detected values are stored with confidence and provenance.

## 3. Conflict analysis

The skill compares source values and identifies conflicts before asking questions.

Examples:

- website says CC BY-NC 4.0, article footer says CC BY 4.0
- issue archive uses `1(1)` while sample PDF says `Vol. 1 No. 2`
- author name differs between title page and citation block
- abstract dose differs from Methods dose

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
- Should issue descriptions be generated automatically?
- Should missing ORCID values be blank or flagged?
- Which contact email is used for manuscript communication?

## 6. Task selection

The skill supports focused tasks rather than always running the entire workflow.

### QuickSubmit single article
Returns only configured QuickSubmit fields.

### QuickSubmit batch
Processes multiple manuscripts and keeps article order clear.

### Issue preparation
Builds issue metadata, description, short summary, URL path, and cover brief if configured.

### Validation
Compares prepared metadata against source material and reports discrepancies.

### Full production workflow
Runs issue setup, article extraction, QA, and readiness review.

## 7. Article processing

For each article:

1. Identify the authoritative manuscript/version.
2. Extract configured metadata.
3. Clean only layout/OCR artefacts according to extraction mode.
4. Track provenance.
5. Flag conflicts.
6. Return only requested/configured fields.

Recommended default field policy:

- Title: `extract_only`
- Authors: `extract_only`
- Abstract: `extract_only`
- Keywords: `extract_only`
- ORCID: `extract_or_blank`
- Funding: `extract_or_flag`
- Data availability: `extract_or_flag`
- References: `extract_only`

## 8. Whole-issue synthesis

Before generating issue-level editorial copy:

1. Inspect all articles assigned to the issue.
2. Build an internal article map.
3. Identify genuine cross-issue themes.
4. Ensure every article is represented.
5. Generate only the configured output lengths.

Issue-level content may be editorially generated. Article-level source metadata should remain source-faithful.

## 9. OJS formatting

Where the journal wants formatted content, the skill may output conservative OJS-safe HTML using inline CSS.

Formatting should enhance readability without changing wording.

## 10. Final QA

The validation stage checks configured blocking and warning rules.

Possible blocking issues:

- missing required title
- missing required author
- unresolved page overlap
- missing required licence
- issue volume/number mismatch

Possible warnings:

- no ORCID
- inconsistent spelling
- reference formatting anomaly
- publication date variance

Final state:

- `ready`
- `ready_with_warnings`
- `not_ready`

## 11. Reuse

Once confirmed, the journal profile should be reused for future issues. New source evidence can update the provisional profile, but conflicting changes should be surfaced for review rather than silently replacing existing rules.
