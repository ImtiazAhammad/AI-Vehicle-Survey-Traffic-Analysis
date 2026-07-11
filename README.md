# AI-Powered Vehicle Survey & Automated Traffic Analysis System

> An enterprise-grade computer vision and edge computing pipeline engineered to automate multi-class vehicle classification, real-time spatial tracking, and high-density traffic surveying across 250 deployment locations.

---

> **NDA NOTICE**
>
> The source code, model weights, trained artifacts, and proprietary configuration files associated with this system are protected under a strict Non-Disclosure Agreement (NDA). This repository serves exclusively as a **system architecture and visual portfolio showcase**. No executable code, inference pipelines, or trained model checkpoints are disclosed. All performance metrics, architectural diagrams, and technical descriptions presented herein are validated under controlled conditions and are shared for professional evaluation purposes only.

---

## System Demonstration

<video src="ATCC_Demo.mp4" controls autoplay loop muted width="100%"></video>

> *Note: The video file `ATCC_Demo.mp4` must be present in the repository root directory for the playback loop to render correctly.*

---

## System Architecture

```
                                 ┌───────────────────────────────┐
                                 │ 250+  Camera Streams   │
                                 └───────────────┬───────────────┘
                                                 │
                                                 ▼
                                 ┌───────────────────────────────┐
                                 │ Frame Ingestion Engine        │
                                 │ (Supervision / OpenCV / Asynchronous QThread)│
                                 └───────────────┬───────────────┘
                                                 │
                                                 ▼
        ┌─────────────────────────────────────────────────────────────────────────────────┐
        │                   NVIDIA HARDWARE ACCELERATION LAYER                            │
        │                                                                                 │
        │   ┌─────────────────────────────┐             ┌─────────────────────────────┐   │
        │   │ Custom YOLOv11 Model Graphs │             │ Custom YOLOv26 Model Graphs │   │
        │   └──────────────┬──────────────┘             └──────────────┬──────────────┘   │
        │                  │                                           │                  │
        │                  └───────────────────┬───────────────────────┘                  │
        │                                      │                                          │
        │                                      ▼                                          │
        │                 [ TensorRT FP16 Quantization Optimization ]                     │
        └──────────────────────────────────────┬──────────────────────────────────────────┘
                                               │
                                               ▼
                                 ┌───────────────────────────────┐
                                 │ Multi-Object Tracking Core    │
                                 │ (ByteTrack / DeepSORT Engine) │
                                 └───────────────┬───────────────┘
                                                 │
                                                 ▼
        ┌─────────────────────────────────────────────────────────────────────────────────┐
        │                   SPATIAL COGNITION & ANALYSIS ENGINE                           │
        │                                                                                 │
        │  • Dynamic Region of Interest (ROI) Masking                                     │
        │  • Directional Virtual Tripwires (Prevents Redundant Bumper-to-Bumper Counts)   │
        │  • Real-Time Spatial Traffic Flow & Congestion Metrics Generation               │
        └──────────────────────────────────────┬──────────────────────────────────────────┘
                                               │
                                               ▼
        ┌─────────────────────────────────────────────────────────────────────────────────┐
        │                   ENTERPRISE DATA & SYNC RUNTIME                                │
        │                                                                                 │
        │   ┌─────────────────────────────┐             ┌─────────────────────────────┐   │
        │   │ Asynchronous PyQt5 Dashboard│             │ Secure Transaction Layer    │   │
        │   │ (Zero-Lag UI Performance)   │             │ (Oracle DB)  │   │
        │   └─────────────────────────────┘             └──────────────┬──────────────┘   │
        │                                                              │                  │
        │                                                              ▼                  │
        │                                               [ Local SQLite Caching Failover ] │
        └─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Key Engineering Features

### 1. High-Throughput Edge Inference Optimization

At the core of this pipeline lies a pair of custom-trained detection backbones—**YOLOv11** and **YOLOv26**—architecturally tuned for multi-class vehicle classification under real-world deployment constraints. Each model was trained on a proprietary, large-scale traffic dataset with careful attention to long-tail class distributions, where lightweight passenger vehicles vastly outnumber heavy multi-axle trucks, buses, and specialty commercial vehicles.

To neutralize this class imbalance, the training regime incorporates **Focal Loss** scaling factors that down-weight well-classified easy negatives and aggressively penalize misclassifications on underrepresented heavy vehicle classes. Complementing this, **synthetic mosaic data augmentations** were employed during training—stitching quadrants of multiple source frames into composite scenes—to artificially expand the feature diversity of minority classes and improve recall across occluded, partially visible, and edge-case vehicle configurations.

Once convergence was achieved in PyTorch, each model graph was exported and converted into **NVIDIA TensorRT FP16 execution engines** via precision-aware graph fusion. This conversion fuses redundant convolution-batchnorm-activation sequences into single GPU kernels, halves memory bandwidth requirements through FP16 quantization, and unlocks significant FPS throughput gains on edge-class NVIDIA hardware. The result is a detection subsystem capable of sustaining real-time inference across hundreds of concurrent camera streams without frame drops or latency spikes.

### 2. Multi-Object Tracking & Spatial Filtering Logic

Raw detection bounding boxes are insufficient for accurate traffic surveying. This system layers a robust **multi-object tracking (MOT)** engine—built on optimized **ByteTrack** and **DeepSORT** variants—directly atop the detection output. Each detected vehicle is assigned a persistent, unique tracking ID that remains stable across frames, even under severe bumper-to-bumper congestion scenarios where vehicles overlap, occlude one another, and move at sub-pixel velocities. Re-identification embeddings ensure identity continuity through partial and full occlusions.

To translate raw tracking trajectories into accurate traffic count metrics, the system implements **Directional Virtual Tripwires**—mathematically defined line segments placed across lane boundaries in the frame coordinate space. For each tracked vehicle, the centroid of its bounding box is tested against the tripwire's linear equation on a per-frame basis. A count is registered only when a centroid crosses the tripwire in the designated forward direction, and the system enforces a temporal cooldown per tracking ID to prevent duplicate counts from vehicles that hover near or oscillate across the tripwire boundary in stop-and-go traffic.

Complementing the tripwire logic, **Dynamic Region of Interest (ROI) Masking** delineates the drivable road surface from static non-road pixels—sidewalks, medians, foliage, and architectural structures. By suppressing inference and tracking computations within non-ROI zones, the system conserves GPU cycles and reduces false positives from background clutter, achieving a tighter correlation between detected objects and actual traffic participants.

### 3. Enterprise Hardening & Failover Resiliency

Production-grade deployment demanded a system architecture that isolates computational bottlenecks from user-facing responsiveness. The application is built on a **multi-threaded Python architecture** using **Qt QThreads**, where heavyweight model inference, tracking computation, and database I/O each execute on dedicated background threads. The main **PyTorch-Qt5 GUI thread** remains entirely non-blocking, rendering a real-time dashboard with zero perceptible lag, even when multiple camera pipelines are active simultaneously.

Data persistence is handled through a **dual-layer database strategy**. All transaction metadata—vehicle classification events, timestamps, tracking identifiers, and embedded `IMAGE_DATA` blobs for captured vehicle snapshots—are mapped and synchronized to an **Oracle Database** via a secure transaction layer. In the event of a network interruption or Oracle endpoint unavailability, a **transparent SQLite caching layer** intercepts all write operations locally. When network connectivity is restored, the system performs an automatic reconciliation pass, flushing the cached SQLite transactions back into the primary Oracle store with full referential integrity, ensuring zero data loss across intermittent connectivity failures.

---

## Technical Stack

| Layer | Technologies |
|---|---|
| **Core Frameworks** | PyTorch, OpenCV, TensorRT |
| **Model Topologies** | Custom RFDETR / YOLOv11 / YOLOv26 (Multi-Class Weights) |
| **Application Layer** | PyQt5 GUI, Python Multi-threading (Asynchronous Event Processing via QThreads) |
| **Data Management** | Oracle Database, Local SQLite Caching Layer |
| **Infrastructure Support** | Docker Containerization |
