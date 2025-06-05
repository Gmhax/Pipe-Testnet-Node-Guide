# Pipe-Testnet-Node-Guide
Pipe Network is a decentralized Content Delivery Network (CDN) built on Solana's blockchain. It allows permissionless contributors to deploy Points of Presence (PoPs) in specific regions, providing fast access to high-quality media and real-time applications through a secure and scalable solution.


## System Requirements
- 4 CPU Cores
- 16GB+ RAM
- SSD storage with 100GB+ Available space
- 1Gbps+ network connection

## Pre-Requirements
- You have to be Whitelist, if u have to run the Pipe testnet node-
- If U alraedy rcvd the Mail then u can run the Pipe testnet
- Fill this form if u have not rcvd the mail/invite code Yet- https://tinyurl.com/4435uukt

## Install Require Dependecies
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```

## Allow Incoming connections on Ports
```bash
sudo ufw allow 22
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow ssh
sudo ufw enable
sudo ufw reload
```

## Applying Configurations
- To apply these settings, create a configuration file using these following command:
```bash
sudo bash -c 'cat > /etc/sysctl.d/99-popcache.conf << EOL
net.ipv4.ip_local_port_range = 1024 65535
net.core.somaxconn = 65535
net.ipv4.tcp_low_latency = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
EOL'
sudo sysctl -p /etc/sysctl.d/99-popcache.conf
```

- Increase file limits for performance
```
sudo bash -c 'cat > /etc/security/limits.d/popcache.conf << EOL
*    hard nofile 65535
*    soft nofile 65535
EOL'
```

## Create Directories
```bash
sudo mkdir -p /opt/popcache
sudo mkdir -p /opt/popcache/logs
```

## Download the binary
- Open https://download.pipe.network/ 
- Enter Invite Code that u have rcvd from pipe Mail
- Download version 0.3.2

## how u can move pop binary to vps
- Download the pop binary in your local pc
- Open the termius application and move to SFTP
- Now, On your right screen - select the VPS which u want to send the file, left screen will be your local disk
- Drag and drop your pop binary file to your Left screen To Right VPS's Home path.
- Now move & Unrap the pop file with following these commands!
```bash
sudo mv ~/pop-v0.3.1-linux-x64.tar.gz /opt/popcache/
```
```bash
cd /opt/popcache/
```
```bash
sudo tar -xzf pop-v0.3.1-linux-x64.tar.gz
sudo chmod +x ./pop
chmod 777 pop-v0.3.1-linux-x64.tar.gz
sudo ln -sf /opt/popcache/pop /usr/local/bin/pop
```
## Set Configuration File
```bash
sudo nano config.json
```
```bash
{
  "pop_name": "your-pop-name",
  "pop_location": "Your Location, Country",
  "invite_code": "Enter your Invite Code",
  "server": {
    "host": "0.0.0.0",
    "port": 443,
    "http_port": 80,
    "workers": 0
  },
  "cache_config": {
    "memory_cache_size_mb": 4096,
    "disk_cache_path": "./cache",
    "disk_cache_size_gb": 100,
    "default_ttl_seconds": 86400,
    "respect_origin_headers": true,
    "max_cacheable_size_mb": 1024
  },
  "api_endpoints": {
    "base_url": "https://dataplane.pipenetwork.com"
  },
  "identity_config": {
    "node_name": "your-node-name",
    "name": "Your Name",
    "email": "your.email@example.com",
    "website": "https://your-website.com",
    "discord": "your_discord_username",
    "telegram": "your_telegram_handle",
    "solana_pubkey": "YOUR_SOLANA_WALLET_ADDRESS_FOR_REWARDS"
  }
}
```
- Paste the following code in config.json File-
- Dont Forget to make all these changes in File! All are Important Configrations!
- "pop_location": Your VPS Region's Location
- "memory_cache_size_mb": How much RAM memory u want to allocate to node! (In MB)....Like if u have 16 GB RAM then u can allocate 50-60% of total RAM!
- "disk_cache_size_gb": How much disk space do u want to allocate! (In GB) ....like if u have 500 GB free space in your disk then u can allocate around 200GB

## Create a Systemd Service File
```bash
sudo bash -c 'cat > /etc/systemd/system/popcache.service << EOL
[Unit]
Description=POP Cache Node
After=network.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/popcache
ExecStart=/opt/popcache/pop
Restart=always
RestartSec=5
LimitNOFILE=65535
StandardOutput=append:/opt/popcache/logs/stdout.log
StandardError=append:/opt/popcache/logs/stderr.log
Environment=POP_CONFIG_PATH=/opt/popcache/config.json

[Install]
WantedBy=multi-user.target
EOL'
```
- Reload
```bash
sudo systemctl daemon-reload
```
- Enable
```
sudo systemctl enable popcache
```
- Start popcache
```
sudo systemctl start popcache
```
## Managing Logs
- Check node Status
```bash
sudo systemctl status popcache
```

```bash
tail -f /opt/popcache/logs/stdout.log
```

## Get pop_id & more info
```bash
curl -sk https://localhost/metrics | jq . && curl -sk https://localhost/state | jq . && curl -sk https://localhost/health | jq .
```
## Dashboard
https://dashboard.testnet.pipe.network/node/
- Add your pop id at the end of the link to get info about your node-




------------------------------------------------------------------------------------------------------------------------------------------

# Upgrade to new release/version (v0.3.2) {Vps}

- Stop popcache service
```
sudo systemctl stop popcache
```
- Delete old binary
```
sudo rm -f /usr/local/bin/pop
sudo rm -f /opt/popcache/pop-v0.3.0-linux-x64.tar.gz
```

- Delete old logs data
```
sudo rm ~/opt/popcache/logs/stderr.log
sudo rm ~/opt/popcache/logs/stdout.log
```

- Now follow the Download the binary process:


















































                        


  














                        


































