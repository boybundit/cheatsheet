# Ubuntu

## Initial Server Setup

Create a new super user
```bash
adduser demo
gpasswd -a demo sudo
```

Restrict root login
```bash
nano /etc/ssh/sshd_config
PermitRootLogin no
service ssh restart
```

Configure a basic firewall
```bash
sudo ufw allow ssh
sudo ufw allow 80/tcp (HTTP)
sudo ufw allow 443/tcp (SSL/TLS)
sudo ufw allow 25/tcp (SMTP)
sudo ufw show added
sudo ufw enable
```

Configure timezones
```bash
sudo dpkg-reconfigure tzdata
```

Configure NTP synchronization
```bash
sudo apt-get update
sudo apt-get install ntp
```

Create a swap file
```bash
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'
```

Take a Snapshot of your Current Configuration
```bash
sudo poweroff
```

Install Fail2ban
```bash
sudo apt-get install fail2ban
sudo nano /etc/fail2ban/jail.local
sudo service fail2ban restart
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
