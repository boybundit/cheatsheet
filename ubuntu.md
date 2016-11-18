# Ubuntu

## Pre-requisite

- Run `puttykeygen.exe` to generate public/private keys.
- Upload the public key using DigitalOcean console.
- Create a new server using the public key.
- Connect to the server with SSH as `root` using the private key.

## Initial Server Setup

```bash
# Create a new user
$ adduser newuser
# Add user into sudo group
$ gpasswd -a newuser sudo

# SSH public key in DigitalOcean console is auto-added
$ cat $HOME/.ssh/authorized_keys

# Manually add public key (optional)
$ su - newuser # Switch to newuser
$ whoami
$ mkdir .ssh
$ chmod 700 .ssh
$ nano .ssh/authorized_keys
ssh-rsa public_key comment
$ chmod 600 .ssh/authorized_keys
$ exit # Back to root

# Restrict root login
$ sudo nano /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
$ sudo service ssh restart

# Setup uncomplicated firewall
$ sudo ufw limit ssh      # SSH with rate limit
$ sudo ufw allow 80/tcp   # HTTP
$ sudo ufw allow 443/tcp  # SSL/TLS
$ sudo ufw allow 25/tcp   # SMTP
$ sudo ufw show added
$ sudo ufw enable

# Configure timezones
$ sudo dpkg-reconfigure tzdata
$ timedatectl

# Configure NTP synchronization
$ sudo apt-get update
$ sudo apt-get install ntp
$ sudo ntpq -p

# Create a swap file
$ sudo fallocate -l 4G /swapfile
$ sudo chmod 600 /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
$ sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'

# Install Fail2Ban
$ sudo apt-get install fail2ban
$ sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
$ sudo nano /etc/fail2ban/jail.local
$ sudo service fail2ban restart

# Shutdown to take a snapshot
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

## Update

```bash
# Update
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt-get autoremove

# Restart
$ sudo shutdown -r now
```

# Node.JS
```bash
# Install nvm https://github.com/creationix/nvm
$ sudo apt-get install build-essential libssl-dev
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
$ source ~/.profile

# Install Node.js using nvm
$ nvm ls-remote
$ nvm ls
$ nvm install 6.9.1
$ nvm use 6.9.1
$ node -v

# Set NODE_ENV
$ sudo nano /ect/environment
NODE_ENV=production
```

# MongoDB
```bash
# Migrate database (export db to file, upload using SFTP, then import)
$ mongodump -d some_database -c some_collection
$ mongorestore -d some_other_db -c some_or_other_collection dump/some_collection.bson
```

# References

- https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04
- https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers
- https://www.linode.com/docs/security/securing-your-server
- https://www.digitalocean.com/community/tutorials/how-to-monitor-system-authentication-logs-on-ubuntu
