# RealTime AI Camera

This is a fun, offline-first iOS app that shows people how a robot "sees" the world — using real-time object detection and language translation with zero internet required. 

I’m just a small-time programmer building something fun, useful, and private. No cloud, no data collection — just a lightweight tool for anyone curious about computer vision or building robots. Whether you're protecting your chickens or exploring AI for the first time, this app is for you.

## Core Features
- **600+ object classes** (Open Images V7 via YOLOv8)
- **Real-time detection**: object label + confidence centered on object (no bounding box)
- **Speech output** for accessibility or hands-free use
- **Offline OCR** for English and Spanish
- **Live translation**: Spanish, Japanese, and German → English
- **Model pack manager**: supports downloading, switching, deleting, and exporting custom model sets

## Not Yet Working
- On-device object training (planned feature)
- Model fine-tuning through thumbs-up/down feedback

## Use Cases
- Robotic patrols
- Object-aware automation
- Accessibility tools
- Multilingual text scanning for travel and signage

## Current Status
Everything works in Python except for object model training. CoreML export and iOS Vision integration are in progress inside a clean Python 3.10 environment using verified ONNX and CoreML tooling.
