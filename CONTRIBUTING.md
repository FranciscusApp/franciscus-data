# Contributing to franciscus-data

Thank you for helping improve the corpus. Whether you're fixing a single
mis-transcribed word, drafting a topic page, reviewing a machine translation, or
adding a whole new work, your help is genuinely valuable — much of this corpus is
a machine-generated first pass awaiting exactly this kind of human attention.

This is a **CC0 / public-domain** project. By contributing you place your work in
the public domain too — see [The CC0 dedication](#the-cc0-dedication) below. This
is a hard requirement, but it's the only one.

## What you can contribute

| Contribution | Where it lives | Notes |
|--------------|----------------|-------|
| **Text corrections** (typos, bad OCR, wrong reading) | `books/<id>.md` | The Latin should faithfully match the cited reference edition. |
| **Translations** | `books/<id>.<lang>.md` | A standalone file per language; same IDs as the Latin source. |
| **Annotations** (topics, cross-work relations) | `books/<id>.yaml` | Values must come from `topics/topics.yaml`. |
| **Topic pages** | `topics/<type>/<value>.md` | Long-form prose about a person/place/event/theme/virtue. |
| **New vocabulary** | `topics/topics.yaml` | Propose new topic values here before using them in annotations. |
| **New works** | a new set of the files above | Get in touch first so we can agree on the reference edition. |
| **Editorial notes** | `editor-notes/<id>.md` | Free-text; not ingested, for human editors only. |

## How to submit

### Via GitHub (preferred)

1. **Open an issue first** for anything non-trivial (a new work, a new
   translation language, a vocabulary change). For a small fix you can skip
   straight to a PR.
2. Fork, make your change, and **run the validator** (see below).
3. Open a pull request. In the PR description, include the **CC0 dedication line**
   and how you know the change is correct (which edition/page, which manuscript
   reading, etc.).

### Via email

If GitHub isn't for you, email **as@salata.ovh**. To be usable without a lot of
back-and-forth, please send:

- **What** you're changing and **which file(s)** (`books/2Cel.md`, paragraph
  `c3-12`, etc.).
- The **corrected text or data** — ideally as the finished file(s) or a diff, not
  a description of the change.
- A **source/justification**: the reference edition and page, the manuscript
  reading, or "machine translation, unreviewed" if that's what it is.
- The **CC0 dedication line**, and the **name (and optional email)** you want
  recorded in [CREDITS.md](CREDITS.md) — or a note that you'd rather stay
  anonymous.

Email contributions are committed on your behalf with attribution to the name you
provide.

## Getting the formatting right

All texts, translations, and topic pages follow the format in **[`spec/`](spec/)**.
Read the relevant spec file before submitting:

- [`spec/books.md`](spec/books.md) — texts and translations
- [`spec/annotations.md`](spec/annotations.md) — annotation sidecars
- [`spec/topics.md`](spec/topics.md) — topic pages

A few things that trip people up:

- **Translations preserve the source IDs.** A translation of `1Cel.md` keeps the
  same `<p id="…">` and chapter IDs — only the prose and the `title:` change.
- **Translations have no provenance field (yet).** Unlike annotations, the format
  can't record which passages of a translation are human-reviewed — a file is
  all-or-nothing. If you human-author or review a translation, say so in your PR /
  email and in [CREDITS.md](CREDITS.md) so it's at least recorded out of band.
- **Annotation values must exist in `topics/topics.yaml`.** `virtue:poverty` is valid;
  `virtue:temperance` is not unless you add it. Add new values to `topics/topics.yaml` in
  the same change.
- **Mark provenance honestly.** New annotations you write by hand are
  `by_type: human`; set `verified: true` only for content a human has actually
  checked.

### Validate before you submit

The format validator lives in the sibling
[`franciscus-scripts`](https://github.com/snowstorm-alfredosalata/franciscus-scripts)
repo:

```bash
python validate/validate_format.py path/to/books/1Cel.md
```

A clean run isn't a guarantee of correctness, but a failing run means the file
won't ingest — please fix it before sending.

## The CC0 dedication

Every contribution must be released under
[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/). Include this line
in your pull request description or email (DCO-style):

> I dedicate my contribution to the public domain under CC0-1.0, waiving all
> copyright and related rights to it worldwide, to the extent permitted by law.
>
> Signed-off-by: Your Name \<your@email>

By signing off you confirm that the work is yours to dedicate (or is itself
public domain) and that you place it in the public domain with no rights
reserved. The name/email you sign off with is what we record in
[CREDITS.md](CREDITS.md) unless you ask to remain anonymous.
