# I. Basics

## ☀️ Carte réseau WiFi

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :  
   Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6   AX201 160MHz  
   ☀️ **Adresse physique . . . . . . . . . . . : 00-93-37-9D-52-7E**  
   DHCP activé. . . . . . . . . . . . . . : Oui  
   Configuration automatique activée. . . : Oui  
   Adresse IPv6 de liaison locale. . . . .:   fe80::a507:f583:203:1111%22(préféré)  
   ☀️ **Adresse IPv4. . . . . . . . . . . . . .: 10.33.73.78(préféré)**  
   ☀️ **Masque de sous-réseau. . . . . . . . . : 255.255.240.0**   
   Bail obtenu. . . . . . . . . . . . . . : jeudi 3 octobre 2024 13:59:45  
   Bail expirant. . . . . . . . . . . . . : vendredi 4 octobre 2024 13:59:45  
   Passerelle par défaut. . . . . . . . . : 10.33.79.254  
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254  
   IAID DHCPv6 . . . . . . . . . . . : 151032631  
   DUID de client DHCPv6. . . . . . . . :   00-01-00-01-2B-48-F3-DF-A0-36-BC-6C-20-80  
   Serveurs DNS. . .  . . . . . . . . . . : 1.1.1.1  
                                         DoH: https://cloudflare-dns.com/dns-query  
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé


## ☀️ Déso pas déso

L'adresse de réseau du LAN : 10.33.73.78 

L'adresse de broadcast : 10.33.73.254

L'adresse IP dispo : 254 hôtes


## ☀️ Hostname

PS C:\Users\qchai> hostname :
LAPTOP-B5KBDAGV

## ☀️ Passerelle du réseau
```
Interface : 10.33.73.78 --- 0x16  
  Adresse Internet      Adresse physique      Type  
 ☀️ 10.33.79.254          7c-5a-1c-d3-d8-76     dynamique  
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique  
  224.0.0.2             01-00-5e-00-00-02     statique  
  224.0.0.22            01-00-5e-00-00-16     statique  
  224.0.0.251           01-00-5e-00-00-fb     statique  
  224.0.0.252           01-00-5e-00-00-fc     statique  
  239.255.255.250       01-00-5e-7f-ff-fa     statique  
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique  
```
## ☀️ Serveur DHCP et DNS

**Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254**  
**Serveurs DNS. . .  . . . . . . . . . . : 1.1.1.1**

## ☀️ Table de routage
```
Itinéraires actifs :  
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique  
          0.0.0.0          0.0.0.0     10.33.79.254      10.33.73.78     30  
```

# II. Go further

## ☀️ Hosts ?
```
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host
# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
	1.1.1.1		b2.hello.vous
```

## ☀️ Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...
``` 
TCP    10.33.73.78:49799      172.65.251.78:443      ESTABLISHED

- adresse ip du serveur : 172.65.251.78
- le port du serveur auquel vous êtes connectés : 49799
- le port que votre PC a ouvert en local pour se connecter au port du serveur distant : 443
```

## ☀️ Requêtes DNS

```
PS C:\Users\qchai>☀️ nslookup www.thinkerview.com
Serveur :   one.one.one.one
Address:  1.1.1.1

Réponse ne faisant pas autorité :
Nom :    www.thinkerview.com
Addresses:  2a06:98c1:3120::6
          2a06:98c1:3121::6
          188.114.96.6
          188.114.97.6


PS C:\Users\qchai>☀️ nslookup 143.90.88.12
Serveur :   one.one.one.one
Address:  1.1.1.1

Nom :    EAOcf-140p12.ppp15.odn.ne.jp
Address:  143.90.88.12
```

## ☀️ Hop hop hop

2^8 – 2 = 254 machines.

## ☀️ IP publique

# III. Le requin

## ☀️ Capture ARP
[Lien vers capture ARP](../ARP.pcap)
## ☀️ Capture DNS
[Lien vers capture DNS](../DNS.pcap)
## ☀️ Capture TCP
[Lien vers capture TCP](../TCP.pcap)