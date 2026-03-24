# Automating backups with rsync and systemd timers on Linux

> This repository provides tutorials and scripts for automating backups with rsync and systemd timers on Linux systems

**Category:** linux  
**Tags:** `backup` `rsync` `systemd`

---

## Table of Contents

1. [How to set up automatic daily backups of a Linux server using rsync and systemd timers](#how-to-set-up-automatic-daily-backups-of-a-linux-server-using-rsync-and-systemd-timers)
2. [How to exclude certain files or directories from the rsync backup](#how-to-exclude-certain-files-or-directories-from-the-rsync-backup)
3. [How to automate the rotation of backup files to save disk space](#how-to-automate-the-rotation-of-backup-files-to-save-disk-space)
4. [How to receive email notifications when the backup fails or completes](#how-to-receive-email-notifications-when-the-backup-fails-or-completes)
5. [How to monitor the backup process and view the backup logs](#how-to-monitor-the-backup-process-and-view-the-backup-logs)

---

## How to set up automatic daily backups of a Linux server using rsync and systemd timers

**Configure rsync and systemd to automate daily backups of a Linux server**

### Prerequisites

- Linux server with rsync installed
- Systemd installed and configured

### Solution

**Step 1:**

```bash
Install the rsync package by running the command `sudo apt-get update && sudo apt-get install rsync`
```

**Step 2:**

```bash
Create a new file for the systemd service, for example `sudo nano /etc/systemd/system/backup.service`
```

**Step 3:** In the backup.service file, add the following lines to define the service: `[Unit] Description=Daily Backup Service [Service] ExecStart=/usr/bin/rsync -avz /source/directory/ /destination/directory/`

**Step 4:**

```bash
Create a new file for the systemd timer, for example `sudo nano /etc/systemd/system/backup.timer`
```

**Step 5:** In the backup.timer file, add the following lines to define the timer: `[Unit] Description=Daily Backup Timer [Timer] OnUnitActiveSec=1day AccuracySec=1min Unit=backup.service`

**Step 6:**

```bash
Reload the systemd daemon and start the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl start backup.timer`
```

> [!WARNING]
> **Common Pitfalls:**
> - Forgetting to reload the systemd daemon after creating or modifying systemd files

*Tags: `linux` `backup` `rsync` `systemd`*

---

## How to exclude certain files or directories from the rsync backup

**Use the --exclude option with rsync to exclude files or directories from the backup**

### Prerequisites

- Rsync installed and configured
- Backup service file created and configured

### Solution

**Step 1:**

```bash
Create a new file for the exclude list, for example `sudo nano /etc/backup/exclude.txt`
```

**Step 2:** Add the files or directories to exclude, one per line, to the exclude.txt file

**Step 3:** Modify the rsync command in the backup.service file to include the --exclude option, for example `ExecStart=/usr/bin/rsync -avz --exclude-from=/etc/backup/exclude.txt /source/directory/ /destination/directory/`

**Step 4:**

```bash
Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
```

**Step 5:** Verify that the excluded files or directories are not included in the backup by running the command `rsync -avz --exclude-from=/etc/backup/exclude.txt /source/directory/ /destination/directory/`

**Step 6:**

```bash
Test the backup by running the command `sudo systemctl start backup.service` and verifying the output
```

> [!WARNING]
> **Common Pitfalls:**
> - Not using the correct syntax for the --exclude option

*Tags: `linux` `backup` `rsync` `exclude`*

---

## How to automate the rotation of backup files to save disk space

**Use a script to rotate backup files and remove old backups**

### Prerequisites

- Rsync installed and configured
- Backup service file created and configured

### Solution

**Step 1:**

```bash
Create a new file for the rotation script, for example `sudo nano /etc/backup/rotate.sh`
```

**Step 2:** Add the following lines to the script to rotate the backups: `find /destination/directory/ -type f -mtime +30 -delete`

**Step 3:**

```bash
Make the script executable by running the command `sudo chmod +x /etc/backup/rotate.sh`
```

**Step 4:** Add the script to the systemd timer by modifying the backup.timer file to include the script, for example `ExecStart=/etc/backup/rotate.sh`

**Step 5:**

```bash
Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
```

**Step 6:**

```bash
Verify that the rotation script is working by running the command `sudo /etc/backup/rotate.sh` and verifying the output
```

> [!WARNING]
> **Common Pitfalls:**
> - Not testing the rotation script before adding it to the systemd timer

*Tags: `linux` `backup` `rotation` `script`*

---

## How to receive email notifications when the backup fails or completes

**Use a script to send email notifications when the backup fails or completes**

### Prerequisites

- Rsync installed and configured
- Backup service file created and configured

### Solution

**Step 1:**

```bash
Install the mailutils package by running the command `sudo apt-get install mailutils`
```

**Step 2:**

```bash
Create a new file for the notification script, for example `sudo nano /etc/backup/notify.sh`
```

**Step 3:** Add the following lines to the script to send email notifications: `echo 'Backup completed successfully' | mail -s 'Backup Notification' user@example.com`

**Step 4:** Modify the backup.service file to include the notification script, for example `ExecStart=/usr/bin/rsync -avz /source/directory/ /destination/directory/ && /etc/backup/notify.sh`

**Step 5:**

```bash
Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
```

**Step 6:**

```bash
Verify that the notification script is working by running the command `sudo /etc/backup/notify.sh` and verifying the output
```

> [!WARNING]
> **Common Pitfalls:**
> - Not configuring the mail server correctly

*Tags: `linux` `backup` `notification` `email`*

---

## How to monitor the backup process and view the backup logs

**Use systemd to monitor the backup process and view the backup logs**

### Prerequisites

- Rsync installed and configured
- Backup service file created and configured

### Solution

**Step 1:**

```bash
Use the command `sudo systemctl status backup.service` to view the status of the backup service
```

**Step 2:**

```bash
Use the command `sudo journalctl -u backup.service` to view the backup logs
```

**Step 3:**

```bash
Use the command `sudo journalctl -u backup.service -f` to follow the backup logs in real-time
```

**Step 4:** Modify the backup.service file to increase the log level, for example `ExecStart=/usr/bin/rsync -avz --log-file=/var/log/backup.log /source/directory/ /destination/directory/`

**Step 5:**

```bash
Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
```

**Step 6:**

```bash
Verify that the log level has been increased by running the command `sudo journalctl -u backup.service` and verifying the output
```

> [!WARNING]
> **Common Pitfalls:**
> - Not checking the backup logs regularly

*Tags: `linux` `backup` `monitoring` `logs`*

---

## Contributing

Found an error or want to add more solutions? Open a pull request or create an issue!

## License

MIT — free to use, share, and improve.

---

*Auto-generated by [repo-auto-generator](https://github.com/Arunachalam-gojosaturo/repo-auto-generator)*