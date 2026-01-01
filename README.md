# AWS Zabbix Monitoring Infrastructure

## 📋 Project Overview
Centralized monitoring infrastructure deployed on AWS using Zabbix in Docker containers to monitor a hybrid environment (Linux & Windows servers).

**Supervisor:** Prof. Azeddine KHIAT  
**Academic Year:** 2025/2026

---

## 🏗️ Architecture

### Cloud Infrastructure
- **Provider:** AWS (us-east-1)
- **Network:** Custom VPC (10.0.0.0/16) with public subnet
- **Security:** Dedicated security groups for server and clients

### EC2 Instances
| Instance | Type | OS | Purpose |
|----------|------|----|----|
| Zabbix Server | t3.large | Ubuntu 22.04 | Monitoring server (Docker) |
| Linux Client | t3.medium | Ubuntu 22.04 | Monitored host |
| Windows Client | t3.large | Windows Server 2022 | Monitored host |

---

## 🚀 Quick Start

### Prerequisites
- AWS Learner Lab account
- SSH key pair
- Basic knowledge of Linux, Docker, and AWS

### 1. AWS Infrastructure Setup

**VPC Configuration:**
- VPC: 10.0.0.0/16
- Subnet: 10.0.1.0/24 (public)
- Internet Gateway attached
- Route table configured for internet access

**Security Groups:**
- **Zabbix-Server-SG:** Ports 22, 80, 443, 10051
- **Zabbix-Clients-SG:** Ports 22, 3389, 10050

### 2. Deploy Zabbix Server

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Deploy Zabbix
cd ~/zabbix
docker-compose up -d
```

### 3. Configure Agents

**Linux Client:**
```bash
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo apt update && sudo apt install zabbix-agent -y

# Edit config: /etc/zabbix/zabbix_agentd.conf
# Set: Server=<Zabbix-Server-Private-IP>
# Set: Hostname=Linux-Client

sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent
```

**Windows Client:**
1. Download Zabbix Agent from https://www.zabbix.com/download_agents
2. Install with Server IP during setup
3. Verify service is running in services.msc

---

## 📁 Repository Structure

```
aws-zabbix-monitoring/
├── README.md
├── configs/
│   ├── docker-compose.yml
│   └── zabbix_agentd.conf
├── screenshots/
│   └── [project screenshots]
└── docs/
    └── [additional documentation]
```

---

## 🌐 Access Zabbix

**Web Interface:** `http://<Zabbix-Server-Public-IP>`

**Default Credentials:**
- Username: `Admin`
- Password: `zabbix`

---

## 📊 Monitoring

### Add Hosts in Zabbix
1. Go to **Configuration → Hosts → Create host**
2. Set hostname and IP address
3. Add template: "Linux by Zabbix agent" or "Windows by Zabbix agent"
4. Verify green ZBX icon in **Monitoring → Hosts**

### View Metrics
- **Latest Data:** Real-time metrics (CPU, RAM, disk, network)
- **Graphs:** Visual representation of performance
- **Problems:** Active alerts and triggers

---

## 🔧 Useful Commands

**Restart Zabbix (after AWS Lab restart):**
```bash
cd ~/zabbix
docker-compose up -d
```

**Check container status:**
```bash
docker-compose ps
```

**View logs:**
```bash
docker logs zabbix-server
```

**Check agent status (Linux):**
```bash
sudo systemctl status zabbix-agent
```

---

## ⚠️ AWS Learner Lab Notes

- **Region:** Use only us-east-1 (N. Virginia)
- **Cost Management:** Stop instances when not in use
- **Budget:** Monitor the 50$ limit
- **Auto-shutdown:** Lab stops automatically - restart containers after reboot

---

## 📝 Project Files

- **docker-compose.yml:** Zabbix stack configuration (MySQL, Server, Web)
- **zabbix_agentd.conf:** Linux agent configuration
- **screenshots/:** Project implementation screenshots

---

## 🎯 Key Features

✅ Centralized monitoring dashboard  
✅ Real-time metrics collection  
✅ Hybrid environment support (Linux/Windows)  
✅ Docker containerized deployment  
✅ Automated alerts and triggers  
✅ Scalable architecture

---

## 📚 References

- [Zabbix Documentation](https://www.zabbix.com/documentation)
- [Docker Documentation](https://docs.docker.com/)
- [AWS EC2 Guide](https://docs.aws.amazon.com/ec2/)

---

## 👤 Author

[BENHIMA Mohamed-amine]  
Under supervision of Prof. Azeddine KHIAT  
Academic Year 2025/2026