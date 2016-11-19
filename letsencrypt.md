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

> For testing, add `--staging` flag

```bash
$ sudo apt-get install certbot 
$ certbot certonly --standalone -d example.com -d www.example.com
$ ls /etc/letsencrypt/live/example.com/
cert.pem  chain.pem  fullchain.pem  privkey.pem

# Automating renewal
$ certbot renew --dry-run 
```

## HAProxy ACME domain validation plugin

https://github.com/janeczku/haproxy-acme-validation-plugin

```bash
# Concat certificate chain and private key to a PEM file suitable for HAProxy
$ sudo cat /etc/letsencrypt/live/www.example.com/privkey.pem \
  /etc/letsencrypt/live/www.example.com/fullchain.pem \
  | sudo tee /etc/letsencrypt/live/www.example.com/haproxy.pem >/dev/null
```
```bash
# Check version
$ haproxy -vv
# Install plugin
$ wget https://github.com/janeczku/haproxy-acme-validation-plugin/archive/0.1.1.tar.gz
$ tar -xf 0.1.1.tar.gz
$ sudo mv haproxy-acme-validation-plugin-0.1.1 /etc/haproxy
$ rm 0.1.1.tar.gz
```
```cfg
# /etc/haproxy/haproxy.cfg
global
    lua-load /etc/haproxy/haproxy-acme-validation-plugin-0.1.1/acme-http01-webroot.lua
frontend https
    bind *:443 ssl crt /etc/letsencrypt/live/www.example.com/haproxy.pem
    acl url_acme_http01 path_beg /.well-known/acme-challenge/
    http-request use-service lua.acme-http01 if METH_GET url_acme_http01
```
```bash
# Soft restart HAProxy
$ sudo service haproxy reload
```

## Automatic renewal

```bash
# Update parameters in renewal script
$ sudo nano /etc/haproxy/haproxy-acme-validation-plugin-0.1.1/cert-renewal-haproxy.sh
# Setup weekly cronjob
$ sudo crontab -e
```
```
5 8 * * 6 /etc/haproxy/haproxy-acme-validation-plugin-0.1.1/cert-renewal-haproxy.sh
```
