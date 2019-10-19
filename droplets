Initial Server Setup with Ubuntu 14.04
https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04

Create a New User
adduser demo
gpasswd -a demo sudo

Restrict Root Login
nano /etc/ssh/sshd_config
PermitRootLogin no
service ssh restart

https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers

Configure a Basic Firewall
sudo ufw allow ssh
sudo ufw allow 4444/tcp (if using custom ssh port)
sudo ufw allow 80/tcp (HTTP)
sudo ufw allow 443/tcp (SSL/TLS)
sudo ufw allow 25/tcp (SMTP)
sudo ufw show added
sudo ufw enable

Configure Timezones
sudo dpkg-reconfigure tzdata

Configure NTP Synchronization
sudo apt-get update
sudo apt-get install ntp

Create a Swap File
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'

Take a Snapshot of your Current Configuration
sudo poweroff

Install NodeJS
/ect/environment
NODE_END=production

Migrate MongoDB (export db to file, upload using SFTP, then import)
mongodump -d some_database -c some_collection
mongorestore -d some_other_db -c some_or_other_collection dump/some_collection.bson


https://www.linode.com/docs/security/securing-your-server
sudo apt-get install fail2ban
sudo nano /etc/fail2ban/jail.local
sudo service fail2ban restart

https://www.digitalocean.com/community/tutorials/how-to-monitor-system-authentication-logs-on-ubuntu


# HAproxy

apt list --installed

lua-load /etc/haproxy/haproxy-acme-validation-plugin-0.1.1/acme-http01-webroot.lua
sudo service haproxy restart
--webroot --webroot-path /var/lib/haproxy

https://github.com/janeczku/haproxy-acme-validation-plugin

        acl url_blog path_beg /blog
        use_backend blog-backend if url_blog

backend web-backend
        balance leastconn
        server web1 127.0.0.1:8080 check
        stats enable
