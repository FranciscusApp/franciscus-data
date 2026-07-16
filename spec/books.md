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
| `category`          | string | –   | Machine key of the collection this work belongs to (see [Categories](#categories)) |
| `sequence`          | int    | –   | Ordinal of this work **within its category** (ascending) |

`source` is this rendition's provenance (the source text's counterpart to a
translation's `translation_source`); the book page turns it into a localized
"Source text from …" note.

`category` and `sequence` place the work in the browsing hierarchy: books are
grouped by `category` (ordered by the category's own sequence) and, within a
group, ordered by `sequence`. Both are language-invariant, so — like `date` and
`reference_edition` — they live only on the source `<id>.md`, never repeated in a
translation file. A `category` value MUST match a `category:` key defined in
`books/categories.yaml`. Editorial **descriptions** are NOT frontmatter: they describe the work (not a
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
category: biographiae
sequence: 1
---
```

### Categories

`books/categories.yaml` defines the closed set of collections a book's
`category` may reference (values not defined there are rejected, as the topic
vocabulary is). It is a YAML **list**; each entry is one category:

| Field         | Type                | Req | Description                                          |
|---------------|---------------------|-----|------------------------------------------------------|
| `category`    | string              | ✓   | Machine key (a foreign key from book `category`); never localized |
| `sequence`    | int                 | ✓   | Order of this category among the others (ascending)  |
| `title`       | map `lang → string` | ✓   | Group heading, keyed by **UI** language              |
| `description` | map `lang → string` | –   | Short subtext under the heading, keyed by UI language |

`title` / `description` are keyed by UI language (like the per-book cover
descriptions), because the group heading is application chrome, not corpus text.
English (`en`) is the fallback when the active UI language is absent.

```yaml
- category: biographiae
  sequence: 1
  title:
    en: Early Biographies
    it: Biografie antiche
  description:
    en: The earliest lives of Francis, by his companions and successors.
    it: Le prime vite di Francesco, dai suoi compagni e successori.
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

### Optional `label_format` attribute

A `label_format` attribute MAY be added to control how the `label` is
presented. It has two values:

- `normal` (the default when the attribute is absent) — the label renders as
  the inline paragraph marker, in the muted number column beside the text.
- `heading` — the label renders as a **section heading** above the paragraph
  text, giving a sub-chapter division real visual weight.

```html
<p id="epfid1-1" label="De illis qui faciunt poenitentiam" label_format="heading">…</p>
```

`label_format="heading"` is how a work's internal chapters are expressed **when
the work is not split into its own book**: the sub-chapter title lives in the
paragraph's `label`, promoted to a heading, instead of being mis-encoded as an
`<aside>` (which the spec reserves for rubrics/incipits/explicits) or as a
forbidden deeper heading level. A paragraph with `label_format="heading"` SHOULD
carry a `label`; the heading has nothing to render otherwise. This does not
introduce a new heading level — `##` remains the only heading — and the `no
heading levels beyond ##` invariant is unchanged.

`label_format` lives only on the source `<p>`; translation `<p>` elements
inherit it (the label itself is translated per the localized heading text where
one is authored on the translation's own `<p>`).

### Optional `layout` attribute

By default a `<p>` is **prose**: the renderer reflows its body as a single
running paragraph and the source's own line wrapping (a convenience for editing)
is not significant — internal newlines collapse to spaces. Some texts, however,
are *not* prose: their line structure is part of the text. A `layout` attribute
MAY be added to preserve it. It has three values:

- `prose` (the default when the attribute is absent) — running paragraph;
  source line breaks are insignificant.
- `verse` — **each source line is a line of the text.** The renderer preserves
  the newlines authored in the `<p>` body. Use for hymns and free-verse pieces
  (e.g. the *Canticum fratris solis*), where the line — not the sentence — is the
  unit. Verse markers (`[N]`) still open their line as usual.
- `psalm` — **pointed chant.** Like `verse`, source lines are preserved (one
  psalm verse per line); additionally, the `<caesura>` marks within each verse
  (§ [Recitation and apparatus marks](#recitation-and-apparatus-marks)) become
  the intra-verse line breaks, so each verse is displayed as its pointed
  half-lines. Use for the psalters (e.g. the *Officium Passionis*). The `<var>`
  psalter sigla stay hidden as under any other layout.

```html
<p id="cantsol-1" layout="verse">
[1] Altissimu onnipotente bon signore,
tue so le laude, la gloria e l'onore et onne benedictione.
…
</p>
```

Because the line structure is carried by the `<p>` body itself (newlines) and by
the `<caesura>` marks — both already preserved through ingestion — `layout` only
tells the renderer to honor what is there; it changes no other encoding. Like
`label_format`, `layout` lives only on the source `<p>` and translation `<p>`
elements inherit it (a translation preserves its own authored line breaks, which
SHOULD mirror the source's).

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
