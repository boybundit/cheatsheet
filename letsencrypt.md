# Let's Encrypt

Using HAproxy on Ubuntu 16.10 (yakkety)

## Pre-requisite
- Stop cloudflare on this domain.

```bash
# Stop service on port 80
# Stop HAproxy
$ sudo service haproxy stop
# Check no service on port 80
$ netstat -na | grep ':80.*LISTEN'
```

> For production, remove `--staging` flag

```bash
$ sudo apt-get install certbot 
$ certbot certonly --staging --standalone -d example.com -d www.example.com
$ ls /etc/letsencrypt/live/example.com/
cert.pem  chain.pem  fullchain.pem  privkey.pem


Automating renewal
$ certbot renew --staging --dry-run 
```
