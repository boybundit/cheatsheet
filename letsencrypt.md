# Let's Encrypt

Using Haproxy on Ubuntu 16.10 (yakkety)

```bash
# Stop service on port 80
$ sudo service haproxy stop
$ netstat -na | grep ':80.*LISTEN'
```

--staging

```bash
$ sudo apt-get install certbot 
$ certbot certonly --standalone -d example.com -d www.example.com

Automating renewal
$ certbot renew --dry-run 
```
