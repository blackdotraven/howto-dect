Example snippet for `dnsmasq-dhcp-server`'s `dnsmasq.conf`:

```
# set interface dnsmasq listens to
interface=enp0s31f6

# set dnsmask as the authoritative dhcp server
dhcp-authoritative

# dhcp pool (start, end, netmask, leasetime)
dhcp-range=10.10.10.100,10.10.10.199,255.255.255.0,12h

# static leases for RFPs
# for every RFP, set a tag to choose on which tftp boot file to take (tags are definded below: rfp2g/rfp3g/rfp4g)
dhcp-host=00:30:42:cc:23:42,set:rfp3g,10.10.10.10,rfp-1
dhcp-host=00:30:42:cc:23:43,set:rfp2g,10.10.10.11,rfp-2
dhcp-host=00:30:42:cc:23:44,set:rfp2g,10.10.10.12,rfp-3

# tell the RFPs where to find the omm by setting vendor-specific option 10 to rfp-1's adress.
# this also tells rfp-1 to host the omm on the rfp.
dhcp-option=vendor:OpenMobility,10,10.10.10.10

# 2nd gen RFPs will only take the dhcp offer when option 224 is set to the magic_str "OpenMobilitySIP-DECT".
# It might be possible to have co-existing DHCP servers where only the one responsible for RFPs sends the magic_str.
dhcp-option=224,"OpenMobilitySIP-DECT"

# set the tftp server adress (option 150) and tftp boot file name (option 67) depending on the tag set in the dhcp-host above
dhcp-option=150,10.10.10.1
dhcp-option=tag:rfp2g,67,"iprfp2G.tftp"
dhcp-option=tag:rfp3g,67,"iprfp3G.dnld"
dhcp-option=tag:rfp4g,67,"iprfp4G.dnld"

# location of the leasefile
dhcp-leasefile=/var/lib/dnsmasq/dnsmasq.leases

# disable dnsmasq's dns server
port=0

# DNS servers to use
server=9.9.9.9
server=149.112.112.112

# enable built-in tftp server (e.g. for gen 2 boot files, ...)
enable-tftp
tftp-root=/srv/tftp
```