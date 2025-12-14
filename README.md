# Personal Backup Cloud using Docker + Tailscale

A **self-hosted private backup cloud** built using **Docker**, **Syncthing**, and **Tailscale**.  
This project enables secure, continuous, and automatic backup of personal data across devices without relying on third-party cloud services.

---

## 📌 Project Options

You can deploy this project in two modes:

1. **Option 1 (Basic)** – Syncthing only (background file sync)
2. **Option 2 (Advanced)** – Syncthing + Web Dashboard (File Browser)

---

## ✨ Features

- 🔐 Private VPN using **Tailscale** (Free tier: 100 devices, 3 users)
- 💾 Continuous & automatic file backups using **Syncthing**
- 🖥️ Works on laptops, servers, Raspberry Pi, old hardware
- 📱 Cross-platform (Linux, Windows, macOS, Android)
- ⚡ Simple deployment using **Docker Compose**
- 🌍 Secure access via **Tailscale CGNAT IP**
- 🚫 No public ports or port-forwarding required

### 🚀 Advanced Features (Option 2)

- 📂 Web-based file dashboard
- 🖼️ Image & video preview
- 🛠️ File management (rename, move, delete)
- 🔐 Browser access without SSH

---

## 🏗️ Architecture Overview

### 1️⃣ Tailscale Network
- All devices join a private VPN
- Each device receives a CGNAT IP
- One device acts as the backup server

### 2️⃣ Containers
- **Syncthing** → Handles synchronization
- **File Browser** → Provides web UI (Advanced only)
- Both containers share the same `/data` directory

```
Client Devices
│
▼
Tailscale VPN
│
▼
Backup Server
├── Syncthing
└── File Browser (Optional)
```

---

## 📦 Option 1: Basic Setup (Syncthing Only)

Use this setup if you only need background syncing.

### 📄 docker-compose.yml

```yaml
services:
  syncthing:
    image: lscr.io/linuxserver/syncthing:latest
    container_name: syncthing
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - ./config:/config
      - ./data:/data
    ports:
      - 8384:8384
      - 22000:22000/tcp
      - 22000:22000/udp
      - 21027:21027/udp
    restart: always
```

▶️ Start the service

```bash
docker-compose up -d
```

🌐 Access Syncthing UI

```
http://<SERVER-IP>:8384
```

---

## 🚀 Option 2: Advanced Setup (Syncthing + Dashboard)

This setup provides a Google Drive–like interface for your private cloud.

### ⚠️ Pre-Requisites (Mandatory)

Before starting containers, initialize File Browser configuration:

```bash
mkdir -p fb_config
touch fb_config/settings.json
touch fb_config/filebrowser.db
echo "{}" > fb_config/settings.json
```

### 📄 docker-compose.yml (Advanced)

```yaml
services:
  syncthing:
    image: lscr.io/linuxserver/syncthing:latest
    container_name: syncthing
    hostname: syncthing
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - ./config:/config
      - ./data:/data
    ports:
      - 8384:8384
      - 22000:22000/tcp
      - 22000:22000/udp
      - 21027:21027/udp
    restart: always

  filebrowser:
    image: filebrowser/filebrowser:latest
    container_name: filebrowser
    user: 1000:1000
    command: >
      -d /database/filebrowser.db
      -c /config/settings.json
      -a 0.0.0.0
      -p 80
      -r /srv
    ports:
      - 8080:80
    volumes:
      - ./data:/srv
      - ./fb_config/filebrowser.db:/database/filebrowser.db
      - ./fb_config/settings.json:/config/settings.json
    restart: always
```

▶️ Run the Project

```bash
docker-compose up -d
```

### 🌍 Accessing the Web Dashboard

Connect your device to Tailscale

Open a browser and visit:

```
http://<SERVER-TAILSCALE-IP>:8080
```

### 🔑 Default Login

```
Username: admin
Password: admin
```

⚠️ Change the password immediately after login.

---

## 🔐 Security Notes

- End-to-end encrypted traffic via Tailscale
- No public IP exposure
- Proper file ownership using PUID/PGID
- Ideal for zero-trust home lab setups

---

## 🧠 Use Cases

- Personal backup cloud
- Secure device-to-device sync
- Home lab storage
- Privacy-first cloud alternative
- DevOps / Self-hosting portfolio project

---

## 🧰 Tech Stack

- Docker
- Docker Compose
- Syncthing
- File Browser
- Tailscale
- Linux

---

## 📁 Recommended Directory Structure

```bash
backup-cloud/
├── docker-compose.yml
├── config/
├── data/
├── fb_config/
│   ├── settings.json
│   └── filebrowser.db
└── README.md
```

---

## 📄 License

MIT License

---

## 🤝 Contributing

Contributions, improvements, and suggestions are welcome.  
Fork the repo and submit a pull request.

---

## ⭐ Final Note

This project is resume-ready, DevOps-oriented, and ideal for showcasing:

- Self-hosting
- Secure networking
- Containerization
- Zero-trust architecture

---

If you want next:
- 🧾 **Git commit messages**
- 📊 **Mermaid architecture diagram**
- 🧠 **Interview explanation script**
- 🧑‍💼 **Resume project description**

Just tell me 💪
