# franciscus-data

Machine-readable Franciscan source texts with separate semantic annotations.

The medieval Latin texts are in the public domain; structure, tags and annotations are released under **CC0-1.0**.

## Language conventions

- **Source texts** (`books/`) are in medieval Latin. The canonical text has no language suffix (e.g. `1Cel.md`); translations use a BCP-47 tag (e.g. `1Cel.it.md`).
- **Annotation evidence** (`annotations/`) and **attribute page descriptions** (`attributes/`) are written in English — they are application-level content, not source material.
- Attribute values (e.g. `paupertas`, `Assisium`) and relation types (e.g. `dipende_da`) are Latin/domain identifiers used as machine keys.

## Contents

```
books/            Structured Markdown sources (see FORMAT.md)
annotations/      Annotations and relations in JSON, one file per work
```

### Works included (v1)

| ID   | Work               | Author              | Reference edition                  |
|------|--------------------|---------------------|------------------------------------|
| 1Cel | Vita prima         | Thomas of Celano    | Analecta Franciscana X (Quaracchi) |
| 2Cel | Vita seconda       | Thomas of Celano    | Quaracchi                          |
| LMj  | Legenda maior      | Bonaventure         | Quaracchi                          |

## Book format

Each work is a Markdown file with YAML frontmatter and a restricted set of inline HTML (`<p>`, `<aside>`, `<ref>`). The full specification is in [FORMAT.md](FORMAT.md).

Minimal example:

```markdown
---
id: 1Cel
title: "Vita Prima S. Francisci"
author: "Tommaso da Celano"
date: "1228"
reference_edition: "Analecta Franciscana X (Quaracchi)"
license: CC0-1.0
---

# VITA PRIMA S.FRANCISCI

## PROLOGUS <a id="prolog"></a>

<p id="prolog-1">
[1] Actus et vitam beatissimi patris nostri Francisci...
</p>
```

## Annotations

JSON files in `annotations/`, one per work (e.g. `1Cel.json`). Each entry is paragraph-keyed. A file contains:

- **annotations** — typed attributes per paragraph: `virtue`, `topic`, `event`, `place`, `person`. Each entry records provenance (`by`: `ai` or `human`), verification status (`verified`), and optional `evidence`.
- **relations** — cross-work paragraph parallels: `idem_episodium`, `rielaborazione_di`, `dipende_da`, `cita`. Same provenance tracking.

## Attribute Pages

Markdown files under `attributes/` (e.g. `attributes/humilitas.md`) with frontmatter fields `name` and `type`. Each page provides curated content about a virtue, topic, person, or place; the app auto-generates a list of relevant passages from the annotations.

Translatable via the same duplicated-file convention as books (e.g. `attributes/humilitas.it.md`).

## Translations

Content translations use duplicated `.md` files per language (e.g. `books/1Cel.it.md`). Each translation follows the same FORMAT.md spec and preserves the same paragraph and aside IDs as the Latin original — it is a standalone, distributable document.

The canonical Latin text has no language suffix (`books/1Cel.md`). Translations are identified by a BCP-47 language tag before the extension.

## Contributing

Textual corrections and annotation proposals are welcome. Every contribution requires a CC0 dedication (DCO-style).

## License

[CC0-1.0](LICENSE) — see the LICENSE file for the full text.
