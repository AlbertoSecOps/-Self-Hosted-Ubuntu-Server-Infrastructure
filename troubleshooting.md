# 🔧 Troubleshooting Guide  
Ubuntu Server + 1TB Storage + CasaOS + Tailscale

This document outlines common issues encountered during deployment and their solutions.  
Use this guide to quickly diagnose and resolve system, storage, service, or VPN-related problems.

---

# 1️⃣ External Drive Not Showing

## Problem
The external 1TB USB drive does not appear after connecting it.

## Diagnostics

```bash
lsblk
dmesg | tail
```

---

# 2️⃣ Drive Mount Fails at Boot

## Problem

/mnt/storage is empty after reboot.

## Diagnostics

```bash
cat /etc/fstab
lsblk -f
```

# Common Cause

Incorrect UUID configured in /etc/fstab.

# Resolution

```bash
sudo blkid
sudo nano /etc/fstab
sudo mount -a
```

If sudo mount -a returns no errors, the configuration is correct.

---

## 3️⃣ Permission Denied on /mnt/storage

# Problem

CasaOS or Docker containers cannot write to the mounted drive.

# Diagnostics

```bash
ls -ld /mnt/storage
```

# Resolution

```bash
sudo chown -R 1000:1000 /mnt/storage
```

Explanation:
UID 1000 typically corresponds to the first user created during Ubuntu installation.

---

## 4️⃣ CasaOS Dashboard Not Loading

# Problem

Cannot access CasaOS in the browser.

# Diagnostics

```bash
sudo systemctl status casaos
ip a
```

# Resolution

```bash
sudo systemctl restart casaos
```

Verify the correct local IP address and try accessing it again:

```bash
http://SERVER-IP
```

---

## 5️⃣ Docker Command Not Found

# Problem

docker --version returns command not found.

# Diagnostics

```bash
sudo systemctl status docker
```

# Resolution

CasaOS installs Docker automatically.
If Docker service is stopped:

```bash
sudo systemctl start docker
```

---

## 6️⃣ Tailscale Not Connecting

# Problem

tailscale status shows offline or no devices.

# Diagnostics

```bash
sudo systemctl status tailscaled
tailscale status
```

# Resolution

```bash
sudo tailscale up
sudo systemctl restart tailscaled
```

Verify device is visible in the Tailscale admin panel:

```bash
https://login.tailscale.com/admin
```

---

## 7️⃣ Cannot Access Server From Remote Device

# Checklist

- Tailscale installed on both devices

- Both devices logged into the same account

- Server shows "active" in tailscale status

# Test Connectivity

```bash
ping 100.x.x.x
```

If ping succeeds:

- Network connection is working

- Issue may be related to CasaOS or service configuration

If ping fails:

- Verify Tailscale connection

- Restart Tailscale service

---

## 8️⃣ Services Not Starting After Reboot

# Diagnostics

```bash
sudo systemctl status tailscaled
sudo systemctl status casaos
sudo systemctl status docker
```

# Enable Services at Boot

```bash
sudo systemctl enable tailscaled
sudo systemctl enable docker
```

Restart if needed:

```bash
sudo reboot
```

---

## 📘 Lessons Learned

-Always verify UUID before editing /etc/fstab

-Test mounts using sudo mount -a before rebooting

-Confirm service health with systemctl status

-VPN-based access is safer than port forwarding

-Document changes during deployment

-Validate permissions when using external storage with Docker

---

✅ Deployment Stability Checklist

✔ External drive mounts automatically
✔ Proper ownership set on /mnt/storage
✔ CasaOS accessible locally
✔ Docker running
✔ Tailscale active
✔ Remote device successfully connected
