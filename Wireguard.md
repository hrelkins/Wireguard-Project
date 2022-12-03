# Wireguard VPN Server Using Digital Ocean Droplets

Wireguard Install Guide here <https://thematrix.dev/setup-wireguard-vpn-server-with-docker/>

## Creating A Droplet 

I created an Ubuntu 20.04 with Docker pre-installed droplet using Digital Ocean. 

## Wireguard Setup 

I ran the following commands to setup Wireguard: 

```bash
mkdir wireguard
mkdir wireguard/config
cd wireguard
nano docker-compose.yml
```
I then used the following for the yml file: 

```bash
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America
      - SERVERURL=147.182.244.236
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```

## Starting Wireguard

While in the Wireguard directory I ran the command 

```bash
sudo docker compose up -d 
```
Notice there is no `-` between the words docker and compose. My version did not accept it, your version may differ. 

Wireguard should now be created. 

## Connecting Phone to Wireguard 

To display the QR codes I used the following command: 
```bash
docker logs wireguard
```
I scanned the QR code and was able to create a Wireguard Tunnel to connect my phone. 

## Connecting PC/Laptop to Wireguard

Navigating to the information needed I used the following commands in order: 
```bash
wireguard ls
cd config/
cd peer_pc1/
ll
cat peer_pc1.conf
```

I then copied the Interface and Peer information to create a Wireguard Tunnel on my PC. 

