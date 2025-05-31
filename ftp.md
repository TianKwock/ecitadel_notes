# FTP Basics 

### Step 1: Harden

Disable anonymous access: ``` anonymous_enable=NO ``` in vsftpd.conf

Prevent users from navigating outside their home directory: ``` chroot_local_user=YES ``` and ``` allow_writeable_chroot=YES ```

Disallow uploads: ``` write_enable=NO ```

Check for aliases: ``` alias | grep ftp ``` and ``` type ftp ```

RESTART: ``` systemctl restart vsftpd ```

Fail2ban:
```
sudo apt install fail2ban

[vsftpd]
enabled = true
port = ftp
filter = vsftpd
logpath = /var/log/vsftpd.log
maxretry = 3
```

### Step 2: Monitor

Check logs: ``` tail -f /var/log/vsftpd.log ```


