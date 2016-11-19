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
# /etc/haproxy/haproxy.cfg
global
    lua-load /etc/haproxy/haproxy-acme-validation-plugin-0.1.1/acme-http01-webroot.lua
    maxconn 2048
    tune.ssl.default-dh-param 2048    
defaults
    option forwardfor
    option http-server-close
frontend www-http
    bind *:80
    # Handle Let's Encrypt
    acl url_acme_http01 path_beg /.well-known/acme-challenge/
    http-request use-service lua.acme-http01 if METH_GET url_acme_http01
    # Redirect to https
    redirect scheme https code 301 if !{ ssl_fc }
frontend www-https
    bind *:443 ssl crt /etc/letsencrypt/live/example.com/haproxy.pem
    acl url_stats path_beg /haproxy
    use_backend haproxy-stats if url_stats
    default_backend web-backend
backend web-backend
    balance roundrobin
    server web-nodjs-1 127.0.0.1:8080 check
    server web-nodjs-2 127.0.0.1:8080 check
backend haproxy-stats
    stats enable
    stats hide-version
    stats uri /haproxy
    stats realm Haproxy\ Statistics
    stats auth user:password
```

```bash
# Check valid syntax 
$ sudo haproxy -f /etc/haproxy/haproxy.cfg -c

# Soft restart service
$ sudo service haproxy reload

# Restart service
$ sudo service haproxy restart
```
