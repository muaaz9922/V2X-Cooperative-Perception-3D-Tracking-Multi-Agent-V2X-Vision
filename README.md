# 🛜 V2X Cooperative Perception: Multi-Vehicle 3D Tracking

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![ROS / Carla](https://img.shields.io/badge/Simulation-CARLA_/_ROS-000000?style=for-the-badge)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)

A cutting-edge perception pipeline for connected autonomous vehicles (CAVs). By simulating Vehicle-to-Vehicle (V2V) and Vehicle-to-Infrastructure (V2I) communication, this system shares compressed neural feature maps between agents. This cooperative approach expands the effective field of view (FoV), penetrates visual occlusions, and dramatically improves the accuracy of 3D object detection and trajectory tracking across complex traffic scenarios.

---

## ✨ Key Features

* **Multi-Agent Feature Fusion:** Fuses intermediate neural feature maps (rather than raw point clouds) from multiple connected vehicles to balance bandwidth constraints and spatial awareness.
* **Occlusion Penetration:** Successfully tracks dynamic objects (pedestrians, vehicles) that are entirely hidden from the ego-vehicle's direct line of sight but visible to neighboring connected agents.
* **Spatio-Temporal Synchronization:** Handles latency and poses synchronization between moving agents to accurately project external sensor data into the ego-vehicle's coordinate frame.
* **Graph-Based Association:** Utilizes a spatio-temporal graph neural network (GNN) or advanced Hungarian matching for robust multi-object tracking (MOT) across fused 3D bounding boxes.
* **Bandwidth Optimization:** Implements spatial attention mechanisms to only broadcast highly informative regions of the feature map to network peers.

---

## 🏗️ Pipeline Architecture

```text
[Ego Vehicle]                           [Connected Agent]
+-----------------+                     +-----------------+
| Local LiDAR/RGB |                     | Local LiDAR/RGB |
+--------+--------+                     +--------+--------+
         |                                       |
         v                                       v
+--------+--------+                     +--------+--------+
| Feature Encoder |                     | Feature Encoder |
+--------+--------+                     +--------+--------+
         |                                       |
         |         V2X Communication Channel     | (Compressed Features)
         |<--------------------------------------+
         |
         v
+--------+--------+
| V2X Fusion Block| (Spatial Attention / Feature Warping)
+--------+--------+
         |
         v
+--------+--------+
| 3D MOT Tracker  | (Kalman Filter + Graph Matching)
+--------+--------+
         |
         v
+--------+--------+
| Ego-Centric 3D  | (Track IDs, Velocities, Trajectories)
| Scene Graph     |
+-----------------+
