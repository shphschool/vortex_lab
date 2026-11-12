---
title: Developments
layout: page
nav_order: 1
parent: Jetson Orin Nano
---
## Development Environment Set up

### Ask for jetson account and permission
* write email to admin, with your preferred username and password( strong password)

once your account is created you will the email like below
Note, the IP address is a dynamic IP. you will need to join virtual network to get a fixed IP
```
  ============================================================
  Student User Created Successfully
  ============================================================
  Username:        sarah
  Password:        **********
  UID / GID:       2004 / 2004
  Home Directory:  /home/sarah (Permissions: 750)
  Assigned Groups: sarah dialout video render
  GPU/AI Access:   Enabled (video, render)
  Hardware Access: Enabled (dialout / cameras / serial)
  Sudo Access:     Disabled (student isolation)
  ------------------------------------------------------------
  SSH Login Command for Student:
  ssh sarah@10.66.81.12
  ============================================================
```

### Ask for jetson virtual network IP address
* install TailScale app on your pc
* sign up a TailScale account nwith your personally email address. then sign in TailScale
* provide your email address to admin, to add your TailScale account to the jetson virtual network

### Verify the virtual network and device
* on your TailScale console, you will see the jetson under **Machine** tab
* get the IP address of the jetson. i.e. 100.96.127.88
* login to jetson using SSH 

```shell
ssh sarah@100.96.127.88
```


## Optional
### To login without typing your password everytime, set up SSH keys
* generate ssh key pair
```shell
ssh-keygen -t ed25519 -C "your-device-name"
```
* copy your public key to your jetson
```shell
ssh-copy-id -i ~/.ssh/id_ed25519.pub <jetson-user>@<jetson-tailscale-name>
```

* verify connection by ssh without providing password
```
ssh <jetson-user>@<jetson-tailscale-name>
```

### To ssh without typing the IP address
* open ssh config
```shell
vim ~/.ssh/config
```
* adding the following to your ssh config
```shell
Host jetson
  HostName <jetson fixed virtual IP address>
  User <your username of jetson>
  IdentityFile <your ssh key. i.e.~/.ssh/id_jetson>
```
