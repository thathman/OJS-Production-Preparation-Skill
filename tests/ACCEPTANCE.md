# Acceptance Scenarios

These scenarios define expected skill behaviour. They are behavioural tests, not implementation-specific unit tests.

## A01: First contact asks for sources before questionnaire

**Given** no journal profile and no sources

**When** user says: `Help me prepare an OJS issue.`

**Then** the skill asks for available files and/or journal website first.

**And** it does not present the full configuration questionnaire.

---

## A02: Existing sources are inspected immediately

**Given** the user already uploaded sample articles and a journal policy file

**When** user asks to start production

**Then** the skill inspects the supplied sources first.

**And** it does not ask the user to upload the same source types again.

---

## A03: Website fills profile automatically

**Given** a journal URL with a current About page, Submissions page, Open Access page, Contact page, and Current Issue

**When** journal detection runs

**Then** the profile attempts to detect identity, sections, licence, contacts, publication model, and citation conventions.

**And** only unresolved or conflicting material settings are asked about.

---

## A04: Unused fields are not returned

**Given** the profile disables Coverage, Subjects, Disciplines, and Source

**When** `quicksubmit_single` runs

**Then** those fields are not displayed.

**Even if** they can be inferred from the article.

---

## A05: Exact article metadata is not editorially improved

**Given** a source title with unusual capitalization or grammar

**When** extraction mode is `strict` or `clean`

**Then** wording and capitalization remain source-faithful except allowed layout cleanup in `clean` mode.

---

## A06: OCR cleanup does not become copyediting

**Given** `neurobehav- ioral` is split by a line break

**When** extraction mode is `clean`

**Then** output may use `neurobehavioral`.

**But given** `randomly assign to 5 groups`

**Then** the skill must not change it to `randomly assigned to five groups` unless copyediting is enabled.

---

## A07: Whole-issue description uses all articles

**Given** four articles assigned to an issue

**When** a standard issue description is requested

**Then** all four articles are inspected first.

**And** the description represents all four rather than using the first article as a proxy for the issue.

---

## A08: Article duplicates across PDF and DOCX are matched

**Given** the same article appears as PDF and DOCX

**When** issue ingestion runs

**Then** the skill groups both files into one article record based on title/authors/DOI/citation evidence.

---

## A09: Conflicting author lists block silent reconciliation

**Given** PDF author order differs from DOCX author order

**When** author extraction runs

**Then** a conflict is reported.

**And** the skill does not silently choose or reorder authors unless profile authority rules resolve the conflict unambiguously.

---

## A10: Corresponding author is mapped from explicit evidence

**Given** an author has `*` and a correspondence block contains the same email

**When** author extraction runs

**Then** that author is marked corresponding with high confidence.

---

## A11: Missing ORCID remains missing

**Given** no ORCID appears in any supplied source

**When** QuickSubmit metadata is prepared

**Then** the ORCID field follows the configured missing-value policy.

**And** no ORCID is inferred or searched unless explicitly enabled.

---

## A12: Licence mismatch is flagged

**Given** a policy page states one licence and a current article states another

**When** rights metadata is prepared

**Then** the conflict is reported with both sources.

**And** no licence is silently substituted unless journal profile precedence already authorizes that substitution.

---

## A13: Contact emails remain role-specific

**Given** website lists editorial and technical-support emails

**When** a manuscript-related contact value is needed

**Then** editorial contact is selected.

**And** technical support is not reused merely because it is an available email address.

---

## A14: Scientific inconsistency is reported, not fixed

**Given** abstract says `20 mg/kg` and methods says `100 mg/kg`

**When** QA runs

**Then** a scientific-content discrepancy is reported.

**And** neither value is changed automatically.

---

## A15: Pagination overlap prevents clean readiness

**Given** two articles have overlapping page ranges in a journal requiring continuous pagination

**When** production QA runs

**Then** issue status is not `ready`.

---

## A16: DOI is not invented

**Given** DOI is absent and no DOI-generation pattern is configured

**When** DOI metadata preparation runs

**Then** DOI remains missing or is flagged according to profile.

---

## A17: Generated values are labeled generated

**Given** issue description and URL path are authorized for generation

**When** they are generated

**Then** machine-readable provenance marks them as `generated`, not `exact` or `inferred`.

---

## A18: Task overrides do not permanently mutate profile

**Given** profile normally includes References

**When** user says `Skip references this time`

**Then** References are omitted for that run.

**And** the saved profile remains unchanged unless user explicitly asks to save the override.

---

## A19: OJS HTML styling preserves content

**Given** source text is supplied for styling

**When** `style_ojs_html` runs without copyediting permission

**Then** output may add safe HTML, emphasis, italics, and inline CSS.

**But** it may not rewrite substantive wording.

---

## A20: Full production workflow pauses only for material blockers

**Given** enough sources exist to continue through most production steps

**When** one optional profile field is unresolved

**Then** workflow continues.

**But when** a required field or material conflict blocks downstream accuracy

**Then** the skill asks a targeted question at that point.

---

## A21: Issue preparation follows OJS field groups

**Given** the user asks to prepare an issue

**When** `prepare_issue` runs

**Then** the core output is grouped as `Issue Data`, `Issue Galley`, and `Identifiers`.

**And** `Issue Data` contains Date Published; Identification with Volume, Number, Year and Title; Description; Cover image Alternate text; and URL Path.

**And** `Issue Galley` contains the galley file, Galley Label, Publisher ID when configured, and URL Path.

**And** `Identifiers` contains the issue-level Publisher ID when configured.

---

## A22: Existing whole-issue PDF becomes candidate issue galley

**Given** the user already supplied the final complete issue PDF

**When** issue preparation reaches the Issue Galley step

**Then** that file is treated as the candidate Issue Galley.

**And** the user is not asked to upload the same file again.

---

## A23: Publisher ID questions are conditional

**Given** Publisher ID usage cannot be detected from existing OJS records, published issues, or journal documentation

**When** issue preparation runs

**Then** the skill asks whether the journal uses an issue-level Publisher ID and whether it uses an issue-galley Publisher ID.

**And** it asks for a value or pattern only for the scope where the user confirms Publisher IDs are used.

---

## A24: Issue and galley Publisher IDs remain separate

**Given** an issue-level Publisher ID exists

**When** no evidence shows an issue-galley Publisher ID

**Then** the skill does not copy the issue-level Publisher ID into the Issue Galley field.

**And** the reverse is also true.

---

## A25: Issue and galley URL paths remain separate

**Given** a journal uses one URL-path convention for issues and another for issue galleys

**When** issue preparation runs

**Then** both conventions are preserved independently.

**And** the skill does not assume the Issue Data URL Path should be reused as the Issue Galley URL Path.

---

## A26: Configured Publisher ID is validated

**Given** the journal profile requires a Publisher ID with an established pattern

**When** production QA runs

**Then** a missing required Publisher ID prevents clean readiness.

**And** a value that conflicts with the established pattern is reported.
