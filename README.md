```markdown
# AI-Vehicle-Survey-Traffic-Analysis

Production-grade computer vision pipeline utilizing YOLOv11/v26, TensorRT, and multi-camera tracking for large-scale automated vehicle surveying and traffic analysis.

# AI-Powered Vehicle Survey & Automated Traffic Analysis System

An enterprise-grade computer vision and edge computing pipeline engineered to automate multi-class vehicle classification, real-time spatial tracking, and high-density traffic surveying across 250 deployment locations. 

> 🔒 **NDA Notice:** The core source code, proprietary weights, and backend enterprise integration details of this project are protected under a strict Non-Disclosure Agreement (NDA). This repository serves exclusively as an architectural portfolio, system showcase, and performance validation hub.

---

## System Demonstration

Behold the system processing edge-node streams in real time.

<video src="./ATCC_Demo.mp4" controls autoplay loop muted width="100%"></video>

*(Note: Ensure your uploaded screen recording is named `demo.mp4` and placed in the root directory of this repository for the auto-play loop to render correctly).*

---

## System Architecture & Data Pipeline

                        
                              [ 250+ Edge IP Camera Streams ]
                                              │
                                              ▼
                      [ Dual-Buffered Frame Ingestion Engine (OpenCV) ]
                                              │
                                              ▼
                  ┌───────────────────────────────────────────────────────────┐
                  │         NVIDIA Hardware Inference Optimization Layer      │
                  │                                                           │
                  │  ┌───────────────────────┐     ┌───────────────────────┐  │
                  │  │  Custom YOLOv11 Engine │     │  Custom YOLOv26 Engine │  │
                  │  └───────────┬───────────┘     └───────────┬───────────┘  │
                  │              │                             │              │
                  │              ▼                             ▼              │
                  │       [ TensorRT Graph Optimizer & FP16 Quantization ]    │
                  └─────────────────────────────┬─────────────────────────────┘
                  │
                  ▼
                  [ DeepSORT / ByteTrack State Tracking Engine ]
                  │
                  ▼
                  ┌───────────────────────────────────────────────────────────┐
                  │               Spatial Analysis & Business Logic           │
                  │                                                           │
                  │   * Dynamic Region of Interest (ROI) Masking              │
                  │   * Directional Crossing-Line Logic (Anti-Double Count)   │
                  │   * Real-Time Traffic Density & Flow Metrics Calculation  │
                  └─────────────────────────────┬─────────────────────────────┘
                  │
                  ▼
                  ┌───────────────────────────────────────────────────────────┐
                  │            Enterprise Data & UI Synchronization           │
                  │                                                           │
                  │   * Asynchronous PyQt5 Dashboard Updates (QThreads)       │
                  │   * High-Throughput Oracle Database Connector│
                  │   * Local SQLite Caching System (Network-Drop Failover)   │
                  └───────────────────────────────────────────────────────────┘

## Key Engineering Features

### 1. High-Throughput Edge Inference Optimization
* **Architecture Selection:** Implemented customized YOLOv11 and YOLOv26 deep learning structures specifically tuned for fine-grained multi-class vehicle profiling.
* **Quantization & Acceleration:** Exported and optimized model graphs into TensorRT FP16 execution engines, dramatically scaling frame-per-second (FPS) throughput on localized hardware environments while preserving precision integrity.
* **Class Imbalance Mitigation:** Resolved severe long-tail distribution challenges (e.g., heavily skewed ratios of standard sedans vs. specialized heavy transport vehicles) via localized dataset re-balancing and synthetic mosaic augmentations.

### 2. Multi-Object Tracking & Spatial Filtering Logic
* Implemented deterministic multi-frame object tracking via ByteTrack/DeepSORT variants to prevent duplicate counts during severe bumper-to-bumper city congestion.
* Built dynamic, customizable spatial regions-of-interest (ROIs) to ignore non-road noise and isolate specific highway lanes.

### 3. Enterprise Hardening & Failover Resiliency
* Designed an asynchronous multi-threaded ingestion worker architecture utilizing Python `QThreads` to prevent UI blocking during peak workloads.
* Implemented a robust local-caching transaction matrix ensuring zero-downtime operations and local data preservation during unpredictable network drops or database connection disruptions.

---

## Technical Stack & Production Environment

* **Core Frameworks:** PyTorch, OpenCV, TensorRT
* **Model Topologies:** Custom YOLOv11 / YOLOv26 (Multi-Class Weights)
* **Application Layer:** PyQt5 GUI, Python Multi-threading (Asynchronous Event Processing via QThreads)
* **Data Management:** Oracle Database, Local SQLite Caching Layer
* **Infrastructure Support:** Docker Containerization

```
