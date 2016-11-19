# HAproxy

```bash
# Find private ip
$ curl 169.254.169.254/metadata/v1/interfaces/private/0/ipv4/address && echo

# Find anchor ip (private ip of the floating ip)
$ curl 169.254.169.254/metadata/v1/interfaces/public/0/anchor_ipv4/address && echo
```

```bash
$ sudo apt-get update
$ sudo apt-get install haproxy

# Make it started automatically on reboot
$ sudo nano /etc/default/haproxy
  ENABLED=1

$ sudo nano /etc/haproxy/haproxy.cfg
```
https://github.com/janeczku/haproxy-acme-validation-plugin

```cfg
  global
    lua-load /etc/haproxy/haproxy-acme-validation-plugin-0.1.1/acme-http01-webroot.lua
    maxconn 2048
    tune.ssl.default-dh-param 2048    
  defaults
    option forwardfor
    option http-server-close
  frontend www
    bind *:443 ssl crt /etc/letsencrypt/live/bundit.net/haproxy.pem
    bind *:80
    acl url_acme_http01 path_beg /.well-known/acme-challenge/
    http-request use-service lua.acme-http01 if METH_GET url_acme_http01
    default_backend web-backend
  backend web-backend
    balance roundrobin
    server web-nodjs-1 127.0.0.1:8080 check
    stats enable
    stats hide-version
    stats uri    /admin?stats
    stats realm  Haproxy\ Statistics
```

```bash
# Check valid syntax 
$ sudo haproxy -f /etc/haproxy/haproxy.cfg -c

# Restart service
$ sudo service haproxy restart
```
