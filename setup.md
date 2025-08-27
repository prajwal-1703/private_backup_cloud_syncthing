
---

### 📌 `SETUP.md`
```markdown
# 🚀 Setup Guide: Personal Backup Cloud with Docker + Tailscale

Follow these steps to set up your **private backup cloud**.

---

## 1️⃣ Prerequisites
- A **server device** (Raspberry Pi, laptop, old PC) with enough storage (recommended: 8GB RAM + 1TB storage).
- Installed **Docker** and **Docker Compose**.
- A **Tailscale account** (free tier is enough).
- Client devices (Android, Windows, Linux, macOS).

---

## 2️⃣ Install Tailscale
### On Server
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
