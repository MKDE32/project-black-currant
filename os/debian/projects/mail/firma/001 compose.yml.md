```
mkdir -p ~/mailserver
cd ~/mailserver
```
Domain:      firma.test
Mailserver:  mail01.firma.test
```
mkdir -p docker-data/dms/mail-data
mkdir -p docker-data/dms/mail-state
mkdir -p docker-data/dms/mail-logs
mkdir -p docker-data/dms/config
```
| Verzeichnis  | Zweck                                  |
| ------------ | -------------------------------------- |
| `mail-data`  | eigentliche E-Mails                    |
| `mail-state` | Zustandsdaten von Postfix/Dovecot usw. |
| `mail-logs`  | Mailserver-Logs                        |
| `config`     | unsere Mailserver-Konfiguration        |

```
nano compose.yml
```
```
services:
  mailserver:
    image: ghcr.io/docker-mailserver/docker-mailserver:latest

    container_name: mailserver

    hostname: mail01.firma.test

    ports:
      - "25:25"
      - "587:587"
      - "993:993"

    volumes:
      - ./docker-data/dms/mail-data/:/var/mail/
      - ./docker-data/dms/mail-state/:/var/mail-state/
      - ./docker-data/dms/mail-logs/:/var/log/mail/
      - ./docker-data/dms/config/:/tmp/docker-mailserver/
      - /etc/localtime:/etc/localtime:ro

    environment:
      - ENABLE_IMAP=1
      - ENABLE_CLAMAV=0
      - ENABLE_FAIL2BAN=0

    restart: unless-stopped
```




