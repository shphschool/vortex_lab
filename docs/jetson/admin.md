---
title: Administration
layout: page
nav_order: 1
parent: Jetson Orin Nano
---

# Manage Connectivity

### Connect jetson device with your PC using USB-TypeC cable
Verify the connectivity
```shell 
ls -l /dev/cu.usbmodem* /dev/tty.usbmodem
```
it prints out the following
```
crw-rw-rw-  1 root  wheel  0x9000005 12 Nov 09:15 /dev/cu.usbmodem14221260061803
crw-rw-rw-  1 root  wheel  0x9000004 12 Nov 09:10 /dev/tty.usbmodem14221260061803

```

### Login jetson
```shell 
screen /dev/cu.usbmodem14221260061803 115200
```
now you login the device, and get the IP address
```shell
hostname -I
```
### Setup wifi for jetson
```shell
sudo nmcli device wifi connect "<WIFI_NAME>" password "<WIFI_PASSWORD>"
```

verify the wifi
Run hostname -I again. You will now see a second IP address (such as 192.168.1.x or 10.0.0.x) next to 192.168.55.1. You can use either IP to SSH in from your Mac.


### Set up TailScale, to manage the devices over virtual network

* install on jetson
```shell
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```
* install TailScale on my pc
* get a fixed IP for the jetson device

### Generate the TailScale shareURL
* student will join the virtual network

### Set up students accounts and permissions
- Auto-generate a secure random password: 
```
sudo create_student_user.sh sarah
```
- Specify a custom password:
```
sudo create_student_user.sh sarah StudentPass2026!
```


