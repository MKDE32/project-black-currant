sudo apt update
sudo apt install -y git openssl curl gawk coreutils grep jq

cd ~
git clone https://github.com/mailcow/mailcow-dockerized.git mailcow-firma2
cd ~/mailcow-firma2

./generate_config.sh

mail02.firma2.test
Europe/Amsterdam
1

nano ~/mailcow-firma2/mailcow.conf

```
HTTP_PORT=80
HTTP_BIND=192.168.178.99

HTTPS_PORT=443
HTTPS_BIND=192.168.178.99

SMTP_PORT=192.168.178.99:25
SMTPS_PORT=192.168.178.99:465
SUBMISSION_PORT=192.168.178.99:587

IMAP_PORT=192.168.178.99:143
IMAPS_PORT=192.168.178.99:993

POP_PORT=192.168.178.99:110
POPS_PORT=192.168.178.99:995

SIEVE_PORT=192.168.178.99:4190
```
grep -E '^(MAILCOW_HOSTNAME|HTTP_PORT|HTTP_BIND|HTTPS_PORT|HTTPS_BIND|SMTP_PORT|SMTPS_PORT|SUBMISSION_PORT|IMAP_PORT|IMAPS_PORT|POP_PORT|POPS_PORT|SIEVE_PORT)=' mailcow.conf

cd ~/mailcow-firma2

docker compose pull
docker compose up -d
docker compose ps

# dns

sudo nano /etc/hosts

192.168.178.99 mail02.firma2.test
ping mail02.firma2.test
