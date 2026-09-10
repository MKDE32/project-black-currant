# create folder
```
sudo mkdir -p /var/backups/nextcloud
sudo chown mkde:mkde /var/backups/nextcloud
sudo chmod 700 /var/backups/nextcloud
```
```
sudo nano /usr/local/sbin/nextcloud-backup
```
# the script
```
#!/bin/bash

set -euo pipefail

BACKUP_ROOT="/var/backups/nextcloud"
COMPOSE_DIR="/home/mkde/nextcloud"
DATE=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_DIR="$BACKUP_ROOT/$DATE"

echo "=== Nextcloud Backup ==="
echo "Start: $(date)"

mkdir -p "$BACKUP_DIR"

cd "$COMPOSE_DIR"

echo "[1/3] PostgreSQL dump..."

docker compose exec -T db pg_dump \
    -U nextcloud \
    -d nextcloud \
    > "$BACKUP_DIR/nextcloud-db.sql"

echo "[2/3] Nextcloud-Dateien..."

docker run --rm \
    -v nextcloud_nextcloud_data:/source:ro \
    -v "$BACKUP_DIR":/backup \
    alpine \
    tar czf /backup/nextcloud-files.tar.gz -C /source .

echo "[3/3] Alte Backups löschen..."

find "$BACKUP_ROOT" \
    -mindepth 1 \
    -maxdepth 1 \
    -type d \
    -mtime +7 \
    -exec rm -rf {} \;

echo "Backup abgeschlossen: $BACKUP_DIR"
echo "Ende: $(date)"
```

```
sudo chmod 750 /usr/local/sbin/nextcloud-backup
```

# testing

```
sudo /usr/local/sbin/nextcloud-backup
```
```
sudo ls -lh /var/backups/nextcloud/
```



















