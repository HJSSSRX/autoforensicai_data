---
tags: [cloud_phone, docker, android, waydroid, pve, port_forwarding, mobile_forensics, bluetooth, tim_qq, remote_forensics]
tools: [docker, ssh, waydroid, modprobe, qtscrcpy, finalshell, adb]
category: mobile_forensics
difficulty: hard
source: 2024FIC_finals
date: 2026-05-05
verified: true
---
# Title: 2024FIC Finals - Cloud Phone Forensics (10 Questions)

## Problem
Analyze cloud phones running as Docker containers inside PVE VMs (node1=101, node2=102). Answer 10 questions about Docker configs, phone containers, remote forensics, and chat records.

## Evidence
- PVE VM 101 (node1) — Linux host running Docker cloud phone containers
- PVE VM 102 (node2) — Similar setup
- Software router port forwarding rules
- PersonalPC with QtScrcpy for phone display

## Solution Steps

### Setup: Starting node1 VM
```
# Reduce vCPU to 2 (PVE free license limit)
qm config 101
qm set 101 --cores 2
```
Login: droid / droid2024fic (from MobaXterm saved sessions)

### Q1: node1 droid user login password
MobaXterm stored credentials (after master password unlock).
→ **droid2024fic**

### Q2: Docker image ID first 6 chars (node1)
```
sudo docker images
```
→ **d1d4bf**

### Q3: Container phone count in node1
```
sudo docker ps -a
```
Lists all containers — 5 phone containers.
→ **5**

### Q4: Pre-requisite commands before starting phone containers (excluding docker)
Check bash history (both droid and root users):
```
# Root history shows two required kernel module loads:
modprobe binder_linux devices="binder,hwbindre,vnbinder"
modprobe ashmem_linux
```
These load IPC driver and shared memory modules required by Waydroid/Android containers.
Also confirmed by VM notes/备注.
→ **2**

### Q5: Bluetooth MAC of "priceless_knuth" container (node1)
```
# Start container
modprobe binder_linux devices="binder,hwbindre,vnbinder"
modprobe ashmem_linux
docker start priceless_knuth
docker exec -it priceless_knuth /bin/sh
```
Then connect via QtScrcpy from PersonalPC (need VMnet9 network adapter for 192.168.100.x subnet).
Phone → Settings → About Phone → Bluetooth address.
→ **3c:5a:b4:01:02:03**

### Q6: Remote forensics port (multiple choice)
A.11111 B.12222 C.13333 D.23333 E.24444 F.35555
Only node2 has forensic tool (弘联/Honglian) artifacts. Check port forwarding rules for node2 ports.
→ **DE** (23333, 24444)

### Q7: loving_shtern container port mapping to router (node2)
```
# Start node2 similarly
qm set 102 --cores 2
# If filesystem errors:
fsck.ext4 -y /dev/sda2
exec /sbin/init
```
```
sudo docker ps -a
```
Container bound to port 5555. Check router port forwarding → mapped to 25555.
→ **25555**

### Q8: "抖音" APK MD5 in loving_shtern (node2)
Locate the APK file in container filesystem, calculate MD5.
→ **0ce8f95ba0401769a9f4860749cc8206**

### Q9: QQ number used for MLM recruitment in comments
node1 has Tim (QQ client). Analyze Tim data → find "lalala小李" account.
→ **3791621185**

### Q10: MLM website domain from chat records
Analyze Tim chat records for domain references.
→ **shop.jshcloud.cn**

## Key Takeaways
- **Kernel modules required**: `modprobe binder_linux` and `modprobe ashmem_linux` must be loaded before Docker Android containers can run
- **vCPU limit workaround**: `qm set <vmid> --cores 2` to reduce CPU cores for free PVE license
- **Network bridging for QtScrcpy**: Add VMnet9 (192.168.100.0/24) to both PVE and forensic workstation to enable phone display via QtScrcpy
- **Filesystem repair**: `fsck.ext4 -y /dev/sda2` then `exec /sbin/init` if node VM fails to boot
- **Port forwarding chain**: Phone container port → node VM port → software router external port
- **Remote forensics artifacts**: Presence of forensic tool files indicates which nodes were actually examined by police
- **Tim = QQ variant**: Tim stores QQ chat data, searchable for evidence of MLM recruitment

## Answer
Q1: droid2024fic
Q2: d1d4bf
Q3: 5
Q4: 2
Q5: 3c:5a:b4:01:02:03
Q6: DE
Q7: 25555
Q8: 0ce8f95ba0401769a9f4860749cc8206
Q9: 3791621185
Q10: shop.jshcloud.cn
