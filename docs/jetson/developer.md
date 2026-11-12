---
title: Developer Setup
layout: default
nav_order: 2
parent: Jetson Orin Nano
description: "Developer onboarding, Tailscale setup, SSH configuration, and VS Code remote workflow for NVIDIA Jetson Orin Nano."
---

# Jetson Orin Nano Developer Setup
{: .no_toc }

Developer onboarding guide for connecting to the NVIDIA Jetson Orin Nano, configuring SSH keys and aliases, verifying CUDA, and developing remotely with VS Code.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Onboarding Workflow

```
[1. Request Account] ──> [2. Join Tailscale] ──> [3. First Login & passwd] 
                                                          │
[6. VS Code Remote]  <── [5. Verify CUDA]    <── [4. Setup SSH Keys & Config]
```

---

## 1. Request an Account

Send an email to the lab administrator (`admin@vortexlab.local` or your instructor):

```text
Subject: Jetson Orin Nano Account Request - [Your Full Name]

Hi Admin,

Please set up an account for me on the Vortex Lab Jetson Orin Nano:
- Full Name: Sarah Chen
- Preferred Username: sarah
- Tailscale Account Email: sarah.chen@example.com
```

You will receive an onboarding confirmation email:

```text
============================================================
Student User Created Successfully
============================================================
Username:        sarah
Password:        StudentPass2026!
UID / GID:       2004 / 2004
Home Directory:  /home/sarah (Permissions: 750)
Assigned Groups: sarah dialout video render
GPU/AI Access:   Enabled (video, render)
Hardware Access: Enabled (dialout / cameras / serial)
Sudo Access:     Disabled (student isolation)
------------------------------------------------------------
SSH Login Command for Student:
ssh sarah@100.96.127.88
============================================================
```

{: .note }
Accounts have GPU (`video`, `render`) and serial hardware (`dialout`) access. Sudo is disabled, and home directories are isolated (`750`).

---

## 2. Join Tailscale Network

1. **Install Tailscale**: Download from [tailscale.com/download](https://tailscale.com/download) (macOS, Windows, or Linux).
2. **Sign In**: Log into Tailscale using the same email address provided in your account request.
3. **Accept Share**: Open your email invitation or accept the device share in the [Tailscale Console](https://login.tailscale.com/).
4. **Locate Jetson**: Find the Jetson's 100.x.y.z IP in your Tailscale app under **Machines** / **Shared with you** (e.g., `100.96.127.88`).

---

## 3. Initial SSH Login & Password Change

With Tailscale running:

```bash
# Log in with temporary password
ssh sarah@100.96.127.88

# Change your password immediately
passwd
```

---

## 4. SSH Key Setup & Shortcut

### 4.1 Generate SSH Key (on Your Workstation)

```bash
ssh-keygen -t ed25519 -C "vortex-jetson-sarah"
```
*(Press Enter to accept default path `~/.ssh/id_ed25519`.)*

### 4.2 Copy Public Key to Jetson

**macOS / Linux:**
```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub sarah@100.96.127.88
```

**Windows (PowerShell):**
```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh sarah@100.96.127.88 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
```

### 4.3 Configure SSH Alias (`~/.ssh/config`)

Add this entry to your local `~/.ssh/config` file (`notepad $env:USERPROFILE\.ssh\config` on Windows):

```ssh-config
Host vortex-jetson
    HostName 100.96.127.88
    User sarah
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
```

On macOS / Linux, protect permissions:
```bash
chmod 600 ~/.ssh/config
```

### 4.4 Quick Login

Connect directly with:
```bash
ssh vortex-jetson
```

---

## 5. Verify CUDA & Environment

Run these quick checks inside your Jetson shell:

### Check CUDA Compiler (`nvcc`)

```bash
nvcc --version
```

{: .tip }
If `nvcc` is not in your PATH, add it:
```bash
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### Check PyTorch GPU Access

```bash
python3 -c "import torch; print('CUDA Available:', torch.cuda.is_available(), '| Device:', torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'None')"
```

### Check Real-Time Telemetry

```bash
tegrastats
```
*(Press `Ctrl + C` to exit.)*

---

## 6. Remote Development with VS Code

1. Install **Visual Studio Code** on your workstation.
2. Install the **Remote - SSH** extension (`ms-vscode-remote.remote-ssh`).
3. Press `F1` (or `Cmd+Shift+P` on Mac), type **Remote-SSH: Connect to Host...**, and select **`vortex-jetson`**.
4. Click **Open Folder** and select your directory: `/home/<username>/`.
5. Select `/usr/bin/python3` as your Python interpreter to access pre-installed JetPack CUDA libraries.

---

## 7. Lab Etiquette

- **Workspace**: Keep all code, virtual environments, and datasets in your home directory (`/home/<username>/`).
- **GPU Memory**: Stop background training loops and shut down unused Jupyter kernels so peers can access the 8 GB shared VRAM.
- **Packages**: Use `pip install --user <pkg>` or Python virtual environments. Contact the administrator if system-level packages (`apt`) are required.
- **Storage**: Monitor and clean up large checkpoints:
  ```bash
  du -sh ~
  ```
