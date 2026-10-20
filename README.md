# feather-ledger-geomodel-assets

Hosts one file for the Feather Ledger app: an **unofficial TensorFlow Lite conversion** of the BirdNET+ Geomodel V3.0.4 (Global 10K-pruned, FP16).

> **Unofficial.** This is not an official BirdNET release and has not been reviewed or endorsed by the BirdNET team. "BirdNET" is used only to identify the upstream source.

## What the model is

A location-and-season model. Given a latitude, longitude and week, it outputs one probability for each of 9,789 species.

| | |
|---|---|
| File | `BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_FP16.tflite` |
| Size | 13,406,660 bytes |
| Input | `[1, 3]` float32 (latitude, longitude, week) |
| Output | `[1, 9789]` float32 |
| Weights | float16 |
| Operators | TensorFlow Lite built-in operators only (no Flex / Select TF ops) |
| Labels | Not included here. Use the upstream `BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_Labels.txt` from the source repository below. |

## Upstream source

- Project: https://github.com/birdnet-team/birdnet-live-app
- Revision used: `76f456daf10ce758e9a75d1668b30ebaf6888169`
- Original file: `assets/models/BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_FP16.onnx`
- Model licence: https://github.com/birdnet-team/birdnet-live-app/blob/76f456daf10ce758e9a75d1668b30ebaf6888169/MODEL_LICENSE
- Acceptable use policy: https://github.com/birdnet-team/birdnet-live-app/blob/76f456daf10ce758e9a75d1668b30ebaf6888169/ACCEPTABLE_USE.md

## Licence

Apache License 2.0, as granted by upstream for the model weights. See [LICENSE](LICENSE) (the upstream `MODEL_LICENSE`, unchanged) and [NOTICE](NOTICE).

This licence covers the model weights only. It does **not** cover the BirdNET audio model, which has its own, separate licence.

## Modifications

This is a modified version of the original. In summary:

1. Converted from ONNX to TensorFlow Lite using onnx2tf, with a fixed batch size of 1.
2. The 17 `Erf` operations in the network were replaced with an equivalent approximation (Abramowitz and Stegun 7.1.26) built from basic operators, because a direct conversion produces a `FlexErf` operator that standard TensorFlow Lite runtimes cannot run.

Details are in [NOTICE](NOTICE). Because of the approximation, outputs are very close to, but not identical to, the original.

## Checksums (SHA-256)

| File | SHA-256 |
|---|---|
| Original ONNX (upstream, 13,608,653 bytes) | `198adb7bcb3cf03341ffa51fbfa7b2d295e5cd900ba09183dcfdbf6d12b5c63b` |
| Converted TFLite (13,406,660 bytes) | `367f1005e4f31e34ecd5a2985435b3eb0fb9dc10c864e6f93b8cecb5873756cb` |

```sh
sha256sum BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_FP16.tflite
```

## Validation (desktop only)

Compared against the original ONNX model in ONNX Runtime:

- Cogenhoe, UK (52.2400, -0.7841), weeks 1, 20 and 39: maximum absolute difference 0.0016 to 0.0025; top-10 and top-20 species identical.
- 112-case sweep (14 locations x 8 weeks): top-1 species identical in 112/112 cases; maximum difference 0.077 (a Sydney test point); 81 species-week cases crossed the 0.03 "expected locally" threshold differently.

**Not yet tested on an Android device.**

## Conversion tooling

Converted with the Feather Ledger project's `tools/geo-conversion/` scripts (`convert.sh`, `patch_erf.py`, `compare.py`), commit `f6d7ec8`. These are currently kept in a private repository and are not available here. Environment: Python 3.12.15, TensorFlow 2.19.0, tf_keras 2.19.0, onnx2tf 1.28.8, onnx 1.22.0, onnxruntime 1.31.0, onnxsim 0.8.1, onnx_graphsurgeon 0.6.1, sng4onnx 2.0.1, ai-edge-litert 2.3.0, numpy 2.1.3. onnx2tf flags: `-ois input:1,3 -b 1`.

## Responsible use

Model outputs are probabilistic. Please follow the upstream acceptable use policy linked above, and take care when publishing sensitive species locations.

## Disclaimer

Provided "AS IS", without warranties of any kind. See the licence.
