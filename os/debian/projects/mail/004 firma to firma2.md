idiot@firma2.test




```
docker exec mailserver setup config dkim
docker exec mailserver setup relay add-domain firma2.test 192.168.178.99 25
docker exec mailserver postconf sender_dependent_relayhost_maps
docker exec mailserver postmap -q '@firma2.test' texthash:/etc/postfix/relayhost_map
docker exec mailserver bash -c 'timeout 5 bash -c "</dev/tcp/192.168.178.99/25" && echo SMTP_OK || echo SMTP_FAILED'
```

```
nano docker-data/dms/config/recipient_transport_map
```
> firma2.test    smtp:[192.168.178.99]:25
```
nano docker-data/dms/config/postfix-main.cf
```
> transport_maps = texthash:/etc/postfix/recipient_transport_map
```
printf '%s\n' 'transport_maps = texthash:/etc/postfix/recipient_transport_map' > docker-data/dms/config/postfix-main.cf
nano docker-data/dms/config/user-patches.sh
```
```
#!/bin/bash


cp /tmp/docker-mailserver/recipient_transport_map \
   /etc/postfix/recipient_transport_map

chmod 0644 /etc/postfix/recipient_transport_map
```
```
chmod +x docker-data/dms/config/user-patches.sh
docker compose up -d --force-recreate mailserver
docker exec mailserver postconf transport_maps
docker exec mailserver cat /etc/postfix/recipient_transport_map
docker exec mailserver postmap -q firma2.test texthash:/etc/postfix/recipient_transport_map
```








































