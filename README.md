## DevOps All-in-One Monitoring Sandbox

A local DevOps sandbox environment built on Ubuntu (inside VirtualBox) for learning and experimenting with monitoring and alerting tools.

This sandbox includes:

Prometheus – metrics collection

Node Exporter – system metrics exporter

Grafana – dashboards & visualization

Alertmanager – alert delivery (e-mail, etc.)

## 🎯 Objective

Provision a VM with a pre-installed monitoring stack

Pre-configure alerts for CPU, disk, and service health

Provide dashboards and alert configs for easy learning

Share via GitHub for reproducibility
---

## 📂 Project Structure

devops-sandbox-monitoring/  
 ├── setup.sh  
 ├── prometheus.yml 
 ├── Project Report.pdf 
 ├── alerts/  
 │    └── grafana_cpu_alert.json  
 ├── dashboards/  
 │    └── node_exporter_dashboard.json  
 ├── screenshots/  
 │    └── (images of working dashboards & alerts)  
 └── README.md  

---

## 🚀 Tools Used

- Ubuntu VM (on VirtualBox)  
- Prometheus → for metrics collection  
- Node Exporter → for exposing system-level metrics (CPU, memory, disk)  
- Grafana → for visualization & dashboards  
- Alertmanager → for handling alerts (with email integration)  

---

## 🛠️ Installation Steps

Follow these steps to set up the sandbox manually (no Vagrant used here):

1. Update System  
   sudo apt update && sudo apt upgrade -y  

2. Install Node Exporter  
   wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.linux-amd64.tar.gz  
   tar xvf node_exporter-*.linux-amd64.tar.gz  
   sudo mv node_exporter-*/node_exporter /usr/local/bin/  

   Create systemd service file at /etc/systemd/system/node_exporter.service with:  
   [Unit]  
   Description=Node Exporter  
   After=network.target  

   [Service]  
   User=nodeusr  
   ExecStart=/usr/local/bin/node_exporter  

   [Install]  
   WantedBy=default.target  

   Then run:  
   sudo systemctl daemon-reexec  
   sudo systemctl start node_exporter  
   sudo systemctl enable node_exporter  

3. Install Prometheus  
   wget https://github.com/prometheus/prometheus/releases/download/v*/prometheus-*.linux-amd64.tar.gz  
   tar xvf prometheus-*.linux-amd64.tar.gz  
   sudo mv prometheus-*/prometheus /usr/local/bin/  
   sudo mv prometheus-*/promtool /usr/local/bin/  
   sudo mv prometheus-*/consoles /etc/prometheus  
   sudo mv prometheus-*/console_libraries /etc/prometheus  
   sudo mv prometheus.yml /etc/prometheus/prometheus.yml  

   Create systemd service file at /etc/systemd/system/prometheus.service with:  
   [Unit]  
   Description=Prometheus  
   After=network.target  

   [Service]  
   User=prometheus  
   ExecStart=/usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/  

   [Install]  
   WantedBy=multi-user.target  

   Then run:  
   sudo systemctl daemon-reexec  
   sudo systemctl start prometheus  
   sudo systemctl enable prometheus  

4. Install Grafana  
   sudo apt-get install -y apt-transport-https software-properties-common wget  
   wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -  
   sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"  
   sudo apt-get update  
   sudo apt-get install grafana -y  

   Start Grafana:  
   sudo systemctl start grafana-server  
   sudo systemctl enable grafana-server  

5. Install Alertmanager  
   wget https://github.com/prometheus/alertmanager/releases/download/v*/alertmanager-*.linux-amd64.tar.gz  
   tar xvf alertmanager-*.linux-amd64.tar.gz  
   sudo mv alertmanager-*/alertmanager /usr/local/bin/  

   Create systemd service file at /etc/systemd/system/alertmanager.service with:  
   [Unit]  
   Description=Alertmanager  
   After=network.target  

   [Service]  
   User=alertmanager  
   ExecStart=/usr/local/bin/alertmanager --config.file=/etc/alertmanager/alertmanager.yml  

   [Install]  
   WantedBy=multi-user.target  

   Then run:  
   sudo systemctl daemon-reexec  
   sudo systemctl start alertmanager  
   sudo systemctl enable alertmanager  

---

## ⚡ Usage

- Access Prometheus → http://localhost:9090  
- Access Grafana → http://localhost:3000 (default login: admin/admin)  
- Access Alertmanager → http://localhost:9093  

In Grafana:  
- Import dashboards from dashboards/ folder  
- Configure Prometheus as a data source (http://localhost:9090)  
- Create alert rules (example JSON available in alerts/)  

---

## 📧 Alerts

- Alerts for CPU usage, disk usage, and service health are configured.  
- Grafana alert rules (like grafana_cpu_alert.json) can be imported in Alerting → Alert Rules → Import.  
- Email alerts can be configured in Contact Points in Grafana.  

---

## 🔄 Service Management

To manage services manually:

- Start a service → sudo systemctl start <service-name>  
- Stop a service → sudo systemctl stop <service-name>  
- Enable auto-start on boot → sudo systemctl enable <service-name>  
- Disable auto-start → sudo systemctl disable <service-name>  
- Check if enabled → systemctl is-enabled <service-name>  

Examples:  
sudo systemctl start prometheus  
sudo systemctl stop grafana-server  
systemctl is-enabled node_exporter  

---

## 📸 Screenshots

Screenshots of dashboards and alerts are available in the screenshots/ folder.  



---


