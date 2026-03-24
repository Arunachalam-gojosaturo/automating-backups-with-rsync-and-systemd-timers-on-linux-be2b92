# How to automate the rotation of backup files to save disk space

_Use a script to rotate backup files and remove old backups_

## Steps

1. Create a new file for the rotation script, for example `sudo nano /etc/backup/rotate.sh`
2. Add the following lines to the script to rotate the backups: `find /destination/directory/ -type f -mtime +30 -delete`
3. Make the script executable by running the command `sudo chmod +x /etc/backup/rotate.sh`
4. Add the script to the systemd timer by modifying the backup.timer file to include the script, for example `ExecStart=/etc/backup/rotate.sh`
5. Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
6. Verify that the rotation script is working by running the command `sudo /etc/backup/rotate.sh` and verifying the output

## Pitfalls

- Not testing the rotation script before adding it to the systemd timer