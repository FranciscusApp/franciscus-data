# Franciscus Book Format Specification

**Version:** 1.0  
**Status:** Normative  
**Applies to:** all files under `books/`

## 1. Overview

Each source text is encoded as a single Markdown file augmented with YAML frontmatter and a restricted set of inline HTML elements. The format is designed to preserve the paragraph numbering of the reference print edition while remaining machine-parseable.

## 2. File Structure

A conforming document MUST contain the following sections, in order:

1. YAML frontmatter (§3)
2. Document title as a level-1 heading (§4)
3. One or more chapters, each introduced by a level-2 heading (§5)

No other heading levels (level-3 through level-6) are permitted.

## 3. Frontmatter

The file MUST begin with a YAML frontmatter block delimited by `---`. The following fields are REQUIRED:

| Field               | Type   | Description                                              |
|---------------------|--------|----------------------------------------------------------|
| `id`                | string | Unique short identifier for the work (e.g. `1Cel`)      |
| `title`             | string | Full title of the work                                   |
| `author`            | string | Author or attributed author                              |
| `date`              | string | Date or date range of composition                        |
| `reference_edition` | string | Print edition whose numbering this file follows          |
| `license`           | string | SPDX license identifier (e.g. `CC0-1.0`)                |

Example:

```yaml
---
id: 1Cel
title: "Vita Prima S. Francisci"
author: "Tommaso da Celano"
date: "1228"
reference_edition: "Analecta Franciscana X (Quaracchi)"
license: CC0-1.0
---
```

## 4. Document Title

A single level-1 heading (`#`) containing the title of the work.

```markdown
# VITA PRIMA S.FRANCISCI
```

## 5. Chapters

Each chapter MUST be introduced by a level-2 heading (`##`) containing an inline anchor element that provides a machine-readable chapter identifier.

```markdown
## PROLOGUS <a id="prolog"></a>
```

The `id` attribute value serves as the chapter's canonical identifier.

## 6. Paragraphs

Body text MUST be wrapped in `<p>` elements with an `id` attribute that preserves the paragraph number from the reference edition.

```html
<p id="prolog-1">
Text of the paragraph...
</p>
```

### 6.1 Paragraph `id` Construction

- When the edition uses **continuous numbering**, `id` is the paragraph number as a bare integer (e.g. `id="1"`).
- When the edition **restarts numbering per chapter**, `id` is formed by joining the chapter id and the paragraph number with a hyphen (e.g. `id="prolog-1"`).

### 6.2 Optional `label` Attribute

A `label` attribute MAY be added when the display label differs from the `id`:

```html
<p id="42" label="XLII">...</p>
```

If omitted, renderers SHOULD use the `id` value as the display label.

## 7. Aside Blocks

Text that is not part of a numbered paragraph (subtitles, rubrics, incipits, explicits) MUST be wrapped in `<aside>` elements.

```html
<aside>
IN NOMINE DOMINI. AMEN.
INCIPIT PROLOGUS SUPER VITAM BEATI FRANCISCI.
</aside>
```

Renderers SHOULD display aside content only when it falls within the block currently being viewed (e.g. a chapter or episode). Aside blocks MUST NOT contain `<p>` elements.

## 8. Scripture References

Cross-references to biblical passages MUST use the `<ref>` inline element. The `to` attribute contains the passage identifier in **anglophone biblical notation** (abbreviated book name, space, chapter, colon, verse or verse range).

```html
<ref to="Act 1:1">quia omnia quae fecit et docuit</ref>
```

The `<ref>` element wraps the source text to which the reference applies, not the reference citation itself.

## 9. Verse Numbers

Within a `<p>` element, a number enclosed in square brackets denotes a verse number from the reference edition:

```
[1] Actus et vitam beatissimi patris nostri Francisci...
[2] Sed utinam eius merear esse discipulus...
```

Verse numbers are presentational. They are consumed by the renderer for display purposes and carry no semantic meaning for the data engine.

## 10. Conformance Summary

| Requirement                            | Level    |
|----------------------------------------|----------|
| YAML frontmatter with all six fields   | REQUIRED |
| Exactly one `#` heading (title)        | REQUIRED |
| Chapters as `##` with `<a id="…">`     | REQUIRED |
| No headings below level 2              | REQUIRED |
| Numbered paragraphs in `<p id="…">`    | REQUIRED |
| Non-paragraph text in `<aside>`        | REQUIRED |
| Scripture refs in `<ref to="…">`       | REQUIRED |
| `label` attribute on `<p>`             | OPTIONAL |
| `[n]` verse markers inside `<p>`       | OPTIONAL |
