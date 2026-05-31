# sayfable-models
Model files for the Sayfable iOS app.

## Releases

### v2.1 — Silero accentor stress models (UKR, BEL, RU)

Neural stress prediction for Ukrainian, Belarusian, and Russian. FastText embedding + MLP
architecture, ~30 MB total for all 3 languages. Output: `+` before stressed vowel,
compatible with Silero TTS.

Used by `AccentorModelManager` via LibTorch-Lite. Russian model serves as word-level
fallback after the primary RuStress CoreML pipeline for words left unstressed.
Ukrainian and Belarusian are the primary stress predictors for those languages.

| File | Size | SHA-256 |
|------|------|---------|
| `accentor_ukr_jit.pt` | 11 MB | `679d8ce997b55f9dac16c6295446dde625d29a3661baf526f9a4edb90ea72d14` |
| `accentor_bel_jit.pt` | 8.6 MB | `4d5707f11038b54629e2033f631ecb463d34e68a244dcc3d877aa7b70ebf1f89` |
| `accentor_ru_simple_jit.pt` | 10 MB | `581fcd9f8bcdce23ba033956f2a19f4441766ecd99f030585e46039b67be48f6` |

**Architecture:**
- Input: word as string (single word per call)
- Embedding: FastText character n-grams → dense vector
- Classifier: MLP → softmax over vowel positions
- Output: index of stressed vowel (0-based from left)
- `+` insertion handled by `AccentorEngine.predict()`

**Usage in Swift/LibTorch:**
```swift
let model = TorchModule(jitFileAtPath: path)
let output = model.forwardWithStrings(["привіт", "як", "справи"])
// → [N_words, max_vowels] Float32 tensor
// argmax over last dim gives stressed vowel index
```

### v2.0 — Silero TE punctuation model
4-language neural punctuation restoration (en, de, es, ru).
DistilBERT + 2 LSTM heads, 85 MB quantized TorchScript.
Used by `SileroPunctuator` via full LibTorch.

| File | SHA-256 |
|------|---------|
| `te_model_jit.pt` | `7864669213cdb8046cb484243ac1120eeb8f5bf8c025db20007537d8283122a2` |
| `vocab.json` | `4e2868c97457d35cd35e04ec0b10bd4d079d27508835b988171d3112e2a596fb` |

### v1.0 — Silero v5 CIS Base (TTS)
On-device neural TTS for 20 languages, 36 voices.
Model: silero v5_cis_base_nostress, surgically optimized for mobile.
Format: LibTorch-Lite (.ptl), 81 MB.

| File | SHA-256 |
|------|---------|
| `silero_surgery.ptl` | `68eb40cc7553c648ee64675e7f402808c568b3cc10a4023e92e9fd7a5a4ffca4` |
