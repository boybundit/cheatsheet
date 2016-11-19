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
  defaults
    log     global
    mode    tcp
    option  tcplog
  frontend www
    bind    load_balancer_anchor_ip:80
    default_backend nginx_pool
  backend nginx_pool
    balance roundrobin
    mode tcp
    server web1 web_server_1_private_ip:80 check
    server web2 web_server_2_private_ip:80 check

# Check valid syntax 
$ sudo haproxy -f /etc/haproxy/haproxy.cfg -c

# Restart service
$ sudo service haproxy restart
```
