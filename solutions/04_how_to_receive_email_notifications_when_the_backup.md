# How to receive email notifications when the backup fails or completes

_Use a script to send email notifications when the backup fails or completes_

## Steps

1. Install the mailutils package by running the command `sudo apt-get install mailutils`
2. Create a new file for the notification script, for example `sudo nano /etc/backup/notify.sh`
3. Add the following lines to the script to send email notifications: `echo 'Backup completed successfully' | mail -s 'Backup Notification' user@example.com`
4. Modify the backup.service file to include the notification script, for example `ExecStart=/usr/bin/rsync -avz /source/directory/ /destination/directory/ && /etc/backup/notify.sh`
5. Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
6. Verify that the notification script is working by running the command `sudo /etc/backup/notify.sh` and verifying the output

## Pitfalls

- Not configuring the mail server correctly