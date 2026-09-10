```
docker compose exec nextcloud php occ app:disable eurooffice
docker compose exec nextcloud php occ app:remove eurooffice
docker compose exec nextcloud php occ app:list | grep -E 'office|richdocuments|euro'
```
Der eigentliche Collabora-Connector fehlt.
richdocuments ist der WOPI-Connector




```
docker compose exec nextcloud php occ app:install richdocuments
docker compose exec nextcloud php occ app:enable richdocuments
docker compose exec nextcloud php occ app:list | grep -E 'office|richdocuments'
```

```
docker compose exec nextcloud php occ richdocuments:activate-config
docker compose exec nextcloud php occ config:app:set richdocuments wopi_url --value=http://collabora:9980
docker compose exec nextcloud php occ config:app:set richdocuments public_wopi_url --value=http://192.168.178.25:9980
docker compose exec nextcloud php occ config:app:set richdocuments wopi_callback_url --value=http://nextcloud

docker compose exec nextcloud php occ config:app:get richdocuments wopi_url
docker compose exec nextcloud php occ config:app:get richdocuments public_wopi_url
docker compose exec nextcloud php occ config:app:get richdocuments wopi_callback_url
```
