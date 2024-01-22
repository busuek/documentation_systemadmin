>Node exporter

1. Download node-exporter
```
wget \
  https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
```
2. Create user 
```
sudo groupadd -f node_exporter
sudo useradd -g node_exporter --no-create-home --shell /bin/false node_exporter
sudo mkdir /etc/node_exporter
sudo chown node_exporter:node_exporter /etc/node_exporter
```
3. Unpacking Node Exporter Binary
```
tar -xvf node_exporter-1.0.1.linux-amd64.tar.gz
mv node_exporter-1.0.1.linux-amd64 node_exporter-files
```
4. Installation of Node Exporter
```
sudo cp node_exporter-files/node_exporter /usr/bin/
sudo chown node_exporter:node_exporter /usr/bin/node_exporter
```
5. Setup Node Exporter Service
- Create a node_exporter service file.
```
sudo vi /usr/lib/systemd/system/node_exporter.service
```
- Add the following configuration
```
[Unit]
Description=Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
ExecStart=/usr/bin/node_exporter \
  --web.listen-address=:9200

[Install]
WantedBy=multi-user.target
```
6. Reload systemd and Start Node Exporter
```
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```
