# Portainer Installation on Ubuntu VM

This guide walks you through installing Portainer on a fresh Ubuntu VM using Docker in a headless, modern, Docker-official way.

---

## Step 0: Update Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

---

## Step 1: Install Docker Engine

### 1️⃣ Add Docker’s official GPG key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 2️⃣ Add Docker repository

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 3️⃣ Install Docker

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 4️⃣ Verify Docker

```bash
sudo docker run hello-world
```

You should see a message confirming Docker is working.

---

## Step 2: Add your user to the Docker group (optional)

This allows running Docker commands without `sudo`.

```bash
sudo usermod -aG docker $USER
# Log out and back in for group changes to take effect
```

---

## Step 3: Install Portainer

Portainer can be installed via Docker run or Docker Compose. This guide uses `docker run`.

### 1️⃣ Create persistent data volume

```bash
docker volume create portainer_data
```

### 2️⃣ Run Portainer container

```bash
docker run -d \
  -p 9443:9443 \
  -p 9000:9000 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

**Explanation:**

- `-p 9443:9443` → Portainer HTTPS web UI (recommended)
- `-p 9000:9000` → Legacy HTTP UI (optional)
- `--restart=always` → Auto-start on reboot
- `-v /var/run/docker.sock:/var/run/docker.sock` → Portainer can control Docker
- `-v portainer_data:/data` → Persistent storage

---

## Step 4: Access Portainer

1. Open your browser at `https://<your-server-ip>:9443`
2. First-time setup:
   - Create an admin username/password
   - Connect to the local Docker environment

Done! Your Portainer is now managing your Docker host.

