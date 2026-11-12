---
title: Jetson Orin Nano
layout: default
nav_order: 3
has_children: true
description: "Vortex Lab NVIDIA Jetson Orin Nano setup and administration."
permalink: /jetson/
---

# NVIDIA Jetson Orin Nano
{: .no_toc }

The **NVIDIA Jetson Orin Nano** Developer Kit serves as the edge AI compute node for Vortex Lab. It provides shared GPU acceleration for robotics, computer vision, and deep learning over a secure virtual network.

```
       +---------------------------------------------------------+
       |                   Tailscale Mesh VPN                    |
       +---------------------------------------------------------+
                     /                             \
                    /                               \
     +--------------------------+     +--------------------------+
     |   Administrator Laptop   |     |      Student Laptop      |
     |   (macOS / Linux / Win)  |     |   (VS Code / SSH Client) |
     +--------------------------+     +--------------------------+
                    \                               /
                     \                             /
              +------------------------------------------+
              |      Jetson Orin Nano (Vortex Lab)       |
              |  - Ubuntu 22.04 LTS (JetPack 5.x / 6.x)  |
              |  - Isolated Multi-User (750 Home Dirs)   |
              |  - GPU Groups: video, render             |
              |  - Hardware Groups: dialout, i2c, gpio   |
              +------------------------------------------+
```

---

## Documentation Tracks

| Guide | Description |
|:------|:------------|
| **[Administration Guide]({% link jetson/admin.md %})** | Initial bring-up, Wi-Fi, Tailscale, student user provisioning, and maintenance. |
| **[Developer Setup]({% link jetson/developer.md %})** | Account onboarding, Tailscale setup, SSH config, CUDA validation, and VS Code. |

---

## Hardware Specifications

| Component | Specification |
|:----------|:--------------|
| **SoC** | NVIDIA Orin (Ampere architecture) |
| **GPU** | 1024 CUDA Cores + 32 Tensor Cores (up to 40 TOPS INT8) |
| **CPU** | 6-core Arm Cortex-A78AE |
| **Memory** | 8 GB 128-bit LPDDR5 (68 GB/s) |
| **Storage** | NVMe M.2 SSD |
| **Networking** | Gigabit Ethernet, Wi-Fi, Tailscale Mesh VPN |
| **Software** | JetPack (Linux for Tegra, CUDA, cuDNN, TensorRT) |
