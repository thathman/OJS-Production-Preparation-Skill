# OJS Article Publication Metadata

This document defines how the skill should prepare an existing OJS submission for publication. It mirrors the field meanings used by OJS and prevents semantic mistakes such as putting a DOI in Publisher ID, treating the editorial section as Dublin Core Type, or returning article fields in an editor-unfriendly order.

## Required response order

For article publication preparation, return the user-facing record in exactly this order unless the user explicitly requests another structure:

1. **Title & Abstract**
2. **Contributors**
3. **Metadata**
4. **References**
5. **Galleys**
6. **Issue**

Do not create a separate top-level `Identifiers` block in this six-step response format. Article Publisher ID, when actually used, is displayed under **Metadata** while remaining semantically an identifier internally.

## 1. Title & Abstract

Include:

- Section
- Prefix
- Title
- Subtitle
- Abstract

### Prefix, Title and Subtitle

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

Preserve structured abstract labels and source wording according to the active extraction mode.

## 2. Contributors

List every contributor in publication order. Include only source-supported contributor information such as:

- published/preferred name
- given name
- middle name or initials
- family name
- email
- affiliation
- country
- ORCID
- corresponding-author status

Preserve author order exactly. Do not invent missing contributor fields or guess ambiguous personal-name splits.

## 3. Metadata

### Keywords

Keywords are typically one- to three-word phrases indicating the submission's main topics.

Author-supplied keywords remain source-faithful even when longer. If generation is authorised because no keywords are supplied, generated keywords should normally be concise one- to three-word phrases.

### Supporting Agencies

Supporting Agencies identify research funding or other institutional support that facilitated the research.

Include explicit funders, sponsors, grant agencies or institutional support. Do not populate this field from author affiliations alone.

### Coverage

Coverage normally records:

- spatial location: place name or geographic coordinates
- temporal period: period label, date or date range
- jurisdiction: named administrative or legal entity

Coverage is not a general subject/topic field.

### Rights

Rights record rights held over the submission, which may include intellectual-property rights, copyright, licence rights or other property rights.

Use authoritative article or journal rights evidence according to the journal profile. Do not place DOI metadata here.

### Source

Source identifies another work or resource from which the submission is derived. The value may be an identifier such as the DOI of that source work.

Do not use the submission's own DOI as Source. Do not automatically use the journal citation as Source. Leave Source blank when the submission is not derived from another identified work and the journal has no special Source convention.

### Type

Type describes the nature/genre of the main content using Dublin Core-style resource types.

For ordinary journal articles, editorials, reviews and commentaries, `Text` is normally appropriate. OJS Section and Dublin Core Type are separate:

- Section: `Editorial`, `Original Articles`, `Review Articles`, etc.
- Type: normally `Text` for those textual publications

### Data Availability Statement

Extract the authors' statement. Never invent a repository, accession number, availability status or data-sharing claim. The absence of a statement must not be converted automatically into `Data not available` unless the journal explicitly authorises that wording.

### Publisher ID

Publisher ID may record an identifier from an external database or deposit workflow.

Publisher ID is **not a DOI field**.

Rules:

- never put the article DOI in Publisher ID
- detect whether the journal actually uses article-level Publisher IDs
- detect the established identifier pattern when possible
- ask only when usage or pattern cannot be determined
- leave the field unused when the journal does not use it

In the six-step user-facing response, display article Publisher ID under **Metadata** rather than as a separate top-level Identifiers section.

## 4. References

When the supplied source contains references, paste the **complete copy-ready reference list** into the response. Do not return only a reference count or a statement that references should be entered later.

Supported modes:

- `exact`
- `clean`
- `normalize`
- `validate`

In clean mode, repair PDF line-wrap/layout artefacts while preserving bibliographic content. Flag unnumbered entries, numbering anomalies, duplicates, malformed references and conflicts without silently altering substantive content unless the active mode permits it.

Do not search for or insert missing DOIs unless DOI enrichment is explicitly enabled.

## 5. Galleys

For article galleys include:

- Galley file/candidate file when known
- **Galley Label**
- **URL Path**

A supplied final PDF/HTML/XML file may be treated as the candidate galley without asking the editor to upload it again.

Detect the journal's actual label convention. `PDF` is common but should not override an established journal-specific label.

The Galley URL Path is distinct from the article publication URL Path. The concise-title-summary rule applies to the article publication URL Path, not automatically to the Galley URL Path.

## 6. Issue

Include, when applicable:

- Issue assignment
- Pages
- Date Published
- DOI
- Article URL Path

### Article URL Path

When the journal permits a generated article URL Path and does not already have a stronger established pattern, generate a **short semantic summary of the article title**, not a slugified copy of the complete title.

Default behaviour:

- aim for roughly **3–6 meaningful terms**
- use lowercase words separated by hyphens
- preserve the article's most distinctive topic, intervention/exposure, population, condition or geographic context
- omit filler words and leading articles when they add no meaning
- omit generic title scaffolding when the remaining terms still identify the article clearly
- a familiar abbreviation may be used when well established and more readable
- do not automatically include the subtitle
- do not reproduce every meaningful word from the title
- do not include DOI, page range, volume or issue merely to make the slug unique unless required by journal convention
- check for collisions; if one exists, add the smallest useful distinguishing term

Example:

`Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

Preferred generated URL Path:

`tbi-management-nigeria`

Not preferred:

`traumatic-brain-injury-management-in-nigeria-a-critical-review-of-systemic-challenges-and-integrated-context-specific-solutions`

A journal's established article URL convention overrides this default summary strategy.

## QA rules

Flag at minimum:

- article response groups returned in the wrong order
- references omitted when the source contains a reference list
- Pages or article URL Path omitted when required by the publication screen
- title/subtitle colon left in the Title field
- main-title Prefix left attached to Title when the journal uses Prefix
- title reconstruction mismatch
- generated article URL Path that reproduces the complete title
- generated article URL Path that is too vague
- duplicate/colliding article URL Paths
- submission DOI used as Source
- any DOI used as Publisher ID
- Section name used as Dublin Core Type
- affiliation copied into Supporting Agencies without support/funding evidence
- topical words inserted into Coverage without spatial/temporal/jurisdictional meaning
- invented Data Availability wording
- missing required Galley Label
- Galley URL Path conflicting with the journal's established convention
- article Publisher ID conflicting with the journal's established external-ID convention
