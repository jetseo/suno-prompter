# ONNX Runtime Web (vendored)

Vendored from the `onnxruntime-web` npm package (Microsoft, MIT license) so the
site never depends on a third-party CDN — everything is served same-origin.

- Source: https://www.npmjs.com/package/onnxruntime-web
- Version: 1.19.2
- Files:
  - `ort.wasm.min.js` — WASM-only backend loader (must be served, not just
    `file://` opened directly — it does a dynamic `import()` of the `.mjs`
    below, which Chromium blocks under `file://`'s cross-origin rules)
  - `ort-wasm-simd-threaded.mjs` — companion loader glue for the wasm binary
    (required alongside the `.js` above; without it session creation fails
    with "Failed to resolve module specifier ...mjs")
  - `ort-wasm-simd-threaded.wasm` — the actual CPU inference runtime; falls
    back to single-threaded execution when cross-origin isolation headers
    (COOP/COEP) aren't set on the deployment, just slower
- License: MIT (Microsoft Corporation)

Verified working (this session): loaded over a plain local HTTP server
(not `file://`), created an `InferenceSession` for a 57MB ONNX model, and
produced output numerically matching a Python `onnxruntime` reference to
float32 precision. See `models/README.md` for the full validation.

To update: `npm pack onnxruntime-web@<version>`, extract, and replace these
three files with the same names from `package/dist/`.
