# AI-Vehicle-Survey-Traffic-Analysis
Production-grade computer vision pipeline utilizing YOLOv11/v26/RFDETR, TensorRT, and multi-camera tracking for large-scale automated vehicle surveying and traffic analysis.




# AI-Powered Vehicle Survey & Automated Traffic Analysis System

An enterprise-grade computer vision and edge computing pipeline engineered to automate multi-class vehicle classification, real-time spatial tracking, and high-density traffic surveying. 

> 🔒 **NDA Notice:** The core source code, proprietary weights, and backend enterprise integration details of this project are protected under a strict Non-Disclosure Agreement (NDA). This repository serves exclusively as a architectural portfolio, system showcase, and performance validation hub.

---

## 🎥 System Demonstration
Behold the system processing edge-node streams in real time.

https://github.com/YourGitHubUsername/AI-Vehicle-Survey-Traffic-Analysis/blob/main/your_video_name.mp4

*(Note: Replace the link above with the exact filename of the video you uploaded in Step 2 so GitHub renders it as a video player).*


## 🏗️ System Architecture & Data Pipeline


[Edge Camera Stream]
│
▼
[Pre-Processing & Frame Ingestion (OpenCV / QThreads)]
│
▼
[TensorRT Inference Engine (YOLOv11/v26 FP16 Quantized)]
│
▼
[Multi-Object Tracking Layer (ByteTrack / DeepSORT State Engine)]
│
▼
[Spatial Logic Core (Counting Zones & Trajectory Analytics)]
│
▼
[Local Secure Storage / Oracle Database Failover Synchronization]



## Key Engineering Features

### 1. High-Throughput Edge Inference Optimization
* **Architecture Selection:** Implemented customized YOLOv11 and YOLOv26 deep learning structures specifically tuned for fine-grained multi-class vehicle profiling.
* **Quantization & Acceleration:** Exported and optimized model graphs into **TensorRT FP16 execution engines**, dramatically scaling frame-per-second (FPS) throughput on localized hardware environments while preserving precision integrity.
* **Class Imbalance Mitigation:** Resolved severe long-tail distribution challenges (e.g., heavily skewed ratios of standard sedans vs. specialized heavy transport vehicles) via localized dataset re-balancing and synthetic mosaic augmentations.

### 2. Multi-Object Tracking & Spatial Filtering Logic
* Implemented deterministic multi-frame object tracking to prevent duplicate counts during severe bumper-to-bumper city congestion.
* Built dynamic, customizable spatial regions-of-interest (ROIs) to ignore non-road noise and isolate specific highway lanes.

### 3. Enterprise Hardening & Failover Resiliency
* Designed an asynchronous multi-threaded ingestion worker architecture utilizing Python `QThreads` to prevent UI blocking during peak workloads.
* Implemented a robust local-caching transaction matrix ensuring zero-downtime operations and local data preservation during unpredictable network drops or database connection disruptions.

---

## 💻 Technical Stack & Production Environment

* **Core Frameworks:** PyTorch, OpenCV, TensorRT, NVIDIA DeepStream
* **Model Topologies:** Custom YOLOv11 / YOLOv26 (Multi-Class Weights)
* **Application Layer:** PyQt5 GUI, Python Multi-threading (Asynchronous Event Processing)
* **Data Management:** Oracle Database, Local SQLite Caching Layer
* **Infrastructure Support:** Docker Containerization
