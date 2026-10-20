# Suggested release details

## Settings

- **Tag:** `geomodel-v3.0.4-fp16-tflite-1`
- **Target:** `main`
- **Release title:** `BirdNET+ Geomodel V3.0.4 FP16 - TFLite conversion (unofficial) - build 1`
- **Pre-release:** yes (not yet tested on a device)
- **Asset:** `BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_FP16.tflite`

## Release body (paste this into GitHub)

Unofficial TensorFlow Lite conversion of the BirdNET+ Geomodel V3.0.4 (Global 10K-pruned, FP16), for use in the Feather Ledger app. Not endorsed by the BirdNET team.

**File:** `BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_FP16.tflite` (13,406,660 bytes)
**SHA-256:** `367f1005e4f31e34ecd5a2985435b3eb0fb9dc10c864e6f93b8cecb5873756cb`

**Model:** input `[1,3]` float32 (latitude, longitude, week); output `[1,9789]` float32 species probabilities. Built-in TFLite operators only.

**Upstream:** https://github.com/birdnet-team/birdnet-live-app at revision `76f456daf10ce758e9a75d1668b30ebaf6888169`. Original ONNX SHA-256: `198adb7bcb3cf03341ffa51fbfa7b2d295e5cd900ba09183dcfdbf6d12b5c63b`.

**Licence:** Apache 2.0. See `LICENSE` and `NOTICE` in this repository.

**Modifications:** converted from ONNX to TFLite (onnx2tf 1.28.8, TensorFlow 2.19.0, batch size 1). The 17 `Erf` operations were replaced with an equivalent approximation built from basic operators. Outputs differ slightly from the original: maximum difference 0.0016 to 0.0025 at Cogenhoe, UK (weeks 1, 20, 39), and top-1 species identical in 112 of 112 desktop test cases.

**Status:** validated against the ONNX original on a desktop only. Android device testing is pending.

**Verify:** `sha256sum BirdNET+_Geomodel_V3.0.4_Global_10K-pruned_FP16.tflite`

Please follow the upstream acceptable use policy: https://github.com/birdnet-team/birdnet-live-app/blob/76f456daf10ce758e9a75d1668b30ebaf6888169/ACCEPTABLE_USE.md

## Later use (for the app's setup script)

Download URL pattern, once published:

`https://github.com/joepeterlarkins-maker/feather-ledger-geomodel-assets/releases/download/geomodel-v3.0.4-fp16-tflite-1/<asset file name>`

Use the exact asset file name GitHub shows on the release page. GitHub can rename uploaded files containing unusual characters (the `+` in this name), so confirm it after upload before using it in a script.
