# 🛠️ Build Guide: Ubuntu Server to Secure Remote 1TB Storage

## 🎯 Objective

Deploy a headless Ubuntu Server on low-resource hardware and configure it to provide secure remote access to a 1TB external storage device using a VPN-only access model.

The final system enables authenticated remote access to storage without exposing public ports.

---

## 1️⃣ Create Ubuntu Server Installation Media

- Download Ubuntu Server ISO from the official Ubuntu website.
- Use a secondary computer to create a bootable USB (8GB+ recommended).
- Flash the ISO using a trusted USB imaging tool.

---

## 2️⃣ Install Ubuntu Server

- Insert the bootable USB into the target laptop.
- Enter BIOS/UEFI boot menu.
- Select USB device.
- Perform minimal Ubuntu Server installation:
  - No graphical desktop environment
  - Default disk partitioning for OS drive
  - OpenSSH server enabled

After installation, update system packages:

```bash
sudo apt update && sudo apt upgrade -y
```
---

## 3️⃣ Configure External 1TB Storage

1. Connect the external USB drive.
2. Identify the device:

```bash
lsblk
```

3.	If needed, format the partition as ext4:
```bash
sudo mkfs.ext4 /dev/sdX1
```
⚠️ Replace /dev/sdX1 with the correct device name. This will erase all data on the drive.

4.	Create the mount point:

```bash
sudo mkdir -p /mnt/storage
```

5.	Mount the drive:

```bash
sudo mount /dev/sdX1 /mnt/storage
```

6.	Retrieve the UUID for persistent mounting:

```bash
sudo blkid
```

7.	Edit /etc/fstab to mount the drive automatically at boot:

```bash
sudo nano /etc/fstab
```

Add the following line:

```bash
UUID=YOUR-UUID-HERE  /mnt/storage  ext4  defaults  0  2
```

8.	Test the fstab configuration:

```bash
sudo mount -a
```

9.	Set ownership for Docker access:

```bash
sudo chown -R 1000:1000 /mnt/storage
```


The drive is now persistently mounted at:

```bash
/mnt/storage
```

---

## 4️⃣ Install CasaOS (Includes Docker Runtime)

Install CasaOS using the official installation script:

```bash
curl -fsSL https://get.casaos.io | sudo bash
```

Note: CasaOS automatically installs and configures Docker as its container runtime.

Verify Docker installation:

```bash
docker --version
```

---

## 5️⃣ Configure Storage Access via CasaOS

1.	On a device in the same network, open a browser and navigate to the server’s IP.
2.	Access the CasaOS dashboard.
3.	Open the Files application.
4.	Confirm access to:

```bash
/mnt/storage
```

The Files app provides web-based management of the mounted 1TB drive.

---

## 6️⃣ Install and Configure Tailscale

Install Tailscale:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

•	Authenticate the device as prompted.
	•	Verify connection:

```bash
tailscale status
```

The server is now part of a private, encrypted mesh network.

---

## 7️⃣ Validate Remote Access

From an authorized remote device:
	1.	Connect to Tailscale.
	2.	Use the server’s Tailscale IP address.
	3.	Open the CasaOS dashboard in a browser.
	4.	Confirm that /mnt/storage is accessible and functional.

All access is routed through the secure VPN. No public ports are exposed.

---

🏗️ Deployment Outcome

The final system provides:

-	Headless Ubuntu Server
-	Persistent 1TB external storage
-	Web-based file management via CasaOS
-	Docker container runtime (installed automatically)
- Secure VPN-only remote access using Tailscale
- No router port forwarding
- No publicly exposed services

This setup enables secure remote access to storage while maintaining system efficiency and isolation on low-resource hardware (4GB RAM).
