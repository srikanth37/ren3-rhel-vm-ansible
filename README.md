# 🛠️ Ren3 Deployment & Update Automation (Ansible)

![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

This repository contains Ansible playbooks and roles to deploy and update the **Ren3 components**:

- ✅ Web (Frontend)
- ✅ Server (Backend API)
- ✅ Ingestor (Data Collector)
- ✅ EJ2 API Services
- ✅ Nginx
- ✅ Mariadb
- ✅ Quadrant
- ✅ Telegraf
- ✅ Prometheus
- ✅ Alert Manager
- ✅ Grafana
---

## 📋 Prerequisites

VM Prerequisites:

```bash
CPU: 2core
RAM: 8GB
Disk: 50GB root disk, 50GB secondary disk
```

Ensure the following are in place before using these playbooks:

1. Rhel instance with access to run deployment scripts
2. Instance should have **Internet connectivity**.
3. Instance should have a secondary disk storage of 50 GB atleast attached to it and mounted on /opt path
4. You should have valid **domain names and SSL certificates** for frontend and backend.

## 🌐 Network Configuration

Ensure your EC2 instances allow the following **inbound and outbound** traffic:

| Service             | Port |
|---------------------|------|
| SSH                 | 22   |
| Web (Frontend)      | 3000 |
| Server (Backend)    | 5000 |
| Ingestor            | 5001 |
| EJ2 API Services    | 6003 |
| Quadrant            | 6333 |
| HTTP                | 80   |
| HTTPS               | 443  |
| Prometheus          | 9090 |
| Grafana             | 8080 |
| Alert Manager       | 9094 |
| Telegraf            | 9273 |


🔄 Also ensure outbound traffic is allowed for updates and communication with external services.

## 🛠️ Setup Instructions

### 1. 🔥 Download deployment zip

Run the following command on your instance to download the deployment zip

```bash
wget https://apps.ren3.ai/downloads/rhel/installer/ren3-rhel.zip
```
---

Run the following command to unzip the deployment zip

```bash
unzip ren3-rhel.zip
```
---

### 2. 🔧 Update config.json  ren3ssl.crt ren3ssl.key files

- Modify `config.json` to change the domain names, mariadb database password
- Add product license key in the `config.json` file
- Add ssl certificate and key for your domain in files `ren3ssl.crt` `ren3ssl.key`

---

### 3. 🔥 Disable Firewalld

To avoid connectivity issues during deployment, stop `firewalld`:

```bash
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```
---

### 3. 🔥 Run the script for setup

Run the following bash script `run-deploy.sh` to install all the components

```bash
./run-deploy.sh
```
---

## 🔄 Updating Components

Run the following playbooks to update components with the latest builds by going inside the directory `ren3-rhel-vm-ansible`

# Update Web

```bash
ansible-playbook -i inventory.ini playbooks/update-web.yml
```

# Update Server

```bash
ansible-playbook -i inventory.ini playbooks/update-server.yml
```

# Update Ingestor

```bash
ansible-playbook -i inventory.ini playbooks/update-ingestor.yml
```

# Update EJ2 API Services

```bash
ansible-playbook -i inventory.ini playbooks/update-ej2apiservices.yml
```

## 🔐 License the Product

On application UI if you are getting a message like `product is not activated`, activate your Ren3 license from the server instance using this command

```bash
curl -X POST http://localhost:5000/license/register \
  -H "Content-Type: application/json" \
  -d '{"licenseKey": "your_license_key"}'
```
Replace "your_license_key" with the actual license string provided to you.
💡 This step is mandatory to unlock full product functionality.


## 🔐 Add Grafana dashboard and change default password for access

To add the EC2, Services, Mariadb, Quadrant monitoring dashboard in the Grafana please import the dashboard.json file

To access the grafana use default user/password admin:admin. once logged-in changed the password


## 🔐 Prometheus metrics and alerts

In this setup we have used Prometheus for metrics and Prometheus Alert Manager for alerts creation.

Telegraf is used to generate the metrics and send it to Prometheus

Metrics:

1. Instance metrics (CPU, Memory, Disk)
2. Services Health metrics
3. Mariadb metrics
4. Quadrant metrics


Alerts:

1. Services Down alerts
2. High CPU and Memory alerts

## To check services are running fine

```bash
sudo service rn3bp-web status
sudo service rn3bp-server status
sudo service rn3bp-ingestor status
sudo service rn3bp-ej2apiservices status
sudo service telegraf status
sudo service prometheus status
sudo service alertmanager status
sudo service grafana status
```


## 🙋‍♂️ Support

```bash
If you encounter issues or need help with customization, reach out to your infrastructure or DevOps team.
```
