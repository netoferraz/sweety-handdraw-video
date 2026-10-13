# Hosted Assets & Install Size

v1.2 public hand PNGs, synthetic voice reference/audition WAVs, 21 style previews, and README showcase images and videos live as GitHub Release attachments on this repository; the code package keeps only manifests and checksums. Original-film female recordings, extracted vocals, and the user's personal voice catalog are never publicly uploaded.

`assets/hosted-manifest.json` pins each asset's URL, byte size, and SHA-256. The default cache is `~/.cache/sweety-handdraw-video`, overridable via `HANDDRAW_ASSET_CACHE`; the cache is isolated by content hash. First use requires network; only checksum-passing assets are written to cache. `HANDDRAW_OFFLINE=1` reuses only valid cache — missing or corrupt entries fail with an explicit error. There is no fallback to a different image or voice.

```bash
python scripts/hosted_assets.py --all
HANDDRAW_OFFLINE=1 python scripts/hosted_assets.py assets/hands/pen-only.png
```

Style recipes follow upstream `hand-drawn-styles` commit `4132988b8596bfa36321c970a4472d4e3bb25d96`, which is itself already a lightweight text version. What was removed is this project's extra packaging of 21 preview images. The five upstream runtime reference images gain GitHub mirrors; three of them contained source or simulated-camera fields, which were cleaned and verified pixel-identical, with the upstream byte checksums kept separately in the manifest for traceability. Mirrors use the cleaned sizes and SHA-256; when the original URL is unavailable or checksum-mismatched, the mirror is tried. Recipes, anchor pixels, and production acceptance rules are not simplified.

On 2026-10-13 the designated Netlify team quota was found exhausted and the project and deployment were paused; the user confirmed GitHub-first. On a future Netlify migration, the complete current production site files must be fetched, merged with the standalone `sweety-handdraw-video/` directory, and existing sites must never be overwritten with newly added assets. After upload, re-download anonymously item-by-item and compare sizes and SHA-256; update links only after passing; keep the GitHub mirrors for rollback. No Netlify deployment was performed this time.

Third-party director and motion dependencies' runtime code, fonts, and resources stay at locked versions. Shrinking the current code package does not clean git history; no history commits were rewritten.
