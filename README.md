# geospatial-building-detection
Comparing YOLOv8, YOLOv11, and ResNet50 for building detection in satellite imagery.

# Geospatial Object Detection: Building Extraction from Satellite Imagery

This repository contains a comparative study of three distinct deep learning architectures deployed to detect building footprints in high-altitude satellite imagery. 

## Project Workflow
1. **Data Annotation**: Leveraged CVAT.ai to manually annotate raw satellite images into normalized YOLO format coordinates.
2. **Model Training**: Evaluated performance across 150 epochs using YOLOv8, YOLOv11, and 20 epochs Faster R-CNN (ResNet50).
3. **Leaderboard Submission**: Constructed a robust inference pipeline to generate valid submission strings for Kaggle.

## Performance Summary
| Model Architecture | Pipeline Type | mAP @ 0.5 | mAP @ 0.5:0.95 | Recall |
| :--- | :--- | :--- | :--- | :--- |
| **YOLOv8** | One-Stage (Anchor-Free) | **0.6949** | **0.3507** | **0.5142** |
| **YOLOv11** | One-Stage (Advanced) | 0.6524 | 0.2972 | 0.4565 |
| **ResNet50 (Faster R-CNN)** | Two-Stage (Anchor-Based) | 0.2860 | 0.0890 | 0.3261 |
.
## 🏗️ System Architecture & Pipeline Workflow

The complete end-to-end engineering pipeline is structured into four distinct modules: Data Engineering, Architectural Backbone Modeling, Standardized Evaluation, and Leadership Submission Ingestion.

```text
+---------------------+      +------------------------+      +---------------------------+
|   RAW IMAGES (55)   | ---> |  CVAT.ai ANNOTATION    | ---> |    DATASET CONFIGURATION  |
|  Satellite Patches  |      |  Manual Polygons/Boxes |      |  Image/Label Matrix Split |
+---------------------+      +------------------------+      +---------------------------+
                                                                           |
                                                                           v
+----------------------------------------------------------------------------------------+
|                               MODEL BENCHMARKING ENGINE                                |
|                                                                                        |
|  [Path A] YOLOv8 (One-Stage)   ---> Anchor-Free Regression   ---> 150 Epochs + Mosaic  |
|  [Path B] YOLOv11 (One-Stage)  ---> Spatial Attention Head   ---> 150 Epochs + Mosaic  |
|  [Path C] ResNet50 (Two-Stage) ---> RPN + RoIAlign Feature   ---> 20 Epochs Standard   |
+----------------------------------------------------------------------------------------+

                                                    |
                                                    v                       
      +------------------------+      +----------------------------+
      | UNIFIED METRIC ENGINE  | ---> |   PIPELINE ENFORCEMENT     |
      | torchmetrics (mAP/Rec) |      | Lowercase Schema & " " Fix |
      +------------------------+      +----------------------------+
                                                    |
                                                    v
                                    +----------------------------+
                                    |    FINAL KAGGLE LEADERBOARD|
                                    |    yolov8_submission.csv   |
                                    +----------------------------+
```

# Discussion 
1. Why the YOLO Family Outperformed Faster R-CNN
The two-stage Faster R-CNN architecture scored significantly lower (0.2860 mAP) due to data starvation. Lacking native integration with advanced data augmentations like Mosaic, its deep ResNet50 backbone could not generalize effectively on the raw 55-image distribution. Conversely, the YOLO models multiplied their data variety through aggressive synthetic combinations during training.

2. The Overfitting Phenomenon: YOLOv8 vs. YOLOv11
The empirical results show a trend: YOLOv8 marginally outscored its newer counterpart, YOLOv11. While YOLOv11 has a higher model capacity and complex multi-scale attention layers, it suffered from subtle overfitting given the constraints of the tiny dataset. It began tracking localized environmental noise rather than structural generalities. YOLOv8’s slightly more rigid architecture acted as a structural regularizer, allowing it to generalize better on unseen geospatial data.

# 🚀 Kaggle Inference & Pipeline Compliance
To achieve a successful evaluation on the Kaggle leaderboard engine, the post-processing scripts were hardened against structural formatting rejections:

Null Value Handling: Kaggle's evaluation engine flags missing prediction tokens as Null/NaN, rejecting submissions. The pipeline was updated to append a single white-space string fallback (" ") for any test frame featuring zero confident detections, preserving row consistency.

Format Conversion: Automated parsing functions normalize raw bounding coordinates to match the required evaluation schema:

Submission Column Headers: image_id and prediction_string

Prediction String Layout: [class_id, confidence, x_center_norm, y_center_norm, w_norm, h_norm]

# Final Conclusion:
While YOLOv11 represents the cutting edge of the YOLO lineage, YOLOv8 proved to be the most robust, resilient, and optimal architecture for data-constrained geospatial tasks. It is your best candidate for the Kaggle private leaderboard and stands as the definitive champion of your comparative study.
