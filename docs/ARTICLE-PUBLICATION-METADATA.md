# OJS Article Publication Metadata

This document defines how the skill should prepare metadata for an existing OJS submission during publication. It mirrors the field meanings used by OJS and prevents common semantic mistakes such as putting a DOI in Publisher ID or treating the editorial section as Dublin Core Type.

## Field order

When the OJS interface supports these fields, prepare the article in this order:

1. Section
2. Prefix
3. Title
4. Subtitle
5. Abstract
6. Metadata
   - Keywords
   - Supporting Agencies
   - Coverage
   - Rights
   - Source
   - Type
   - Data Availability Statement
7. Publication details
   - Pages
   - URL Path
   - Date Published
   - DOI
   - Issue assignment
8. Identifiers
   - Publisher ID, only when used
9. Galleys
   - Galley Label
   - URL Path

## Prefix, Title and Subtitle

For journals using a colon to separate title and subtitle:

- split on the first colon
- remove the colon from both OJS fields
- everything before the colon is the main title
- everything after the colon is the subtitle
- if the main title begins with standalone `A`, `An` or `The`, move it to Prefix
- do not move a leading article from the subtitle into Prefix

Example:

`The Effects of X on Y: A Systematic Review`

becomes:

- Prefix: `The`
- Title: `Effects of X on Y`
- Subtitle: `A Systematic Review`

The reconstructed display title should match the source title.

## Article URL Path

When the journal permits a generated article URL Path and does not already have a stronger established pattern, generate a **short semantic summary of the article title**, not a slugified copy of the complete title.

Default behaviour:

- aim for roughly **3–6 meaningful terms**
- use lowercase words separated by hyphens
- preserve the article's most distinctive topic, intervention/exposure, population, condition or geographic context
- omit filler words and leading articles such as `a`, `an` and `the` when they add no meaning
- omit generic title scaffolding such as `a study of`, `an assessment of`, `effects of`, `prevalence of`, `review of` or similar phrases when the remaining terms still identify the article clearly
- a familiar abbreviation may be used when it is well established by the title/article context and improves readability
- do not automatically include the subtitle merely because it exists
- do not reproduce every meaningful word from the title
- do not include the DOI, page range, volume or issue merely to make the slug unique unless the journal's established convention requires it
- check for collisions with other article paths; if a collision occurs, add the smallest useful distinguishing term rather than expanding to the full title

Example:

`Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

Preferred generated URL Path:

`tbi-management-nigeria`

Not preferred:

`traumatic-brain-injury-management-in-nigeria-a-critical-review-of-systemic-challenges-and-integrated-context-specific-solutions`

A journal's established article URL convention overrides this default summary strategy.

## Metadata definitions

### Keywords

Keywords are typically one- to three-word phrases indicating the submission's main topics.

Author-supplied keywords remain source-faithful even when a phrase is longer than three words. If the user authorises generation because no keywords are supplied, generated keywords should normally be concise one- to three-word phrases.

### Supporting Agencies

Supporting Agencies identify research funding or other institutional support that facilitated the research.

Include explicit funders, sponsors, grant agencies or institutional support. Do not populate this field from author affiliations alone.

### Coverage

Coverage normally records:

- spatial location: place name or geographic coordinates
- temporal period: period label, date or date range
- jurisdiction: named administrative or legal entity

Coverage is not a general subject or topic field.

### Rights

Rights record rights held over the submission, which may include:

- intellectual-property rights
- copyright
- licence rights
- other property rights

Use authoritative article or journal rights evidence according to the journal profile. Do not place DOI metadata here.

### Source

Source identifies another work or resource from which the submission is derived. The value may be an identifier such as the DOI of that source work.

Do not use the submission's own DOI as Source. Do not automatically use the journal citation as Source. Leave Source blank when the submission is not derived from another identified work and the journal has no special Source convention.

### Type

Type describes the nature or genre of the main content using Dublin Core-style resource types.

For ordinary journal articles, editorials, reviews and commentaries, `Text` is normally appropriate. Other types may include `Dataset`, `Image` or another Dublin Core type when that actually describes the main content.

OJS Section and Dublin Core Type are separate:

- Section: `Editorial`, `Original Article`, `Review Article`, etc.
- Type: normally `Text` for those textual publications

### Data Availability Statement

This is a short statement describing whether the authors made research data available and, when applicable, where readers can access it.

Extract the authors' statement. Never invent a repository, accession number, availability status or data-sharing claim. The absence of a statement must not be converted automatically into `Data not available` unless the journal explicitly authorises that wording.

## Identifiers

### Publisher ID

Publisher ID may record an identifier from an external database or deposit workflow. For example, an item exported for PubMed deposit may include an external publisher/database identifier.

Publisher ID is **not a DOI field**.

Rules:

- never put the article DOI in Publisher ID
- detect whether the journal actually uses article-level Publisher IDs
- detect the established identifier pattern when possible
- ask only when usage or pattern cannot be determined
- leave the field unused when the journal does not use it

Article-level Publisher ID is distinct from issue-level and Issue Galley Publisher IDs.

## Galleys

For article galleys, prepare the OJS data fields:

- **Galley Label** — required where OJS requires it
- **URL Path**

A supplied final PDF/HTML/XML file may be treated as the candidate galley without asking the editor to upload it again.

Detect the journal's actual label convention. `PDF` is common but should not override an established journal-specific label.

The Galley URL Path is distinct from the article publication URL Path and should follow the journal's existing pattern when one exists. The concise-title-summary rule applies to generated **article publication URL Paths**, not automatically to Galley URL Paths.

## QA rules

Flag at minimum:

- title/subtitle colon left in the Title field
- main-title Prefix left attached to Title when the journal uses Prefix
- title reconstruction mismatch
- generated article URL Path that simply reproduces the complete title
- generated article URL Path that is too vague to identify the article
- duplicate/colliding article URL Paths
- submission DOI used as Source
- any DOI used as Publisher ID merely because an identifier field is empty
- Section name used as Dublin Core Type
- author affiliation copied into Supporting Agencies without support/funding evidence
- topical words inserted into Coverage without spatial/temporal/jurisdictional meaning
- invented Data Availability wording
- missing required Galley Label
- Galley URL Path conflicting with the journal's established convention
- article Publisher ID conflicting with the journal's established external-ID convention
