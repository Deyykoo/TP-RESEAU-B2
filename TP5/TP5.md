# I. Tester

🌞 Récupérer l'application dans la VM hosting.tp5.b1  
- *client.py* & *server.py*

🌞 Essayer de lancer l'app
```
- [quentin@hosting ~]$ 🌞python server.py
    ^CTraceback (most recent call last):
    File "/home/quentin/server.py", line 8, in <module>
        conn, addr = s.accept()
    File "/usr/lib64/python3.9/socket.py", line 293, in accept
        fd, addr = self._accept()
    KeyboardInterrupt


- [quentin@hosting ~]$ss -natu | grep 127.0.0.1
udp   UNCONN    0      0             127.0.0.1:323        0.0.0.0:*
tcp   TIME-WAIT 0      0             🌞127.0.0.1:57150🌞    127.0.0.1:13337
```

🌞 Tester l'app depuis hosting.tp5.b1
```
- [quentin@hosting ~]$ sudo nano client.py

    import socket

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    🌞 s.connect(('127.0.0.1', 13337))
    s.send('Hello'.encode())

    # On reçoit la string Hello
    data = s.recv(1024)

    # Récupération d'une string utilisateur
    msg = input("Calcul à envoyer: ")

    # On envoie
    s.send(msg.encode())

    # Réception et affichage du résultat
    s_data = s.recv(1024)
    print(s_data.decode())
    s.close()


- [quentin@hosting ~]$🌞 python client.py
  Calcul à envoyer: 3+3
  6
```

# II. Intégrer

🌞 Créer un dossier /opt/calculatrice
```
- [quentin@hosting ~]$ sudo mkdir /opt/calculatrice


- [quentin@hosting ~]$ sudo mv server.py /opt/calculatrice/


- [quentin@hosting ~]$ sudo mv client.py /opt/calculatrice/
```

🌞 Créer le fichier /etc/systemd/system/calculatrice.service

```
- [quentin@hosting ~]$ sudo touch /etc/systemd/system/calculatrice.service


- [quentin@hosting ~]$ sudo chmod 644 /etc/systemd/system/calculatrice.service


- [quentin@hosting ~]$ sudo cat /etc/systemd/system/calculatrice.service
    [Unit]
    Description=Super calculatrice réseau

    [Service]
    ExecStart=/usr/bin/python /opt/calculatrice/server.py

    [Install]
    WantedBy=multi-user.target
```

🌞 Démarrer le service
```
[quentin@hosting ~]$ sudo systemctl status calculatrice.service
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; disabled; preset: disabled)
     Active: 🌞active (running)🌞 since Thu 2024-10-24 16:50:33 CEST; 8min ago
   Main PID: 1820 (python)
      Tasks: 1 (limit: 23159)
     Memory: 3.3M
        CPU: 10ms
     CGroup: /system.slice/calculatrice.service
             └─1820 /usr/bin/python /opt/calculatrice/server.py



- [quentin@hosting ~]$ ss -natu
Netid      State       Recv-Q      Send-Q              Local Address:Port            Peer Address:Port      Process
udp        ESTAB       0           0                10.0.2.15%enp0s8:68                  10.0.2.2:67
udp        UNCONN      0           0                       127.0.0.1:323                  0.0.0.0:*
udp        UNCONN      0           0                           [::1]:323                     [::]:*
tcp        LISTEN      0           128                       0.0.0.0:22                   0.0.0.0:*
tcp        LISTEN      0           1                         0.0.0.0:13337                0.0.0.0:*
tcp        ESTAB       0           0                       10.1.1.10:22                  10.1.1.2:51263
tcp        LISTEN      0           128                          [::]:22                      [::]:*
```

🌞 Configurer une politique de redémarrage dans le fichier

```
[quentin@hosting ~]$ sudo cat /etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
ExecStart=/usr/bin/python /opt/calculatrice/server.py
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```


🌞 Tester que la politique de redémarrage fonctionne

```
- [quentin@hosting ~]$ systemctl status calculatrice
Warning: The unit file, source configuration file or drop-ins of calculatrice.service changed on disk. Run 'systemctl daemon-reload' to reload units.
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; disabled; preset: disabled)
     Active: active (running) since Thu 2024-10-24 17:38:47 CEST; 2min 31s ago
   Main PID: 2938 (python)
      Tasks: 1 (limit: 23159)
     Memory: 3.3M
        CPU: 11ms
     CGroup: /system.slice/calculatrice.service
             └─2938 /usr/bin/python /opt/calculatrice/server.py

Oct 24 17:38:47 hosting.tp5.b1 systemd[1]: Started Super calculatrice réseau.


- [quentin@hosting ~]$ sudo kill -9 2938


- [quentin@hosting ~]$ systemctl status calculatrice
Warning: The unit file, source configuration file or drop-ins of calculatrice.service changed on disk. Run 'systemctl daemon-reload' to reload units.
● calculatrice.service - Super calculatrice réseau
     Loaded: loaded (/etc/systemd/system/calculatrice.service; disabled; preset: disabled)
     Active: active (running) since Thu 2024-10-24 17:41:40 CEST; 6s ago
   Main PID: 3321 (python)
      Tasks: 1 (limit: 23159)
     Memory: 3.4M
        CPU: 13ms
     CGroup: /system.slice/calculatrice.service
             └─3321 /usr/bin/python /opt/calculatrice/server.py

Oct 24 17:41:40 hosting.tp5.b1 systemd[1]: Started Super calculatrice réseau.
```

🌞 Ouverture automatique du firewall dans le fichier 

```
[quentin@hosting ~]$ sudo cat /etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
ExecStartPre=/usr/bin/firewall-cmd --zone=public --add-port=13337/tcp --permanent
ExecStartPre=/usr/bin/firewall-cmd --reload
ExecStart=/usr/bin/python /opt/calculatrice/server.py
Restart=on-failure
RestartSec=10
ExecStopPost=/usr/bin/firewall-cmd --zone=public --remove-port=13337/tcp --permanent
ExecStopPost=/usr/bin/firewall-cmd --reload

[Install]
WantedBy=multi-user.target
```
🌞 Vérifier l'ouverture automatique du firewall
```
[quentin@hosting ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 13337/tcp 
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

# 3. Monitoring

🌞 Installer Netdata
```
[quentin@hosting ~]$ wget -O /tmp/netdata-kickstart.sh https://get.netdata.cloud/kickstart.sh && sh /tmp/netdata-kickstart.sh
--2024-10-25 17:04:31--  https://get.netdata.cloud/kickstart.sh
Resolving get.netdata.cloud (get.netdata.cloud)... 104.22.78.229, 104.22.79.229, 172.67.36.172, ...
Connecting to get.netdata.cloud (get.netdata.cloud)|104.22.78.229|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 93645 (91K) [application/octet-stream]
Saving to: ‘/tmp/netdata-kickstart.sh’

/tmp/netdata-kickstart.sh     100%[=================================================>]  91.45K  --.-KB/s    in 0.001s

2024-10-25 17:04:31 (89.6 MB/s) - ‘/tmp/netdata-kickstart.sh’ saved [93645/93645]
```

🌞 Configurer une sonde TCP
