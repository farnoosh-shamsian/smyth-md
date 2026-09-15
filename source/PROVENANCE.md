# Provenance of `smyth_eng_u.xml`

## Chain of custody

1. **Herbert Weir Smyth**, *A Greek Grammar for Colleges*. American Book
   Company, 1920.
2. **Perseus Project**, Tufts University — digitized into TEI P4 XML under the
   supervision of Lisa Cerrato, William Merrill, Elli Mylonas and David Smith;
   principal Gregory Crane; funded by the National Endowment for the
   Humanities. Perseus text id `1999.04.0007`.
3. **Alpheios Project** — corrections, formatting and indexes added on top of
   the Perseus XML, published as
   [`alpheios-project/grammar-smyth`](https://github.com/alpheios-project/grammar-smyth).
   The XML here also carries Alpheios' LSJ lexicon cross-references in the
   Appendix (`data-ref="Perseus:text:1999.04.0057:entry=..."`).
4. **This repository** — the file is vendored unmodified and all Markdown in
   `docs/` is generated from it by `tools/convert.py`.

## Why the Alpheios copy and not Perseus directly

Perseus digitized the text, but does not currently publish this XML anywhere
retrievable. Checked 2026-09-15:

- **Not on Perseus' GitHub.** All 46 `PerseusDL` repositories were enumerated.
  Reference works live in
  [`canonical-pdlrefwk`](https://github.com/PerseusDL/canonical-pdlrefwk),
  which holds only four items — Zoega's Old Icelandic dictionary, *Allen and
  Greenough's New Latin Grammar*, the *Intermediate Greek-English Lexicon*, and
  Smith's dictionaries. Smyth is absent. Perseus' migration to CTS/TEI P5 took
  the Latin grammar across but not the Greek one; Smyth exists only as a P4
  text inside the old Hopper.
- **The Hopper's XML endpoints are down.** `dltext?doc=Perseus:text:1999.04.0007`,
  the per-section `xmlchunk?…` behind every page's "view as XML" icon, and the
  bulk `opensource/downloads/texts/hopper-texts-GreekRoman.tar.gz` (125 MB,
  described upstream as "the original XML text files") all return
  `503 Backend fetch failed`. The HTML pages serve normally, so this is the XML
  backend specifically. Perseus marked the Hopper source deprecated in May 2016.

**The Perseus HTML is also, in places, worse than this XML.** Section 1011
contains two quotations that use a flatter `<cit>` shape, with an English
`<gloss lang="en">` nested inside a `<quote lang="greek">`. Perseus' renderer
treats the whole quote as Greek and transliterates the English into Greek
letters; this file has it intact:

| | |
|---|---|
| Perseus HTML, §1011 | `ὦ τέκνον, ἦ πάρεστον; μψ ξηιλδρεν, αρε ψε ηερε̣` |
| This XML, §1011 | `ὦ τέκνον, ἦ πάρεστον;` *my children, are ye here?* |

The same passage also shows `ὑ_μῖν` on Perseus where the XML has a proper
combining macron, `ὑ̄μῖν`. So the Alpheios file is not merely the most
accessible copy of the Perseus text — at these points it is the more faithful
one. (Note that the same flat `<cit>` shape is what a naive converter drops:
see the `greek_quote` branch in `tools/convert.py`.)

What cannot be established without a pristine Perseus copy is the full extent
of Alpheios' changes. Visible ones: the `<title type="sub">Machine readable
text</title>` is commented out, `<sourceDesc>` is emptied, and the Appendix
carries added LSJ lexicon cross-references. The Perseus `teiHeader` is
otherwise intact.

## The vendored file

| | |
|---|---|
| Source | `https://raw.githubusercontent.com/alpheios-project/grammar-smyth/d7796df9b3cbccd0a04cdc2b7aebe7d7869e1d28/src/smyth_eng_u.xml` |
| Upstream commit | `d7796df9b3cbccd0a04cdc2b7aebe7d7869e1d28` (2020-05-13) |
| Fetched | 2026-09-15 |
| Size | 11,011,454 bytes |
| SHA-256 | `3d5ba15fe7d3449def129f5f83c92f96718177f7aba2bdeffa8bb64bf1a39d8e` |

Verify with:

```bash
sha256sum source/smyth_eng_u.xml
```

`.gitattributes` marks this path `-text` so Git never rewrites its line
endings; without that the checked-out bytes would not match the hash on
Windows.

**The XML is never edited.** The few known character errors are corrected at
render time by the `ERRATA` table in `tools/convert.py`, where each correction
is listed with its reason and is reported on every build.

## What the file contains

| | |
|---|---|
| Format | TEI P4 (`<TEI.2>`), no namespace, no DTD subset |
| Encoding | UTF-8, Unicode polytonic Greek (not betacode) |
| Entities | `&lt;` and `&gt;` only — nothing else to resolve |
| Numbered sections | 3,048, `id="s1"`…`id="s3048"`, no gaps and no duplicates |
| Dialect notes | 213 further sections (`n="3 D"`, `n="314 a. D"`) with **no** `id` |
| Print page breaks | 717 `<pb n="…"/>` from the 1920 printing |
| Greek runs | 40,627 `<foreign lang="greek">` |
| English glosses | 17,440 `<gloss>` |
| Citations | 3,047 `<cit>`, each with a canonical `bibl/@n` such as `Hom. Il. 14.472` |
| Cross-references | 4,333 `<ref>` — 3,760 by `target="sN"`, 573 by `n="37 D."` |
| Tables | 289, 2,356 rows, no row or column spans anywhere |

Two hierarchies are interleaved and neither maps onto the `div1`…`div7`
nesting: an outline (`type="Part"`, `"Chapter"`, `"Section"`, `"Subsection"`,
`"Subsub"`, `"Sub"`) and the numbered sections (`type="smythp"`). A section can
be a `div3` in one chapter and a `div6` in another, and the `type` attribute is
inconsistently cased. The converter therefore dispatches on `type`, never on
element name.

## Known upstream defects

These are in the Perseus/Alpheios source, not introduced by the conversion.
They are preserved verbatim, and `tools/verify.py` counts them on every run so
a change in their number is visible.

- **148 `<*>` markers** — Perseus' notation for a character that could not be
  read or represented, e.g. `τῑμάητα<*>`. Rendered as escaped text.
- **265 unresolved glyph placeholders** — Perseus entity names that were never
  substituted: `[ιγλιδε]` (193), `[υγλιδε]` (26), `[macrdot]` (24), `[τνυμ ]`,
  `[lins ]`, `[sampi ]`, `[rough ]`, `[smooth]`. Note that several have had
  their Latin names typed in Greek letters upstream — `[ιγλιδε]` is *iglide*.
- **3 empty `<figure/>`** elements with no image data.
- **16 chapters without a heading** — §§822–837 are encoded as `div2`
  (chapter-level) with no `<head>`. They are the opening of Part III and are
  gathered into one file, `part-3-formation-of-words/01-word-formation-introductory.md`.
- **Sigma forms** — five places encode a medial or initial σ as final ς,
  destroying a contrast the text is explicitly drawing: §1's alphabet table,
  the consonant table, note 1a ("written ς, elsewhere ς"), and the appendix
  heading "Verbs beginning with ς". Elsewhere sigma is encoded correctly:
  16,715 σ against 14,841 ς, with no final sigma occurring inside a word.
- **A stray `>`** inside a chapter heading: "III. FIRST (SIGMATIC>) AORIST
  SYSTEM".
- **A lone `lang="de"`** on one `<foreign>`; every other one is `lang="greek"`.

The last two groups are the ones the `ERRATA` table corrects — four rules,
seven substitutions in total, each reported on every build. Nothing else in the
list is touched.

## Licence

The text is licensed **CC BY-NC-SA 3.0 US** (see `../LICENSE`). The NEH
provided support for entering the text; the XML was provided by the Trustees of
Tufts University, Medford MA, Perseus Project. That attribution is required by
the licence and is reproduced in `README.md` and in every generated file's
front matter.
