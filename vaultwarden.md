# Vaultwarden

These containers manage a secure password storage system using Vaultwarden. The "vaultwarden" container functions as the main server for password storage and access, while the "vaultwarden-backup" container ensures regular backups of this data, maintaining its security and integrity.

```yaml
version: '3'

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    user: 1000:1000
    restart: always
    volumes:
      - ./vw-data/:/data/
    ports:
      - 8080:80
    environment: 
      - SMTP_HOST= #smtp server
      - SMTP_FROM= #email
      - SMTP_PORT= #smtp port
      - SMTP_SECURITY= #force_ssl or starttls or off
      - SMTP_USERNAME= # email 
      - SMTP_PASSWORD= # email password
      - SIGNUPS_ALLOWED=false
      - SHOW_PASSWORD_HINT=true
      - ADMIN_TOKEN= #ARGON2 use $$ to string escape 

      # echo -n "secretPasswordForAdminToken" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4

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
