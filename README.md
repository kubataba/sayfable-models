# sayfable-models

Model files for the [Sayfable](https://sayfable.io) iOS app — on-device neural TTS, punctuation restoration, and word stress prediction, all running locally on iPhone with no cloud dependency.

---

## iOS Porting

All models originate from PyTorch. Two porting paths were used:

| Path | Pipeline | Models |
|------|----------|--------|
| **TorchScript → LibTorch** | `torch.jit.script()` → `.jit` / `torch.jit.trace()` → `.ptl` → ObjC++ `TorchModule` bridge | Silero TTS, Silero Punctuation, AccentorEngine (×3), Piper Latvian TTS |
| **CoreML** | `coremltools.convert()` → `.mlpackage` → Xcode compiles to `.mlmodelc` | RuStress encoder + decoder |

Export scripts for AccentorEngine: [`export_accentor.py`](https://github.com/snakers4/silero-models) from the original Silero repo.  
RuStress CoreML export: [`Russian-Stress-Accent-Predictor`](https://github.com/kubataba/Russian-Stress-Accent-Predictor) — original proprietary model by Sayfable.

---

## Releases

### v3.0 — Piper Latvian TTS (Aivars)

Native Latvian neural TTS voice. No more Cyrillic transliteration — real Latvian synthesis.  
Architecture: VITS (Variational Inference with adversarial learning for end-to-end Text-to-Speech).  
Source: [rhasspy/piper](https://github.com/rhasspy/piper) — Piper v2.0.0 checkpoint, exported to TorchScript via `torch.jit.trace()`.  
License: CC0 (public domain, per Piper's `lv_LV` voice pack).  
Input: Phoneme IDs (rule-based Latvian phonemizer in Swift, no espeak-ng dependency).  
Output: 22050 Hz mono PCM float32.

Used by `PiperTTSEngine` via LibTorch 2.1.0 ObjC++ `TorchModule` bridge (`jitFileAtPath:`).

| File | Size | SHA-256 |
|------|------|---------|
| `lv_LV-aivars-medium.ptl` | 91 MB | `dac75c8f9d3ab6612ea31fc90d94f60247f45e3da24504472dbf890ab28e4084` |

**Swift / LibTorch usage:**
```swift
let model = TorchModule(jitFileAtPath: path)
let output = model.forward([seqTensor, lenTensor, sidTensor, noiseScale, lengthScale, noiseW])
// → [1, 1, N] Float32 tensor — audio samples at 22050 Hz
```

---

### v2.1 — Silero AccentorEngine stress models (RU, UKR, BEL)

Neural word stress prediction for Russian, Ukrainian and Belarusian.  
Architecture: FastText character n-gram embedding + MLP.  
Output: `+` before the stressed vowel, compatible with Silero TTS input format.

Used by `AccentorEngine` via LibTorch 2.1.0. Russian model is the word-level fallback after the primary RuStress CoreML pipeline. Ukrainian and Belarusian are the primary stress predictors for those languages.

| File | Size | SHA-256 |
|------|------|---------|
| `accentor_ru_simple_jit.pt` | 10 MB | `581fcd9f8bcdce23ba033956f2a19f4441766ecd99f030585e46039b67be48f6` |
| `accentor_ukr_jit.pt` | 11 MB | `679d8ce997b55f9dac16c6295446dde625d29a3661baf526f9a4edb90ea72d14` |
| `accentor_bel_jit.pt` | 8.6 MB | `4d5707f11038b54629e2033f631ecb463d34e68a244dcc3d877aa7b70ebf1f89` |

**Swift / LibTorch usage:**
```swift
let model = TorchModule(jitFileAtPath: path)
let output = model.forwardWithStrings(["привіт", "як", "справи"])
// → [N_words, max_vowels] Float32 tensor — argmax = stressed vowel index
```

---

### v2.0 — Silero TE punctuation model

4-language neural punctuation and capitalisation restoration (EN, DE, ES, RU).  
Architecture: DistilBERT + 2 LSTM heads (punctuation + capitalisation). Max 512 tokens.  
Output: 7 punctuation classes + 3 capitalisation classes per WordPiece token.

Used by `SileroPunctuator` via LibTorch 2.1.0. 512 max tokens; 200-word chunking in the app for longer texts.

| File | Size | SHA-256 |
|------|------|---------|
| `te_model_jit.pt` | 85 MB | `7864669213cdb8046cb484243ac1120eeb8f5bf8c025db20007537d8283122a2` |
| `vocab.json` | 1.3 MB | `4e2868c97457d35cd35e04ec0b10bd4d079d27508835b988171d3112e2a596fb` |

---

### v1.1 — Silero v5 CIS Base TTS

On-device neural TTS for 20+ languages and 60+ speaker voices.  
Architecture: Original Silero v5 CIS base model. Loaded directly via `torch::jit::load` — no modifications needed with full LibTorch.  
Output: 48 kHz mono PCM float32 audio.

Used by `SileroTTSEngine` via LibTorch 2.1.0 ObjC++ `TorchModule` bridge (`jitFileAtPath:`).

| File | Size | SHA-256 |
|------|------|---------|
| `v5_cis_base_nostress.jit` | 87 MB | `d7d361caf78b8480bcd65a0c367af665a2bf6f06c8507306e3781dc7c6ce781b` |

**Supported languages:** Russian (29 voices) · Ukrainian (2) · Belarusian (3) · Kazakh (2) · Uzbek · Azerbaijani · Turkish · Armenian · Georgian (2) · Kyrgyz · Tajik (2) · Bashkir (5) · Tatar (2) · Kabardian · Sakha · Khakas (2) · Kalmyk (2) · Chuvash · Udmurt · Erzya · Moksha  
Latin-script languages (English, German, Spanish, Turkish, Estonian, Latvian, Lithuanian) are supported via Cyrillic transliteration.

---

## License

**RuStress CoreML models** (`accentor_encoder.mlpackage`, `accentor_decoder.mlpackage`) — Copyright © 2024 Sayfable. MIT License.  
Source: https://github.com/kubataba/Russian-Stress-Accent-Predictor

**All other models** are derived from [Silero Models](https://github.com/snakers4/silero-models) by Silero Team, used under the **MIT License**.

```
MIT License

Copyright (c) 2020 Silero Team

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

Original repository: https://github.com/snakers4/silero-models
