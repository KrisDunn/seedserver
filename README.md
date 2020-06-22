# Seedserver
Docker Compose file for a VPN protected seed server. Seedserver uses the following images:

* watchtower: https://hub.docker.com/r/v2tec/watchtower
* portainer:  https://hub.docker.com/r/portainer/portainer/
* heimdall: https://hub.docker.com/r/linuxserver/heimdall
* transmission VPN: https://hub.docker.com/r/haugene/transmission-openvpn
* jackett: https://hub.docker.com/r/linuxserver/jackett
* radarr: https://hub.docker.com/r/linuxserver/radarr
* sonarr: https://hub.docker.com/r/linuxserver/sonarr
* bazarr: https://hub.docker.com/r/linuxserver/bazarr

## Getting Started
To get started with Seedserver follow the quickstart guide below. For more detailed configuration of any of the images, please refer to the links provided.

### Prerequisits
In order to follow this guide docker and docker-compose must be installed on your host. The following host operating systems have been tested:

* Ubuntu Server 20.04
* MacOS 10.13 or above

In addition, a VPN service is required. Refer to the haugene/transmission [documentation](https://haugene.github.io/docker-transmission-openvpn/) for supported providers and options. 

### Modify the .ENV file
The .ENV contains variables used in the docker-compose.yaml file. Modify the .ENV file to reflect your local environment.

#### ENV variables
The variables below are available in the .ENV file. 
```bash
HOST=[host_ip]
MOUNT_POINT=[seedserver_path]
CONFIG_POINT=[docker_home]
VPN_PROVIDER=[provider_name]
VPN_CONFIG=[additional_configuration]
VPN_USERNAME=[username]
VPN_PASSWORD=[password]
TRANSMISSION_USERNAME=[username]
TRANSMISSION_PASSWORD=[password]
PUID=[puid]
PGID=[guid]
TZ=/etc/localtime:/etc/localtime:ro
```

#### ENV example file
```bash
HOST=192.168.0.10
MOUNT_POINT=/mnt/external_drive
CONFIG_POINT=~/docker
VPN_PROVIDER=IPVANISH
VPN_CONFIG=ipvanish-UK-London-lon-a01,ipvanish-UK-Manchester-man-c01,ipvanish-UK-London-lon-a02,ipvanish-UK-Manchester-man-c02
VPN_USERNAME=user@email.com
VPN_PASSWORD=secretpassword
TRANSMISSION_USERNAME=transmissionuser
TRANSMISSION_PASSWORD=secretpassword
PUID=1000
PGID=1000
TZ=/etc/localtime:/etc/localtime:ro
```

### Run docker compose
Run the docker compose command to bring up the environment:

```bash
docker-compose up -d
```

Applications can be accessed from the host IP uisng the ports listed:

| Application  | Port |
|--------------|------|
| heimdall     | 80   |
| portainer    | 9000 |
| transmission | 9001 |
| jackett      | 9002 |
| radarr       | 9003 |
| sonarr       | 9004 |
| bazarr       | 9005 |

## Notes
While the quickstart guide will bring up the environment quickly, there are many more options available in the images which can be used to better suit your environment.

### docker-compose.yaml
```bash
- RUN_OPTS=ProxyConnection=${HOST}:8888
```
This variable will proxy the jackett container through the tinyproxy running in the transmission-openvpn container. Many UK ISPs block access to trackers so this allows connections to be made over VPN. There are notable issues with some trackers which prevent this from being a viable solution. It is better to configure proxy settings directly within jackett. This variable has been left for those that need it. 

