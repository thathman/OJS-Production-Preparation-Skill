# QuickSubmit Presets

## Goal

Avoid returning unnecessary metadata. A journal profile should define which QuickSubmit fields are active, and a task may override that selection.

## Presets

### Minimal
Use when the journal only needs core article metadata.

- Section
- Title
- Abstract
- Keywords
- Authors
- References

### Standard
Adds common production metadata.

- Section
- Prefix
- Title
- Subtitle
- Abstract
- Type
- Rights
- Data Availability Statement
- Keywords
- Supporting Agencies
- References
- Authors
- Article History

### Full
Use only when the journal actually uses these fields.

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
- Authors
- Article History
- Declarations

### Custom
The user or journal profile selects fields individually.

## Detection before questions

If the user provides:
- a screenshot of QuickSubmit
- an OJS form field list
- an export
- prior production instructions

use that evidence to infer the active field set before asking the user.

## Field policies

Each active field should have a behaviour policy:

```yaml
quicksubmit:
  fields:
    title:
      enabled: true
      policy: extract_only
    subtitle:
      enabled: true
      policy: extract_or_blank
    abstract:
      enabled: true
      policy: extract_only
    rights:
      enabled: true
      policy: extract_or_flag
    subjects:
      enabled: false
      policy: ignore
```

## Output order

Use the user's OJS field order when known. Otherwise use:

1. Section
2. Prefix
3. Title
4. Subtitle
5. Abstract
6. Coverage Information
7. Type
8. Source
9. Rights
10. Data Availability Statement
11. Subjects
12. Disciplines
13. Keywords
14. Supporting Agencies
15. References

Authors and publication history may be returned in separate blocks if that matches the user's workflow.

## Batch mode

For `quicksubmit_batch`:
- process one article at a time
- preserve issue order
- show only configured fields
- attach article-specific warnings directly to that article
- provide a final batch QA summary

Do not repeat journal-level explanations for every article.

## Missing values

Respect the profile's missing-value policy:
- blank
- not_stated
- flag
- ask
- generate_if_allowed

Never fill article metadata simply because the OJS field exists.

## Task overrides

Examples:

`Only title, abstract, keywords and authors.`

`Skip references for this batch.`

`Use strict extraction for title and abstract, clean extraction for references.`

Task-level overrides apply only to the current run unless the user explicitly saves them to the journal profile.
