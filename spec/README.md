# Franciscus corpus format — specification

**Version:** 1.3 · **Status:** Normative

This folder is the normative specification for everything under
`franciscus-data`. It is split by file kind:

| Spec | Covers |
|------|--------|
| [books.md](books.md) | Source texts and translations — `books/<id>.md`, `books/<id>.<lang>.md` |
| [annotations.md](annotations.md) | Semantic annotation sidecars — `books/<id>.yaml` |
| [topics.md](topics.md) | Topic pages — `topics/<type>/<value>.md` |

The controlled vocabulary of topic values referenced by annotations and topic
pages lives in [`../topics/topics.yaml`](../topics/topics.yaml).

The keywords MUST, MUST NOT, REQUIRED, SHOULD and MAY are used as in
[RFC 2119](https://www.rfc-editor.org/rfc/rfc2119).

## Overview

Each source text is a single Markdown file augmented with YAML frontmatter and a
restricted set of inline HTML elements. The format preserves the paragraph
numbering of the reference print edition while remaining machine-parseable. A
Rust CLI ([`franciscus/server`](https://github.com/FranciscusApp/franciscus))
ingests the corpus into the SQLite database the app ships.

## Conformance summary

| Requirement                            | Level    | Spec |
|----------------------------------------|----------|------|
| Source frontmatter `title`/`author`/`date`/`reference_edition` | REQUIRED | [books](books.md#frontmatter) |
| Work `id` from filename stem (not frontmatter) | REQUIRED | [books](books.md#frontmatter) |
| Exactly one `#` heading (title)        | REQUIRED | [books](books.md#document-title) |
| Chapters as `##` with `<a id="…">`     | REQUIRED | [books](books.md#chapters) |
| No headings below level 2              | REQUIRED | [books](books.md#file-structure) |
| Numbered paragraphs in `<p id="…">`    | REQUIRED | [books](books.md#paragraphs) |
| Non-paragraph text in `<aside>`        | REQUIRED | [books](books.md#aside-blocks) |
| Scripture refs in `<ref to="…">`       | REQUIRED | [books](books.md#scripture-references) |
| `label` attribute on `<p>`             | OPTIONAL | [books](books.md#optional-label-attribute) |
| `[n]` verse markers inside `<p>`       | OPTIONAL | [books](books.md#verse-numbers) |
| Translation `title`/`author`/`translator`/`provenance`/`status` | REQUIRED | [books](books.md#translation-files) |
| `provenance` / `by` on translation `<p>` | OPTIONAL | [books](books.md#per-paragraph-provenance) |
| Annotations in sidecar `<id>.yaml`     | OPTIONAL | [annotations](annotations.md) |
| `paragraph` + (`topics` or `relations`) + `by` | REQUIRED (per annotation) | [annotations](annotations.md) |
| `provenance` on annotation             | OPTIONAL | [annotations](annotations.md) |
| Topic page `description` (`type` from `topics/<type>/`) | REQUIRED | [topics](topics.md#frontmatter) |
