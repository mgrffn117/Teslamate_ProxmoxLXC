# TeslaMate on Proxmox LXC

This repository contains the complete configuration and step-by-step instructions for running a self-hosted **TeslaMate** stack inside an unprivileged **LXC container** on a **Proxmox** host.

## 🚀 About This Project

This setup provides a lightweight, efficient, and isolated environment for logging and visualizing your Tesla's data. The entire stack runs within Docker inside a single LXC container.

### The Stack

* **TeslaMate:** The core data-logging application.
* **PostgreSQL 16:** The database for storing all logged data.
* **Grafana:** For visualizing the data with pre-built dashboards.
* **Mosquitto:** The MQTT broker that TeslaMate uses internally.
* **Docker & Docker Compose:** To containerize and manage the services.

---

## 🔧 How to Use This Repository

1.  **`INSTALLATION.md`**: Start here. This file contains the complete, step-by-step guide, from creating the Proxmox LXC to launching the services.
2.  **`docker-compose.yml`**: This is the required compose file. You will copy this to your server and modify it with your own secure passwords.
3.  **`TROUBLESHOOTING.md`**: If you run into "password authentication" errors in TeslaMate or Grafana, consult this file for the fix.

---

## 🙏 Acknowledgements

This setup relies on the amazing work of the TeslaMate community.

* **Tesla API Authentication:** The most complex part of the setup (generating vehicle tokens) is made simple by using the **[tesla_auth tool by Adrian Kumpf](https://github.com/adriankumpf/tesla_auth?tab=readme-ov-file)**. This guide assumes you are using this tool (or a similar one) as prompted by the TeslaMate UI.
