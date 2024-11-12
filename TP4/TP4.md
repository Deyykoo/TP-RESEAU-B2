# I. Topo 1 : VLAN et Routing

🌞 Adressage
```
- VPCS> ip 10.1.10.1
  Checking for duplicate address...
  VPCS : 10.1.10.1 255.255.255.0


- VPCS> ip 10.1.10.2
  Checking for duplicate address...
  VPCS : 10.1.10.2 255.255.255.0


- VPCS> ip 10.1.20.1
  Checking for duplicate address...
  VPCS : 10.1.20.1 255.255.255.0


- [quentin@web1 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:4b:c2:7d brd ff:ff:ff:ff:ff:ff
    🌞inet 10.1.30.1/24🌞 brd 10.1.30.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe4b:c27d/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:49:59:a0 brd ff:ff:ff:ff:ff:ff
    inet 192.168.95.3/24 brd 192.168.95.255 scope global dynamic noprefixroute enp0s8
       valid_lft 323sec preferred_lft 323sec
    inet6 fe80::4ca9:39c9:9f48:5feb/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

🌞 Configuration des VLANs
```
- VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et1/1, Et1/2, Et1/3
                                                Et2/0, Et2/1, Et2/2, Et2/3
                                                Et3/0, Et3/1, Et3/2, Et3/3
10   VLAN0010                         active   🌞 Et0/1, Et0/2 🌞
20   VLAN0020                         active   🌞 Et0/3 🌞
30   VLAN0030                         active   🌞Et1/0 🌞
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
10   enet  100010     1500  -      -      -        -    -        0      0
20   enet  100020     1500  -      -      -        -    -        0      0
30   enet  100030     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0
1003 tr    101003     1500  -      -      -        -    -        0      0
1004 fdnet 101004     1500  -      -      -        ieee -        0      0


- IOU1# show int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       none
```

🌞 Config du routeur
```
- R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES unset  up                    up
FastEthernet0/0.10         10.1.10.254     YES manual up                    up
FastEthernet0/0.20         10.1.20.254     YES manual up                    up
FastEthernet0/0.30         10.1.30.254     YES manual up                    up
FastEthernet1/0            unassigned      YES unset  administratively down down
FastEthernet1/1            unassigned      YES unset  administratively down down
FastEthernet2/0            unassigned      YES unset  administratively down down
FastEthernet2/1            unassigned      YES unset  administratively down down
```

🌞 Vérif
```
- VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.1.10.1/24
GATEWAY     : 🌞10.1.10.254🌞
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500


- VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.1.10.2/24
GATEWAY     : 🌞10.1.10.254🌞
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20013
RHOST:PORT  : 127.0.0.1:20014
MTU         : 1500


- VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.1.20.1/24
GATEWAY     : 🌞10.1.20.254🌞
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20015
RHOST:PORT  : 127.0.0.1:20016
MTU         : 1500


- [quentin@web1 ~]$ ip r s
🌞default via 10.1.30.254 dev enp0s3 proto static metric 100🌞
  10.1.30.0/24 dev enp0s3 proto kernel scope link src 10.1.30.1 metric 100
  192.168.95.0/24 dev enp0s8 proto kernel scope link src 192.168.95.3 metric 101




- VPCS> ping 10.1.20.1

84 bytes from 10.1.20.1 icmp_seq=1 ttl=63 time=31.241 ms
84 bytes from 10.1.20.1 icmp_seq=2 ttl=63 time=16.838 ms
84 bytes from 10.1.20.1 icmp_seq=3 ttl=63 time=28.644 ms
84 bytes from 10.1.20.1 icmp_seq=4 ttl=63 time=19.183 ms
84 bytes from 10.1.20.1 icmp_seq=5 ttl=63 time=17.197 ms


- VPCS> ping 10.1.30.1

84 bytes from 10.1.30.1 icmp_seq=1 ttl=63 time=23.008 ms
84 bytes from 10.1.30.1 icmp_seq=2 ttl=63 time=17.407 ms
84 bytes from 10.1.30.1 icmp_seq=3 ttl=63 time=14.544 ms
84 bytes from 10.1.30.1 icmp_seq=4 ttl=63 time=15.449 ms
84 bytes from 10.1.30.1 icmp_seq=5 ttl=63 time=17.333 ms
```