# Topic pages

**Applies to:** `topics/<type>/<value>.md` and `topics/<type>/<value>.<lang>.md`.

Topic pages are long-form Markdown descriptions of a single annotation target
(a person, place, event, theme, virtue, …). They live under `topics/<type>/`,
one file per `(type, value)` pair, named `<value>.md`. Translations are named
`<value>.<lang>.md`.

```
topics/person/st_clare_of_assisi.md       # source page (English by convention)
topics/person/st_clare_of_assisi.it.md    # Italian translation
```

The topic `type` is the subdirectory name; one of the topic types in
[`../topics.toml`](../topics.toml) (`person`, `place`, `event`, `theme`,
`virtue`, …). The `value` is the filename stem, split on the first `.` to
separate the optional `<lang>` suffix. The path is authoritative — there is no
`type:` field in the frontmatter.

## Frontmatter

| Field         | Type   | Where                | Level    | Description |
|---------------|--------|----------------------|----------|-------------|
| `description` | string | source + translation | REQUIRED | Short label / page heading; used as the topic's pill label. In a translation file, MUST be in the target language. |

Body is the page's long-form Markdown, rendered as-is by the SPA.

```yaml
---
description: "Saint Clare of Assisi"
---

Saint Clare of Assisi (1194–1253) was the first female follower of Saint Francis...
```

```yaml
---
description: "Santa Chiara d'Assisi"
---

Santa Chiara d'Assisi (1194–1253) fu la prima seguace femminile di San Francesco...
```

## URLs

The canonical (and only) URL of a topic page is `/topics/<type>/<value>`, where
`<value>` is the source filename's value. There are no per-language URL slugs.
