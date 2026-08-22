# Website-Assisted Journal Discovery

## Goal

Use the journal website as a structured evidence source before asking configuration questions. The skill should inspect the website only when the user provides a journal URL or explicitly asks it to use the website.

## Prime rule

Do not ask the user for journal settings that can be determined confidently from the current website and supplied files.

## Discovery order

Start with the supplied URL, then inspect likely public pages when reachable:

1. Home page
2. About / About the Journal
3. Submissions / Author Guidelines
4. Editorial Policies / Policies
5. Open Access / Copyright / Licensing
6. Contact
7. Editorial Team
8. Current Issue
9. Archives
10. A representative article landing page
11. Announcements if publication model or frequency is unclear

Do not crawl unrelated pages.

## OJS path patterns

Recognize common OJS routes without assuming they are always present:

- `/index.php/{journal}/about`
- `/index.php/{journal}/about/submissions`
- `/index.php/{journal}/about/editorialPolicies`
- `/index.php/{journal}/issue/current`
- `/index.php/{journal}/issue/archive`
- `/index.php/{journal}/article/view/{id}`
- `/about`
- `/submissions`
- `/issue/current`
- `/issue/archive`

If a custom theme or rewritten URLs are used, follow visible navigation rather than forcing these paths.

## What to detect

### Identity
- full journal title
- abbreviation or short title
- publisher / institution
- journal URL
- ISSN and eISSN
- DOI prefix if displayed

### Publication model
- volume and issue usage
- continuous publication
- page ranges vs article numbers
- publication frequency
- issue title conventions
- issue URL conventions

### Sections
Infer only from repeated evidence on submissions pages, current/archive tables of contents, or article labels.

Examples:
- Original Article
- Review Article
- Case Report
- Editorial
- Commentary
- Short Communication

Do not create sections based only on generic publishing knowledge.

### Policies
- licence
- copyright ownership
- open-access model
- peer review model
- ethics requirements
- data availability requirements
- conflict-of-interest requirements
- author contribution requirements
- funding disclosure requirements

### Contacts
Detect email addresses together with their functional context.

Store contact records as role-aware values, for example:

```yaml
contacts:
  editorial:
    email: editorial@example.org
    source: contact page
  technical_support:
    email: support@example.org
    source: contact page
```

Do not collapse all detected emails into one generic address.

## Conflict handling

When website sources disagree:

1. Prefer the most specific current policy page over a generic footer.
2. Prefer current issue/article pages over old archived pages for current publication conventions.
3. Record both values when the conflict is material.
4. Ask the user only if the conflict affects the requested production task.

Example:

```text
Licence conflict detected
- Open Access page: CC BY-NC 4.0
- 2023 article PDF: CC BY 4.0
Action: review required before rights metadata is generated.
```

## Confidence rules

### High
The value appears consistently in at least two authoritative locations, or appears on a dedicated policy/metadata page.

### Medium
The value appears once in a plausible authoritative location.

### Low
The value is inferred from layout, historical content, or a single ambiguous mention.

## Website-to-profile merge

Website findings should be merged with file findings rather than replacing them.

Every merged field should retain:
- selected value
- source(s)
- confidence
- conflict state
- selection reason if conflicting sources exist

## Stop condition

Stop website discovery when the journal profile is sufficiently complete for the requested task. Do not continue browsing merely to fill unused profile fields.
