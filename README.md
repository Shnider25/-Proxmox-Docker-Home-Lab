# 🐳 Proxmox + Docker Home Lab

A self-hosted home lab environment built on Proxmox VE, running Docker and managed via Portainer. This project demonstrates infrastructure setup, containerization, and systems administration skills.

---

## 🧰 Tech Stack

| Technology | Purpose |
|---|---|
| [Proxmox VE](https://www.proxmox.com/) | Bare-metal hypervisor for VM management |
| [Ubuntu Server 24.04 LTS](https://ubuntu.com/download/server) | Guest OS running inside Proxmox VM |
| [Docker](https://www.docker.com/) | Container engine for running applications |
| [Portainer CE](https://www.portainer.io/) | Web-based Docker management dashboard |

---

## 🏗️ Architecture

```
Physical Server (Proxmox VE)
└── Virtual Machine (Ubuntu Server 24.04 LTS)
    └── Docker Engine
        └── Portainer CE (Container Management UI)
            └── [Your future containers go here]
```

---

## 🚀 Setup Guide

### Prerequisites
- A physical server or PC with Proxmox VE installed
- At least 4GB RAM and 32GB disk space for the VM
- A local network connection

---

### Step 1 — Create the VM in Proxmox

1. Download the **Ubuntu Server 24.04 LTS** ISO from [ubuntu.com](https://ubuntu.com/download/server)
2. Upload the ISO to Proxmox: `local storage → ISO Images → Upload`
3. Click **Create VM** with these recommended settings:

| Setting | Value |
|---|---|
| OS | Ubuntu Server 24.04 LTS |
| CPU | 2 cores |
| RAM | 4096 MB |
| Disk | 32 GB |
| Network | VirtIO |

4. During Ubuntu install, make sure to enable **OpenSSH Server**
5. When prompted about Featured Snaps — **skip Docker from snap**, we install it manually

---

### Step 2 — Install Docker (Official Method)

SSH into your VM or use the Proxmox console and run the following commands one at a time:

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install dependencies
sudo apt install ca-certificates curl -y

# Add Docker's GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y

# Allow user to run Docker without sudo
sudo usermod -aG docker $USER

# Reboot to apply changes
sudo reboot
```

Verify Docker is working after reboot:

```bash
docker run hello-world
```

---

### Step 3 — Install Portainer

Create the data directory and launch Portainer:

```bash
# Create data directory
mkdir -p /home/$USER/docker/portainer

# Run Portainer container
sudo docker run -d \
  -p 9000:9000 \
  -p 9443:9443 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /home/$USER/docker/portainer:/data \
  portainer/portainer-ce:latest
```

Access the Portainer dashboard at:
```
https://YOUR-VM-IP:9443
```

Find your VM's IP with:
```bash
ip a
```

---

## 📦 Planned / Future Containers

- [ ] Nginx Proxy Manager — reverse proxy with SSL
- [ ] Pi-hole — network-wide ad blocking
- [ ] Jellyfin — self-hosted media server
- [ ] Nextcloud — self-hosted cloud storage
- [ ] Vaultwarden — self-hosted password manager
- [ ] Home Assistant — smart home automation

---

## 💡 Key Concepts Demonstrated

- **Virtualization** — Creating and managing VMs with Proxmox
- **Linux Administration** — Ubuntu Server setup, package management, user permissions
- **Containerization** — Installing and running Docker from official sources
- **Networking** — Port mapping, accessing services over local network
- **Self-hosting** — Running production-grade services on personal infrastructure
- **Infrastructure as Code** — Documenting reproducible setup steps

---

## 📸 Screenshots

![image alt](https://i.imgur.com/NJtpWNE.png)

![image alt](https://i.imgur.com/akRoZvr.png)



---

## 📄 License

MIT License — feel free to use this as a template for your own home lab.

---

## 👤 Author

**Shnider**  
Feel free to connect on [LinkedIn](https://www.linkedin.com/in/shnider/) | [GitHub](https://github.com/Shnider25)
