# How to set up automatic daily backups of a Linux server using rsync and systemd timers

_Configure rsync and systemd to automate daily backups of a Linux server_

## Steps

1. Install the rsync package by running the command `sudo apt-get update && sudo apt-get install rsync`
2. Create a new file for the systemd service, for example `sudo nano /etc/systemd/system/backup.service`
3. In the backup.service file, add the following lines to define the service: `[Unit] Description=Daily Backup Service [Service] ExecStart=/usr/bin/rsync -avz /source/directory/ /destination/directory/`
4. Create a new file for the systemd timer, for example `sudo nano /etc/systemd/system/backup.timer`
5. In the backup.timer file, add the following lines to define the timer: `[Unit] Description=Daily Backup Timer [Timer] OnUnitActiveSec=1day AccuracySec=1min Unit=backup.service`
6. Reload the systemd daemon and start the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl start backup.timer`

## Pitfalls

- Forgetting to reload the systemd daemon after creating or modifying systemd files