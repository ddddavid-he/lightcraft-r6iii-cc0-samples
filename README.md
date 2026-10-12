# Canon EOS R6 Mark III — CC0 regression samples

Ten original photographs by [@ddddavid-he](https://github.com/ddddavid-he), prepared for LightCraft camera-coverage and rendering regression tests: **8 lossless RAW + 2 C-RAW**, each with its same-stem camera JPEG.

The ten CR3 photographs, camera JPEGs and derived image renders/comparison sheets are dedicated under **[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)**. Copying, modification, redistribution and commercial use do not require attribution, payment or further permission from the photographer. See [LICENSE-CC0.txt](LICENSE-CC0.txt) for the dedication and full legal text.

## Download

**[Dataset release v1.0.0](https://github.com/ddddavid-he/lightcraft-r6iii-cc0-samples/releases/tag/v1.0.0)**

- [Original CR3/JPEG bundle, validation results and reproduction script](https://github.com/ddddavid-he/lightcraft-r6iii-cc0-samples/releases/download/v1.0.0/LightCraft-R6III-10-samples-CC0-1.0-v0.6.0-validation.zip) — 525,080,998 bytes (approximately 525 MB).
- [Archive SHA-256](https://github.com/ddddavid-he/lightcraft-r6iii-cc0-samples/releases/download/v1.0.0/SHA256SUMS.txt)
- [Per-file manifest](manifest.json), [compact CSV](samples.csv), [original-file checksums](SHA256SUMS.txt)

Archive SHA-256:

```text
663d02524efbe6166b2fe801755236cd37979057ef66b940a543f96793118c43
```

Large media files are release assets, not Git-tracked files. Originals retain their camera metadata and are byte-preserving copies.

## Samples

Camera: Canon EOS R6 Mark III, default image area 6960 × 4640 (3:2, approximately 32.3 MP). Lenses: RF24-70mm F2.8 L IS USM and RF70-200mm F2.8 L IS USM. Recording quality was read from Canon MakerNote `Exif.CanonCs.Quality`, not inferred from file size.

| # | Stem | Quality | ISO | Focal length | Camera WB | Scene |
|---|---|---|---:|---:|---|---|
| 01 | `_95A0528` | C-RAW | 100 | 70 mm | Cloudy | Golden crop field and pale blue sky |
| 02 | `_95A3076` | C-RAW | 100 | 70 mm | Auto | Overcast mountains and snow |
| 03 | `_95A1750` | Lossless RAW | 100 | 86 mm | Auto | Portrait autumn forest and walkway |
| 04 | `_95A1895` | Lossless RAW | 100 | 34 mm | Auto | Bright river, clouds and dark forest |
| 05 | `_95A0801` | Lossless RAW | 125 | 84 mm | Cloudy | Lake at sunset |
| 06 | `_95A3034` | Lossless RAW | 12800 | 24 mm | Auto | Warm-lit building, blue sky and reflections |
| 07 | `_95A3028` | Lossless RAW | 320 | 70 mm | Auto | Indoor food and dishes under warm light |
| 08 | `_95A4739` | Lossless RAW | 250 | 70 mm | Auto | White swans on grey-blue water |
| 09 | `_95A4475` | Lossless RAW | 125 | 46 mm | Auto | Bright clouds and dark canyon |
| 10 | `_95A4903` | Lossless RAW | 1000 | 24 mm | Auto | Colorful crafts under artificial light |

These are different real scenes, not same-scene RAW/C-RAW control pairs. They all use 3:2 camera aspect metadata. The Cloudy camera preset is not a Lightroom Custom-WB migration setting.

## ColorData v66

All ten CR3s have **ColorData version 66**, read directly from CTMD maker-note TIFF tag `0x4001`. [colordata-v66.json](colordata-v66.json) records the observed words. Offsets are candidate interpretations from the open LightCraft PRs, not independent sensor-level measurements.

| File | WB / ISO | Words 105–108 | Words 383–386 | Word 661 |
|---|---|---|---|---:|
| `_95A0528` | Cloudy / 100 | 2072,1024,1024,1598 | 512,512,512,512 | 13995 |
| `_95A3076` | Auto / 100 | 2060,1024,1024,1869 | 512,512,512,512 | 13995 |
| `_95A0801` | Cloudy / 125 | 2072,1024,1024,1598 | 512,512,512,512 | 14351 |
| `_95A3034` | Auto / 12800 | 1719,1024,1024,1736 | 2048,2048,2048,2048 | 14351 |
| `_95A3028` | Auto / 320 | 1638,1024,1024,2265 | 512,512,512,512 | 14351 |
| `_95A4903` | Auto / 1000 | 1567,1024,1024,2020 | 2048,2048,2048,2048 | 14351 |

**Word 661 is not labelled as the actual sensor clipping point.** [LightCraft #1029](https://github.com/storytold/lightcraft/pull/1029) reads this field, while [#1071](https://github.com/storytold/lightcraft/pull/1071) retains a data-derived white level. This set can supply additional CC0 regression inputs for that work. Full-sensor histograms and the two PR builds were not tested here.

## Observed release validation

Official macOS universal CLI releases **0.5.0 and 0.6.0**, with verified archive SHA-256 hashes, were run on macOS 27.2 arm64. GPU rendering was disabled. Each original was imported into a fresh in-memory session with isolated home/config/cache and no user camera profiles, queried, exported with default settings to a 1600px-long-edge sRGB JPEG, and queried again.

- **10/10 succeeded on both releases.** All commands returned `ok: true`, process exit status 0 and empty stderr.
- `previewOnly: null` before and after export.
- Nine images exported at 1600 × 1067; portrait `_95A1750` at 1067 × 1600.
- All original CR3/JPEG SHA-256 hashes remained unchanged after testing.

Reports: [0.5.0](validation-v0.5.0/results.json) · [0.6.0](validation-v0.6.0/results.json).

The bundle includes command replies/logs, renders, the original-file checksums and `verify_samples.py`. After extracting it:

```sh
python3 verify_samples.py
python3 verify_samples.py --cli /absolute/path/to/lightcraft-cli
```

The optional rerun writes to a new `validation-rerun/` directory and does not overwrite originals. Passing this smoke test does not verify correct white balance/levels, pixel-exact decoding, measured color calibration or Lightroom parity. Camera JPEG references were converted to sRGB through the same CLI; they are visual references, not an independent decoder reference.

### Visual comparisons: camera JPEG (left), 0.6.0 RAW default (right)

![Samples 01–05](https://github.com/ddddavid-he/lightcraft-r6iii-cc0-samples/releases/download/v1.0.0/comparison-v0.6.0-01.jpg)

![Samples 06–10](https://github.com/ddddavid-he/lightcraft-r6iii-cc0-samples/releases/download/v1.0.0/comparison-v0.6.0-02.jpg)

Two checked camera JPEGs (`_95A0528`, `_95A3034`) carry `ColorSpace=Uncalibrated` and interoperability index `R03` (Adobe RGB metadata); comparisons should honor the source color space.

## Project snapshot

As checked on 2026-10-12, LightCraft's latest release was 0.6.0 and main was `76b5377868d912db69a777bda253a5b1b96b22bd`. PRs #1029/#1071 were open and already provided R6 III sample evidence. This dataset is additional public regression material, not a claim that no R6 III sample exists. [#1073](https://github.com/storytold/lightcraft/pull/1073) proposes Canon inputs for reproducible pooled calibration; these ordinary-scene photographs are not chart measurements.
