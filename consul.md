# Consul

https://www.consul.io/downloads.html

```bash
apt-get update
apt-get install unzip
cd /usr/local/bin
wget https://releases.hashicorp.com/consul/0.7.1/consul_0.7.1_linux_amd64.zip
unzip *.zip
rm *.zip
```

```bash
adduser consul
# Configurations
mkdir -p /etc/consul.d/{bootstrap,server,client}
# Persistent data
mkdir /var/consul
chown consul:consul /var/consul
# Generate key to enter in config
consul keygen
```
`/etc/consul.d/bootstrap/config.json`
```json
{
    "bootstrap": true,
    "server": true,
    "datacenter": "sgp1",
    "data_dir": "/var/consul",
    "encrypt": "6i565nbw4/w5e/SR1A1YYg==",
    "log_level": "INFO",
    "enable_syslog": true
}
```
`/etc/consul.d/server/config.json`
```json
{
    "bootstrap": false,
    "server": true,
    "datacenter": "sgp1",
    "data_dir": "/var/consul",
    "encrypt": "6i565nbw4/w5e/SR1A1YYg==",
    "log_level": "INFO",
    "enable_syslog": true,
    "start_join": ["128.199.195.46", "128.199.236.79"]
}
```
 /etc/consul.d/client/config.json
 ```json
{
    "bootstrap": false,
    "server": false,
    "datacenter": "sgp1",
    "data_dir": "/var/consul",
    "ui_dir": "/home/consul/consul-web-ui",
    "encrypt": "6i565nbw4/w5e/SR1A1YYg==",
    "log_level": "INFO",
    "enable_syslog": true,
    "start_join": ["128.199.72.174", "128.199.195.46", "128.199.236.79"]
}
```
 
```bash
su consul
cd ~
mkdir consul-web-ui
cd consul-web-ui
wget https://releases.hashicorp.com/consul/0.7.1/consul_0.7.1_web_ui.zip
unzip *.zip
rm *.zip
```

nano /etc/init/consul.conf

```conf
description "Consul server process"

start on (local-filesystems and net-device-up IFACE=eth0)
stop on runlevel [!12345]

respawn

setuid consul
setgid consul

exec consul agent -config-dir /etc/consul.d/server -advertise 128.199.195.46
```

Ubuntu 16 is using systemd instead of upstart for service command
 
link to /etc/systemd/system/consul.service
```conf
[Unit]
Description=Consul server process

[Service]
ExecStart=/usr/local/bin/consul agent -config-dir /etc/consul.d/server -advertise 128.199.195.46
Restart=always
```

```conf
description "Consul client process"

start on (local-filesystems and net-device-up IFACE=eth0)
stop on runlevel [!12345]

respawn

setuid consul
setgid consul

exec consul agent -config-dir /etc/consul.d/client
```


consul members

