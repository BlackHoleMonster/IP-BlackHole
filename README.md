# IP BlackHole

IP.blackhole.monster is an IP blacklist that uses multiple sensors to identify network attacks (e.g. SSH brute force) and spam incidents. All reports are evaluated and in case of too many incidents the responsible IP holder is informed to solve the problem.

![stats](https://ip.blackhole.monster/img)

```
🚫 ALL IPs:
https://ip.blackhole.monster/blackhole

🚫 TODAY IPs:
https://ip.blackhole.monster/blackhole-today
```

How to use?
----
To get a fresh and ready-to-deploy auto-ban list of "bad IPs" you can run:
```
sudo su
apt-get -qq install iptables ipset
ipset -q flush blackhole
ipset -q create blackhole hash:net
for ip in $(curl --compressed https://ip.blackhole.monster/blackhole-today 2>/dev/null | grep -v "#" | grep -v -E "\s[1-2]$" | cut -f 1); do ipset add blackhole $ip; done
iptables -D INPUT -m set --match-set blackhole src -j DROP 2>/dev/null
iptables -I INPUT -m set --match-set blackhole src -j DROP
```
