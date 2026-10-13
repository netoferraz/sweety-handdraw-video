# Sources and licenses

Project code and docs use MIT. Upstream licenses stay with each dependency; the following are bundled snapshots that do not require users to install the corresponding Skills separately.

| Source | Pinned version | License / attribution |
|---|---|---|
| [alchaincyf/huashu-art-motion](https://github.com/alchaincyf/huashu-art-motion) | `9ff59339f934e08483ba0a5b1db20f4c2c421b35` | Original MIT statement kept |
| [tomatoparkdao/omniailab-ai-director](https://github.com/tomatoparkdao/omniailab-ai-director) | `772a1477cd436dd8d9fdff60e626a0338ff2f682` | Original MIT statement kept |
| [threerocks/hand-drawn-styles](https://github.com/threerocks/hand-drawn-styles) | `4132988b8596bfa36321c970a4472d4e3bb25d96` | Original MIT statement kept |
| [threerocks/sweety-image-art-direction](https://github.com/threerocks/sweety-image-art-direction) | Entry SHA-256 `fbbfd33d70f729011b6a6bc232851c116cd4a89ead2adea4462888a6ce26fc1a` | Original MIT statement kept |
| [threerocks/sweety-skills / sweety-image-privacy](https://github.com/threerocks/sweety-skills/tree/main/skills/sweety-image-privacy) | 1.1.0 install snapshot, per-file hashes pinned | Prior contribution by this author, shipped under MIT with this project |

To keep a single install entry, `SKILL.md` files inside dependencies were renamed to `GUIDE.md`, with text references renamed accordingly. Promo showcase images and GIFs from huashu's repo are not packed into the install bundle; its production notes and implementation resources are kept. Original file hashes, bundled file hashes, path changes, and exclusion lists are all in [vendor-manifest.json](skills/sweety-handdraw-video/dependencies/vendor-manifest.json). Upstream method conflicts are resolved by this project's [dependency routing](skills/sweety-handdraw-video/references/dependencies.md).

The six hands are project-generated assets, designed under art direction with privacy processing applied; the "Shangguan Wan'er-inspired" label describes the design reference, not official game assets or an authorization claim. The five timbre references are synthesized audio; see `assets/voices/catalog.json` for sources; no cloning of any real individual is claimed. This project's MIT license does not replace third-party character rights, image or voice-service terms applying to generated content. No system commercial fonts are bundled.
