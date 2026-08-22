# Author and Affiliation Parsing

## Goal

Extract author metadata accurately without guessing name structure or affiliation mapping.

## Preserve author order

Author order is source metadata. Preserve it exactly.

## Parse cautiously

For each author, attempt to extract:
- displayed full name
- given name
- middle name or initials when structurally clear
- family name
- preferred public name if explicitly supplied
- email
- ORCID
- phone
- corresponding-author status
- affiliation marker(s)

If the source does not make name structure clear, return the display name and flag the split for review rather than guessing.

## Affiliation mapping

Recognize common markers:
- numeric superscripts: `1`, `2`, `3`
- letters: `a`, `b`, `c`
- symbols used for correspondence: `*`, `†`, `‡`
- multiple affiliations per author

Store affiliation components when separable:
- department
- faculty/school
- institution
- city
- state/region
- postal code
- country

Do not fabricate missing location components.

## Corresponding author

Identify the corresponding author using explicit evidence such as:
- `*Correspondence:`
- `Corresponding author:`
- email linked to a marked author
- OJS/submission metadata when supplied

Match the contact back to the author list. If multiple matches are possible, flag the ambiguity.

## ORCID

Normalize display format only when requested. Never invent or infer an ORCID.

Preferred machine form:

```text
https://orcid.org/0000-0000-0000-0000
```

## Name conflicts

Flag:
- different spelling across PDF/DOCX/OJS export
- initials in one source and full names in another
- changed author order
- corresponding-author marker attached to different people
- affiliation superscript mismatch

Do not silently reconcile authorship conflicts.

## Output model

```yaml
authors:
  - order: 1
    display_name: "Jane A. Doe"
    given_name: "Jane"
    middle_name: "A."
    family_name: "Doe"
    corresponding: true
    email: "jane@example.org"
    orcid: null
    affiliation_ids: [1, 2]
    provenance:
      source: "article.pdf"
      location: "page 1"
      confidence: high
```

## OJS-specific note

Only return author fields enabled by the journal profile or requested by the user. Internal parsing may detect more fields than are displayed.
