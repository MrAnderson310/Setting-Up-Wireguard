# Setting-Up-Wireguard

## Configuring Droplet and Setting up for Mobile
1. Using the link provided create a digital ocean account to get $200 credit for 2 months
2. Create a droplet on your new account and choose an ubuntu version as a basic type with a password
3. Next using your terminal you will need to run 
```
root@ipaddress
```
4. input the password you provided when creating the droplet
5. Now run these three commands
```
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml
```
6. Next paste this 
```
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
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
7. Start up WireGuard by pasting in this 
```
cd ~/wireguard/
docker-compose up -d
```
8. Next paste this 
```
docker-compose logs -f wireguard
```
9. On your phone download the WireGuard app and launch it
10. Click on the plus in the bottom right and select create from QR code
11. Scan the QR code on your terminal and flick the switch to on

## Configuring for computer 

1. First you will need to install wireguard on your computer to do this go to wireguard's website and download the version specific to your hardware
2. Next you will need to copy over your configuration file from the droplet to your local computer.
		1. First you will need to find the file's name
		2. Keep repeating the `ls` and `cd` commands to search around for a file that ends in `.conf`
		3. Now on your local machine run this command 
				
```
scp root@YourIpAddress:wireguard/config/wg_confs/wg0.conf /Users/UserAccountName/Desktop
```
3. Now you should have your .conf file on your desktop
4. Now open up the wireguard application and click the `+` in the bottom left and click `import tunnel from file`
5. Now select the .conf file and click activate
6. Now go to `ipleak.net` and make sure you ip is now the same as your droplet
