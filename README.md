# Smyth's Greek Grammar in Markdown

Herbert Weir Smyth, _A Greek Grammar for Colleges_ (1920) — all 3,048 sections,
converted from the Perseus Project's TEI XML.

Start at **[docs/SUMMARY.md](docs/SUMMARY.md)**.

## Where it came from

The TEI P4 XML digitized by the Perseus Project (text `1999.04.0007`), taken
from [alpheios-project/grammar-smyth](https://github.com/alpheios-project/grammar-smyth)
and vendored unchanged as `source/smyth_eng_u.xml`. Everything in `docs/` is
generated from it — see [source/PROVENANCE.md](source/PROVENANCE.md) for the
full chain of custody and the known defects in the source.

```
source/    the TEI XML, vendored unmodified
tools/     convert.py (TEI -> Markdown) and verify.py (integrity checks)
docs/      178 Markdown files, one per chapter
data/      sections.json: section -> file, anchor, print page, heading path
```

## Licence

The text is licensed [CC BY-NC-SA 3.0 US](LICENSE). Required attribution:

> Herbert Weir Smyth, _A Greek Grammar for Colleges_. Digitized in TEI XML by
> the Perseus Project, Tufts University; XML provided by the Trustees of Tufts
> University, Medford MA. The National Endowment for the Humanities provided
> support for entering this text. Corrections and indexes by the
> [Alpheios Project](https://github.com/alpheios-project/grammar-smyth).
