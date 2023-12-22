# Vaultwarden

These containers manage a secure password storage system using Vaultwarden. The "vaultwarden" container functions as the main server for password storage and access, while the "vaultwarden-backup" container ensures regular backups of this data, maintaining its security and integrity.

```yaml
version: '3'

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    volumes:
      - ./vw-data/:/data/
    ports:
      - 8080:80

  vaultwarden-backup:
    image: bruceforce/vaultwarden-backup
    container_name: vaultwarden-backup
    restart: always
    volumes:
      - ./backup/:/backup/
    volumes_from:
      - vaultwarden
    environment:
      - CRON_TIME=0 0 * * *
      - BACKUP_DIR=/backup
      - BACKUP_ON_START=true
      - TIMESTAMP=true
      - DELETE_AFTER=2
      - UID=1000
      - GID=1000
```
