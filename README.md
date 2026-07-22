# sayfable-models

Model and data assets for the **SayFable** iOS app, downloaded on demand at runtime. All
inference and analysis run **on-device** (ONNX Runtime or plain table lookups) — no cloud,
nothing leaves the device. The app verifies every download's SHA-256 against its built-in
catalog before installing.

## Releases used by the current app

| Release | Assets | Contents |
|---|---|---|
| **`lang-v1`** | 36 × `lang-<id>.zip` + `ATTRIBUTION.md` | **Language packs** — per-language dictionary tables for on-device text analysis: morphology (word form → lemma + part of speech), proper-name gazetteer, word glosses for the immersive reader, sentiment valence lexicon. One zip per language; ~50 MB zipped / ~340 MB installed in total |
| **`v5.1`** | `silero_v5_full.zip` (~88 MB) | **Silero v5 CIS Base TTS** — five ONNX blocks (duration & pitch predictors, mel encoder/decoder, vocoder) + Silero AccentorEngine stress models (UKR, BEL). 43 voices across 21 languages, 48 kHz output |
| **`v5.2`** | `silero_ru_stress.zip` (~30 MB) | **Silero AccentorEngine stress models (RU)** — accentor, exceptions dictionary, BERT homograph resolver (ONNX), seed patch. Together with the UKR/BEL models in `v5.1` they complete the stress engine (RU, UKR, BEL); required for the Russian Silero voices |
| **`v4.0`** | `lv_LV-aivars-medium.onnx`, `lv_LV-rudolfs-medium.onnx` (+ `.json` configs) | **Piper Latvian TTS** — two native Latvian voices (Aivars, Rudolfs), VITS in ONNX, 22 050 Hz output |

## Legacy releases

`v1.1`, `v2.0`, `v2.1`, `v3.0`, `v5.0` are assets of earlier app versions (LibTorch/CoreML
era — TorchScript TTS, DistilBERT punctuation, CoreML stress). The current app does not
download them; they are kept for archival purposes only.

## Licenses

- **Silero models** — MIT (© Silero Team)
- **Piper Latvian voices** — CC0 1.0
- **Silero AccentorEngine stress models** — MIT
- **Language-pack tables** — per-source licenses and citations in [ATTRIBUTION.md](ATTRIBUTION.md): Kaikki/Wiktextract CC BY-SA, Russian Wiktionary CC BY-SA, GiellaLT LGPL-3.0, Apertium GPL-3.0, GrammarDB CC BY-SA 4.0, Sloleks 3.0 CC BY-SA 4.0, and printed dictionary editions (1890/1942/2006)
