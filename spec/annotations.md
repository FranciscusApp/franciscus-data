# Per-book sidecar files

**Applies to:** `books/<id>.yaml`.

Each book has a single YAML sidecar beside it, sharing the book's stem:
`<id>.yaml` (e.g. `1Cel.yaml` accompanies `1Cel.md`). The `book_id` is taken
from the filename and is NOT repeated inside the file. The sidecar holds two
sections, both optional:

1. **Cover properties** — book-level editorial metadata, keyed by **UI
   language** (`en`, `it`, …), at the top of the file.
2. **`annotations`** — the list of paragraph annotation mappings.

```yaml
description_short:
  en: The first biography of Saint Francis.
  it: La prima biografia di San Francesco.
description:
  en: |
    The *Vita prima* is the first biography of Saint Francis…
  it: |
    La *Vita prima* è la prima biografia di San Francesco…
annotations:
  - paragraph: prolog-1
    topics: person:st_francis_of_assisi, person:pope_gregory_ix
    relations: same_episode:LMj-prol-1
    by: Claude <noreply@anthropic.com>
    provenance: ai
```

## Cover properties

These describe the **work**, not any one rendition, so they are keyed by UI
language (the same blurb, translated) rather than by corpus language — they are
an annotation *about* the book, deliberately kept out of the text files' YAML
frontmatter.

| Field               | Type                | Description                                                              |
|---------------------|---------------------|--------------------------------------------------------------------------|
| `description_short` | map `lang → string` | One-line blurb; shown on the home book list.                             |
| `description`       | map `lang → string` | Long description, authored as **Markdown** (rendered to HTML at ingest). |

English (`en`) is the default and the fallback when a UI language has no entry.
The book page's editorial "Notes" are **not** authored here — they are generated
from each rendition's provenance (see [`books.md`](books.md)).

## `annotations`

A list of annotation mappings:

```yaml
annotations:
  - paragraph: prolog-1
    topics: person:st_francis_of_assisi, person:pope_gregory_ix
    relations: same_episode:LMj-prol-1
    by: Claude <noreply@anthropic.com>
    provenance: ai
```

| Field          | Type    | Level    | Description                                                                |
|----------------|---------|----------|----------------------------------------------------------------------------|
| `paragraph`    | string  | REQUIRED | The `id` of the `<p>` this annotation applies to                           |
| `paragraph_to` | string  | OPTIONAL | Last paragraph `id` when the annotation spans a range starting at `paragraph` |
| `topics`       | string  | OPTIONAL | Comma-separated list of `type:value` pairs (§ [topics](#topics-construction)) |
| `relations`    | string  | OPTIONAL | Comma-separated list of `reltype:target` pairs (§ [relations](#relations-construction)) |
| `by`           | string  | REQUIRED | Identity of the annotator (name, optionally `Name <email>`)                |
| `provenance`   | string  | OPTIONAL | `ai` · `reviewed` · `human`; defaults to `ai` (§ [provenance](books.md#per-paragraph-provenance)) |
| `comment`      | string  | OPTIONAL | Free-text note (English); applies to every pair expanded from the entry    |

An entry MUST carry at least one of `topics` or `relations`. A paragraph MAY
appear in more than one object; the data engine merges them.

## `topics` construction

Each item is a `type:value` pair, items separated by commas. Whitespace around
commas and the colon is insignificant. The data engine expands each pair into
one annotation record.

- `type` is one of the topic tables defined in [`../topics/topics.yaml`](../topics/topics.yaml)
  (`person`, `place`, `event`, `theme`, `virtue`).
- `value` MUST be a value listed under that type in `topics/topics.yaml`.

```
person:st_francis_of_assisi, place:assisi, event:canonization_1228
```

## `relations` construction

Each item is a `reltype:target` pair, items separated by commas. Each pair
records a cross-work parallel from this paragraph to another.

- `reltype` is one of `same_episode` (the same narrated episode) or `related_to`
  (otherwise connected).
- `target` is a fully-qualified paragraph key `<book_id>-<paragraph_id>`, where
  `book_id` is the target work's id and `paragraph_id` is its `<p>` id. The split
  is on the first hyphen, so `book_id` MUST NOT contain a hyphen (the paragraph
  id may, e.g. `LMj-mir10-6` → book `LMj`, paragraph `mir10-6`).

```
same_episode:LMj-mir10-6, related_to:2Cel-121
```
