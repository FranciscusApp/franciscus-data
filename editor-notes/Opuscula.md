# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 12:20 CEST**

**Subject**: Machine extraction of the Opuscula + citation/apparatus processing

Extracted `books/Opuscula.md` (the *Opuscula Omnia Sancti Francisci*) from the
franciscanos.org / Directorio Franciscano HTML page (`opuscola_omnia_sancti_francisci.html`),
whose text follows Kajetan Esser, *Opuscula sancti patris Francisci Assisiensis*
(Grottaferrata, 1978). A new driver `franciscus-scripts/extract_opomnia.py`
unwraps the page (its own `<A NAME>` anchors become `## … <a id>` chapters,
`<SUP>N</SUP>` become `[N]` verse markers, centered/bracketed rubrics become
`<aside>`) and reuses the shared postprocess stages.

The page indexes 38 works; **37 are emitted**. The *Testamentum* (anchor `test`)
is split out to its own book — see `editor-notes/Testamentum.md` and the
`OMITTED_SLUGS` set in the driver. The *Testamentum Senis factum* (`testsen`) is
a different, shorter text and stays here.

**Scripture references.** The franciscanos.org abbreviation set (`Mt`, `Mc`,
`Lc`, `Joa`, `1 Petr`, `Jac`, `Hebr`…) differs from the Quaracchi/Analecta set the
Celano extractors use (`Mat`, `Mar`, `Luc`, `Ioa`…), so an initial `convert_refs`
pass converted only a fraction. A new `postprocess/normalize_refs.py` stage plus
franciscanos aliases in `lib/scripture.py` now bring it to **715 `<ref>` tags**.
The `(cfr. …)` → `<ref>…</ref>` clause boundaries are still placed by the
backward-looking heuristic and **should be reviewed** — most urgently in the
*Officium Passionis Domini*, a dense cento of psalm verses where a single wrap
can over-/under-reach or straddle a verse marker.

**Reference-edition apparatus → tags** (see `spec/books.md`; hidden by default
via the app's CSS):
- psalm-pointing marks → `<caesura kind="mediant">` (`*`) / `"flexa"` (`'`) — 187.
- psalter-recension sigla `(R)`/`(G)` → `<var psalter="…">` — 33.
- Lossy simplifications made so the citations parse: verse sub-letters reduced
  (`Ps 95,7-8a` → `Ps 95:7-8`), the emphatic `R!` folded to `R`, and combined
  `G/R` split into two `<var>` markers. Revisit if the sub-verse precision or the
  emphasis is ever wanted.

Still needs a human eye:
- Source OCR typos preserved faithfully (franciscanos.org is an old web text):
  e.g. `caelornm` (Officium, *Sancta Maria* antiphon), `recipiumt` (Epistola ad
  Fideles I), `sustinebumt` (Epistola ad populorum Rectores), and an impossible
  `Ps 177,24` in the *Exhortatio ad Laudem Dei* (should be Ps 117) — kept as-is.
- The *Admonitiones* Cap. V rubric ("[Cap. V: Ut nemo superbiat, sed glorietur in
  cruce Domini]") spans two source lines; the driver now keeps bracketed rubrics
  whole by tracking bracket depth, but similar multi-line rubrics elsewhere are
  worth a glance.
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 15:00 CEST**

**Subject**: Empty-content `<ref>` markers filled (Opuscula)

- `Opuscula.en.md`, Regula non bullata: whitespace-only `Matt 23:10` → wraps
  "«let yourselves be called masters»" (Latin *Ne «vocemini magistri»*).
- `Opuscula.it.md`, parable of the sower (synoptic harmony): two empties.
  `Matt 13:23` in v.17 now wraps "fu seminato", interleaved with `Luke 8:15`
  ("Ciò che invece" … "nella terra buona"), mirroring the Latin
  *Quod autem in terram bonam / seminatum est* split. The trailing
  `Mark 4:16` empty in v.14 (Latin *illud*) was **removed**: Italian fuses the
  object into the clitic of "l'accolgono", so there is no separate word to wrap,
  and `Mark 4:16` is already attributed earlier in the verse ("subito con gioia").
---