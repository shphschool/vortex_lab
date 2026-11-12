---
title: Administration
layout: default
nav_order: 1
parent: Jetson Orin Nano
description: "Administration and provisioning guide for NVIDIA Jetson Orin Nano."
---

# Jetson Orin Nano Administration Guide
{: .no_toc }

Administration guide for setting up hardware connectivity, networking, Tailscale, student accounts, and system maintenance on the NVIDIA Jetson Orin Nano.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites

- **Jetson Orin Nano Developer Kit** with NVMe SSD (JetPack 5.x / 6.x installed).
- **Power**: 19V / 45W+ DC power adapter.
- **Cable**: USB Type-C data cable.
- **Admin PC**: macOS, Linux, or Windows.

---

## 1. Initial Serial & USB Connectivity

Connect the Jetson's USB-C port to your workstation to access the serial console and USB Ethernet gadget interface (`192.168.55.1`).

### 1.1 Verify Connection

**macOS:**
```bash
ls -l /dev/cu.usbmodem* /dev/tty.usbmodem*
```

**Linux:**
```bash
ls -l /dev/ttyACM*
```

### 1.2 Connect via Serial Console

```bash
screen /dev/cu.usbmodem14221260061803 115200
```
*(Replace device path with your output from step 1.1.)*

{: .tip }
> **Exit `screen`:** Press `Ctrl + A`, then `k` (kill), and confirm with `y`. To detach without terminating, press `Ctrl + A` then `d`.

### 1.3 Alternative: Direct SSH via USB Gadget

You can also SSH directly over USB-C without using `screen`:

```bash
ssh <admin-user>@192.168.55.1
```

---

## 2. Wi-Fi Configuration (`nmcli`)

Configure the Jetson's Wi-Fi connection via terminal:

```bash
# Scan available Wi-Fi networks
sudo nmcli device wifi list

# Connect to lab Wi-Fi
sudo nmcli device wifi connect "<WIFI_NAME>" password "<WIFI_PASSWORD>"

# Check assigned IP addresses
hostname -I
```

---

## 3. Remote Access via Tailscale

Tailscale provides an encrypted mesh VPN with a permanent IP (`100.x.y.z`) and MagicDNS hostname, bypassing local firewall and DHCP changes.

### 3.1 Install & Connect on Jetson

```bash
# Install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh

# Authenticate and start
sudo tailscale up --operator=$USER
```

Follow the printed URL to authenticate with your administrator account.

### 3.2 Disable Key Expiry (Important)

In the [Tailscale Admin Console](https://login.tailscale.com/admin/machines):
1. Find the Jetson device under **Machines**.
2. Click `...` $$\rightarrow$$ **Disable key expiry** (prevents node disconnection after 180 days).

### 3.3 Get Tailscale IP

```bash
tailscale ip -4
```

### 3.4 Share Device with Students

In the [Tailscale Admin Console](https://login.tailscale.com/admin/machines):
1. Click `...` next to Jetson $\rightarrow$ **Share machine...**.
2. Generate an invite link and share it with the student.

---

## 4. Student Account Management

Student accounts enforce:
- **No `sudo` access**: Protects shared system stability.
- **Home directory isolation**: Permissions `750` (`drwxr-x---`).
- **Hardware access**: Assigned to `video` and `render` (CUDA/TensorRT) and `dialout` (serial/cameras/robotics).

### 4.1 Create Student Account
-  Auto-generate a secure random password:
-  Specify a custom password:

```bash
    sudo ~/tools/create_student_user.sh sarah
    sudo ~/tools/create_student_user.sh sarah StudentPass2026!

```

### 4.2 Remove or Lock Accounts

```bash
# Delete user and home directory
sudo userdel -r sarah

# Lock user account
sudo usermod -L sarah
```

---

## 5. System Maintenance & Telemetry

### 5.1 Real-Time Monitoring (`jtop`)

```bash
# Install jetson-stats
sudo apt update && sudo apt install -y python3-pip
sudo -H pip3 install -U jetson-stats
sudo systemctl restart jtop.service

# Launch dashboard
jtop
```

Monitors GPU utilization, 6-core CPU usage, RAM, temperatures, and power draw.

### 5.2 Power Profiles (`nvpmodel`)

```bash
# Query current power mode
sudo nvpmodel -q

# Set MAXN mode (maximum performance)
sudo nvpmodel -m 0

# Optional: Lock maximum clocks for benchmarking
sudo jetson_clocks
```

### 5.3 Disk Monitoring

```bash
# Check NVMe storage
df -h /

# Check student directory usage
sudo du -sh /home/* | sort -h
```
