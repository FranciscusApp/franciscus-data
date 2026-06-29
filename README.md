<div align="center">

# Franciscus - Data

**The Franciscan sources, as open data.**

Machine-readable medieval Latin biographies of Francis of Assisi — with
translations, semantic annotations, and curated topic pages — dedicated to the
public domain under **CC0-1.0**.

[![Corpus license: CC0-1.0](https://img.shields.io/badge/license-CC0--1.0-lightgrey)](LICENSE)
[![Live app: franciscus.app](https://img.shields.io/badge/app-franciscus.app-green)](https://franciscus.app)
[![Format spec](https://img.shields.io/badge/format-spec-blue)](spec/)

</div>

---

## What this is

This repository is the **source corpus** behind [franciscus.app](https://franciscus.app):
the early biographies of Francis of Assisi, encoded as plain Markdown + JSON so
they can be read, searched, cited, re-edited, and re-used by anyone — scholars,
students, developers, or curious readers.

The app ([`franciscus`](https://github.com/snowstorm-alfredosalata/franciscus))
compiles this folder into a SQLite database it ships to the browser. But the
data stands on its own: every text and annotation here is a portable file you
can take elsewhere.

### The corpus

| ID | Work | Author | Reference edition |
|----|------|--------|-------------------|
| **1Cel** | Vita Prima | Thomas of Celano | Analecta Franciscana X (Quaracchi) |
| **2Cel** | Vita Secunda | Thomas of Celano | Quaracchi |
| **LMj** | Legenda Maior | Bonaventure | Quaracchi |
| **3Soc** | Legenda Trium Sociorum | The Three Companions | Quaracchi |
| **Testamentum** | Testamentum S. Francisci | Francis of Assisi | Quaracchi |

Each work is present in the original **Latin** and an **Italian** translation.
(English translations are planned — see [Roadmap](#roadmap).)

## 📜 License — public domain (CC0-1.0)

**Everything in this repository is dedicated to the public domain under
[CC0-1.0](LICENSE).** No rights reserved. You may copy, modify, distribute, and
build upon the corpus — including commercially — without asking permission and
without attribution.

The medieval Latin texts are themselves long out of copyright; the CC0 dedication
covers the parts that involved editorial effort — the structural encoding, the
paragraph/verse IDs, the annotations, the topic pages, and the translations — so
that those, too, are unambiguously free to reuse.

Attribution is never required, but it is always appreciated: a link back to
[franciscus.app](https://franciscus.app) helps others find the source.

## 🤖 A note on AI

We believe in being upfront about how this corpus was made.

- **The Latin source texts** were extracted from public-domain print editions
  (chiefly the Quaracchi *Analecta Franciscana*) and corrected. These are
  transcriptions of real editions, **not AI-generated text** — though OCR and
  cleanup were machine-assisted and may still contain errors.
- **The translations** (currently Italian) are, except where noted, **machine
  translations** produced with a large language model. The Italian *Testamentum*
  uses an official OFM translation. Machine translations are useful but
  **not authoritative**; do not cite them as a scholarly translation. Unlike
  annotations, translations currently carry **no per-passage provenance** in the
  format — a translation file is all-or-nothing, with no way yet to mark which
  passages a human has reviewed. Treat every translation as machine-generated
  until that changes.
- **The annotations** (topics, cross-work relations) and the **draft topic pages**
  are largely **machine-generated** and not yet human-verified. Each annotation
  records its `by` and a `provenance: ai | reviewed | human` field, so you can
  always tell machine output from reviewed work.

In short: trust the Latin as a transcription, treat everything generated around
it as a helpful first pass awaiting human review. Corrections are very welcome —
see [Contributing](#contributing).

## Repository layout

```
books/         Source texts and translations, plus annotation sidecars:
                 <id>.md         canonical Latin text
                 <id>.<lang>.md  translation (e.g. 1Cel.it.md)
                 <id>.yaml       semantic annotations for that work
topics/        Topic pages — <type>/<value>.md (+ translations)
editor-notes/  Free-text editorial notes (not ingested into the app)
topics/topics.yaml    Controlled vocabulary of valid topic values
spec/          The normative format specification
LICENSE        CC0-1.0 public-domain dedication
```

## Format at a glance

A source text is one Markdown file: YAML frontmatter, a `#` title, `##` chapters,
and body text in `<p id="…">` paragraphs that preserve the print edition's
numbering. A handful of inline elements (`<aside>`, `<ref>`, verse markers) carry
the rest.

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

Annotations live in a YAML sidecar (`books/1Cel.yaml`) keyed by paragraph; topic
pages live under `topics/`. The full normative spec is in **[`spec/`](spec/)**.

## Roadmap

Near-term plans for the corpus:

- **English translations** of all works.
- **Human review** of the Latin sources and of the machine translations.
- **Annotation clean-up** — fixing AI-introduced topic values not in the
  controlled vocabulary, and translating book titles.
- **Cross-work parallels** seeded from *Fontes Franciscani* concordances
  (pending review).
- A **Vulgata edition** added to the corpus for scripture cross-referencing.

The app's full roadmap lives in the
[`franciscus`](https://github.com/snowstorm-alfredosalata/franciscus) repo.

## Contributing

Corrections, translations, and annotation proposals are all welcome — this is
exactly the kind of work that benefits from many eyes. Every contribution is
made under the same CC0 dedication.

See **[CONTRIBUTING.md](CONTRIBUTING.md)** for how to submit changes (via GitHub
or email), what metadata and formatting we need, and the validator. Contributors
are recorded in **[CREDITS.md](CREDITS.md)**.

## Get in touch

<div align="center">

<a href="https://verbumcaro.it"><img src="https://raw.githubusercontent.com/snowstorm-alfredosalata/franciscus/refs/heads/master/app/static/vc-inline-dark.png" alt="Verbum Caro" width="240"></a>

</div>

A **Verbum Caro** project. Questions, corrections, or collaboration —
reach us at [info@verbumcaro.it](mailto:info@verbumcaro.it).
