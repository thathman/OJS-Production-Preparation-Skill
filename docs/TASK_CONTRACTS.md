# Operational Task Contracts

This document turns the skill architecture into concrete execution recipes. Each task defines what must be inspected, what may be generated, what should be returned, and what blocks completion.

## `prepare_issue`

### Required inspection
- all supplied issue files
- issue-level metadata source if present
- every article assigned to the issue
- current journal profile

### Detect
- volume
- number
- year
- publication date
- issue title if present
- article order
- page ranges/article numbers
- issue DOI if present
- cover if supplied

### Generate only when allowed
- issue title
- issue description
- short summary
- URL path
- cover brief
- SEO description

### Blocking conflicts
- contradictory volume/issue/year
- duplicate or overlapping pagination when continuous pagination is required
- articles apparently belonging to a different issue

### Output
Only enabled issue fields plus warnings.

---

## `quicksubmit_single`

### Required inspection
- target article source(s)
- journal profile
- QuickSubmit field configuration

### Return
Only enabled QuickSubmit fields in configured order.

### Extraction rules
- article title: exact/clean according to mode
- abstract: source-faithful
- keywords: source-faithful
- references: configured reference mode
- declarations: mapped without paraphrasing
- generated article metadata: prohibited unless the field policy explicitly allows it

### Blocking conflicts
- title conflict that cannot be resolved by source hierarchy
- author list conflict
- article belongs to a different journal/issue where that matters to the task

---

## `quicksubmit_batch`

Run `quicksubmit_single` for each article in issue order, then run cross-article QA.

Return:
1. article-by-article QuickSubmit values
2. article-specific warnings
3. one batch summary

Do not repeat journal-level configuration on every article.

---

## `extract_article_metadata`

### Detect broadly
- title
- subtitle
- section/type
- abstract
- keywords
- journal citation metadata
- DOI
- article history
- declarations
- references
- pagination/article number

### Display narrowly
Return only requested or profile-enabled fields.

---

## `extract_authors`

Inspect the author line, affiliation block, correspondence block, ORCID metadata, and any supplied submission/export metadata.

Return only enabled author fields and flag ambiguous name splits.

---

## `extract_declarations`

Search the article for explicit declarations and common equivalent labels.

Do not infer a declaration from the body text.

For a missing required declaration, apply the profile missing-value rule.

---

## `prepare_references`

Use configured mode:
- exact
- clean
- normalize
- validate

For `validate`, report issues without rewriting unless normalization/copyediting is also enabled.

---

## `validate_metadata`

Compare existing OJS/user-supplied metadata against authoritative sources.

For each checked field classify:
- match
- formatting_only_difference
- mismatch
- missing_in_ojs
- missing_in_source
- ambiguous

Do not overwrite values automatically.

---

## `generate_issue_description`

### Required
Every article assigned to the issue must be inspected first.

### Internal map
For each article identify:
- article type
- main research question/topic
- method/research level
- major contribution
- population/model where relevant

### Generation constraint
Do not claim a unifying theme unless supported by the article set.

Return only requested length/style.

---

## `generate_issue_cover_brief`

Use whole-issue synthesis plus journal branding.

Brief should normally specify:
- concept/story
- visual hierarchy
- imagery direction
- palette direction
- journal logo/name placement
- issue title/theme if used
- volume, issue, year
- elements to avoid

Do not generate playful/cartoon styling by default for scholarly journals.

---

## `style_ojs_html`

Input text is authoritative unless user asks for copyediting.

Allowed transformation:
- paragraph structure
- emphasis
- scientific italics
- safe inline CSS
- simple lists/tables

Forbidden transformation without permission:
- substantive rewriting
- unsupported claims
- silent corrections

---

## `production_qa`

Check configured issue, article, author, rights, pagination, declaration, reference, and galley requirements.

Return:
- `ready`
- `ready_with_warnings`
- `not_ready`

Blocking requirements must prevent `ready`.

---

## `prepare_doi_metadata`

Prepare structured DOI/Crossref-ready metadata only from confirmed or profile-authorized generated values.

Do not imply registration has occurred.

---

## `full_production_workflow`

Run in this order:
1. source intake
2. source classification
3. provisional journal profile detection if needed
4. gap/conflict questions only when necessary
5. issue grouping
6. per-article extraction
7. author/declaration/reference extraction
8. issue metadata preparation
9. issue-level editorial generation where enabled
10. pagination and cross-article QA
11. DOI metadata preparation if enabled
12. final readiness report

Pause for user input only when an unresolved item materially affects a required downstream step.
