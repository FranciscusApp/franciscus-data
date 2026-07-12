# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 12:20 CEST**

**Subject**: Testamentum split out of the Opuscula; text/edition reconciled

**Omission from the Opuscula.** The *Testamentum* (chapter anchor `test`) is
published as its own book (`books/Testamentum.md`, with `Testamentum.it.md`,
`Testamentum.en.md`, and the `Testamentum.yaml` sidecar) rather than as a chapter
of `books/Opuscula.md`, which therefore carries 37 of the source's 38 works.
`extract_opomnia.py` lists it in `OMITTED_SLUGS`: the `test` anchor is still
parsed (so it correctly bounds the preceding chapter, *Salutatio Virtutum*) but
is dropped at emit time, so a re-extraction reproduces the curated 37-chapter
file. (The *Testamentum Senis factum*, anchor `testsen`, is a different, shorter
text and stays in the Opuscula.)

**Edition reconciled.** `Testamentum.md` originally came from Wikisource with
`reference_edition: "unknown"`. A word-by-word diff against the Esser/franciscanos
copy that had been sitting in the Opuscula showed the Latin is **identical** —
863 words, character-for-character the same once verse markers and citations are
stripped, and the same three scripture loci (John 6,64 · Tob 1,3 · 1 Pet 2,11).
So the Wikisource text *is* the Esser text: `reference_edition` was set to
"Kajetan Esser, *Opuscula sancti patris Francisci Assisiensis* (Grottaferrata,
1978)" and the redundant Esser fragment was deleted.

The Wikisource five-paragraph structure (`id="1"`…`"5"`) was kept — both
translations and every annotation in `Testamentum.yaml` are keyed to it — and the
three `<ref>` tags were ported into the Latin so all three renditions now agree
on the same loci.
---
