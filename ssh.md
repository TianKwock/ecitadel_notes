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

ONLY IF SENSIBLE: Enable only key-based auth: ``` Passwordauthentication no ``` 

``` sudo systemctl restart sshd ```

### Step x: Monitor

Monitor /var/log/secure: ``` sudo tail -F /var/log/secure | grep --line-buffered -E "Failed password|Invalid user|Accepted password|Accepted publickey" ```

Watch /etc/ssh/sshd_config and /etc/ssh/ssh_host_* with a file integrity tool like AIDE or tripwire:

```
sudo dnf install -y aide
sudo aide --init
sudo mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz

sudo aide --check
```







