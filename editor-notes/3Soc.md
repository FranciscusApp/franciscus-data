# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 12:20 CEST**

**Subject**: Machine extraction of 3Soc

Extracted `books/3Soc.md` (Legenda Trium Sociorum) from an HTML source rather
than a PDF. The source `raw-sources/3Soc.html` is a Wayback Machine
"view-source" capture of https://web.archive.org/web/20081120014736/http://www.paxetbonum.net/biographies/3companions_lat.html,
so the real page markup was HTML-escaped inside a `<td class="line-content">`
table. A new driver `franciscus-scripts/extract_3soc.py` unwraps that capture,
parses the FrontPage HTML (chapter `<a name>` anchors, `<b>N</b>` paragraph
markers with continuous numbering 1–73, `<br>N` verses) and reuses the shared
`convert_refs` / `fix_ref_verse` stages. Result: 19 sections (Epistola + 18
chapters), all 73 paragraphs, validator PASS.

Three things still need a human eye:
- `reference_edition` is a best guess ("Fontes Franciscani", Editiones Collegii
  S. Bonaventurae, 1995) — chosen because the para+verse numbering matches it,
  but the web source never stated its edition. `author` and `date` (1246) were
  taken from the Epistola's signatories and dateline.
- Source typos preserved faithfully (the text is from an old OCR'd web page):
  e.g. `ad laud~n` (¶1), `Perusli` / `intelleetis` / `quse` / `atgue` in chapter
  headings, and a malformed citation abbreviation or two. Verse numbering in the
  source is occasionally non-sequential (e.g. ¶2 runs 1,2,3,4,6,6,7,8 — no "5");
  kept as-is since `[N]` is presentational.
- The `(cfr. …)` → `<ref>` clause boundaries were placed by the backward-looking
  heuristic and should be reviewed.
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 13:00 CEST**

**Subject**: `<ref>` sanitization + a reconstructed English paragraph (3Soc)

Part of the corpus-wide citation pass (see `editor-notes/2Cel.md` for the tool).
Only a couple of stray citations here (`(Mat 19,21; cfr. Luc 18,22)`,
`(cfr. 2Timm 3,2)` — the `2Timm` OCR typo mapped to `2 Tim`).

**Needs your eyes — newly machine-translated content.** `3Soc.en.md`'s
`<p id="54">` was found **truncated to a single fragment** (`[1] <ref to=`) —
the whole English paragraph (Chapter XIII, four verses) had been lost, while the
Latin `3Soc.md` and Italian `3Soc.it.md` were intact. I **reconstructed the
English by translating from the Latin**, matching the Italian's structure and
the surrounding English register, and restored the two refs the parallels carry
(`Matt 9:35`, `1 Cor 2:4`). This is the one place in this pass where I generated
prose rather than moving markup around — please review it against the Latin.
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 15:30 CEST**

**Subject**: Broken `cfr` citation (lost opening paren) wrapped as `<ref>` (3Soc)

One dangling `cfr. 1Ioa 3,16)` (the sanitizer skips citations whose opening `(`
was OCR-dropped) in the two brothers / stone-thrower episode, in Latin `3Soc.md`
(*ponere vitam ... suam*) and English `3Soc.en.md` (*lay down his life for the
other*). Key `1 John 3:16`; span mirrors the already-fixed Italian
("a tal punto erano pronti l'uno per l'altro a dare la propria vita"). No prose
altered.
---
