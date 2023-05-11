Liste des commandes 
===============

# Switchs

## S1
```
Switch>en
S1#conf t
Switch#hostname S1 # déclarer le nom du switch
S1(config)#enable secret ^NqVqT$t7Jt8^A
S1(config)#vlan 10
S1(configt-vlan)#name commercial #déclarer le nom du vlan
S1(configt-vlan)#exit
S1(config)#vlan 20
S1(configt-vlan)#name administration
S1(configt-vlan)#exit
S1(config)#vlan 30
S1(configt-vlan)#name SI
S1(configt-vlan)#exit
S1(config)#vlan 40
S1(configt-vlan)#name services_internes
S1(configt-vlan)#exit
S1(config)#int fa0/1
S1(config-if)#switchport mode access #configurer le mode d'appartenance VLAN
S1(config-if)#switchport access vlan 10 #déclarer le vlan attaché au port
S1(config-if)#exit
S1(config)#int fa0/2
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 10
S1(config-if)#exit
S1(config)#int fa0/3
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 30
S1(config-if)#exit
S1(config)#int fa0/4
S1(config-if)#switchport mode access
S1(config-if)#switchprt access vlan 20
S1(config-if)#exit
S1(config)#int g0/1
S1(config-if)#switchport mode trunk #déclarer le mode trunk sur l'interface
S1(config-if)#exit
```

## S2

```
Switch>en
Switch#conf t
Switch(config)#hostname S2
S2(config)#enable secret &W!AaBUUR98n&t
S2(config)#int fa0/1
S2(config-if)#switchport mode access
S2(config-if)#switchport access vlan 30
S2(config-if)#exit
S2(config)#int fa0/2
S2(config-if)#switchport mode access
S2(config-if)#switchport access vlan 20
S2(config-if)#exit
S2(config)#int fa0/3
S2(config-if)#switchport mode access
S2(config-if)#switchport access vlan 10
S2(config-if)#exit
S2(config)#int fa0/4
S2(config-if)#switchport mode access
S2(config-if)#switchprt access vlan 10
S2(config-if)#exit
S2(config)#int fa0/5
S2(config-if)#switchport mode access
S2(config-if)#switchprt access vlan 40
S2(config-if)#exit
S2(config)#int fa0/6
S2(config-if)#switchport mode access
S2(config-if)#switchprt access vlan 40
S2(config-if)#exit
S2(config)#int g0/1
S2(config-if)#switchport mode trunk
S2(config-if)#exit
S2(config)#int g0/2
S2(config-if)#switchport mode trunk
S2(config-if)#exit
```

# Routeurs

## R1

