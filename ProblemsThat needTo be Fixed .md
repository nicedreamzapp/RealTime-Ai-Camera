# 🧠 RealTime-Ai-Camera: CoreML Export Issues (Open Images V7, YOLOv8)

This document outlines all the problems encountered so far when exporting a YOLOv8 model trained on Open Images V7 for iOS use via CoreML, as well as whether each issue has been addressed.

---

## ✅ Current Problems & What We've Tried

| #  | **Problem**                                                                                   | **Status / Attempted Fix**                                                                                          |
|----|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 1  | CoreML export breaks class label mapping (Open Images classes not preserved)                 | ✅ Tried `.pt → .onnx → .mlmodel` pipeline, `class_names_600_FULL.yaml`, embedding with Netron — still broken         |
| 2  | Converted CoreML model does not decode detections properly (anchors, confidence, NMS)        | ✅ Tried `coremltools==5.2.0`, `5.0b5`, and `onnx-coreml` — still fails to retain proper YOLOv8 behavior              |
| 3  | Mismatch between macOS (Python) inference and iOS CoreML results                             | ✅ Confirmed `.pt`/`.onnx` works on Mac, but same model fails or mislabels in iOS                                    |
| 4  | No official Ultralytics support for Open Images → CoreML pipeline                            | ✅ Just filed this as a feature request on GitHub                                                                    |
| 5  | No simple way to embed 600+ Open Images labels into CoreML                                   | ✅ Tried several hacks — `userDefinedMetadata`, classifier layers, and JSON edits — not consistent                   |
| 6  | CoreML model does not replicate YOLO confidence/NMS behavior                                 | ✅ Implemented Swift-side decoding — still unreliable with false positives / missed detections                      |
| 7  | Output shape from CoreML model does not match expectations                                   | ✅ Verified tensor shapes in ONNX and CoreML — layout is valid, decoding still off                                   |
| 8  | Vision framework not suited for YOLOv8-style decoding                                        | ✅ Building a manual Swift decoder — still requires clean YOLO output from CoreML to work properly                   |
| 9  | Manual `.mlmodel` conversion requires strict Python env setup                                | ✅ Created `coreml_env_38_real` with validated package versions, but still hit `nnssa` and other issues              |
| 10 | Unpacked `.mlpackage` and tested extracted files directly                                    | ✅ Used `.mlmodel`, `Manifest.json`, and `weights.bin` — same behavior when loaded into Xcode                        |

---

## 🔍 Ultralytics Suggestions from HUB Docs

| #  | **Suggestion**                                            | **Tried?** | **Notes**                                                                                      |
|----|-----------------------------------------------------------|------------|------------------------------------------------------------------------------------------------|
| 1  | Use HUB for training and exporting CoreML models          | ❌         | HUB does not support Open Images V7 format directly, only COCO or YOLO-format datasets         |
| 2  | Use the Ultralytics iOS app for on-device inference       | ❌         | Not applicable — this project is a standalone offline-first iOS app, not a HUB client          |
| 3  | Follow HUB export + CoreML integration guides             | ✅         | Guides assume COCO-style YOLO exports — not compatible with Open Images V7 setup               |
| 4  | Wait for Ultralytics engineer response                    | ⏳         | Filed the issue formally, awaiting a real response beyond the bot-generated assistant message  |

---

## 🛠️ Next Step Ideas

1. Write a YOLOv8-style Swift decoder that re-implements:
   - Grid, stride, and anchor decoding
   - Confidence thresholding + sigmoid
   - Class probability extraction
   - Manual NMS
2. Build a CoreML converter that injects:
   - `NeuralNetworkBuilder` class labels
   - Custom metadata for anchors
3. Reach out to Apple CoreML engineers via Feedback Assistant
4. Ask Ultralytics team for:
   - Known-good `.mlmodel` examples with >500 classes
   - Export scripts for Open Images V7 → CoreML (if internal support exists)

---

## 📎 Reference

Our iOS app: [github.com/nicedreamzapp/RealTime-Ai-Camera](https://github.com/nicedreamzapp/RealTime-Ai-Camera)
