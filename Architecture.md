# 🏗️ Architecture Design

## 🎯 Primary Objective

The primary objective of this infrastructure is to provide secure, reliable remote access to a 1TB external storage device from any authorized location using a private VPN connection.

The system is intentionally designed to:
- Avoid public port exposure
- Eliminate router port forwarding
- Minimize attack surface
- Operate efficiently on low-resource hardware (4GB RAM)

---

## 🖥️ Core Components

### 1️⃣ Host System
- Hardware: VivoBook ASUS E21
- CPU: Intel Celeron N4020
- RAM: 4GB
- Operating System: Ubuntu Server (Headless)

The host system functions as the central infrastructure node.

---

### 2️⃣ Storage Layer

- 1TB External USB Drive
- Filesystem: ext4
- Mount Point: `/mnt/storage`
- Persistent mount via `/etc/fstab`

All persistent data is stored on the external drive, separating operating system files from user-accessible storage.

This separation improves:
- Data integrity
- Backup flexibility
- System recovery capability

---

### 3️⃣ Container Management Layer

- Docker (Container Runtime)
- CasaOS (Web Management Interface)

Docker provides isolation and service control.
CasaOS simplifies container management and service visibility.

Containers are configured to use volumes mapped to `/mnt/storage`.

---

### 4️⃣ Secure Access Layer

- Tailscale (Mesh VPN)

Tailscale establishes an encrypted private network between authorized devices.

Key design decision:
No public IP exposure and no port forwarding.

All remote access occurs through authenticated VPN nodes only.

---

## 🔐 Security Design Principles

1. VPN-only remote access model  
2. No publicly exposed services  
3. Minimal installed packages  
4. Headless server (no GUI)  
5. Docker container isolation  
6. Separate OS and storage architecture  

This design significantly reduces external attack surface while maintaining remote accessibility.

---

## 📊 Resource-Conscious Architecture

Because the system operates on 4GB RAM:

- No desktop environment installed
- Minimal background services
- Limited container deployment
- External storage offloads data pressure

The architecture prioritizes efficiency and stability under constrained hardware conditions.

---

## 📌 Architectural Outcome

The final system enables:

- Secure remote access to 1TB storage from anywhere
- No router modifications required
- No exposed ports
- Controlled, authenticated access only
- Efficient operation on low-end hardware

The architecture reflects a balance between usability, security, and hardware limitations.
