# B1A-tp-6-reseau

Mon gns.
![Le gns trop beau](Putain-c-bo.PNG)


Petit tableau volé comme un flemmard

Réseaux | `area 0` | `area 1` | `area 2` | Commentaire
--- | --- | --- | --- | ---
`10.6.100.0/30` | X | - | - | Liaison entre `r1` et `r2`
`10.6.100.4/30` | X | - | - | Liaison entre `r1` et `r4`
`10.6.100.8/30` | X | - | - | Liaison entre `r2` et `r3` 
`10.6.100.12/30` | X | - | - | Liaison entre `r3` et `r4`
`10.6.101.0/30` | - | X | - | Liaison entre `r3` et `r5`
`10.6.201.0/24` | - | X | - | Réseau des clients
`10.6.202.0/24` | - | - | X | Réseau des serveurs

### Adressage IP de chacune des machines

Machines | `10.6.100.0/30` | `10.6.100.4/30` | `10.6.100.8/30` | `10.6.100.12/30` | `10.6.101.0/30` | `10.6.201.0/24` | `10.6.202.0/24`
--- | --- | --- | --- | --- | --- | --- | --- 
`r1.tp6.b1` | `10.6.100.1` | `10.6.100.5` | - | - | - | - | `10.6.202.254`
`r2.tp6.b1` | `10.6.100.2` | - |  `10.6.100.9` | - | - | - | -
`r3.tp6.b1` | - | - | `10.6.100.10` | `10.6.100.14` | `10.6.101.1` | - | -
`r4.tp6.b1` | - |  `10.6.100.6` | - | `10.6.100.13` | - | - | -
`r5.tp6.b1` | - | - | - | - |  `10.6.101.2` |  `10.6.201.254` | -
`client1.tp6.b1` | - | - | - | - | - |  `10.6.201.10` | -
`client2.tp6.b1` | - | - | - | - | - |  `10.6.201.11` | -
`server1.tp6.b1` | - | - | - | - | - | - | `10.6.202.10`

Les petits traceroutes de la beauté:

Serveur 1:
```
[root@server1 ~]# traceroute client2
traceroute to client2 (10.6.201.11), 30 hops max, 60 byte packets
 1  gateway (10.6.202.254)  10.634 ms  10.550 ms  10.545 ms
 2  10.6.100.2 (10.6.100.2)  20.494 ms  20.440 ms  20.374 ms
 3  10.6.100.10 (10.6.100.10)  41.650 ms  41.614 ms  41.550 ms
 4  10.6.101.2 (10.6.101.2)  61.679 ms  61.633 ms  61.567 ms
 5  client2 (10.6.201.11)  71.237 ms !X  71.194 ms !X  71.175 ms !X
[root@server1 ~]# traceroute client1
traceroute to client1 (10.6.201.10), 30 hops max, 60 byte packets
 1  gateway (10.6.202.254)  2.854 ms  2.721 ms  2.649 ms
 2  10.6.100.6 (10.6.100.6)  24.591 ms  24.610 ms  24.533 ms
 3  10.6.100.14 (10.6.100.14)  44.786 ms  44.747 ms  44.676 ms
 4  10.6.101.2 (10.6.101.2)  65.408 ms  65.394 ms  65.473 ms
 5  client1 (10.6.201.10)  77.607 ms !X  77.614 ms !X  77.558 ms !X
```

Client 1:
```
[root@client1 ~]# traceroute server1
traceroute to server1 (10.6.202.10), 30 hops max, 60 byte packets
 1  gateway (10.6.201.254)  8.317 ms  8.206 ms  8.136 ms
 2  10.6.101.1 (10.6.101.1)  18.699 ms  18.647 ms  18.682 ms
 3  10.6.100.13 (10.6.100.13)  39.497 ms  39.435 ms  39.775 ms
 4  10.6.100.5 (10.6.100.5)  60.513 ms  60.472 ms  60.492 ms
 5  server1 (10.6.202.10)  60.417 ms !X  60.338 ms !X  60.245 ms !X
[root@client1 ~]# traceroute client2
traceroute to client2 (10.6.201.11), 30 hops max, 60 byte packets
 1  client2 (10.6.201.11)  1.153 ms !X  1.053 ms !X  1.152 ms !X
```

