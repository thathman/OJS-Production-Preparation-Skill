# Article Publication Metadata Acceptance Scenarios

These scenarios supplement `tests/ACCEPTANCE.md` for OJS article-publication preparation.

## AM01: Colon separates Title and Subtitle

**Given** the published title is `Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

**When** article publication metadata is prepared

**Then** Title is `Traumatic Brain Injury Management in Nigeria`.

**And** Subtitle is `A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`.

**And** the colon is not stored in either field.

---

## AM02: Main-title article becomes Prefix

**Given** the published title is `The Effects of X on Y: A Systematic Review`

**When** title metadata is prepared

**Then** Prefix is `The`.

**And** Title is `Effects of X on Y`.

**And** Subtitle is `A Systematic Review`.

**And** the `A` beginning the subtitle is not moved into Prefix.

---

## AM03: Author keywords remain source-faithful

**Given** an author-supplied keyword contains more than three words

**When** clean or strict extraction runs

**Then** the keyword is preserved rather than shortened merely to satisfy the typical one- to three-word guideline.

**But when** keyword generation is explicitly authorised

**Then** generated keywords normally use concise one- to three-word phrases.

---

## AM04: Supporting Agencies are not affiliations

**Given** an author is affiliated with University X

**And** no funding, sponsorship or institutional support from University X is stated

**When** Supporting Agencies is prepared

**Then** University X is not copied into Supporting Agencies solely from the affiliation.

---

## AM05: Coverage keeps Dublin Core meaning

**Given** an article discusses hypertension generally

**And** no spatial, temporal or jurisdictional coverage is stated or safely inferable

**When** Coverage is prepared

**Then** `Hypertension` is not inserted as Coverage.

**Given** the study explicitly took place in Ekiti State, Nigeria

**Then** that place may be used as spatial Coverage when the field is enabled.

---

## AM06: Submission DOI is not Source

**Given** the submission DOI is `10.1234/example.1`

**And** the article is not derived from another identified work

**When** Source is prepared

**Then** `10.1234/example.1` is not entered into Source.

**And** Source follows the configured missing-value policy.

---

## AM07: Type and Section remain separate

**Given** an OJS submission is in the section `Editorial`

**And** the main content is a conventional textual editorial

**When** metadata is prepared

**Then** Section remains `Editorial`.

**And** Type is `Text` when the field is enabled and the textual nature is unambiguous.

---

## AM08: Data Availability is never invented

**Given** no Data Availability Statement appears in the sources

**When** metadata is prepared

**Then** the skill does not invent `Data not available`, a repository, an accession number or any equivalent claim unless journal policy explicitly authorises a standard statement.

---

## AM09: Publisher ID is never DOI

**Given** a DOI exists for the article

**When** article Identifiers are prepared

**Then** the DOI is not copied into Publisher ID.

**And** Publisher ID is populated only from an established external-database/deposit identifier convention or explicit evidence.

---

## AM10: Article galley fields are Label and URL Path

**Given** the final article PDF is already supplied

**When** article Galleys are prepared

**Then** the supplied file is treated as the candidate galley.

**And** the OJS galley information returned includes Galley Label and URL Path.

**And** the skill detects the journal's label/path convention before generating values.

---

## AM11: Article Galley URL Path is distinct from article URL

**Given** the journal uses a public article path convention and a separate galley path convention

**When** article publication is prepared

**Then** the two paths remain independent.

**And** the article URL is not automatically copied into Galley URL Path.

---

## AM12: Generated article URL Path summarizes the title

**Given** the title is `Traumatic Brain Injury Management in Nigeria: A Critical Review of Systemic Challenges and Integrated, Context-Specific Solutions`

**When** the journal permits a generated article URL Path and has no stronger established article-path convention

**Then** the skill proposes a short semantic summary such as `tbi-management-nigeria`.

**And** it does not slugify the full title.

**And** it normally targets roughly 3–6 meaningful terms.

**And** it checks for path collisions before finalising the suggestion.

**If** a collision exists

**Then** it adds the smallest useful distinguishing term rather than expanding to the complete title.

---

## AM13: Article publication response follows the editor's six-step order

**Given** an article publication record is being prepared

**When** the skill returns the user-facing OJS record

**Then** the top-level groups appear in exactly this order:

1. `Title & Abstract`
2. `Contributors`
3. `Metadata`
4. `References`
5. `Galleys`
6. `Issue`

**And** Section, Prefix, Title, Subtitle and Abstract appear under `Title & Abstract`.

**And** all contributors are returned in publication order under `Contributors`.

**And** article Publisher ID, when used, is displayed under `Metadata` rather than as a seventh top-level group.

**And** the complete reference list is pasted under `References` when the source contains references.

**And** Galley file, Galley Label and Galley URL Path appear under `Galleys`.

**And** Issue assignment, Pages, Date Published, DOI and article URL Path appear under `Issue`.
