# Personal Backup Cloud using Docker + Tailscale

This project provides a **self-hosted backup solution** that uses **Docker** and **Tailscale** to create a secure, private cloud for continuous and automatic backup of your personal data.

With **Tailscale**, you create a private VPN (Virtual Private Network) that allows secure communication between your devices, even if they are behind NAT or firewalls.  
With **Syncthing** (running in Docker), you can sync data across multiple devices in this private network.

---

## ✨ Features
- 🔐 **Private VPN** using Tailscale (free plan supports up to **100 devices** and **3 users**).
- 💾 **Continuous & automatic data backup** with Syncthing.
- 🖥️ Use any device (Raspberry Pi, old laptop, server) as your backup server.
- 📱 Multi-device support (Android, Linux, Windows, macOS, etc.).
- ⚡ Easy setup using **Docker Compose**.
- 🌍 Access your backup devices anywhere through Tailscale’s CGNAT IP.

---

## 🏗️ Architecture
1. **Tailscale Network**  
   - Devices join a shared private network.
   - Each device gets a unique **CGNAT IP**.
   - One device is chosen as the **server** (recommended: a laptop or Raspberry Pi with decent storage, e.g., 8GB RAM + 1TB HDD/SSD).

2. **Docker + Syncthing on Server**  
   - Runs inside Docker container.
   - Exposes Syncthing UI (port **8384**) and sync ports (**22000**, **21027**).

3. **Client Devices**  
   - Join the same Tailscale network.
   - Install **Syncthing client app** (e.g., Syncthing-Fork on Android).
   - Connect to server’s Tailscale IP and select folders to sync.

---

## 🐳 Docker Compose Configuration

Below is the **docker-compose.yaml** file used for Syncthing:

```yaml
services:
  syncthing:
    image: lscr.io/linuxserver/syncthing:latest
    container_name: syncthing
    environment:
      - PUID=1000       # user ID (adjust if needed)
      - PGID=1000       # group ID (adjust if needed)
      - TZ=Asia/Kolkata # timezone
    volumes:
      - ./config:/config  # persistent Syncthing configuration
      - ./data:/data      # folder where synced data is stored
    ports:
      - 8384:8384        # Syncthing Web UI
      - 22000:22000/tcp  # TCP sync port
      - 22000:22000/udp  # UDP sync port
      - 21027:21027/udp  # Local discovery
    restart: always
