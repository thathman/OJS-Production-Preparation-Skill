# Example: Adaptive OJS Production Session

## User

I need to prepare Volume 2, Issue 1 for OJS.

## Skill

Upload or provide whatever you have for the journal first. This may include the journal website, sample published articles, a complete issue, submission guidelines, policy/setup documents, production spreadsheets, or branding files. I will inspect these first, build a provisional journal profile automatically, and only ask about information I cannot determine confidently.

## User

Uploads:

- journal website URL
- one complete previous issue PDF
- four accepted article DOCX files

## Internal detection

The skill detects:

- Journal name: high confidence
- Publisher: high confidence
- Volume/issue model: high confidence
- Pagination: page ranges
- Article sections observed: Original Article, Review Article
- Licence: medium confidence
- Corresponding author format: high confidence
- QuickSubmit fields visibly used: Title, Abstract, Type, Rights, Keywords, References
- Data availability statements appear in all four manuscripts
- Funding statements appear in three manuscripts
- No evidence that Subjects or Disciplines are used

Conflict detected:

- website licence text says CC BY-NC 4.0
- previous issue PDF says CC BY 4.0

## Skill

I detected most of the journal configuration. I only need two decisions before I prepare the issue:

1. The current website says the journal uses CC BY-NC 4.0, while the previous issue PDF says CC BY 4.0. Which is authoritative for new publications?
2. I found no evidence that the journal uses the OJS Subjects or Disciplines fields. Should I leave both disabled?

## User

Use CC BY-NC 4.0. Disable Subjects and Disciplines.

## Confirmed profile behaviour

- Rights: journal policy override enabled
- Subjects: ignored
- Disciplines: ignored
- Extraction mode: clean
- QuickSubmit output: compact
- Missing ORCID: blank
- Missing funding: flag
- Issue descriptions: editorial generation allowed

## User

QuickSubmit article 1.

## Skill output

Only configured fields are returned:

- Section
- Title
- Abstract
- Type
- Rights
- Data Availability Statement
- Keywords
- Supporting Agencies
- References

The skill does not return Coverage, Subjects, Disciplines, or unrelated declarations.

## User

Now create the issue description.

## Internal issue synthesis

The skill reads all four assigned articles and builds an internal map:

| Article | Type | Main topic | Translational level |
|---|---|---|---|
| 1 | Original Article | Topic A | Preclinical |
| 2 | Original Article | Topic B | Clinical |
| 3 | Review Article | Topic C | Public health |
| 4 | Original Article | Topic D | Molecular |

## Skill output

A three-paragraph issue description is generated from all four articles because the profile requires whole-issue synthesis.

## User

Validate before publication.

## Skill

Returns:

- Issue metadata checks
- Article metadata checks
- Cross-article pagination checks
- Licence consistency checks
- Missing required declarations
- Conflicts requiring editorial review
- Final state: `ready`, `ready_with_warnings`, or `not_ready`