Client 2:
```
[root@client2 ~]# traceroute server1
traceroute to server1 (10.6.202.10), 30 hops max, 60 byte packets
 1  gateway (10.6.201.254)  9.656 ms  9.597 ms  9.514 ms
 2  10.6.101.1 (10.6.101.1)  32.498 ms  32.466 ms  32.385 ms
 3  10.6.100.13 (10.6.100.13)  52.868 ms  52.804 ms  52.829 ms
 4  10.6.100.5 (10.6.100.5)  73.870 ms  73.810 ms  73.783 ms
 5  server1 (10.6.202.10)  84.462 ms !X  84.422 ms !X  84.597 ms !X
[root@client2 ~]# traceroute client1
traceroute to client1 (10.6.201.10), 30 hops max, 60 byte packets
 1  client1 (10.6.201.10)  0.665 ms !X  0.783 ms !X  0.694 ms !X
```


# Lab 3 : Let's end this properly


Service | Qui porte le service ? | Pour qui ? | Pourquoi ? 
--- | --- | --- | ---
**NAT** | `r4.tp6.b1` | tout le monde (routeurs & VMs) | Le NAT permet d'accéder à l'extérieur, il permet de sortir du LAN. Toutes les machines peuvent en avoir besoin dans notre petite infra
**Serveur Web** | `server1.tp6.b1` | réseau client `10.6.201.0/24` | Le serveur Web symbolise un service d'infra en interne. Dispo pour nos clients. Plus de détails dans la section dédiée.
**DHCP** | `client2.tp6.b1` | réseau client `10.6.201.0/24` | Le DHCP (qui permet d'attribuer des IPs automatiquement) c'est pour des clients. Pas pour des serveurs. Un serveur, on veut qu'il ait une IP fixe. 
**DNS** | `server1.tp6.b1` | tout le monde (routeurs & VMs) | Le DNS nous permettra de résoudre les noms de domaines en local et nous passer du fichier `/etc/hosts`
**NTP** | `server1.tp6.b1` | réseau serveur `10.6.202.0/24` | Le NTP, qui permet la synchronisation de l'heure, est souvent indispensable pourdes serveurs mais totalement négligeable pour des clients (genre vos PCs, s'ils sont pas à l'heure, tout le monde s'en fout)

**Avec tout ça, on a un petit réseau propre, carré, et (presque) autonome !**

---

## 1. NAT : accès internet

J'ai internet sur R2:
```
router2#telnet trip-hop.net 80
Translating "trip-hop.net"...domain server (8.8.8.8) [OK]
Trying trip-hop.net (213.186.33.4, 80)... Open
GET/
HTTP/1.1 400 Bad Request
Server: squid
Mime-Version: 1.0
Date: Tue, 05 Mar 2019 16:12:35 GMT
Content-Type: text/html;charset=utf-8
Content-Length: 4104
X-Squid-Error: ERR_INVALID_REQ 0
Vary: Accept-Language
Content-Language: fr
X-Cache: MISS from PF1-BOR1FR
X-Cache-Lookup: NONE from PF1-BOR1FR:3128
Connection: close

```

J'ai internet sur mes VM:
```
[root@client1 network-scripts]# dig google.com @8.8.8.8
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             287     IN      A       216.58.213.174

;; Query time: 135 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Tue Mar 05 17:08:27 CET 2019
;; MSG SIZE  rcvd: 55
```

## 2. Un service d'infra

Curl depuis le client 1 vers le localhost de server 1:
```
[root@client1 etc]# curl server1
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Fedora</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <style type="text/css">
            /*<![CDATA[*/
```
Y'a encore plein d'autre ligne, mais bon, c'est long





## 3. Serveur DHCP

Bonjour, je suis la nouvelle adresse IP de client1 avec un nouveau `dhclient -v` et un `ip a` à la suite:

```
[root@client1 network-scripts]# dhclient -v
Internet Systems Consortium DHCP Client 4.2.5
Copyright 2004-2013 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/enp0s8/08:00:27:9b:9f:67
Sending on   LPF/enp0s8/08:00:27:9b:9f:67
Listening on LPF/enp0s3/08:00:27:be:8b:15
Sending on   LPF/enp0s3/08:00:27:be:8b:15
Sending on   Socket/fallback
DHCPDISCOVER on enp0s8 to 255.255.255.255 port 67 interval 3 (xid=0x4f731e4a)
DHCPDISCOVER on enp0s3 to 255.255.255.255 port 67 interval 7 (xid=0x40d6f405)
DHCPREQUEST on enp0s8 to 255.255.255.255 port 67 (xid=0x4f731e4a)
DHCPOFFER from 192.168.15.2
DHCPACK from 192.168.15.2 (xid=0x4f731e4a)
bound to 192.168.15.5 -- renewal in 535 seconds.
[root@client1 network-scripts]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:be:8b:15 brd ff:ff:ff:ff:ff:ff
    inet 10.6.201.50/24 brd 10.6.201.255 scope global noprefixroute dynamic enp0s3
       valid_lft 587sec preferred_lft 587sec
    inet 10.6.201.51/24 brd 10.6.201.255 scope global secondary noprefixroute dynamic enp0s3
       valid_lft 470sec preferred_lft 470sec
    inet6 fe80::a00:27ff:febe:8b15/64 scope link
       valid_lft forever preferred_lft forever
```

