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
