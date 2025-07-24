# 🛠️ ELK Stack Setup on Ubuntu Server 22.04.5 (VMware)

This guide provides full instructions to install and configure the **ELK Stack (Elasticsearch, Logstash, Kibana)** on **Ubuntu Server 22.04.5** in a VMware environment.

---

## 🧰 System Requirements

- OS: Ubuntu Server 22.04.5 (Virtualized in VMware)
- RAM: 4 GB minimum (8 GB recommended)
- Disk: 40–80 GB
- Access: sudo/root privileges

---

## ⚙️ Step-by-Step Installation

### 🔁 1. Update the System

```bash
sudo apt update && sudo apt full-upgrade -y
🔐 2. Add Elastic GPG Key
bash
Copy
Edit
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
📥 3. Add Elastic APT Repository
bash
Copy
Edit
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update
📦 4. Install Elasticsearch
bash
Copy
Edit
sudo apt install -y elasticsearch
Configure:
bash
Copy
Edit
sudo nano /etc/elasticsearch/elasticsearch.yml
Uncomment/modify:

yaml
Copy
Edit
network.host: 0.0.0.0
http.port: 9200
Enable & Start:
bash
Copy
Edit
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
🖥️ 5. Install Kibana
bash
Copy
Edit
sudo apt install -y kibana
Configure:
bash
Copy
Edit
sudo nano /etc/kibana/kibana.yml
Uncomment:

yaml
Copy
Edit
server.host: "0.0.0.0"
Enable & Start:
bash
Copy
Edit
sudo systemctl enable kibana
sudo systemctl start kibana
sudo systemctl status kibana
🧱 6. Install Logstash
bash
Copy
Edit
sudo apt install -y logstash
Enable & Start:
bash
Copy
Edit
sudo systemctl enable logstash
sudo systemctl start logstash
sudo systemctl status logstash
🔑 7. Generate Kibana Encryption Keys
bash
Copy
Edit
cd /usr/share/kibana/bin/

./kibana-encryption-keys generate

./kibana-keystore add xpack.security.encryptionKey
./kibana-keystore add xpack.reporting.encryptionKey
./kibana-keystore add xpack.encryptedSavedObjects.encryptionKey

sudo systemctl restart kibana
🔥 8. Configure UFW (Firewall)
bash
Copy
Edit
sudo ufw allow 5601/tcp  # Kibana
sudo ufw allow 9200/tcp  # Elasticsearch
sudo ufw allow 5044/tcp  # Logstash (for Beats)
sudo ufw disable         # Or keep it disabled if not needed
🌐 9. Access ELK Web Interfaces
Elasticsearch: https://<VM-IP>:9200

Kibana: http://<VM-IP>:5601

🧾 10. Retrieve Usernames and Passwords
bash
Copy
Edit
# Kibana system user
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana_system

# Elastic superuser
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
🪪 11. Create Enrollment Token for Kibana
bash
Copy
Edit
cd /usr/share/elasticsearch/bin/
sudo ./elasticsearch-create-enrollment-token --scope kibana
Enter this token on the Kibana web interface.

✅ 12. Get Kibana Verification Code
bash
Copy
Edit
cd /usr/share/kibana/bin/
./kibana-verification-code
Enter this OTP on the Kibana screen.

