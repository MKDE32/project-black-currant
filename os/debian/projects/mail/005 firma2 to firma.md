```
cd ~/mailcow-firma2

docker exec mailcowdockerized-postfix-mailcow-1 \
  bash -c 'timeout 5 bash -c "</dev/tcp/192.168.178.25/25" && echo SMTP_OK || echo SMTP_FAILED'

mkde@server:~/mailcow-firma2$ docker exec mailcowdockerized-postfix-mailcow-1 \
  bash -c 'mysql -h mysql-mailcow -u mailcow -p"$DBPASS" mailcow -e "SELECT * FROM transports;"'

docker exec mailcowdockerized-postfix-mailcow-1 \
  postmap -q firma.test \
  mysql:/opt/postfix/conf/sql/mysql_transport_maps.cf
```

Configuration → Routing → Transport Maps

| Feld        | Wert                |
| ----------- | ------------------- |
| Destination | `firma.test`        |
| Nexthop     | `192.168.178.25:25` |
| Username    | **leer**            |
| Password    | **leer**            |
| Active      | aktiviert           |
```
mkde@server:~/mailcow-firma2$ docker exec mailcowdockerized-postfix-mailcow-1 \
  postmap -q firma.test \
  mysql:/opt/postfix/conf/sql/mysql_transport_maps.cf
```

Die entscheidende Einstellung fehlt uns noch: smtpd_sender_restrictions.
```
mkde@server:~/mailcow-firma2$ docker exec mailserver postconf smtpd_sender_restrictions
smtpd_sender_restrictions = $dms_smtpd_sender_restrictions
```

```
docker exec mailserver postconf -h dms_smtpd_sender_restrictions
docker exec mailserver grep -R "smtpd_sender_restrictions\|reject_unknown_sender_domain" \
  -n /etc/docker-mailserver /usr/local/bin 2>/dev/null | head -30
docker exec mailserver cat /tmp/docker-mailserver/postfix-send-access.cf
cd ~/mailserver
printf '%s\n' 'firma2.test OK' > docker-data/dms/config/postfix-send-access.cf
docker exec mailserver cat /tmp/docker-mailserver/postfix-send-access.cf
```

```
docker compose up -d --force-recreate mailserver
docker exec mailserver postconf -h dms_smtpd_sender_restrictions
```
