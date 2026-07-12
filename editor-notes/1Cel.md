# Editor Notes

This file contains free-form editorial notes, useful for future/deferred review or for
opening GitHub issues.

---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 13:00 CEST**

**Subject**: Corpus-wide `<ref>` / verse-marker sanitization (1Cel)

A new pass repaired citation and verse defects that survived the original
(pre-`normalize_refs`) pipeline and had been carried, in localized form, into
the translations. Tooling: `postprocess/sanitize_refs.py` (new, segment-safe and
idempotent — it converts stray citations only in the plain-text gaps between
existing tags, so finished `<ref>`s are never re-wrapped) plus a handful of
manual edits for structurally-broken sites. See `editor-notes/2Cel.md` for the
fuller description of the tool; the same run touched every book.

In **1Cel** specifically:
- **Stray citations converted.** Latin `(cfr. Iob 4,13)`, Italian
  `(cfr. Sal 79,12)` and English `(cf. Job 4:13)` parentheticals that never
  became `<ref>` tags now do, funnelled onto the same anglophone `to=` keys the
  Latin uses. The clause boundaries come from the same backward-looking
  heuristic as the rest of the corpus and are worth a spot-check.
- **`(Ioa 13,1 (12,1))`** (1.109) — the conflated Johannine incipit
  ("Ante sex dies paschae…" = John 12:1; "…transeat ex hoc mundo ad Patrem" =
  John 13:1) is now `<ref to="John 12:1; John 13:1">`, in all three renditions.
- **Unparsed verse numbers re-bracketed.** Verse markers that had lost their
  `[ ]` and were sitting as bare numerals in the prose: Latin `[3]`,`[6]`,`[9]`
  and their `it`/`en` counterparts (`[3]`,`[9]`; `[9]`). Each was verified to
  fill a gap in the paragraph's `[N]` run before bracketing.
- **`1Cel.it.md` place/empty refs removed.** An abandoned place-linking
  experiment left `<ref to="place:assisi">`, `<ref to="place:siena">`,
  `<ref to="place:portiuncula">` (including three inside the Chapter VII
  **title**, which does not support refs) and two empty `<ref to="">`
  (Assisi, Perugia). All were unwrapped to plain text; the Latin and English
  never had them.
  
---
**Alfredo Salata <as@salata.ovh> - 2026-07-12 15:00 CEST**

**Subject**: Empty-content `<ref>` markers filled (1Cel)

Follow-up to the sanitization pass: the `<ref to="…"></ref>` markers that
carried a locus but no wrapped text (left untouched earlier) now wrap the words
they cite. Nothing was rewritten — text already present in the sentence was
moved inside the tag.

- Latin `1Cel.md`: `Acts 6:15` → "vultum eius quasi vultum Angeli";
  `2 Cor 1:3` → "Pater misericordiarum"; `Jer 25:10` → "vocem gaudii, vocem
  laetitiae"; `1 Tim 1:15` → "sermo fidelis et omni acceptione dignus".
- The `it` and `en` renditions wrap the corresponding phrase for each of the
  four loci above (plus, in `en`, the whitespace-only `2 Macc 14:35; Eph 1:3`
  at 1.90 → "amid the things of heaven in the richness of grace before the most
  gentle and most serene Lord of all things").

These four loci were empty in the Latin source itself, so the span choice is
newly authored (matched to the Vulgate phrase); worth a glance on review.

---
