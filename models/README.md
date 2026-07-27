# UVR-MDX-NET-Inst_HQ_5.onnx (vendored)

An MDX-Net instrumental-extraction model from the [Ultimate Vocal Remover
(UVR)](https://github.com/Anjok07/ultimatevocalremovergui) community model
set, as packaged by the [`audio-separator`](https://pypi.org/project/audio-separator/)
project (MIT-licensed wrapper). Given a stereo mix, it predicts the
**instrumental** stem; the vocal stem is derived by subtracting the
instrumental (scaled by `compensate`) from the original mix.

- Source: `https://github.com/TRvlvr/model_repo/releases/download/all_public_uvr_models/UVR-MDX-NET-Inst_HQ_5.onnx`
  (downloaded directly via GitHub Releases — no Zenodo/HuggingFace/CDN
  dependency; `objects.githubusercontent.com`/`release-assets.githubusercontent.com`
  reachable the same way GitHub itself is)
- MD5: `1b124d679eba49f7748dcbc31b2b4923`
- Size: ~57MB (well under GitHub's 100MB single-file limit — no Git LFS needed)
- License: distributed freely by the UVR project for personal/commercial use;
  the exact weight license terms are less formal than the MIT-licensed
  wrapper code around them — re-verify before any commercial redistribution
  beyond this tool's own use.

## Model I/O contract

Input/output tensor: `[batch, 4, 2560, 256]` (float32) — a cropped,
channel-stacked STFT spectrogram, not raw audio. To use this model you must
reproduce the *exact* preprocessing below (verified against the reference
Python implementation in `audio-separator`'s `MDXSeparator`):

- `n_fft = 5120`, `hop_length = 1024`, Hann window (periodic), STFT with
  centered padding (same convention as `torch.stft(..., center=True)`)
- Per chunk: `dim_t = 256` frames, `dim_f = 2560` (cropped from
  `n_fft//2+1 = 2561` bins — the top bin is dropped)
- The "4" channel axis is `[L_real, L_imag, R_real, R_imag]` (i.e. reshape
  channel-major then real/imag, not interleaved)
- Zero out the first 3 frequency bins of the input before inference
  (matches the reference: reduces low-frequency noise)
- `compensate = 1.01` — multiply the predicted instrumental by this before
  subtracting from the mix to derive the vocal stem
- Chunking for full tracks uses `trim = n_fft/2`, `chunk_size = hop_length*(dim_t-1)`,
  `gen_size = chunk_size - 2*trim`, with Hann-windowed overlap-add between
  chunks (25% overlap by default) — **not yet implemented in JS**; this
  session only validated a single chunk (see below)

## Validation performed (this session)

Built a Python reference (`torch.stft`/`torch.istft` + `onnxruntime`) that
runs this exact model on one ~6s chunk of a synthetic test tone, then fed
the *identical* input tensor to this vendored `ort-wasm-simd-threaded.wasm`
running in a real headless Chromium browser via `onnxruntime-web`.

Results matched to float32 precision:

| | Python (`onnxruntime`, native) | Browser (`onnxruntime-web`, WASM) |
|---|---|---|
| min | -546.433000 | -546.432861 |
| max | 560.427500 | 560.427795 |
| mean | 0.019899422 | 0.019899363 |
| sumSq | 77230623.45 | 77230625.94 |
| NaN count | 0 | 0 |

(Differences are ~1e-5 relative — expected float32 cross-engine numerical
noise from different summation order, not a correctness issue.)

Also sanity-checked plausibility on the (non-vocal, purely synthetic) test
tone: the model passed ~98% of the signal through as "instrumental"
(correlation 0.9999 with the original mix) and extracted almost nothing as
"vocal" (energy ~4 orders of magnitude smaller, near-zero correlation) —
correct behavior for content with no vocal-like formant structure, and
evidence the model is doing something semantically sensible rather than
producing noise. **Not yet validated on a real vocal recording** — that's
a natural next check once this is wired into the actual UI.

Browser-side single-chunk inference took ~30s on this sandbox's CPU
(single-threaded WASM, no COOP/COEP cross-origin isolation, no SIMD
threading). Real deployments with proper headers and modern
hardware should be meaningfully faster, but full-track processing on
mobile is still expected to take real time — this is expected and
was accepted up front.

## Not yet done (next session / Part 3 Step 2)

- JS-side STFT/ISTFT matching the exact `torch.stft` convention above (this
  session's validation used Python-precomputed tensors, not a JS STFT)
- Full-track chunking with overlap-add
- Wiring into `index.html`'s mastering pipeline (`state.stems`, UI module,
  fallback to the existing band-limited approximation on failure)
