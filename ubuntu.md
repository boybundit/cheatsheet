# Ubuntu

## Pre-requisite

- Run `puttykeygen.exe` to generate public/private keys.
- Upload public key using DigitalOcean console.

## Initial Server Setup

```bash
# Login with root
$ ssh root@server_ip_address

# Create a new user
$ adduser newuser
# Add user into sudo group
$ gpasswd -a newuser sudo

# SSH public key in DigitalOcean console is auto-added
$ cat $HOME/.ssh/authorized_keys

# Manually add public key (optional)
$ su - newuser # Switch to newuser
$ mkdir .ssh
$ chmod 700 .ssh
$ nano .ssh/authorized_keys
ssh-rsa public_key comment
$ chmod 600 .ssh/authorized_keys
$ exit # Back to root

# Restrict root login
$ nano /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
$ service ssh restart

# Setup uncomplicated firewall
$ sudo ufw allow ssh
$ sudo ufw allow 80/tcp   # HTTP
$ sudo ufw allow 443/tcp  # SSL/TLS
$ sudo ufw allow 25/tcp   # SMTP
$ sudo ufw show added
$ sudo ufw enable

# Configure timezones
$ sudo dpkg-reconfigure tzdata

# Configure NTP synchronization
$ sudo apt-get update
$ sudo apt-get install ntp

# Create a swap file
$ sudo fallocate -l 4G /swapfile
$ sudo chmod 600 /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
$ sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'

# Install Fail2ban
$ sudo apt-get install fail2ban
$ sudo nano /etc/fail2ban/jail.local
$ sudo service fail2ban restart

# Power-off to take a snapshot
$ sudo poweroff
```

## Monitoring

```bash
$ htop
$ landscape-sysinfo

# Process
$ ps aux | grep process_name

# Storage
$ df -h

# Memory
$ free -m

# Fail2Ban
$ sudo tail /var/log/fail2ban.log
$ sudo iptables -S | grep fail2ban
```
