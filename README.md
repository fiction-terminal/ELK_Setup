# ELK Stack Setup on Ubuntu Server 22.04.5 (VMware)
This guide helps you install and configure the **ELK Stack (Elasticsearch, Logstash, Kibana)** on **Ubuntu Server 22.04.5** running in **VMware**.


---

## 📦 System Requirements

- OS: Ubuntu Server 22.04.5 (VMware or any virtualization)
- RAM: Minimum 4 GB (8 GB recommended)
- Disk: Minimum 40 GB (80 GB recommended)
- Access: sudo or root

---


# Step-by-Step Installation

## 1-> Update the system
```
sudo apt update && sudo apt full-upgrade -y
```

## 2-> Add Elastic GPG Key
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

## 3-> Add Elastic APT Repository
```
sh -c 'echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list'
```

## 4-> Install Elasticsearch
```
sudo apt install -y elasticsearch
```

## 5-> Configure, Enable, Start, Check Status of elasticsearch
- Uncomment server.host(0.0.0.0) & port
```
sudo nano /etc/elasticsearch/elasticsearch.yml
```



```
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
```

## 6-> Install Kibana
```
sudo apt install -y kibana
```

## 7-> Configure, Enable, Start, Check Status of kibana
- Uncomment Server host(0.0.0.0) & port
```
sudo nano /etc/kibana/kibana.yml
```
```
sudo systemctl enable kibana
sudo systemctl start kibana
sudo systemctl status kibana
```

## 8-> Install Logstash
```
sudo apt install -y logstash
```

## 9-> Enable, Start, Check status of logstash
```
sudo systemctl enable logstash
sudo systemctl start logstash
sudo systemctl status logstash
```

## 10-> Generate Kibana-encryption-keys
```
cd /usr/share/kibana/bin/
```
```
./kibana-encryption-keys generate
```
```
./kibana-keystore add xpack.security.encryptionKey
```
- >>Enter value for xpack.security.encryptionKey: *********************************
```
./kibana-keystore add xpack.reporting.encryptionKey
```
- >>Enter value for xpack.reporting.encryptionKey: *********************************
```
./kibana-keystore add xpack.encryptedSavedObjects.encryptionKey
```
- >>Enter value for xpack.encryptedSavedObjects.encryptionKey: *********************************
```
systemctl restart kibana
systemctl status kibana
```

## 11-> Check ufw status
```
ufw status
```
- >>if it's inactive than ok otherwise disable this
```
ufw disable
```
- >>if it's active than allow used ports
```
sudo ufw allow 5601/tcp  # Kibana
sudo ufw allow 9200/tcp  # Elasticsearch
sudo ufw allow 5044/tcp  # Logstash (for Beats)
```

## 12->Access the elasticsearch from browser of host or localhost of VMware
```
https://192.168.VM.IP:9200
```
- >>it will ask for username and passwd

## 13->Retrieve username and passwd for kibana
```
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana_system
```
- >>this will provide you username"kibana_system" with password"4_Z51fhDRUVnkuLxrO16"

## 14->Retrieve username and passwd for elastic
```
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```
- >>this will provide you username"elastic" with password"Ntbl6ZB*FT9J7CUDROT_"

## 15->Now acces kibana through web browser
```
http://192.168.VM.IP:5601
```
- >>this will ask for the enrollment token

## 16->for Create enrollment token 
```
cd /usr/share/elasticsearch/bin/
```
```
sudo ./elasticsearch-create-enrollment-token --scope kibana
```
- >>in my case this will something like-- eyJ2ZXIiOiI4LjE0LjAiLCJhZHIiOlsiMTkyLjE2OC4zMy4xNDM6OTIwMCJdLCJmZ3IiOiI5YmU5ZGVhZjExOTU0ZDJhNDYzNzgxMmVkYjgyODRmOWNlNDQxNTNjODU5ODMzYzkyMjMxNWRlMDg0OGExMzZjIiwia2V5IjoiSUxwTVBKZ0JsaUxoUEpnR2t4Ulg6aUhpbDM4Ul9KVmwtdFEwcTRla1FyQSJ9>>

>>after enter this code into kibana this will ask for verification

## 17->for generate the verification otp
```
cd /usr/share/kibana/bin/
```
```
./kibana-verification-code
```
- >>in my case the verification code is-- 369 294

- >>after that this will ask for username & passwd of elastic, So Enter...from step 4
thats it:-)
