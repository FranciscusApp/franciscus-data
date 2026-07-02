# Per-book sidecar files

**Applies to:** `books/<id>.yaml`.

Each book has a single YAML sidecar beside it, sharing the book's stem:
`<id>.yaml` (e.g. `1Cel.yaml` accompanies `1Cel.md`). The `book_id` is taken
from the filename and is NOT repeated inside the file. The sidecar holds two
sections, both optional:

1. **Cover properties** — book-level editorial metadata, keyed by **UI
   language** (`en`, `it`, …), at the top of the file.
2. **`annotations`** — a map from paragraph id to that paragraph's annotations.

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
  prolog-1:
    - person:st_francis_of_assisi
    - person:pope_gregory_ix
    - same_episode:LMj-prol-1
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

A **map keyed by paragraph id** → a list of annotation items. Each item is **one
`(topic | relation)` pair** with an optional comment — the source is flat on
`(paragraph, pair)`, mirroring the DB:

```yaml
annotations:
  prolog-1:
    - person:st_francis_of_assisi   # topic, AI-authored (implicit)
    - same_episode:LMj-prol-1       # relation, AI-authored
  '40':
    - theme:prayer
    - topic: virtue:fortitude       # map form: needed when a pair has overrides
      by: snowstorm-alfredosalata   # a handle in contributors.yaml ⇒ human-authored
      comment: courage fits the Latin better than fortitude
```

The map key is the target paragraph's `<p>` id. A paragraph key MAY list any
number of items; each becomes exactly one annotation record.

### Item forms

An item is either a **scalar** or a **map**.

- **Scalar** — a bare `type:value` (topic) or `reltype:target` (relation) string.
  Equivalent to a map with only that one field set: AI-authored, no comment.
- **Map** — used when a pair needs overrides. Exactly one of `topic` / `relation`
  is REQUIRED; the rest are OPTIONAL:

| Field      | Type   | Description                                                                 |
|------------|--------|-----------------------------------------------------------------------------|
| `topic`    | string | One `type:value` pair (§ [topics](#topics-construction)). XOR `relation`.    |
| `relation` | string | One `reltype:target` pair (§ [relations](#relations-construction)). XOR `topic`. |
| `by`       | string | Contributor **handle** (see [`contributors.yaml`](#contributorsyaml)). Its presence marks the item **human**-authored. |
| `comment`  | string | Free-text editorial note (English).                                         |
| `to`       | string | Last paragraph `id` when the annotation spans a range starting at the key.   |

### Authorship & provenance

Authorship is **implicit**. No `by` ⇒ the item was authored by the project AI
(Claude); a `by: <handle>` ⇒ human-authored. **The presence of `by` is the human
signal**, so items carry **no `provenance` field** — ingest derives the DB
columns: `by_whom` resolves `by` via [`contributors.yaml`](#contributorsyaml)
(or the Claude default when absent), and `provenance` is `human` when `by` is
present, else `ai`.

The `reviewed` provenance state exists at **paragraph level only** (a human vetted
the AI text; see [`books.md`](books.md#per-paragraph-provenance)) — never on an
annotation item.

## `contributors.yaml`

A registry of **human** contributors at the data-repo root,
[`../contributors.yaml`](../contributors.yaml), **keyed by GitHub login** — the
same handle the in-app contribution flow writes into `by:`. An email-only
contributor (no GitHub account) uses a plain handle as the key instead. Claude is
never listed — it is the default author when `by` is absent.

```yaml
snowstorm-alfredosalata:      # key == the GitHub login when present
  name: Alfredo Salata
  email: as@salata.ovh
  github: snowstorm-alfredosalata
```

| Field    | Type   | Description                                                          |
|----------|--------|---------------------------------------------------------------------|
| `name`   | string | Display name; becomes DB `by_whom` (`Name <email>`).                |
| `email`  | string | OPTIONAL. Appended as `<email>` when present.                       |
| `github` | string | OPTIONAL. GitHub login; **when present it MUST equal the key**. Its presence marks the person a GitHub contributor. |

An unknown `by:` handle falls back to the raw handle string (and a build warning).

## `topics` construction

A topic pair is `type:value`. Whitespace around the colon is insignificant.

- `type` is one of the topic tables defined in [`../topics/topics.yaml`](../topics/topics.yaml)
  (`person`, `place`, `event`, `theme`, `virtue`).
- `value` MUST be a value listed under that type in `topics/topics.yaml`.

```
person:st_francis_of_assisi
```

## `relations` construction

A relation pair is `reltype:target`, recording a cross-work parallel from this
paragraph to another.

- `reltype` is one of `same_episode` (the same narrated episode) or `related_to`
  (otherwise connected). A bare scalar item is read as a relation when its type is
  one of these; otherwise it is a topic.
- `target` is a fully-qualified paragraph key `<book_id>-<paragraph_id>`, where
  `book_id` is the target work's id and `paragraph_id` is its `<p>` id. The split
  is on the first hyphen, so `book_id` MUST NOT contain a hyphen (the paragraph
  id may, e.g. `LMj-mir10-6` → book `LMj`, paragraph `mir10-6`).

```
same_episode:LMj-mir10-6
```
