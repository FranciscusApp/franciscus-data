# Annotation sidecar files

**Applies to:** `books/<id>.json`.

Semantic annotations for a book live in a JSON file beside it, sharing the
book's stem: `<id>.json` (e.g. `1Cel.json` accompanies `1Cel.md`). The `book_id`
is taken from the filename and is NOT repeated inside the file.

The file is a JSON array of annotation objects:

```json
[
  {
    "paragraph": "prolog-1",
    "topics": "person:st_francis_of_assisi, person:pope_gregory_ix",
    "relations": "same_episode:LMj-prol-1",
    "by": "Claude <noreply@anthropic.com>",
    "provenance": "ai"
  }
]
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

- `type` is one of the topic tables defined in [`../topics.toml`](../topics.toml)
  (`person`, `place`, `event`, `theme`, `virtue`).
- `value` MUST be a value listed under that type in `topics.toml`.

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
