# I. Dumb switch

🌞 Commençons simple
```
- PC1> ip 10.1.1.101 255.255.255.0
  Checking for duplicate address...
  PC1 : 10.1.1.101 255.255.255.0



- PC2> ip 10.1.1.102 255.255.255.0
  Checking for duplicate address...
  PC2 : 10.1.1.102 255.255.255.0



- PC2> ping 10.1.1.101

  84 bytes from 10.1.1.101 icmp_seq=1 ttl=64 time=0.865 ms
  84 bytes from 10.1.1.101 icmp_seq=2 ttl=64 time=1.173 ms



- PC1> ping 10.1.1.102

  84 bytes from 10.1.1.102 icmp_seq=1 ttl=64 time=1.439 ms
  84 bytes from 10.1.1.102 icmp_seq=2 ttl=64 time=1.392 ms



- IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/1
   1    0050.7966.6801    DYNAMIC     Et0/2
Total Mac Addresses for this criterion: 2
```