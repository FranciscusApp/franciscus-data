# Source texts and translations

**Applies to:** `books/<id>.md` (canonical source) and `books/<id>.<lang>.md`
(translations).

Annotation sidecars (`books/<id>.yaml`) are specified in
[annotations.md](annotations.md); topic pages in [topics.md](topics.md).

## File structure

A conforming document MUST contain the following sections, in order:

1. YAML frontmatter (§ [Frontmatter](#frontmatter))
2. Document title as a level-1 heading (§ [Document title](#document-title))
3. One or more chapters, each introduced by a level-2 heading (§ [Chapters](#chapters))

No other heading levels (level-3 through level-6) are permitted.

## Frontmatter

The file MUST begin with a YAML frontmatter block delimited by `---`. The work's
`id` is the filename stem (e.g. `1Cel.md` ⇒ `1Cel`), NOT a frontmatter field.
The corpus is CC0-1.0; the license is not repeated per file. Every field below
MUST be present in the order given, even the optional ones — an optional field
with no value is written as a bare `key:` (YAML null), not omitted. Source
frontmatter fields:

| Field               | Type   | Req | Description                                     |
|---------------------|--------|-----|-------------------------------------------------|
| `title`             | string | ✓   | Full title of the work                          |
| `author`            | string | ✓   | Author or attributed author                     |
| `date`              | string | ✓   | Date or date range of composition               |
| `reference_edition` | string | ✓   | Print edition whose numbering this file follows |
| `source`            | string | –   | Where the source-language text was obtained     |

`source` is this rendition's provenance (the source text's counterpart to a
translation's `translation_source`); the book page turns it into a localized
"Source text from …" note. Editorial **descriptions** are NOT frontmatter: they describe the work (not a
rendition) and are keyed by UI language, so they live in the per-book sidecar's
cover properties (see [annotations.md](annotations.md)). The book page's
**notes** are likewise not authored here — they are generated from each
rendition's provenance (`provenance` / `status` / `translation_source`, below).

```yaml
---
title: "Vita Prima S. Francisci"
author: "Tommaso da Celano"
date: "1228"
reference_edition: "Analecta Franciscana X (Quaracchi)"
source: "DocumentaCatholicaOmnia.eu"
---
```

### Translation files

A translation file is named `<id>.<lang>.md` (BCP-47 `<lang>` suffix on the
source stem) and lives beside the source it translates; the shared stem matches
it to its source. The `title:` field MUST be in the target language — the rest
of the application displays it as the book's title whenever the active corpus
language is `<lang>`. `date` and `reference_edition` are language-invariant and
inherited from the source, so they are NOT repeated. `author` is localized (the
same person, spelled for the language).

As with the source, all fields appear in the order below, optional ones included
(bare `key:` when empty).

| Field                | Type   | Req | Description                                          |
|----------------------|--------|-----|------------------------------------------------------|
| `title`              | string | ✓   | Localized title                                      |
| `author`             | string | ✓   | Localized author name                                |
| `translator`         | string | ✓   | Default `by` for every `<p>` (`Name <email>`)        |
| `provenance`         | string | ✓   | File-wide default provenance; per-`<p>` overridable  |
| `status`             | string | ✓   | Editorial status: `draft` · `in-review` · `final`    |
| `translation_source` | string | –   | What this was translated from                        |

`translator`, `provenance`, `status`, and `translation_source` are this
rendition's provenance; the book page turns them into a localized editorial note
(e.g. "Official translation from …", or an AI-draft caveat). As above,
descriptions belong in the sidecar, not here.

```yaml
---
title: "Vita prima di San Francesco"
author: "Tommaso da Celano"
translator: "Claude <noreply@anthropic.com>"
provenance: ai
status: draft
translation_source:
---
```

#### Per-paragraph provenance

A translation's `<p>` elements MAY carry `provenance` and `by` attributes to
record provenance per paragraph. Both are optional; when absent, a `<p>`
inherits the frontmatter value of the same name (`provenance` from frontmatter
`provenance`, `by` from frontmatter `translator`).

`provenance` is one of:

- `ai` — machine-produced, not human-reviewed.
- `reviewed` — machine-produced, then human-reviewed (the human vouches; the
  machine origin is preserved as an audit fact).
- `human` — human-produced.

`mixed` is not a value: it is a file-level aggregate derived from the per-`<p>`
values, never stored.

```html
<p id="prolog-1">…</p>                                  <!-- ai, Claude (inherited) -->
<p id="prolog-2" provenance="reviewed" by="Jane Doe">…</p>  <!-- overridden -->
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

## Recitation and apparatus marks

Some reference editions — notably Esser's *Opuscula* for the liturgical pieces
(the *Officium Passionis*, the *Laudes*) — carry two kinds of editorial markup
that are NOT part of the running text. Where a text preserves them, they MUST be
expressed with the inline elements below rather than left as raw punctuation or
parenthetical sigla, so the renderer can hide or style them independently. Both
are empty inline elements (written with an explicit closing tag, never
self-closed, so they survive `{@html}` rendering intact). They are hidden by
default.

### `<caesura>` — psalm pointing

Chanted psalms divide each verse at fixed pause points. These MUST be encoded as
`<caesura>` with a `kind`:

- `kind="mediant"` — the mediation, the main mid-verse pause (edition mark `*`).
- `kind="flexa"` — the flexa, a minor pause earlier in a long first limb
  (edition mark `'`).

```html
<p id="offpass-2-3">
Et posuerunt adversum me mala pro vobis<caesura kind="mediant"></caesura> et odium pro dilectione mea.
</p>
```

### `<var>` — psalter textual variant

Latin psalters exist in two recensions (Roman and Gallican); critical editions
flag which one a phrase follows. Encode the siglum as `<var>` with a `psalter`
attribute — `"R"` (Romanum) or `"G"` (Gallicanum) — placed where the siglum
occurs. The element wraps no text (the variant applies to the surrounding
phrase); it is a positional marker.

```html
consilium fecerunt in unum<var psalter="G"></var>.
```

The `convert_refs`/`normalize_refs` postprocess step emits both elements
automatically from the reference edition's `*` / `'` / `(R)` / `(G)` markup.
