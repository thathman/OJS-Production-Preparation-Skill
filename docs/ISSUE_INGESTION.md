# Multi-File Issue Ingestion

## Goal

When the user uploads several manuscripts or a complete issue package, identify which files belong together, map article order, and prepare issue-level production without assuming filenames are reliable.

## Source-first rule

Inspect the files before asking for issue metadata. Extract volume, number, year, page ranges/article numbers, journal title, article titles, and publication dates where present.

## Accepted source combinations

The skill should work with any mix of:
- individual article PDFs
- DOCX manuscripts
- one complete issue PDF
- issue cover file
- table of contents file
- production spreadsheet
- OJS export/XML/CSV when supplied

## File classification

Classify each file as one of:
- `article_pdf`
- `article_manuscript`
- `complete_issue_pdf`
- `cover`
- `table_of_contents`
- `production_sheet`
- `metadata_export`
- `policy_or_guideline`
- `unknown`

Do not classify only by filename. Use content evidence.

## Article identity matching

When the same article appears in several files, match using a combination of:
1. exact or near-exact title
2. author list
3. DOI
4. volume/issue/page range
5. internal manuscript ID if present

Store matched files as one article record with multiple sources.

## Issue grouping

Group articles into an issue using, in order of reliability:
- explicit volume/issue/year citation
- issue table of contents
- complete issue PDF structure
- production spreadsheet
- repeated journal headers and pagination

If an article cannot be confidently assigned, leave it unassigned and flag it.

## Internal article map

Before generating issue-level content, create an internal map:

```yaml
issue:
  volume: 1
  number: 1
  year: 2025
articles:
  - order: 1
    title: "..."
    section: "Original Article"
    pages: "1-12"
    main_topic: "..."
    translational_level: "preclinical"
    source_files:
      - article01.pdf
      - article01.docx
```

The topic and translational level may be inferred for issue synthesis, but inferred values must not replace source metadata.

## Article order

Prefer explicit issue/table-of-contents order. If unavailable, use:
1. first page/article number
2. complete issue PDF order
3. production sheet order

Never sort alphabetically unless the journal clearly does so.

## Pagination QA

For page-based issues, check:
- duplicate first pages
- overlapping page ranges
- reversed ranges
- gaps when continuous pagination is expected
- mismatch between PDF footer and citation block

For article-number issues, check:
- duplicate article numbers
- inconsistent prefixes
- missing expected numbering

## Whole-issue synthesis

Issue title, description, summary, and cover concepts must be based on all assigned articles.

Before generation:
- identify the topic of each article
- identify major method or research level
- identify recurring themes only when supported by multiple articles
- preserve outlier topics rather than forcing false thematic unity

## Missing issue metadata

If volume, number, year, date, or issue title is not found:
- use the profile field policy
- ask only for required unresolved values
- generate only fields explicitly allowed by the profile

## Output

Depending on task, return:
- provisional issue metadata
- article order
- issue description
- short summary
- URL path
- cover brief
- pagination warnings
- per-article readiness
- issue readiness state
