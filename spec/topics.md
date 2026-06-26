# Topic pages

**Applies to:** `topics/<type>:<value>.md` and `topics/<type>:<value>.<lang>.md`.

Topic pages are long-form Markdown descriptions of a single annotation target
(a person, place, event, theme, virtue, …). They live under `topics/`, one
file per `(type, value)` pair, named `<type>:<value>.md`. Translations are named
`<type>:<value>.<lang>.md`.

```
topics/person:st_clare_of_assisi.md       # source page (English by convention)
topics/person:st_clare_of_assisi.it.md    # Italian translation
```

The `(type, value)` pair is parsed from the filename: split on `:`, then on the
first `.` of the remainder to separate the optional `<lang>` suffix. The
filename is authoritative; the frontmatter `type:` field is a sanity check.

## Frontmatter

| Field         | Type   | Where                | Level    | Description |
|---------------|--------|----------------------|----------|-------------|
| `type`        | string | source + translation | REQUIRED | One of the topic types in [`../topics.toml`](../topics.toml) (`person`, `place`, `event`, `theme`, `virtue`, …). MUST match the `<type>` prefix of the filename. |
| `description` | string | source + translation | REQUIRED | Short label / page heading. In a translation file, MUST be in the target language. |
| `lang_slug`   | string | translation only     | OPTIONAL | Alternative URL slug for that language (e.g. `st_chiara_di_assisi` for the Italian translation of `st_clare_of_assisi`). Source pages MUST NOT set this. |

Body is the page's long-form Markdown, rendered as-is by the SPA.

```yaml
---
type: person
description: "Saint Clare of Assisi"
---

Saint Clare of Assisi (1194–1253) was the first female follower of Saint Francis...
```

```yaml
---
type: person
lang_slug: st_chiara_di_assisi
description: "Santa Chiara d'Assisi"
---

Santa Chiara d'Assisi (1194–1253) fu la prima seguace femminile di San Francesco...
```

## Resolution of `lang_slug`

A `lang_slug` is an alias only — the canonical URL of a topic page is always
`/topics/<type>/<value>`, where `<value>` is the source filename's value. The
SPA accepts both the canonical value and the active corpus language's
`lang_slug`; lang_slug URLs redirect to the canonical URL.
