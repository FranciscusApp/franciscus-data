# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 13:00 CEST**

**Subject**: Corpus-wide `<ref>` / verse-marker sanitization (tool + 2Cel)

**The tool.** `postprocess/sanitize_refs.py` (new) operates on *already-built*
book/translation files (unlike `convert_refs.py`, which runs once on a tag-free
draft). It is **segment-safe**: conversion runs only in the plain-text gaps
between existing tags, and never inside an open `<ref>…</ref>`, so finished refs
are never re-wrapped or nested. It is idempotent. It:
- converts stray `(cfr. …)` / `(cf. …)` citations to `<ref>`, in Latin, Italian
  and English notation — Italian (`Sal`, `Gb`, `Ger`, `At`, `Ef`, `Gv`…) and
  English (`Job 4:13`, colon form) book names are funnelled onto the **same**
  anglophone `to=` keys the Latin uses, so a locus has one key across renditions;
- tolerates OCR-mangled `cfr` (`cr.`, `c fr.`, `cfr.Gen`, doubled `cfr. Cfr.`),
  drops "and following" suffixes (`50,3ss` / `50:3ff.`);
- unwraps `<ref>` inside `##` titles and `place:` / empty `to=""` refs.
`lib/scripture.py` gained the genuinely-missing Latin books it needed
(Amos, Jonah, Nahum, Habakkuk, Obadiah, 1/4 Esdras, `2Ioa`/`3Ioa`).

Deliberately left for a human (book-ambiguous or structurally broken) and fixed
here by hand:
- **`(cfr. 48,21)`** (book-less continuation) → `Isa 48:21` ("aquam de petra"),
  all three renditions.
- **`(cfr. Iudas 18)`** had been mis-converted to a broken nested
  `<ref to="John 1:6">` — corrected to `Jude 1:18` wrapping "novissimo tempore".
- **`(cfr. Ion 3,5)`** likewise mis-nested as `<ref to="Lam 2:10">` — corrected
  to `Jonah 3:5` ("sacco vestitus").
- **`(cfr. 4Re 4,42)`** — in the Latin the "4" had been mangled into a spurious
  verse marker split across a line (`(cfr.\n[4] Re 4,42)`); joined and converted
  to `4 Kgs 4:42`. English wrote it `(cf. 2 Kgs 4:42)` (Hebrew numbering) →
  remapped to the Vulgate key `4 Kgs 4:42` to match the Latin. **Review the
  Latin↔English numbering convention if you disagree.**
- **Unparsed verse numbers re-bracketed** (had lost their `[ ]`): Latin
  `[2]`,`[5]`,`[6]`,`[7]`,`[8]`,`[10]`,`[17]`; Italian `[8]`,`[10]`; English
  `[5]`,`[7]`,`[8]`,`[10]`,`[17]`. Each verified to fill a sequence gap.

Not addressed (out of scope, flagged for later): a scattering of **empty
`<ref to="…"></ref>`** markers that wrap no text (e.g. `Exod 29:18; John 12:3`,
`Matt 26:7`, `Ps 108:17`). They are structurally valid but cite nothing visible.
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 15:00 CEST**

**Subject**: Empty-content `<ref>` markers filled (2Cel)

The empty markers flagged above are now filled. All four sit in `2Cel.en.md`
only (the Latin already had the correct split, which was used as the template so
the English mirrors it phrase-for-phrase):

- `Judg 13:6` → "The man of God" (paired with `Matt 1:24` "rose from sleep";
  Latin: *vir Dei* / *surgit a somno*).
- `Ps 110:1` → "O how great, in the council of the just and in the congregation,
  are the works of the Lord".
- `Exod 29:18; John 12:3` → "filled with the sweetest fragrances"; `Matt 26:7`
  → "a precious ointment".
- `Ps 108:17` → "The hearts of the hearers, pricked with compunction" (paired
  with `Gen 43:30` "burst into tears").
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 15:30 CEST**

**Subject**: Broken `cfr` citations (lost opening paren) wrapped as `<ref>` (2Cel)

The sanitizer skips citations whose opening `(` was OCR-dropped, leaving a
dangling `cfr. X)` in the prose. One such site here, in all three renditions
(Greccio, on the *Puer Bethlehemita*): Latin `Pueri Bethlehemitis cfr. 1Re 16,18)`,
Italian `<cfr. 1Re 16,18>`, English `<cfr. 1Re 16,18)>`. Vulgate `1Re` → key
`1 Kgs 16:18`; wrapped the Bethlehem phrase in each (Latin *Pueri Bethlehemitis*
/ it *Bambino di Betlemme* / en *Child of Bethlehem*). No prose altered.
---
