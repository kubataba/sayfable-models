# sayfable-models
Model files for the Sayfable iOS app.

## Releases

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
