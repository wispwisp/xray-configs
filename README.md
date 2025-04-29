# xray configs

### build xray
```bash
CGO_ENABLED=0 go build -o xray -trimpath -buildvcs=false -ldflags="-s -w -buildid=" -v ./main
```

### build xray for windows
```bash
GOOS=windows CGO_ENABLED=0 go build -o xray.exe -trimpath -buildvcs=false -ldflags="-s -w -buildid=" -v ./main
```

### run
```
xray run -c /path/to/conf.json
```

### prepare VPS

#### ssh
```bash
adduser me
visudo
```
add to the end of opened file:
```
me ALL=(ALL:ALL) ALL
```

```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
/etc/ssh/sshd_config
systemctl restart ssh
```
```
Port 65534
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
PermitEmptyPasswords no
```

#### fail to ban
```bash
apt install fail2ban
# debian12
# echo "sshd_backend = systemd" >> /etc/fail2ban/paths-debian.conf
systemctl enable fail2ban
systemctl start fail2ban
systemctl status fail2ban
```

#### ufw
```bash
apt install ufw
ufw default deny incoming && ufw default allow outgoing
ufw allow 22
sudo ufw allow 443/tcp
ufw enable
systemctl restart fail2ban
```
```bash
ufw status numbered
ufw delete 2
```

### check all works
```bash
curl -v --resolve www.EXAMPLE.com:443:YOUR.IP.HERE.HERE https://www.EXAMPLE.com 
```
