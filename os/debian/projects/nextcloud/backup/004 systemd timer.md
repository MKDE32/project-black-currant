# create service
```
sudo nano /etc/systemd/system/nextcloud-backup.service
```

```
[Unit]
Description=Nextcloud Backup
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/nextcloud-backup
```


# create timer
```
sudo nano /etc/systemd/system/nextcloud-backup.timer
```

```
[Unit]
Description=Daily Nextcloud Backup

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

# reload systemd
```
sudo systemctl daemon-reload
```
# enable backup
```
sudo systemctl enable --now nextcloud-backup.timer
```

# testing timer
```
systemctl list-timers nextcloud-backup.timer
```
# testing backup
```
sudo systemctl start nextcloud-backup.service
systemctl status nextcloud-backup.service
```


# show logs
```
sudo journalctl -u nextcloud-backup.service -n 30
```













