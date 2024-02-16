Example snippet for `isc-dhcp-server`'s `dhcpd.conf`:

```
option space sipdect;
option local-encapsulation code 43 = encapsulate sipdect;
option sipdect.ommip1 code 10 = ip-address;
option sipdect.ommip2 code 19 = ip-address;
option sipdect.syslogip code 14 = ip-address;
option sipdect.syslogport code 15 = integer 16;
option magic_str code 224 = text;

host myfancyrfp {
    hardware ethernet 00:30:42:cc:23:42;
    fixed-address 10.10.10.10;
    option host-name "myfancyrfp";
    # run OMM on the RFP itself
    option sipdect.ommip1 10.10.10.10;
    # or run OMM elsewhere (on a VM or other RFP)
    #option sipdect.ommip1 10.10.10.123;
    option magic_str = "OpenMobilitySIP-DECT";
}
```