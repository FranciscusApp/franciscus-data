# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Claude <noreply@anthropic.com> - 2026-06-25 11:46 CEST**

**Subject**: Machine extraction of 3Soc

Extracted `books/3Soc.md` (Legenda Trium Sociorum) from an HTML source rather
than a PDF. The source `raw-sources/3Soc.html` is a Wayback Machine
"view-source" capture of http://www.paxetbonum.net/biographies/3companions_lat.html,
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