```

Router>en
Router#conf t
Router(config)#hostname R1
R1(config)#enable secret &4fpN2J!pBr7Hb
R1(config)#int Loopback0 #déclarer une interface de loopback pour les échanges OSPF 
R1(config-if)#ip address 5.5.5.5 255.255.255.255 #définir une adresse IP à l'interface
R1(config-if)#exit
R1(config)#int fa0/0.10
R1(config-if)#ip address 192.168.10.254 255.255.255.0
R1(config-if)#encapsulation dot1Q 10 # déclarater l'utilisation du protocole dot1Q et de l'encapsulation du VLAN 10
R1(config-if)#ip helper-address 192.168.40.67 #déclarer le serveur DHCP
R1(config-if)#ip access-group commercial in #déclarer une ACL attachée à l'interface
R1(config-if)#ip nat inside #déclarer la translation d'adresse du côté privé
R1(config-if)#exit
R1(config)#int fa0/0.20
R1(config-if)#ip address 192.168.20.254 255.255.255.0
R1(config-if)#encapsulation dot1Q 20
R1(config-if)#ip helper-address 192.168.40.67
R1(config-if)#ip access-group administration in
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#int fa0/0.30
R1(config-if)#ip address 192.168.30.254 255.255.255.0
R1(config-if)#encapsulation dot1Q 30
R1(config-if)#ip helper-address 192.168.40.67
R1(config-if)#ip access-group technique in
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#int fa0/0.40
R1(config-if)#ip address 192.168.40.254 255.255.255.0
R1(config-if)#encapsulation dot1Q 40
R1(config-if)#exit
R1(config)#crypto isakmp policy 1 #définir les paramètres du protocole isakmp
R1(config-isakmp)#encr aes #utiliser l'algorithme de chiffrement AES
R1(config-isakmp)#authentication pre-share #authentification par mot de passe qui a été partagé au préalable
R1(config-isakmp)#group 5 #déclarer le groupe Diffie-Hellman
R1(config)#crypto isakmp key secret address 10.0.0.10 #déclaration du mot de passe pré-partagé 
R1(config)#crypto ipsec transform-set TS esp-aes esp-sha-hmac #utiliser AES pour le chiffrement et SHA-HMAC pour garantir l'intégrité des données
R1(config)#crypto map mymap 1 ipsec-isakmp #définir les paramètres de la carte de sécurité
R1(config-map)#set peer 10.0.0.10 #déclaration de l'adresse IP avec lequel le tunnel VPN sera créé 
R1(config-map)#set transform-set TS #utiliser les paramètres contenus dans "TS"
R1(config-map)#match address crypto #autoriser la plage d'adresse  adresses IP déclaver dans l'ACL "crypto"
R1(config-map)#exit
R1(config)#int fa0/1
R1(config-if)#ip address 200.0.0.253 255.255.255.0
R1(config-if)#ip ospf network point-to-point #déclaration de liaison point-à-point sur l'interface
R1(config-if)#ip ospf area 0 # déclaration de l'aire 0
R1(config-if)#ip access-group laptop in #déclaration de l'ACL à utiliser: "laptop"
R1(config-if)#ip nat outside #déclarer la translation d'adresse du côté public
R1(config-if)#crypto map mymap #déclarer la carte de sécurité attaché à l'interface
R3(config-if)#no shut #allumer l'interface
R1(config-if)#exit
R1(config)#router ospf 1 #déclarer le processus OSPF 1
R1(config-router)#network 200.0.0.0 0.0.0.255 area 0 #déclaration des réseaux à partager via OSPF
R1(config-router)#exit
R1(config)#ip route 0.0.0.0 0.0.0.0 200.0.0.254 #déclaration d'une route par défaut
R1(config)#ip access-list extended crypto #création d'une ACL nommée "crypto"
R1(config-ext-nacl)#permit ip 192.168.10.0 0.0.0.255 192.168.1.0 0.0.0.255
R1(config-ext-nacl)#exit
R1(config)#ip access-list extended administration
R1(config-ext-nacl)#deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
R1(config-ext-nacl)#permit tcp 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255 established
R1(config-ext-nacl)#permit icmp 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255 echo-reply
R1(config-ext-nacl)#permit ip any any
R1(config-ext-nacl)#exit
R1(config)#ip access-list extended commercial
R1(config-ext-nacl)#deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
R1(config-ext-nacl)#permit tcp 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255 established
R1(config-ext-nacl)#permit icmp 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255 echo-reply
R1(config-ext-nacl)#deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
permit ip any any
R1(config)#ip access-list extended technique
R1(config-ext-nacl)# permit ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
R1(config-ext-nacl)#permit ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
R1(config-ext-nacl)# permit ip 192.168.30.0 0.0.0.255 192.168.30.0 0.0.0.255
R1(config-ext-nacl)#permit ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
R1(config-ext-nacl)#deny ip any any
R1(config-ext-nacl)#exit
R1(config)#ip access-list extended laptop
R1(config-ext-nacl)#deny ip host 200.0.0.10 any
R1(config-ext-nacl)# permit ip any any
R1(config)#ip access-list extended nat
R1(config-ext-nacl)#deny ip 192.168.10.0 0.0.0.255 192.168.1.0 0.0.0.255
R1(config-ext-nacl)#permit ip 192.168.10.0 0.0.0.255 any
R1(config)#access-list 2 permit 192.168.20.0 0.0.0.255
R1(config)#ip nat inside source list 2 interface FastEthernet0/1 overload #déclarer la traduction d'adresse. Les paquets sortant du réseau auront comme adresse IP source l'adresse IP de l'interface FastEthernet0/1
R1(config)#ip nat inside source list nat interface FastEthernet0/1 overload
```

## R2

```
Router>en
Router#conf t
Router(config)#hostname R2
R2(config)#enable secret c2F4H3Kuf!qk^g
R2(config)#int Loopback0
R2(config-if)#ip address 4.4.4.4 255.255.255.255
R2(config-if)#exit
R2(config)#ip access-list extended laptop
R2(config-ext-nacl)#deny ip host 200.0.0.10 any
R2(config-ext-nacl)#permit ip any any
R2(config)#ip access-list extended laptopExterne
R2(config-ext-nacl)#deny ip any host 200.0.0.10
R2(config-ext-nacl)#permit ip any any
R2(config-ext-nacl)#exit
R2(config)#int fa0/0
R2(config-if)#ip address 200.0.0.254 255.255.255.0
R2(config-if)#ip ospf network point-to-point
R2(config-if)#ip ospf area 0
R2(config-if)#ip access-group laptop in
R3(config-if)#no shut
R2(config-if)#exit
R2(config)#int fa0/1
R2(config-if)#ip address 10.0.0.2 255.255.255.252
R2(config-if)#ip ospf network point-to-point
R2(config-if)#ip ospf area 0
R2(config-if)#ip access-group laptopExterne in
R3(config-if)#no shut
R2(config-if)#exit
```

## R3

