# 🖥️ Self-Hosted Ubuntu Server Infrastructure
> Lightweight • Secure • Containerized • VPN-Only Access

---

## 📌 Project Overview

This project documents the design and deployment of a self-hosted Linux server built on low-resource consumer hardware. The system provides containerized service management, external storage integration, and secure remote access without exposing public network ports.

The objective was to design a secure, efficient, and scalable home infrastructure environment while operating under strict hardware limitations (4GB RAM).

---

## 💻 Hardware Specifications

| Component | Specification |
|------------|--------------|
| Device | VivoBook_ASUSLaptop E21 |
| CPU | Intel® Celeron® N4020 @ 1.10GHz |
| Memory | 4GB RAM |
| Storage | 1TB External USB Drive (ext4) |
| Network | Residential LAN |

---

## 🐧 Operating System

**OS:** Ubuntu Server (Headless Installation)

Ubuntu Server was selected for:

- Minimal memory overhead  
- Improved system stability  
- Reduced attack surface  
- Full control over services  

The device was converted into a headless server for remote management and infrastructure use.

---

## 💾 Storage Configuration

The external 1TB drive was:

- Formatted to `ext4`
- Mounted at `/mnt/storage`
- Persistently configured via `/etc/fstab`
- Assigned Docker permissions (`UID 1000:1000`)

All container volumes map to this external drive, separating operating system files from persistent application data.

---

## 📦 Application Layer

- CasaOS (Container Management UI)
- Docker (Containerization Platform)

Docker provides service isolation and reproducibility.  
CasaOS simplifies deployment, monitoring, and container lifecycle management.

---

## 🔐 Secure Remote Access

Remote access is implemented using a VPN-only model.

- Encrypted mesh VPN connection
- No port forwarding
- No exposed public services
- Access restricted to authenticated devices

This design significantly reduces external attack surface.

---

## 🏗️ Architecture Diagram

---

## 📊 Resource Optimization

Operating within 4GB RAM required strict resource discipline:

- Headless OS installation
- No GUI
- Disabled unnecessary services
- Minimal Docker containers
- Externalized persistent storage

**Typical Usage:**
- Idle RAM: ~800MB–1.2GB
- Idle CPU: <5%

---

## 🛡️ Security Considerations

- No public ports exposed
- VPN-only remote access
- SSH controlled at OS level
- Docker container isolation
- OS and data separation
- Minimal installed packages

---

## 🚀 Future Improvements

- Automated backups (rsync / Borg)
- Firewall rule hardening (UFW)
- Monitoring stack deployment
- Hardware RAM upgrade (8GB)

---
