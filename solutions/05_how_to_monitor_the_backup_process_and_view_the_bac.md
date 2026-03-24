# How to monitor the backup process and view the backup logs

_Use systemd to monitor the backup process and view the backup logs_

## Steps

1. Use the command `sudo systemctl status backup.service` to view the status of the backup service
2. Use the command `sudo journalctl -u backup.service` to view the backup logs
3. Use the command `sudo journalctl -u backup.service -f` to follow the backup logs in real-time
4. Modify the backup.service file to increase the log level, for example `ExecStart=/usr/bin/rsync -avz --log-file=/var/log/backup.log /source/directory/ /destination/directory/`
5. Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
6. Verify that the log level has been increased by running the command `sudo journalctl -u backup.service` and verifying the output

## Pitfalls

- Not checking the backup logs regularly