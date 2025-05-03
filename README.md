# IP BlackHole

IP.blackhole is an IP blacklist that uses multiple sensors to identify network attacks (e.g. SSH brute force) and spam incidents. All reports are evaluated and in case of too many incidents the responsible IP holder is informed to solve the problem.

![stats](https://blackhole.s-e-r-v-e-r.pw/img?blackhole)

```
🚫 ALL IPs:
https://blackhole.s-e-r-v-e-r.pw/blackhole

🚫 TODAY IPs:
https://blackhole.s-e-r-v-e-r.pw/blackhole-today
```

How to use?
----
To block IPs via ipset and get a fresh and ready-to-deploy auto-ban list of "bad IPs" you can run:
```
sudo su
apt-get -qq install iptables ipset
ipset -q flush blackhole
ipset -q create blackhole hash:net
for ip in $(curl --compressed https://blackhole.s-e-r-v-e-r.pw/blackhole-today 2>/dev/null | grep -v "#" | grep -v -E "\s[1-2]$" | cut -f 1); do ipset add blackhole $ip; done
iptables -D INPUT -m set --match-set blackhole src -j DROP 2>/dev/null
iptables -I INPUT -m set --match-set blackhole src -j DROP
```
or 

ConfigServer Security and Firewall (CSF)
```
Edit CSF blocklist file:
nano /etc/csf/csf.blocklists

Navigate to the end of the file and append the following:
# IP.blackhole blacklist
IPBLACKHOLE|3600|0|https://blackhole.s-e-r-v-e-r.pw/blackhole-today

After you finish editing the file, save it and restart CSF and lfd using:
csf -ra

Check the log file to ensure that the blacklist was added correctly:
cat /var/log/lfd.log
```
