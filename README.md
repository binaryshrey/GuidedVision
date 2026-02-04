# GuidedVision

### On-Device Scene Understanding for Accessibility

## Overview

**GuidedVision** is a privacy-focused, on-device computer vision system designed to assist visually impaired users by converting visual scenes into **concise, actionable textual descriptions**.

Unlike traditional computer vision applications that focus solely on object detection accuracy, GuidedVision emphasizes **human utility**, **spatial reasoning** and **accessibility**, all under realistic on-device constraints.

The system runs entirely locally, with no cloud inference or data transmission.

## Problem Statement

Visually impaired users rely on assistive technologies to understand their surroundings and navigate safely.  
However, most existing vision-based assistive systems:

- Depend on cloud-based inference
- Introduce privacy risks (camera data leaves the device)
- Suffer from latency and connectivity issues
- Produce verbose or overwhelming outputs

Additionally, raw object detection outputs (lists of labels and bounding boxes) are often **not useful** for accessibility.

## Goal

Design an **on-device scene understanding system** that:

- Runs fully locally (no cloud APIs)
- Detects relevant objects in indoor scenes
- Reasons about spatial relationships (left / right / near / far)
- Filters out irrelevant information
- Produces short, human-friendly descriptions suitable for accessibility

## Key Design Principles

- **Privacy-first**: All inference is local
- **On-device realism**: Lightweight models and CPU-friendly execution
- **Accessibility-oriented output**: Utility over completeness
- **System-level thinking**: Detection → reasoning → description
- **Deterministic behavior**: No uncontrolled generation

## System Architecture

```
Input Image
    ↓
Lightweight Object Detection (YOLOv8n)
    ↓
Importance Filtering
    ↓
Spatial & Depth-Aware Reasoning
    ↓
Accessible Text Description
```

## Dataset

GuidedVision operates on **static indoor images**, simulating camera frames captured on-device.

- Supports JPG / PNG images
- Can be extended to real camera input
- No images are stored or transmitted

For demonstration, a small set of indoor images is sufficient to validate the system end-to-end.

## Model Choice

### Object Detection Model

**YOLOv8 Nano (YOLOv8n)**

- Lightweight and fast
- Suitable for edge / mobile deployment
- Strong pretrained weights
- Simple integration and deployment

This choice reflects a realistic on-device tradeoff between accuracy and latency.

## Importance Filtering

Not all detected objects are equally relevant for accessibility.

GuidedVision explicitly prioritizes objects that are:

- Important for navigation
- Relevant to user safety
- Common in indoor environments

### Examples of prioritized objects

- Person
- Chair
- Table
- Couch
- Bed
- Door
- Stairs

Objects outside this set are ignored to reduce cognitive load.

---

## Spatial & Depth-Aware Reasoning

GuidedVision goes beyond bounding boxes by reasoning about:

### Horizontal Position

- Left
- In front
- Right

### Approximate Distance

- Very close
- Near
- Far

Distance is estimated using **relative bounding-box area**, a simple but effective proxy for depth in monocular images.

This transforms raw detections into **human-understandable spatial relationships**.

## Accessibility-Oriented Output

The final output is a **short textual description**, optimized for clarity and usefulness.

### Example Output

> “A chair is near in front of you. A person is very close to your left.”

Design goals:

- No probabilities
- No technical jargon
- No clutter
- Focus on actionable information

## Privacy by Design

GuidedVision enforces privacy at every stage:

- No cloud inference
- No network calls
- No image storage
- No logging or telemetry

All processing occurs locally in memory.

## Evaluation Approach

Instead of standard CV metrics like mAP, GuidedVision focuses on **accessibility-aligned evaluation**.

### What We Evaluate

- Responsiveness (latency)
- Description conciseness
- Coverage of important objects
- Overall usefulness to a human user

This reflects real-world assistive technology requirements more accurately than benchmark-centric metrics.

## Limitations

- Uses heuristic depth estimation
- Static images only
- Importance mapping is manually defined

These limitations are intentional to maintain scope discipline and system clarity.

## Future Work

Potential extensions include:

- Video-based scene understanding
- Temporal smoothing across frames
- Speech output for hands-free use
