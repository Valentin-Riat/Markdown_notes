# Jellyfin on docker on a NAS

## To do to setup

 - Open port on the router

[Tuto for setting up jellyfin on a Synology NAS with hardware acceleration](https://www.wundertech.net/how-to-set-up-jellyfin-on-a-synology-nas/)

## Activate hardware acceleration

To know what CPU architecture is in the NAS : https://kb.synology.com/en-global/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have
To know what CPU architecture supports what CODEC for HW accel : https://en.wikipedia.org/wiki/Intel_Quick_Sync_Video



# Video encoding

## General info
H.264 (=AVC)  : is the mode wide spread encoding for streaming because high quality with low bit rate
H.265 (=HEVC) : is the successor to H.264. It's about 2x more efficient (half the bit rate for the same quality)

for max 

## fiddleing with the transcoding settings

    Handbrake recomandation : 
    -  RF 18-22 for 480p/576p Standard Definition
    -  RF 19-23 for 720p High Definition
    -  RF 20-24 for 1080p Full High Definition
    -  RF 22-28 for 2160p 4K Ultra High Definition


For OSS117(2009) (x264 3MB/s) : 
 - no difference between medium - fast - veryfast
 - Fast is the one with the smallest size
 - no difference between RF20 - RF18
 - RF24 is a little bit worst

For Jurassic park (VC1 4.5MB/s) (good quality but grainy)
 - no difference between medium - veryfast
 - RF20 > RF23 but no difference between RF20 and RF18



## Handbrake Software

Used to encode a file 

## MKVToolNix Software

Used to modify .MKV files. useful to modify subtitles

## MakeMKV Software

Used to rip DVD/Blu-ray into mkv files


# Hardware

NAS : synology DS718+  (supports harware acceleration for H.264 and H.265)
> with 2 HDD : WD80EFAX-68KNBN0 (8To, 6Gb/s) in RAID1

TV Val : UE46F8080ST (only supports H.264)
TV GE cave : QE75Q7FAMTXZG (supports H.264 and H.265)
TV GE salon : UE55HU8580Q (supports H.264 and H.265)

# Setting up the Jellyfin server on the NAS

Docker is used to setup the jellyfin server as well as the certbot service (To automatically get and renew the SSL/TLS certificate for HTTPS)

The jellyfin config is pretty straight forward. The only notable things are that the media folder is read only (for security) and the "devices" are mounted. These devises are the drivers for the hardware acceleration of the transcoding.

# Internet and networking

## Setting up HTTPS

Setting up HTTPS can be summurized at getting a SSL/TLS certificate, because after that, it's as simple as flipping a switch.
Here is how to get a certificate.

### Domain name
Fist a domaine name is needed. The (ISP) internet service provider (e.g. Swisscom) or the NAS manufacturer (e.g. Synology) can probably provide this for free using the DDNS (Dynamic DNS) feature.

### SSL Certificate
To allow SSL/TLS encryption and autentication, a certificate signed by a CA (certification authority) is needed.

This certificate links your domain name (e.g. something.internet-box.ch) to the computer on which your server runs. Before producing a certificate, the CA will probe your connection to verify that you really possess the domain name that you clame.

For this the best option is using Certbot that will automatically get and renew the certificate. The CA used is Let's Encrypt which is a non-profit that delivers free certificates. Certbot is installed using docker compose. This docker image will run a script, made by me, that runs the command to ask for a certificate, put it in a file, convert it to a file format that Jellyfin can read, wait for 12h and restarts.

The dockers are setup so that the folder in which certbot puts the certificate is also accessible by Jellyfin. In the network settings of Jellyfin, the path of the certificate file can be provided.

As certbot will need to listen to the CA on port 80 (standard HTTP port), no other process must use or listen to port 80. On synology NAS, this port used to redirect to this UI of the NAS. to disable that, the following command needs to be run on the NAS :

**How to disable Synology NAS from listening to port 80**
``` shell
sudo sed -i -e 's/80/81/' /usr/syno/share/nginx/server.mustache /usr/syno/share/nginx/DSM.mustache /usr/syno/share/nginx/WWWService.mustache && sudo systemctl restart nginx
```

It is also do-able with a script that can re-run the script if the process restarts, for exemple after an update:
``` shell

if grep -e 80  /usr/syno/share/nginx/server.mustache /usr/syno/share/nginx/DSM.mustache /usr/syno/share/nginx/WWWService.mustache; then
echo "Value will be changed"
sudo sed -i -e 's/80/81/' /usr/syno/share/nginx/server.mustache /usr/syno/share/nginx/DSM.mustache /usr/syno/share/nginx/WWWService.mustache && sudo systemctl restart nginx
else
    echo "Do nothing"
fi
```

**Docker compose YAML config file**
``` python
# Docker config for jellyfin on a synology NAS
# Hardware acceleration enabled

version: '3.5' # Version of docker compose to use
services: 
  # First service is the jellyfin server
  jellyfin:  
    image: jellyfin/jellyfin # name of the docker image used for this service
    container_name: jellyfin 
    network_mode: 'host' 
    volumes:
      - /volume1/docker/jellyfin/config:/config
      - /volume1/docker/jellyfin/cache:/cache    
      - /volume1/Multimedia/Jellyfin:/Media:ro   # Where the films are (:ro means read-only)
      - /etc/letsencrypt:/etc/letsencrypt:ro    # For retieving the SSL cert (:ro means read-only)
      - /var/lib/letsencrypt:/var/lib/letsencrypt:ro       # For retieving the SSL cert (:ro means read-only)
    restart: 'unless-stopped'
    environment:
      - TZ='Europe/Zurich'
    
    # the Devives are the drivers for the hardware acceleration 
    devices:
      - /dev/dri/renderD128:/dev/dri/renderD128
      - /dev/dri/card0:/dev/dri/card0
  # Second service is the certbot program that get and maintain the SSL certificate
  certbot:
    container_name: certbot
    image: certbot/certbot:latest
    network_mode: 'host'
    restart: unless-stopped
    ports:
      - 80:80  # Maps the port 80 of the computer to port 80 of the docker container
    # Runs the certonly command to get the certificate
    command: >-
             certonly --reinstall --standalone 
             --email valentinriat@gmail.com --agree-tos --no-eff-email
             -d riat.internet-box.ch
    # certonly :     Gets a certificate without intalling it on the webserver (in this case, there is no webserver)
    # --reinstall    Reinstalls the certificate if there already is one
    # --standalone   Port 80 will be used so that CA can check that your domain exists (no other server must use port 80 at that point)
    # --email        Sets the email address to be used for the certificate request
    # --agree-tos    Auto agrees the terms of services of the CA
    # --no-eff-email Don't share email with EFF (the foundation that created certbot)
    # -d             Domain name for which the certificate will be produced

    volumes:
      - /etc/letsencrypt:/etc/letsencrypt
      - /var/lib/letsencrypt:/var/lib/letsencrypt
```

To view and check the renewal dates for Let's Encrypt certificates, use sudo certbot certificates. To manually renew all certificates, run sudo certbot renew.

# Setting up fail2ban

Problem : the docker image (`linuxserver/fail2ban`) uses the `nf_table` version of the `iptables` and the synology DSM 7.2.1 uses `iptables-legacy`
=> solved using `crazymax/fail2ban` image instead.

Problem : when trying to ban, fail2ban uses REJECT which is not in the kernel
=> solved by changing to DROP in the action.d folder

Problem : fail2ban bans an ip but you can still connect with said ip
=> solved the chain in which fail2ban writes to ban must be INPUT if the docker network setup is 'host' (or if no docker) 

(might be neccessary to enable the firewall of the NAS so that fail2ban works)
(in the `jail.d/fellyfin.local` file the chain must be changed according to the networking mode of docker)

usefull fail2ban commands:

fail2ban-client status jellyfin 
fail2ban-client set jellyfin unbanip IPADDR
fail2ban-client set jellyfin banip IPADDR

# Setting up the VPN

For the Synology NAS, there already exists a first party app call VPN server to make a VPN server. Using this, the server is configured to use the OpenVPN protocol. The following options are used :

 - Dynamic Open VPN server : 10.8.0.1
 - Max connection number : 5
 - Max connection of an account : 3
 - Port : 1194 (Official OpenVPN port)
 - Protocol : UDP (Don't use TCP because of TCP meltdown effect)
 - Encryption AES-256-CBC
 - Authentication : SHA512
 - Mssfix option value : 1450
 - Enable compression on the VPN link : false (chose no to save NAS CPU, also it's deprecated)
 - Allow clients to acces server's LAN : True
 - Verify TLS auth key : False (enabling this one will make the VPN co more secure but will require a private key to be shared securelly between all clients)
 - Verify server CN : Enable (_Don't know if I should_)
 - Enable IPv6 server mode : False

Clicking on *Export Configuration* creates a archive containing instructions in installing the openVPN client.

The Swisscom router was configured to enable port forwarding between port 1194 to [Ip address of NAS]:1194



# Useful bash commands


``` shell
docker container logs [-f (follow) -t(show timestamp) --since] Container
docker export my-container > container-files.tar
docker exec -it <container-name> /bin/bash # get a new bash in the container and attach to it
netstat -tnul | grep -E "RegEx to fine the port you want e.g. :80"
grep -l "string" path1/* # prints the path of all files in path1/ where a line matches "string" 
sed -i -E -e 's/old_str/new_str/g' filnames # subsitute every occurence of old_str for new_str (old_str is a regex)
```
## Send files between two computer

Recieving computer :

``` shell
netcat -l 100 > FILENAME # listen on port 1000 and put what's revieved in FILNAME
```

Sending computer :

``` shell
netcat IP_ADDRESS 100 -q 0 < FILENAME # send the content of the file named FILENAME to the IP_ADDRESS, on port 100 and then quit the connetion
```
For windows, download nmap on its website and use the ncat command in the same manner (but don't include -q 0)

# TODO

 - [ ] Play with encoding to get best quality/file size ratio 
 - [x] Open connection to internet
    - [ ] Add double autentication to the NAS
    - [ ] Implement HTTPS (with let's encrypt)
    - [ ] Implement VPN to be able to manage it all from outside
 - [ ] Install Jellyfin on a TV
 - [ ] Add metrics to the server (see [this link](https://jellyfin.org/docs/general/networking/monitoring))




 Recommendations for safe implementation on internet

A)  Setup a reverse proxy (maybe useless unless something more is done on it, but maybe it's worth it for simpler SSL cert management) ***=> won't do it in the end***
B)  LetsEncrypt certs ***=> done with certbot on docker***
C)  Use a different port for https (configurable in the reverse proxy).  Preferably a port in the ephemeral range (49152-65535). ***=> used 658 (checked that it's unused on wikipedia, ephemeral range don't work with swisscom router)***
D)  Setup fail2ban
E)  Make the jellyfin right to read only where ever possible ***=> media lib is read-only***
F)  Setup ~reverse proxy~ NAS to only use latest encryption algorithms ***=>Change settings in security tab in NAS and checked with ssllabs***
[NNinx reverse proxy](https://jellyfin.org/docs/general/networking/nginx/) | [Apache revers porxy](https://jellyfin.org/docs/general/networking/apache/) | [Let's encrypt SSL cert](https://jellyfin.org/docs/general/networking/letsencrypt/) | [Disable weak cipher after all that](https://forum.jellyfin.org/t-apache-nginx-disable-weak-tls-ciphers)

G) Enable 2factor autentication for NAS 

[Link to test the safety of the SSL config of your server](https://www.ssllabs.com/ssltest/analyze.html)