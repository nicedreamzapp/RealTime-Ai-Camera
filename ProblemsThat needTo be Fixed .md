## ✅ Summary of CoreML Export Issues (YOLOv8 + Open Images V7)

We attempted to export a YOLOv8 model trained on Open Images V7 to CoreML for iOS deployment. Despite multiple strategies and environment setups, we encountered persistent issues and decided to abandon this route in favor of a Create ML–based workflow.

| #  | **Problem**                                                                                   | **Status / Attempted Fix**                                                                                          |
|----|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 1  | CoreML export breaks class label mapping (Open Images classes not preserved)                 | ✅ Tried `.pt → .onnx → .mlmodel` pipeline, `class_names_600_FULL.yaml`, embedding with Netron — still broken         |
| 2  | Converted CoreML model does not decode detections properly (anchors, confidence, NMS)        | ✅ Tried `coremltools==5.2.0`, `5.0b5`, and `onnx-coreml` — still fails to retain proper YOLOv8 behavior              |
| 3  | Mismatch between macOS (Python) inference and iOS CoreML results                             | ✅ Confirmed `.pt`/`.onnx` works on Mac, but same model fails or mislabels in iOS                                    |
| 4  | No official Ultralytics support for Open Images → CoreML pipeline                            | ✅ Filed GitHub issue ([#1125](https://github.com/ultralytics/hub/issues/1125)) — only solution offered was HUB      |
| 5  | No simple way to embed 600+ Open Images labels into CoreML                                   | ✅ Tried `userDefinedMetadata`, classifier layers, and JSON edits — inconsistent or unsupported                      |
| 6  | CoreML model does not replicate YOLO confidence/NMS behavior                                 | ✅ Built Swift-side decoder — unreliable results with false positives / missing detections                           |
| 7  | Output shape from CoreML model does not match expectations                                   | ✅ Tensor layout valid in ONNX/CoreML — decoding logic still failed                                                  |
| 8  | Vision framework not designed for YOLOv8 decoding                                            | ✅ Began manual Swift implementation — blocked by broken upstream output format                                      |
| 9  | `.mlmodel` export requires strict Python env setup                                           | ✅ Used custom env `coreml_env_38_real` — encountered `nnssa` and compatibility issues                               |
| 10 | Unpacking `.mlpackage` and modifying contents didn’t help                                    | ✅ Extracted model, JSON, weights — same issues when reloaded in Xcode                                               |

---

## 🔍 Ultralytics HUB Response

| #  | **Suggestion**                                    | **Tried?** | **Notes**                                                                                      |
|----|---------------------------------------------------|------------|------------------------------------------------------------------------------------------------|
| 1  | Use HUB to export a CoreML-compatible model       | ❌         | We chose not to use HUB due to licensing concerns and project independence                     |
| 2  | Use the Ultralytics iOS app for inference         | ❌         | Not applicable — this project is a standalone, offline-first app                              |
| 3  | Follow HUB export and CoreML guides               | ✅         | Guides assume COCO-style YOLOv8 exports — not applicable to Open Images format                 |
| 4  | Wait for engineer response                        | ⏳         | Received generic recommendation to use HUB ([Issue #1125](https://github.com/ultralytics/hub/issues/1125))           |

---

## ✅ Where the Project is Now

We’ve since shifted to training with **Create ML** using a **268-class dataset** in COCO format for iOS deployment. This new pipeline is fully offline, has no external licensing restrictions, and supports clean, verifiable model iteration using Apple-native tools.

✅ **Create ML working with hand-labeled data**

🧠 **iOS deployment is the current focus**, with YOLOv5 experiments continuing for Android/PC

🛠️ **The goal**: Let users generate their own detection models, label them with guidance, and deploy privately or publicly

---

## 📎 Reference

iOS app: [github.com/nicedreamzapp/RealTime-Ai-Camera](https://github.com/nicedreamzapp/RealTime-Ai-Camera)