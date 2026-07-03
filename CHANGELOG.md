# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/2.0.0/).
This project also tries to adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html),
although it not being a library and not exposing any APIs means the definitions for "major, minor, patch"
might be somewhat loose or subjective.


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