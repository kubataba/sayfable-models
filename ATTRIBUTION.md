# Language pack attribution (release `lang-v1`)

Each `lang-<id>.zip` asset carries the analysis tables (morphology/lemma, proper-name
gazetteer, gloss, sentiment valence) SayFable downloads on demand for one language.
The tables are derived works of the sources below; per-language rows list every source
that contributed to that pack.

## Sources and licenses

| Source | License | Used for |
|---|---|---|
| [Kaikki / Wiktextract](https://kaikki.org) (English Wiktionary extraction) | CC BY-SA | morph/names/gloss/sent of most languages (az, ba, be, bg, ca, cs, da, el, fi, hr, hu, hy, ka, kk, ky, lv, nl, no, pl, ro, ru, sk, sl (glosses), sv, tg, tr, tt, udm, uk, uz) |
| [Русский Викисловарь](https://ru.wiktionary.org) (ru.wiktionary dumps) | CC BY-SA | tt / ba / ky morph+gloss rebuilds (2026-07) |
| [GiellaLT](https://github.com/giellalt) (`lang-myv`, `lang-mdf`, `lang-udm` lexicons) | **LGPL-3.0** | myv / mdf / udm morph, gloss, sentiment pivots |
| [Apertium](https://github.com/apertium) (`apertium-sah`, `apertium-chv`, `apertium-tat`, `apertium-tgk` `.lexc` lexicons) | **GPL-3.0** | sah / chv morph+names; tt and tg enrichment |
| [GrammarDB](https://github.com/Belarus/GrammarDB) (bnkorpus.info) | CC BY-SA 4.0 | be morphology (forms/paradigms) |
| [Sloleks 3.0](http://hdl.handle.net/11356/1745) (CLARIN.SI) | CC BY-SA 4.0 | sl morphology (365k entries, full paradigms) |
| [khakas.altaica.ru](http://khakas.altaica.ru) (Khakas dictionary + glossed parallel corpus) | research resource, used with attribution | kjh morph/gloss |
| Curator's own Kabardian dictionaries (kabrudict/rukabdict; aheku.net name lists) | curator-provided | kbd lemma/names/gloss/sent |
| Curator's russko-udmurtskij slovar' (PDF, inverted) | curator-provided | udm gloss/sentiment enrichment |
| Warriner, A.B., Kuperman, V. & Brysbaert, M. (2013) affective norms | research norms (free for research use) | valence scoring behind every `*-sent.txt` |

## Notes

- Packs containing GiellaLT (LGPL-3.0) material: `lang-myv.zip`, `lang-mdf.zip`, `lang-udm.zip`.
  They are distributed as separate downloadable data files (not linked code); the LGPL source
  lexicons are available from the GiellaLT repositories above.
- Packs containing Apertium (GPL-3.0) material: `lang-sah.zip`, `lang-chv.zip`, and the
  enrichment portions of `lang-tt.zip` / `lang-tg.zip`. Same distribution model: standalone
  data assets, source lexicons available upstream.
- All CC BY-SA derived tables inherit CC BY-SA for the data files themselves.
- The build pipeline (extraction and packing scripts) lives in the SayFableUltra repository
  (`scripts/`), including `build_language_packs.py`, which produced these assets and their
  SHA-256 catalog.
