# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 13:00 CEST**

**Subject**: Corpus-wide `<ref>` sanitization (LMj)

Part of the corpus-wide citation pass — see `editor-notes/2Cel.md` for the tool
(`postprocess/sanitize_refs.py`). Stray Latin/Italian/English citations were
converted to `<ref>`; the following broken sites were repaired by hand:

- **`IcfLuc 10,30)`** (Latin) — an OCR-mangled `(cfr. Luc 10,30)` that had lost
  its opening paren → `<ref to="Luke 10:30">` ("genus humanum et semivivum
  relictum", the man left half-dead of the Good Samaritan).
- **`spiritum 9cfr. 4Re 2,9)`** (Latin) — the `(` had been mis-read as `9` →
  `<ref to="4 Kgs 2:9">` ("duplicem Eliae spiritum"). English wrote this
  `(cf. 2 Kings 2:9)` (Hebrew numbering) → remapped to the Vulgate key
  `4 Kgs 2:9`. Likewise English `(cf. 1 Sam 2:5)` → `1 Kgs 2:5`. **Review the
  numbering convention if you'd rather keep Hebrew forms in the English.**
- **`<ref to="Is 40,26">(cf. Is 40,26)</ref>`** (English) — a broken ref whose
  *content was the citation itself* and whose target used a comma → rewritten to
  `<ref to="Isa 40:26">Calling him by name </ref>`. Same shape fixed for
  `Luc 10,30` in the English.
- **`<ref to="Luc 11,38"></ref>`** (Italian, LMj.it) — an **empty-content** ref
  with a dubious comma target ("cominciò a dire fra sé …"): removed, since it
  wrapped nothing and Luke 11:38 does not fit the sense. Reinstate with a proper
  span if the locus is wanted.

All eight files still validate and the DB rebuilds cleanly.
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 15:00 CEST**

**Subject**: Empty-content `<ref>` marker filled (LMj)

One empty marker, in `LMj.en.md` (the final doxology of the *Legenda Maior*):
`Luke 1:35` → "the magnificence of the power of the Most High" (paired with
`Ps 4:4` "makes known to the faithful that He works wonders in His holy one").
The Latin split — *magnificentia virtutis Altissimi* / *innotescit fidelibus
mirificans Sanctum suum* — was used as the template.
---
