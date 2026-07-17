# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/2.0.0/).
This project also tries to adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html),
although it not being a library and not exposing any APIs means the definitions for "major, minor, patch"
might be somewhat loose or subjective.


## [1.5.0] - 2026-07-17

### Added
- **Four works split out of the Opuscula** into standalone books, each with its
  Italian and English translations and annotation sidecar: *Admonitiones*
  (`adm`), *Officium Passionis Domini* (`offpass`), *Regula bullata* (`regb`),
  and *Regula non bullata* (`regnb`). The `Opuscula` book keeps the remaining
  shorter pieces (letters, prayers, the *Canticum Fratris Solis*, …).
- **Book categories.** A new `books/categories.yaml` defines a closed set of
  collections (spec: *Categories* in `spec/books.md`), and every source book
  now carries `category` / `sequence` frontmatter placing it in the browsing
  hierarchy. Category headings are localized by UI language.
- **Spec: `label_format="heading"`** — a paragraph's `label` may be promoted to
  a section heading, expressing a work's internal chapters when the work is not
  split into its own book (no new heading levels introduced).
- **Spec: `layout` attribute** (`prose` / `verse` / `psalm`) — marks non-prose
  paragraphs whose line structure is part of the text: `verse` preserves the
  authored line breaks (hymns), `psalm` additionally breaks each verse at its
  `<caesura>` marks so pointed chant reads as half-lines.

### Changed
- Normalized author names across all frontmatter, and corrected the attributed
  composition dates of the Opuscula-family books.
- Refined and polished the editorial descriptions (and their translations) of
  every book.

## [1.4.0] - 2026-07-12

### Added
- **Opuscula Omnia Sancti Francisci** — the complete writings of Francis of
  Assisi (Admonitiones, letters, prayers, the Canticum Fratris Solis, and the
  rules), a sixth work added to the corpus from Kajetan Esser's edition
  (Grottaferrata, 1978). Each of the ~37 pieces is a chapter within the work.
- **English translations** of every work (`<id>.en.md`), alongside the existing
  Italian. Machine-generated drafts; see each file's frontmatter provenance.
- **`<caesura>` and `<var psalter>` encoding** for chanted psalms — the pause
  points (mediant/flexa) and Roman/Gallican psalter sigla carried in the
  Opuscula's sung texts, lifted from the reference edition's apparatus.

### Fixed
- Cleaned up many broken, stray, or unparsed `<ref>` reference tags across the
  texts, plus assorted transcription corrections.


## [1.3.0] - 2026-07-05

_(Nothing added data-side on this version.)_


## [1.2.0] - 2026-07-03

### Changed
- **Grouped annotation format.** Annotations are now one topic or relation per
  item, grouped by paragraph, with implicit authorship: a `by:` handle marks a
  human-authored item, and the annotation-level `provenance` field is dropped.
  All book sidecars were migrated to the new shape.

### Added
- **`contributors.yaml`** — a registry of human contributors keyed by GitHub
  login, resolving an item's `by:` to DB attribution and appended to on a
  contributor's first in-app pull request.


## [1.1.0] - 2026-06-30

### Changed
- **Reworked the book metadata schema.** Frontmatter now carries per-rendition
  translation provenance (`provenance` / `status` / `translation_source`) and
  per-paragraph provenance, so machine output can be told apart from reviewed work.
- **Moved cover descriptions out of the Markdown and into the YAML sidecar**,
  keyed by UI language, and added a `description_short` blurb alongside the long
  `description` for every book.
- **Normalized all data formats to YAML.** The per-book `<id>.json` annotation
  files were replaced by `<id>.yaml` sidecars, and the controlled vocabulary
  moved from `topics.toml` to `topics/topics.yaml`.
- **Reorganized topic pages** from flat `type:value.md` filenames into a
  `topics/<type>/<value>.md` directory layout.

### Documentation
- Updated the normative spec (`spec/books.md`, `spec/annotations.md`,
  `spec/topics.md`) to match the new schema, including descriptions as a
  Markdown field and the reworked annotation/topic structure.

## [1.0.0] - 2026-06-27 (Initial Release)

### Added
- The **source corpus**: five early Franciscan works — *Vita Prima* (1Cel),
  *Vita Secunda* (2Cel), *Legenda Maior* (LMj), *Legenda Trium Sociorum* (3Soc),
  and the *Testamentum* — each in the original medieval **Latin** with an
  **Italian** translation.
- **Semantic annotations** (topics and cross-work relations) as per-book sidecars,
  and a closed **controlled vocabulary** of topic values.
- **Topic pages** for persons, places, events, themes, and virtues.
- The **normative format specification** governing the on-disk text, annotation,
  and topic formats.
- Dedicated to the public domain under **CC0-1.0**.