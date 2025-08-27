Got it ✅
Here’s the **complete setup file (`SETUP.md`)** in one clean copy-paste format, no breaks:

````markdown
# 🚀 Setup Guide: Personal Backup Cloud with Docker + Tailscale

This guide will help you set up your **private backup cloud** using **Docker, Tailscale, and Syncthing**.  
By the end, you’ll have a working system where your devices automatically back up data to a central server over a secure private VPN.

---

## 1️⃣ Prerequisites
- **Server device** (Raspberry Pi, old laptop, or PC) with sufficient storage  
  _(recommended: 8GB RAM + 1TB storage or more)_.
- **Docker & Docker Compose** installed.  
- A **Tailscale account** (free plan: 100 devices + 3 users).  
- One or more **client devices** (Android, Linux, Windows, macOS).  

---

## 2️⃣ Install Tailscale

### On the Server
Run the following commands:
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
````

* Log in using your Tailscale account.
* Verify the server is visible in your **Tailscale Admin Console**.

### On Client Devices

* Install the **Tailscale app** (available for Android, iOS, Windows, Linux, macOS).
* Log in with the same account.
* Confirm that all devices are listed in your Tailscale network.

---

## 3️⃣ Setup Syncthing on the Server

1. Create a project folder:

```bash
mkdir ~/backup-cloud && cd ~/backup-cloud
```

2. Create a `docker-compose.yml` file with the following content:

```yaml
services:
  syncthing:
    image: lscr.io/linuxserver/syncthing:latest
    container_name: syncthing
    environment:
      - PUID=1000       # User ID (change if needed)
      - PGID=1000       # Group ID (change if needed)
      - TZ=Asia/Kolkata # Timezone
    volumes:
      - ./config:/config  # Persistent Syncthing config
      - ./data:/data      # Data storage directory
    ports:
      - 8384:8384         # Syncthing Web UI
      - 22000:22000/tcp   # TCP sync port
      - 22000:22000/udp   # UDP sync port
      - 21027:21027/udp   # Local discovery
    restart: always
```

3. Start Syncthing with Docker:

```bash
docker compose up -d
```

4. Open the Syncthing Web UI in your browser:

```
http://<SERVER_TAILSCALE_IP>:8384
```

Example:

```
http://100.101.102.103:8384
```

---

## 4️⃣ Setup Syncthing on Android (Client Device)

1. Install **Syncthing-Fork** from the Play Store.
2. Open the app → Add a **New Device**.
3. Enter your server’s **Tailscale IP** (e.g., `100.101.102.103`).
4. Choose the folders you want to sync (e.g., `DCIM/Camera`, `Documents`).
5. On the server’s Syncthing Web UI, **approve the new device**.

Your phone will now automatically back up selected folders to your server. 🎉

---

## 5️⃣ Adding More Devices

* Repeat **Step 4** for each new device.
* All devices will back up to the central server.
* You can also configure **device-to-device syncing** if needed (mesh mode).

---

## ✅ You’re Done!

You now have a fully working **private backup cloud**:

* 🔐 Secure VPN via **Tailscale**
* 💾 Automatic backup with **Syncthing**
* 🐳 Simple deployment using **Docker**

---

## 🔧 Troubleshooting

* **Find your Tailscale IP**:

```bash
tailscale ip -4
```

* **Restart Syncthing container**:

```bash
docker compose restart syncthing
```

* **Check logs**:

```bash
docker logs syncthing
```

* **Firewall Issues**: Ensure ports `8384`, `22000`, and `21027` are open on the server.

---

## 📚 Useful References

* [Tailscale Docs](https://tailscale.com/kb/)
* [Docker Docs](https://docs.docker.com/engine/install/)
* [Syncthing Docs](https://docs.syncthing.net/)

```

---

Would you also like me to merge both `README.md` + `SETUP.md` into **one big README** (with a setup section inside) so that your GitHub repo has only one polished documentation file?
```
