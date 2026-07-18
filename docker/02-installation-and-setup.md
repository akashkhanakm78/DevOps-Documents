# Chapter 05: Installing Docker on Various Platforms & Post-Installation Setup

This manual guides you through installing Docker Engine and Docker Desktop on various operating systems, setting up post-installation configurations, and validating your setup.

---

## 1. Installation on Linux (Ubuntu / Debian)

The recommended way to install Docker Engine on Linux is using Docker's official repository.

### 1.1 Step-by-Step Installation Commands

```bash
# Step 1: Update the apt package index and install prerequisites
sudo apt-get update
sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Step 2: Add Docker’s official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Step 3: Set up the repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Step 4: Install Docker Engine, containerd, and Docker Compose
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 2. Installation on Windows and macOS

For Windows and macOS, install **Docker Desktop**, which bundles the Docker Daemon, Docker CLI, Docker Compose, and a local Kubernetes environment.

### 2.1 Windows Requirements & Setup
1. **WSL 2 (Windows Subsystem for Linux 2)** is highly recommended. Make sure Windows virtualization is enabled in BIOS.
2. Download the installer from the [Docker Desktop Official site](https://www.docker.com/products/docker-desktop/).
3. Run the installer, ensure "Use WSL 2 instead of Hyper-V" is checked, and complete setup.

### 2.2 macOS Setup
1. Download either the **Apple Silicon** (M1/M2/M3 chips) or **Intel** installer.
2. Double-click the `.dmg` file and drag Docker to the Applications folder.

---

## 3. Post-Installation Configurations (Linux)

By default, the Docker daemon binds to a Unix socket which is owned by the user `root`. Therefore, you must use `sudo` to run Docker commands.

### 3.1 Manage Docker as a Non-Root User
To run Docker commands without prefixing them with `sudo`, add your user to the `docker` group.

```bash
# 1. Create the docker group (if it doesn't already exist)
sudo groupadd docker

# 2. Add your current user to the docker group
sudo usermod -aG docker $USER

# 3. Apply the new group membership (or log out and log back in)
newgrp docker
```

### 3.2 Configure Docker to Start on Boot
```bash
# Enable the docker and containerd services to start automatically on system startup
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

---

## 4. Verification and Sanity Checks

Validate your installation with the following diagnostics:

```bash
# Check installed Docker version
docker --version

# Display system-wide information regarding the Docker installation
docker info

# Run a test image to verify container instantiation
docker run hello-world
```

---

## Common Installation Troubleshooting

* **Error**: `permission denied while trying to connect to the Docker daemon socket...`
  * **Fix**: You haven't run post-install non-root setup. Run `sudo usermod -aG docker $USER` and run `newgrp docker`.
* **Error**: `WSL 2 installation is incomplete...` on Windows.
  * **Fix**: Run `wsl --update` in PowerShell to update your WSL subsystem kernel.
