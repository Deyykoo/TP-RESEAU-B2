# I. Dumb switch

🌞 Commençons simple
```
- PC1> ip 10.3.1.1 255.255.255.0
  Checking for duplicate address...
  PC1 : 10.3.1.1 255.255.255.0



- PC2> ip 10.3.1.2 255.255.255.0
  Checking for duplicate address...
  ping PC2 : 10.3.1.2 255.255.255.0



- PC1> ping 10.3.1.2

  84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=1.510 ms
  84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=0.824 ms



- PC2> ping 10.3.1.1

  84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=0.560 ms
  84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=0.464 ms



- IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/1
   1    0050.7966.6801    DYNAMIC     Et0/2
Total Mac Addresses for this criterion: 2
```

# II. VLAN

🌞 Adressage
```
- PC1> ping 10.3.1.2

  84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=0.542 ms
  84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=0.966 ms


- PC1> ping 10.3.1.3

  84 bytes from 10.3.1.3 icmp_seq=1 ttl=64 time=1.082 ms
  84 bytes from 10.3.1.3 icmp_seq=2 ttl=64 time=1.131 ms




- PC2> ping 10.3.1.1

  84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=1.195 ms
  84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=1.620 ms


- PC2> ping 10.3.1.3

  84 bytes from 10.3.1.3 icmp_seq=1 ttl=64 time=0.445 ms
  84 bytes from 10.3.1.3 icmp_seq=2 ttl=64 time=0.706 ms




- PC3> ping 10.3.1.1

  84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=0.606 ms
  84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=0.661 ms


- PC3> ping 10.3.1.2

  84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=0.748 ms
  84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=0.627 ms
```

🌞 Configuration des VLANs
```
IOU1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et1/0, Et1/1, Et1/2
                                                Et1/3, Et2/0, Et2/1, Et2/2
                                                Et2/3, Et3/0, Et3/1, Et3/2
                                                Et3/3
🌞10   VLAN0010                         active    Et0/1, Et0/2🌞
🌞20   VLAN0020                         active    Et0/3🌞
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup
```

🌞 Vérif
```
- PC1> ping 10.3.1.2

  84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=1.557 ms
  84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=2.245 ms


- PC1> ping 10.3.1.3

  host (10.3.1.3) not reachable  




- PC2> ping 10.3.1.1

  84 bytes from 10.3.1.1 icmp_seq=1 ttl=64 time=0.718 ms
  84 bytes from 10.3.1.1 icmp_seq=2 ttl=64 time=2.336 ms


- PC2> ping 10.3.1.3

  host (10.3.1.3) not reachable




- PC3> ping 10.3.1.1

  host (10.3.1.1) not reachable


- PC3> ping 10.3.1.2

  host (10.3.1.2) not reachable
```

# III. Ptite VM DHCP

🌞 VM dhcp.tp3.b2
```
- PC4> dhcp

  DORA IP 10.1.1.100/24 GW 10.1.1.0


- PC4> ping 10.1.1.100

  10.1.1.100 icmp_seq=1 ttl=64 time=0.001 ms
  10.1.1.100 icmp_seq=2 ttl=64 time=0.001 ms
  10.1.1.100 icmp_seq=3 ttl=64 time=0.001 ms
  10.1.1.100 icmp_seq=4 ttl=64 time=0.001 ms
  10.1.1.100 icmp_seq=5 ttl=64 time=0.001 ms


  - PC5> dhcp
  DDD^C
  Can't find dhcp server
```