```
Router>en
Router#conf t
R3(config)#hostname R3
R3(config)#enable secret kRt#a7!YE7F9CM 
R3(config)#interface Loopback0
R3(config-if)#ip address 2.2.2.2 255.255.255.255
R3(config-if)#exit
R3(config)#interface FastEthernet0/0
R3(config-if)#ip address 10.0.0.1 255.255.255.252
R3(config-if)#no shut
R3(config-if)#ip ospf network point-to-point
R3(config-if)#ip ospf 1 area 0
R3(config-if)#exit
R3(config)#interface FastEthernet0/1
R3(config-if)#ip address 10.0.0.13 255.255.255.252
R3(config-if)#no shut
R3(config-if)#exit
R3(config)#interface Ethernet0/0/0
R3(config-if)#ip address 10.0.0.9 255.255.255.252
R3(config-if)#ip ospf network point-to-point
R3(config-if)#ip ospf 1 area 0
R3(config-if)#no shut
R3(config-if)#exit
R3(config)#interface Ethernet0/1/0
R3(config-if)#ip address 10.0.0.5 255.255.255.252
R3(config-if)#ip ospf network point-to-point
R3(config-if)#ip ospf 1 area 0
R3(config-if)#no shut
R3(config-if)#exit
R3(config)#router ospf 1
R3(config-router)#network 10.0.0.8 0.0.0.3 area 0
R3(config-router)#network 10.0.0.4 0.0.0.3 area 0
R3(config-router)#network 10.0.0.0 0.0.0.3 area 0
R3(config-router)#network 10.0.0.12 0.0.0.3 area 0
R3(config-router)#exit
```

## R4

```
Router>en
Router#conf t
R4(config)#hostname R4 
R4(config)#enable secret 9*TA#f65Wiy$xv
R4(config)#interface Loopback0
R4(config-if)#ip address 1.1.1.1 255.255.255.255
R4(config-if)#exit
R4(config)#ip access-list extended crypto
R4(config-ext-nacl)#permit ip 192.168.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R4(config-ext-nacl)#access-list 1 permit 192.168.1.0 0.0.0.255
R4(config-ext-nacl)#exit
R4(config)#ip access-list extended nat
R4(config-ext-nacl)#deny ip 192.168.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R4(config-ext-nacl)#permit ip 192.168.1.0 0.0.0.255 any
R4(config-ext-nacl)#exit
R4(config)#crypto isakmp policy 1
R4(config-crypto)#encr aes
R4(config-crypto)#authentication pre-share
R4(config-crypto)#group 5
R4(config-crypto)#exit
R4(config)#crypto isakmp key secret address 200.0.0.253
R4(config)#crypto ipsec transform-set TS esp-aes esp-sha-hmac
R4(config)#crypto map mymap 1 ipsec-isakmp 
R4(config-crypto)#set peer 200.0.0.253
R4(config-crypto)#set transform-set TS 
R4(config-crypto)#match address crypto
R4(config-crypto)#exit
R4(config)#interface FastEthernet0/0
R4(config-if)#ip address 10.0.0.6 255.255.255.252
R4(config-if)#no shut
R4(config-if)#ip ospf network point-to-point
R4(config-if)#ip ospf 1 area 0
R4(config-if)#ip nat outside
R4(config-if)#crypto map mymap
R4(config-if)#exit
R4(config)#interface FastEthernet0/1
R4(config-if)#ip address 192.168.1.254 255.255.255.0
R4(config-if)#ip nat inside
R4(config-if)#no shut
R4(config)#router ospf 1
R4(config-router)#network 10.0.0.4 0.0.0.3 area 0
R4(config-router)#exit
```

## R5

```
Router>en
Router#conf t
R5(config)#hostname R5
R5(config)#enable secret !TJLc3NZjXB!cq
R5(config)#interface Loopback0
R5(config-if)#ip address 1.1.1.1 255.255.255.255
R5(config-if)#exit
R5(config)#ip access-list extended crypto
R5(config-ext-nacl)#permit ip 192.168.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R5(config-ext-nacl)#access-list 1 permit 192.168.1.0 0.0.0.255
R5(config-ext-nacl)#exit
R5(config)#ip access-list extended nat
R5(config-ext-nacl)#deny ip 192.168.1.0 0.0.0.255 192.168.10.0 0.0.0.255
R5(config-ext-nacl)#permit ip 192.168.1.0 0.0.0.255 any
R5(config-ext-nacl)#exit
R5(config)#crypto isakmp policy 1
R5(config-crypto)#encr aes
R5(config-crypto)#authentication pre-share
R5(config-crypto)#group 5
R5(config-crypto)#exit
R5(config)#crypto isakmp key secret address 200.0.0.253
R5(config)#crypto ipsec transform-set TS esp-aes esp-sha-hmac
R5(config)#crypto map mymap 1 ipsec-isakmp 
R5(config-crypto)#set peer 200.0.0.253
R5(config-crypto)#set transform-set TS 
R5(config-crypto)#match address crypto
R5(config-crypto)#exit
R5(config)#interface FastEthernet0/0
R5(config-if)#ip address 10.0.0.6 255.255.255.252
R5(config-if)#no shut
R5(config-if)#ip ospf network point-to-point
R5(config-if)#ip ospf 1 area 0
R5(config-if)#ip nat outside
R5(config-if)#crypto map mymap
R5(config-if)#exit
R5(config)#interface FastEthernet0/1
R5(config-if)#ip address 192.168.1.254 255.255.255.0
R5(config-if)#ip nat inside
R5(config-if)#no shut
R5(config-if)#exit
R5(config)#router ospf 1
R5(config-router)#network 10.0.0.8 0.0.0.3 area 0
R5(config-router)#exit
R3(config)#ip nat inside source list nat interface FastEthernet0/0 overload
```

