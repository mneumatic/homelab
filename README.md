<!-- markdownlint-disable MD033 MD041 -->

<div align="center">

<img src="https://img.shields.io/badge/Project-Homelab-1da1f2?style=for-the-badge&logo=serverless" alt="project badge">
<img src="https://img.shields.io/badge/Infrastructure-Podman-3055d7?style=for-the-badge&logo=podman" alt="podman badge">

</div>

<div align="center">

# 🏠 My Homelab Infrastructure

**A self-hosted collection of services running on my homelab**

[![License](https://img.shields.io/badge/license-GPL3-blue?style=for-the-badge)](LICENSE)

</div>

---

## 🌟 Overview

This repository contains the Podman Compose configurations for my homelab, including various self-hosted services for productivity, media, development, and more. Docker Compose is also supported as an alternative.

<div align="center">

## 📦 Services

| Service | Description |
|---------|-------------|
| 🦙 **Ollama** | AI/ML model runner with Open Web UI Interface
| 🔍 **SearXNG** | Private search engine
| 💻 **Forgejo** | Git hosting platform
| 🎥 **Jellyfin** | Media server
| 🗄️ **MongoDB** | Database service
| ☁️ **NextCloud** | Cloud storage & collaboration

</div>

---

## 🚀 Quick Start

### Prerequisites

#### Primary: Podman
- Podman (v4.0 or higher)
- Podman Compose plugin (install with `pip install podman-compose` or your package manager)

#### Alternative: Docker
- Docker Engine (v20.10 or higher)
- Docker Compose plugin (v2.0 or higher)

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/mneumatic/homelab.git
   cd homelab
   ```

2. Configure your environment:
   ```bash
   cp .env.example .env
   # Edit .env with your preferred settings
   # Note: .env file needs to be in the same directory as .yaml files
   ```

3. Start the services using your preferred container runtime:

   **Option 1: Using Podman (Recommended)**
   ```bash
   podman-compose -f <filename>.yaml up -d
   ```

   **Option 2: Using Docker**
   ```bash
   docker compose -f <filename>.yaml up -d
   ```

4. Access your services via your browser. These URLs well be different based on the configuration of the `.env` file:

   - Ollama: `http://localhost:8080`
   - Searxng: `http://localhost:8888`
   - Forgejo: `http://localhost:3000`
   - Nextcloud: `http://localhost:8080`

### Auto Start

To ensure that your services start automatically when the system boots, you can enable them using your container runtime's service management capabilities. For example (Fedora Server 44):

#### Enable User Linger
```
loginctl enable-linger username
```

#### Systemd Service File Example

Create a systemd service file for each stack. For example, create a `podman-nextcloud.service` file in `/etc/systemd/user/`.

```
[Unit]
Description=Nextcloud Container Stack
Requires=basic.target
After=basic.target

[Service]
Type=idle
EnvironmentFile=/Path/To/.env
ExecStart=/usr/bin/podman compose -f /Path/To/nextcloud.yaml up
KillMode=none
TimeoutStopSec=60

[Install]
WantedBy=default.target
```

#### Reload systemd and Enable Service

```
systemctl --user daemon-reload
systemctl --user enable --now nextcloud.service
```