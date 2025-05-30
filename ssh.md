# Installing SSH server on RHEL-based systems

### Step x: Check if installed

``` sudo dnf list installed | grep openssh-server ```

if not installed:

``` sudo dnf install -y openssh-server ```

### Step x: Enable and start 

``` sudo systemctl enable sshd ```

``` sudo systemctl start sshd ```

``` sudo systemctl status sshd ```

### Step x: Adjust firewall if necessary

``` sudo firewall-cmd --state ```

``` sudo firewall-cmd --permanent --add-service=ssh ```

``` sudo firewall-cmd --reload ```

``` sudo firewall-cmd --list-all ``` 

### Step x: Test access

on another machine:

``` ssh <user>@<ip> ```

### Step x: Harden 

``` sudo nano /etc/ssh/sshd_config ```

Diable root login: ``` PermitRootLogin no ```

Use Protocol 2 only: ``` Protocol 2 ```

ONLY IF SENSIBLE: Enable only key-based auth: ``` Passwordauthentication no ``` 

Counter brute force: ``` MaxAuthTries 3 ```

Counter DoS: ``` MaxSessions 2 ``` and ``` LoginGraceTime 30 ```

``` sudo systemctl restart sshd ```

Fail2Ban:

Check the port: ``` ss -tln | grep :22 ```

```
sudo dnf install -y fail2ban
sudo tee /etc/fail2ban/jail.d/ssh.local <<'EOF'
[sshd]
enabled = true
port = 22
maxretry = 3
bantime = 1800
EOF
```

Start and enable: ``` sudo systemctl enable --now fail2ban ```

Check Banned IPs: ``` sudo fail2ban-client status sshd ```

### Step x: Monitor

Monitor /var/log/secure: ``` sudo tail -F /var/log/secure | grep --line-buffered -E "Failed password|Invalid user|Accepted password|Accepted publickey" ```

Watch /etc/ssh/sshd_config and /etc/ssh/ssh_host_* with a file integrity tool like AIDE or tripwire:

```
sudo dnf install -y aide
sudo aide --init
sudo mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz

sudo aide --check
```

#### Process / network inspection:

List sshd processes and their parent / children: ``` ps -ef | grep sshd ``` or ``` ps -eFH | less ```

Show listening ports and processes: ``` sudo ss -tulpn | grep ssh ```

Check for reverse shells: ``` sudo lsof -iTCP -P -n | grep sshd ```

---

Check if recently modified: ``` sudo stat /etc/passwd /etc/group /etc/sudoers ``` 

Check for new authorized keys: ``` sudo grep -R --include=authorized_keys "" /home/* ```







