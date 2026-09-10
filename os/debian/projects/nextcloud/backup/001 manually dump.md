# overview
```
docker volume ls

docker volume inspect nextcloud_nextcloud_data

docker volume inspect nextcloud_db_data
```
# database backup
```
docker compose exec -T db pg_dump \
  -U nextcloud \
  -d nextcloud \
  > backups/nextcloud-db.sql

ls -lh backups/nextcloud-db.sql
```

# nextcloud backup
```
docker run --rm \
  -v nextcloud_nextcloud_data:/source:ro \
  -v "$PWD/backups":/backup \
  alpine \
  tar czf /backup/nextcloud-files.tar.gz -C /source .
```










# database restore

```
cat backups/nextcloud-db.sql | docker compose exec -T db psql \
  -U nextcloud \
  -d nextcloud
```















