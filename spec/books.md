# Source texts and translations

**Applies to:** `books/<id>.md` (canonical source) and `books/<id>.<lang>.md`
(translations).

Annotation sidecars (`books/<id>.json`) are specified in
[annotations.md](annotations.md); topic pages in [topics.md](topics.md).

## File structure

A conforming document MUST contain the following sections, in order:

1. YAML frontmatter (§ [Frontmatter](#frontmatter))
2. Document title as a level-1 heading (§ [Document title](#document-title))
3. One or more chapters, each introduced by a level-2 heading (§ [Chapters](#chapters))

No other heading levels (level-3 through level-6) are permitted.

## Frontmatter

The file MUST begin with a YAML frontmatter block delimited by `---`. The
following fields are REQUIRED:

| Field               | Type   | Description                                              |
|---------------------|--------|----------------------------------------------------------|
| `id`                | string | Unique short identifier for the work (e.g. `1Cel`)      |
| `title`             | string | Full title of the work                                   |
| `author`            | string | Author or attributed author                              |
| `date`              | string | Date or date range of composition                        |
| `reference_edition` | string | Print edition whose numbering this file follows          |
| `license`           | string | SPDX license identifier (e.g. `CC0-1.0`)                |

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

### Translation files

A translation file is named `<id>.<lang>.md` (BCP-47 `<lang>` suffix on the
source stem) and lives beside the source it translates. The frontmatter MUST
repeat the same `id` as the source so the engine can match them, and the
`title:` field MUST be in the target language — the rest of the application
displays it as the book's title whenever the active corpus language is
`<lang>`. `author`, `date`, `reference_edition` and `license` SHOULD match the
source; only `title` (and per-chapter `##` headings) are translated.

> **Known limitation.** Unlike annotation sidecars, a translation file carries no
> provenance or `verified` field — the format has no way to record which
> passages of a translation are human-authored or human-reviewed. A translation
> is provenance-wise all-or-nothing. Until the format gains per-passage
> provenance, consumers SHOULD treat any translation as unverified unless stated
> otherwise out of band.

```yaml
---
id: 1Cel
title: "Vita prima di San Francesco"
author: "Tommaso da Celano"
date: "1228-1229"
reference_edition: "Analecta Franciscana X (Quaracchi, 1926-1941)"
license: CC0-1.0
---
```

## Document title

A single level-1 heading (`#`) containing the title of the work.

```markdown
# VITA PRIMA S.FRANCISCI
```

## Chapters

Each chapter MUST be introduced by a level-2 heading (`##`) containing an inline
anchor element that provides a machine-readable chapter identifier.

```markdown
## PROLOGUS <a id="prolog"></a>
```

The `id` attribute value serves as the chapter's canonical identifier.

## Paragraphs

Body text MUST be wrapped in `<p>` elements with an `id` attribute that
preserves the paragraph number from the reference edition.

```html
<p id="prolog-1">
Text of the paragraph...
</p>
```

### Paragraph `id` construction

- When the edition uses **continuous numbering**, `id` is the paragraph number
  as a bare integer (e.g. `id="1"`).
- When the edition **restarts numbering per chapter**, `id` is formed by joining
  the chapter id and the paragraph number with a hyphen (e.g. `id="prolog-1"`).

### Optional `label` attribute

A `label` attribute MAY be added when the display label differs from the `id`:

```html
<p id="42" label="XLII">...</p>
```

If omitted, renderers SHOULD use the `id` value as the display label.

## Aside blocks

Text that is not part of a numbered paragraph (subtitles, rubrics, incipits,
explicits) MUST be wrapped in `<aside>` elements.

```html
<aside>
IN NOMINE DOMINI. AMEN.
INCIPIT PROLOGUS SUPER VITAM BEATI FRANCISCI.
</aside>
```

Renderers SHOULD display aside content only when it falls within the block
currently being viewed (e.g. a chapter or episode). Aside blocks MUST NOT
contain `<p>` elements.

## Scripture references

Cross-references to biblical passages MUST use the `<ref>` inline element. The
`to` attribute contains the passage identifier in **anglophone biblical
notation** (abbreviated book name, space, chapter, colon, verse or verse range).

```html
<ref to="Act 1:1">quia omnia quae fecit et docuit</ref>
```

The `<ref>` element wraps the source text to which the reference applies, not
the reference citation itself.

## Verse numbers

Within a `<p>` element, a number enclosed in square brackets denotes a verse
number from the reference edition:

```
[1] Actus et vitam beatissimi patris nostri Francisci...
[2] Sed utinam eius merear esse discipulus...
```

Verse numbers are presentational. They are consumed by the renderer for display
purposes and carry no semantic meaning for the data engine.