## 4. Serveur DNS

Et voila !!!!!!

Le DNS qui fonctionne:
```
[root@server1 ~]# ping client1.tp6.b1
PING client1.tp6.b1 (10.6.201.50) 56(84) bytes of data.
64 bytes from 10.6.201.50 (10.6.201.50): icmp_seq=2 ttl=60 time=77.7 ms
64 bytes from 10.6.201.50 (10.6.201.50): icmp_seq=3 ttl=60 time=85.3 ms
```


et en plus comme tu me l'as indiqué, mon DHCP donne l'adresse du DNS:
```
subnet 10.6.201.0 netmask 255.255.255.0 {
  range 10.6.201.50 10.6.201.70;
  option domain-name "tp6.b1";
  option routers 10.6.201.254;
  option broadcast-address 10.6.201.255;
  option domain-name-servers 10.6.202.10, 8.8.8.8;
}
```

## 5. Serveur NTP

Juste histoire de mettre sur le MD, je relance tout, j'oublie pas de start nginx/named  sur server1 et je start le dhcpd sur client2 et je start chronyd et les 2 lignes de commandes suivantes sur toutes mes VM

  * `sudo chronyc sources`
  * `sudo chronyc tracking`

  Machines | Server1 | Client1 | DHCP | Explication
--- | --- | --- | --- | --- |
Reference ID | `05C4A08B (ip139.ip-5-196-160.eu)` | `0A06CA0A (server1.tp6.b1)` | `0A06CA0A (server1.tp6.b1)` | référence et nom de où les vm sont connectées
Stratum | `3` | `4` |  `4` | nombre de saut
Ref time (UTC) | `Fri Mar 15 17:25:21 2019` | `Fri Mar 15 17:25:30 2019` | `Fri Mar 15 17:25:26 2019` | heure de la dernière mesure
System time | `0.000927681 seconds fast of NTP time` |  `0.000743395 seconds slow of NTP time` | `0.000888403 seconds fast of NTP time` | correction de l'heure du aux différents saut et autre
Last offset | `+0.002387839 seconds` | `-0.001347712 seconds` | `-0.000499424 seconds` | décalage estimé lors de la dernière maj de l'horloge
RMS offset | `0.002262098 seconds` | `0.529418588 seconds` | `0.006499424 seconds` | moyenne à long terme de la valeur de décalage
Frequency | `3.558 ppm slow` | `9.011 ppm slow` | `0.372 ppm fast` | vitesse à laquelle l'horloge système serait erronée su chronyd ne la corrigeaut pas
Residual fred | `+0.014 ppm` | `-3.228 ppm` | `-0.010 ppm` | j'ai pas bien compris là
Skew | `3.746 ppm` | `26.287 ppm` | `11.292 ppm`| estimation de la limite de l'erruer sur la fréquence
Root delay | `0.062375706 seconds` | `0.121448161 seconds` | `0.128378615 seconds` | délais du chemin réseau vers l'ordiateur qui donne l'heure (ici server1.tp6.b1)
Root disperson | ` 0.006919123 seconds` | `0.003123883 seconds` | ` 0.003269079 seconds` | délais du chemin vers l'ordiateur qui donne l'heure (ici server1.tp6.b1)
Update interval | `388.9 seconds` | `130.5 seconds` | `150.0 seconds` | depuis quand il est allumé
Leap status | `Normal` | `Normal` | `Normal` | statut de l'intercalaire 
---

# Bilan

* on a des **clients** 
  * configurés automatiquement dès qu'on les ajoute grâce au serveur DHCP
    * IP 
    * passerelle
    * serveur DNS
  * peuvent joindre internet sans pb avec un navigateur web
  * peuvent joindre le serveur web (NGINX) en interne
* on a des **serveurs**
  * ils sont synchros au niveau de l'heure grâce à NTP
  * ils sont en charge de la résolution de nom locale avec le serveur DNS
  * ils fournissent des services d'infra (symbolisé par notre serveur web NGINX)
* on a des **routeurs**
  * qui mettent en place un routage dynamique (OSPF)
  * qui fournissent un accès WAN (Internet) grâce au NAT

# Aller plus loin

Pistes pour aller plus loin (l'ordre n'est pas important), mp si vous voulez + d'infos :
* Switch
    * J'ai réussi à installer les switch de cisco et à les faire fonctionner par gns, mais je n'ai pas réussi à les configs :/
