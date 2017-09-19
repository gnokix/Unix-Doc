[HOW-TO] Solve QRO network problems
====================================

### Set a normalized hostname
```
echo $HOSTNAME.qro.local > /etc/hostname \
&& cat /etc/hostname | xargs hostname \
&& sed -i "s/ubuntu/$HOSTNAME/g" /etc/hosts
```

### Remove QRO trash in interafes configuration
```
rm -fv /etc/init.d/registerDNS.sh \
&& rm -fv /etc/init.d/rscd \
&& rm -fv /etc/apt/apt.conf.d/02proxy \
&& cp /etc/network/interfaces /etc/network/interfaces.ori \
&& cp -p /etc/dhcp/dhclient.conf /etc/dhcp/dhclient.conf.ori \
&& sed -i 's/domain-name, domain-name-servers, domain-search, host-name,/#domain-name, domain-name-servers, domain-search, host-name,/' /etc/dhcp/dhclient.conf \
&& sed -i '/dns/d' /etc/network/interfaces \
&& sed -i '/default\ gw /d' /etc/network/interfaces \
&& sed -i '/localdomain/d' /etc/hosts \
&& sed -i '/cloudtelmex/d' /etc/hosts \
&& vim /etc/network/interfaces
```

### Set the correct information for the interface
> **NOTE:** This is just an *Example*, please make sure the information are the correct for your config.
```
auto ens225
iface ens225 inet static
address 10.20.28.xx
netmask 255.255.252.0
dns-nameservers 10.20.5.35
dns-nameservers 10.20.5.36
up route add default gw 10.20.28.3
dns-search local qro.local dlatv.local dlatv.net
```
> **NOTE:** Please use ```set list``` in vim before you saves the file to make you sure no **alien** character is there.

### Restart the networking service
```
service networking restart
```
> **NOTE:** Even when the service show an error it must work!

### If interface problems persist
```
ip link set ens225 down
ip addr add 10.20.28.xx/22 dev ens225
ip link set ens225 up
ip route add default via 10.20.28.3
```

### Test network configuration
```
ip addr show dev ens225
ip route show
tracepath google.com
```

### Remove unnecesary default gw
```
ip route delete default via 172.29.77.6 \
&& ip route delete default via 172.26.197.131 \
&& ip route delete default via 172.26.197.3
```

### If name resolution fails
```
echo  "nameserver 10.20.5.35" >> /etc/resolv.conf
```
