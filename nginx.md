# Nginx

```bash
sudo apt-get update
sudo apt-get install nginx
sudo service nginx status

# Web root
ll /var/www/html
# Config files
ll /etc/nginx
# Log files
ll /var/log/nginx/
```

```bash
# Change port
$ sudo nano /etc/nginx/sites-enabled/default
  listen 8000 default_server;
$ sudo service nginx restart
$ sudo ufw allow 8000/tcp
$ sudo service ufw restart
```
