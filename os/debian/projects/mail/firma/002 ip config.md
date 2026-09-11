# zweite ip erstellen

| Firma   | Mailserver-IP    | Beispiel-Hostname    |
| ------- | ---------------- | -------------------- |
| Firma 1 | `192.168.178.25` | `mail01.firma1.test` |
| Firma 2 | `192.168.178.99` | `mail01.firma2.test` |

```
sudo ip addr add 192.168.178.99/24 dev wlp2s0
nmcli connection show --active
nmcli device status
sudo nmcli connection modify 'connection' +ipv4.addresses 192.168.178.99/24
sudo nmcli connection up 'connection'
ip -br addr show wlp2s0
```
# firma 1 auf 192.168.178.25 beschränken
```
ports:
  - "192.168.178.25:25:25"
  - "192.168.178.25:143:143"
  - "192.168.178.25:587:587"
  - "192.168.178.25:993:993"
```

```
docker compose down
docker compose up -d
docker compose ps
```
