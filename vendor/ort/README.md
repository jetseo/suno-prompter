# ONNX Runtime Web (vendored)

Vendored from the `onnxruntime-web` npm package (Microsoft, MIT license) so the
site never depends on a third-party CDN — everything is served same-origin.

- Source: https://www.npmjs.com/package/onnxruntime-web
- Version: 1.19.2
- Files: `ort.wasm.min.js` (WASM-only backend loader) + `ort-wasm-simd-threaded.wasm`
  (the CPU inference runtime; falls back to single-threaded execution when
  cross-origin isolation headers aren't set, just slower)
- License: MIT (Microsoft Corporation)

To update: `npm pack onnxruntime-web@<version>`, extract, and replace these
two files with the same names from `package/dist/`.
