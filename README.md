# Minecraft Server Setup

#### Install Dependencies and Download Server

```bash
sudo apt update
sudo apt install git build-essential

sudo apt install openjdk-8-jre-headless

sudo useradd -r -m -U -d /opt/minecraft -s /bin/bash minecraft

sudo su - minecraft

mkdir -p ~/{backups,tools,server}

cd ~/tools && git clone https://github.com/Tiiffi/mcrcon.git

cd ~/tools/mcrcon

gcc -std=gnu11 -pedantic -Wall -Wextra -O2 -s -o mcrcon mcrcon.c

cd /opt/minecraft/server/

wget https://launcher.mojang.com/v1/objects/35139deedbd5182953cf1caa23835da59ca3d7cd/server.jar

```

#### Enable RCON

```
rcon.port=25575
rcon.password=CHANGEPASSWORD
enable-rcon=true
```

There are many other things to setup in here also. Whitelisting, max players etc.

#### Setup Service

```
[Unit]
Description=Minecraft Server
After=network.target

[Service]
User=minecraft
Nice=1
KillMode=none
SuccessExitStatus=0 1
ProtectHome=true
ProtectSystem=full
PrivateDevices=true
NoNewPrivileges=true
WorkingDirectory=/opt/minecraft/server
ExecStart=/usr/bin/java -Xmx4G -Xms4G -jar server.jar nogui
ExecStop=/opt/minecraft/tools/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p CHANGEPASSWORD stop

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload # Reload daemons
sudo systemctl start minecraft # Start the service
sudo systemctl status minecraft # Check status
sudo systemctl enable minecraft # Enable as a service
```



