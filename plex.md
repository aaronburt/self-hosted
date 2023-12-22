# Plex

Plex is enabled with GPU support via NVIDIA-SMI bound to the host adapator


```yaml
---
version: "2.1"
services:
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: host
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - VERSION=docker
      - NVIDIA_VISIBLE_DEVICES=all
      - NVIDIA_DRIVER_CAPABILITIES=compute,video,utility
    volumes:
      - ./config:/config
      - /mnt/expansion/plex/tv:/tv
      - /mnt/expansion/plex/movies:/movies
    restart: unless-stopped
```