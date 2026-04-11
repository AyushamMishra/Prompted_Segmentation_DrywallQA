# Prompted Segmentation for Drywall QA

An industrial inspection system that leverages natural language prompts to perform semantic segmentation on drywall defects and construction joints. This project utilizes a fine-tuned **SAM2 (Segment Anything Model 2)** backbone to achieve high-precision results with a low-latency footprint.

## 🎯 Project Goals
The system is designed to generate binary masks for two specific construction quality assurance tasks:
* **[span_0](start_span)"segment taping area"**: Detects drywall joints and taping seams (Dataset 1)[span_0](end_span).
* **[span_1](start_span)"segment crack"**: Identifies structural fractures and wall cracks (Dataset 2)[span_1](end_span).

## 🏗️ Model Architecture
The project employs a **Two-Stage Adaptation** strategy to balance foundational knowledge with domain-specific precision:
* **[span_2](start_span)Backbone**: `sam2_hiera_tiny` (Hiera-Tiny)[span_2](end_span).
* **[span_3](start_span)Head**: A custom `TinySegHead` consisting of a sequential 2D convolutional architecture (256 → 128 → 64 → 1 channels)[span_3](end_span).
* **[span_4](start_span)Strategy**: The SAM2 encoder was kept frozen to prevent catastrophic forgetting, while the custom head was trained to map visual features to specific defect signatures[span_4](end_span).

## 📊 Performance Metrics

| Task | Prompt | Dataset | mIoU | Status |
| :--- | :--- | :--- | :--- | :--- |
| **Joint Detection** | "segment taping area" | Drywall-Join-Detect | **1.0000** | [span_5](start_span)Highly Stable[span_5](end_span) |
| **Crack Detection** | "segment crack" | Cracks-3ii36 | **0.4055** | [span_6](start_span)Baseline Established[span_6](end_span) |

### **Key Findings**
* **[span_7](start_span)Drywall Joints**: Achieved perfect mIoU (1.0000), showing exceptional consistency in segmenting high-contrast taping areas[span_7](end_span).
* **Cracks**: Established a robust baseline. [span_8](start_span)Performance on hairline fractures remains a challenge due to spatial downsampling at $512 \times 512$ resolution[span_8](end_span).

## ⏱️ Runtime & Footprint
Optimized for edge-case deployment and mobile inspection workflows:
* **[span_9](start_span)Inference Speed**: ~1.35 images per second on CPU (~740ms/image)[span_9](end_span).
* **[span_10](start_span)Train Time**: ~1 hour and 6 minutes per epoch for large-scale datasets (Cracks)[span_10](end_span).
* **[span_11](start_span)Model Size**: Minimal footprint utilizing the `sam2_tiny` hierarchy[span_11](end_span).

## 🛠️ Requirements & Setup
* **[span_12](start_span)Hardware**: Developed and tested on Kaggle/Colab environments[span_12](end_span).
* **[span_13](start_span)Backbone**: Requires SAM2 Hiera-Tiny checkpoints[span_13](end_span).
* **Libraries**: `torch`, `albumentations`, `opencv-python`, `sam2`.

## 📂 Dataset Sources
* **[span_14](start_span)Drywall Join Detect**: [Roboflow Universe](https://universe.roboflow.com/objectdetect-pu6rn/drywall-join-detect)[span_14](end_span)
* **[span_15](start_span)Cracks Dataset**: [Roboflow Universe](https://universe.roboflow.com/fyp-ny1jt/cracks-3ii36)[span_15](end_span)

---
**[span_16](start_span)Reproducibility**: Experiments were conducted with a fixed random seed (42) and standardized image transformations (Resize, Normalize, HorizontalFlip)[span_16](end_span).
