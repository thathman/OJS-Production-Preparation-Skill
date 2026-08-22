# DOI and Crossref Preparation

## Goal

Prepare DOI and Crossref-ready metadata from the journal profile and article sources without inventing missing identifiers.

## Scope

The skill may prepare structured metadata for:
- journal title
- ISSN/eISSN
- publisher
- DOI prefix
- article title
- subtitle
- author names
- ORCID
- affiliations
- publication date
- volume
- issue
- page range or article number
- abstract when required by downstream workflow
- licence URL
- references when configured
- resource URL

## DOI rules

Never invent a DOI suffix or prefix unless the profile explicitly defines a generation pattern and the user requests generation.

If a DOI appears in the manuscript or published article:
- extract it
- normalize presentation only when requested
- flag conflicts across sources

Preferred normalized form:

```text
10.xxxx/example.123
```

Do not store `https://doi.org/` as part of the canonical DOI value unless the target schema requires a URL.

## DOI generation

When DOI generation is enabled, the journal profile must define:
- DOI prefix
- suffix pattern
- allowed characters
- case policy
- collision policy

Example:

```yaml
doi:
  enabled: true
  prefix: "10.12345"
  suffix_pattern: "{journal_abbr}.{year}.{volume}.{issue}.{article_sequence}"
  lowercase: true
  collision_policy: flag
```

Generated DOI values must be marked `generated`, not `extracted`.

## Crossref-ready metadata checks

Before declaring metadata ready, verify configured required fields:
- title
- at least one author or configured group author
- publication year/date
- journal title
- ISSN when required by journal profile
- volume/issue when used by journal
- first page/article number when used by journal
- resource URL
- DOI when deposit includes assigned DOI

## Author handling

Use validated author order. Do not guess name splitting when ambiguous.

If ORCID is present, preserve the identifier exactly and validate basic format only.

## Publication dates

Keep source display dates and machine dates separately.

Example:

```yaml
published:
  display: "12th November, 2025"
  iso: "2025-11-12"
```

## References

Do not search for missing reference DOIs unless the profile or task explicitly enables DOI enrichment.

If reference DOI enrichment is enabled:
- keep original reference text
- add discovered DOI as supplemental metadata
- never rewrite the original reference solely to include the DOI without permission

## Licence metadata

Use the confirmed journal-level licence policy or article-level rights statement according to profile precedence rules.

Flag cases where:
- licence label and URL disagree
- article PDF and website disagree
- copyright owner differs across current sources

## Output modes

Support:
- human review table
- YAML/JSON structured record
- Crossref preparation checklist

This skill prepares metadata. It does not claim a DOI has been registered unless an external registration workflow actually confirms it.
