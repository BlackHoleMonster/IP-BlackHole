# PacketDump

IP BlackList based on tcpdump.

```
🚫 ALL IPs:
https://packetdump.s-e-r-v-e-r.pw/ips
```

How to use?
----
To block IPs via ipset and get a fresh and ready-to-deploy auto-ban list of "bad IPs" you can run:
```
sudo su
apt-get -qq install iptables ipset
ipset -q flush packetdump
ipset -q create packetdump hash:net
for ip in $(curl --compressed https://packetdump.s-e-r-v-e-r.pw/ips 2>/dev/null | grep -v "#" | grep -v -E "\s[1-2]$" | cut -f 1); do ipset add packetdump $ip; done
iptables -D INPUT -m set --match-set packetdump src -j DROP 2>/dev/null
iptables -I INPUT -m set --match-set packetdump src -j DROP
```
or 

ConfigServer Security and Firewall (CSF)
```
Edit CSF blocklist file:
nano /etc/csf/csf.blocklists

Navigate to the end of the file and append the following:
# PacketDump blacklist
IPBLACKHOLE|3600|0|https://packetdump.s-e-r-v-e-r.pw/ips

After you finish editing the file, save it and restart CSF and lfd using:
csf -ra

Check the log file to ensure that the blacklist was added correctly:
cat /var/log/lfd.log
```
