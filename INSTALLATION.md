# TeslaMate LXC Installation Guide

This guide provides the full, step-by-step instructions for installing the TeslaMate stack on a Proxmox LXC container.

## Step 1: Create the LXC Container

1.  In your Proxmox web UI, click **"Create CT"**.
2.  **General:**
    * **Hostname:** `teslamate`
    * **Password:** Set a strong password.
    * Check **"Unprivileged container"**.
3.  **Template:** Select a **Debian 12** or Ubuntu template.
4.  **Disks:** Give it at least **8GB** (16GB is safer for long-term data).
5.  **CPU:** Assign **2 cores**.
6.  **Memory:** Assign **1GB** (or 2GB).
7.  **Network:** Configure DHCP or a static IP.
8.  **Confirm:** Finish the creation, but **do not start it yet.**

## Step 2: Configure Proxmox Host for Docker

Before starting the container, you must enable features for Docker and disable AppArmor for this container.

1.  In the Proxmox UI, select the new `teslamate` container.
2.  Go to **Options** > **Features**. Click **Edit**.
3.  Check the boxes for:
    * **Nesting**
    * **keyctl**
4.  Shut down the container if it's running.
5.  Open the **Proxmox Host shell** and edit the container's config file. Replace `<CT_ID>` with your container's ID (e.g., 101).
    ```bash
    nano /etc/pve/lxc/<CT_ID>.conf
    ```
6.  Add the following line to the bottom of the file:
    ```
    lxc.apparmor.profile: unconfined
    ```
7.  Save the file (`Ctrl+X`, `Y`, `Enter`).
8.  **Start the container** from the Proxmox UI.

## Step 3: Install Docker & Docker Compose

1.  Open the container's console from the Proxmox UI and log in.
2.  Update the system:
    ```bash
    apt update && apt upgrade -y
    ```
3.  Install Docker using the official script:
    ```bash
    curl -fsSL [https://get.docker.com](https://get.docker.com) -o get-docker.sh
    sh get-docker.sh
    ```
4.  Install the Docker Compose plugin:
    ```bash
    apt install docker-compose-plugin -y
    ```

## Step 4: Deploy the TeslaMate Stack

1.  Create a dedicated directory for your configuration:
    ```bash
    mkdir /opt/teslamate
    cd /opt/teslamate
    ```
2.  Create your `docker-compose.yml` file:
    ```bash
    nano docker-compose.yml
    ```
3.  Copy the contents of the `docker-compose.yml` file from this repository and paste it into the editor.
4.  **Important:** You **must** edit the file to set your own secure passwords and a new encryption key:
    * `ENCRYPTION_KEY`: Set to a new, long, random string.
    * `DATABASE_PASS`: Set a secure password.
    * Make sure the `DATABASE_PASS` (for `teslamate` and `grafana`) **matches** the `POSTGRES_PASSWORD` (for `database`).
5.  Save the file (`Ctrl+X`, `Y`, `Enter`).

## Step 5: Launch and Access

1.  Pull all the required Docker images:
    ```bash
    docker compose pull
    ```
2.  Start the entire stack in the background:
    ```bash
    docker compose up -d
    ```
3.  Give it a minute to start. You can check that all services are "running" with:
    ```bash
    docker compose ps
    ```

### Access Your Services

* **TeslaMate:** `http://<lxc-ip>:4000`
    * Follow the on-screen prompts to log in and generate your Tesla API tokens.
* **Grafana:** `http://<lxc-ip>:3000`
    * **Username:** `admin`
    * **Password:** `admin`
    * You will be required to set a new password on your first login.
