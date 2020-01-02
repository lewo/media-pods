## meida-pods
# 

## Replacing docker-compose with podman for linuxserver (plex, sonarr, sab, etc).

## Generate the pod file 
##
sudo podman pod create --name media-pod -p 8989 -p 9091 -p 32400 -p 32469 -p 5353 -p 1900

## Add the containers to the pod
#

## SABNZDB
##
sudo podman run -d --name sabnzbd --pod media-pod  -e PUID=1000 -e PGID=1000 -e TZ="Australia/Brisbane"  -v /home/leigh/dev/docker/config:/config:z -v /media/external/Downloads:/downloads:z -v /media/external/Incomplete:/incomplete-downloads:z linuxserver/sabnzbd

## Sonarr
##
sudo podman run -d --name sonarr --pod media-pod  -e PUID=1000 -e PGID=1000 -e TZ="Australia/Brisbane"  -v /home/leigh/dev/docker/config:/config:z -v /media/external/Downloads:/downloads:z -v /media/external/TV:/tv:z linuxserver/sonarr

## Plex
##
sudo podman run -d --name plex --pod media-pod  -e PUID=1000 -e PGID=1000 -e TZ="Australia/Brisbane"  -v /home/leigh/dev/docker/config:/config:z -v /media/external/Downloads:/downloads:z -v /media/external/TV:/tv:z -v /media/external/Movies:/movies linuxserver/plex

## Generate the podfiles
## 
sudo podman generate kube media-pod > media-pod.yaml

## Add the containers
## Create systemd services
sudo podman generate systemd -n -f media-pods
