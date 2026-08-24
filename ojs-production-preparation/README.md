# OJS Production Preparation Skill v1.0.1

This directory is the installable production skill package.

## Install

Install the `ojs-production-preparation` directory as a skill in any Agent Skills-compatible client. The entry point is `SKILL.md`.

## Runtime behaviour

The skill is journal-agnostic and source-first. It asks for source files and/or the journal website before asking configuration questions, detects as much of the journal setup as possible, asks only about gaps or conflicts, and returns only the fields needed for the selected OJS production task.

Issue preparation is organised into the OJS-facing scopes **Issue Data**, **Issue Galley**, and **Identifiers**. Publisher IDs for the issue and for issue galleys are treated as separate optional conventions and are detected before the skill asks the editor about them.

## Repository documentation

The repository root contains expanded design references, workflow guides, schemas, templates, examples and acceptance criteria. Those files support development and maintenance; the installable runtime package is intentionally compact.
