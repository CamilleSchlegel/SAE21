Liste des commandes 
===============

# Switchs

## S1
```
Switch>en
Switch#hostname S1
S1#conf t
S1(config)#int fa0/1
S1(config-if)#switchport access vlan 10
S1(config-if)#switchport mode access
S1(config-if)#exit
S1(config)#int fa0/2
S1(config-if)#switchport access vlan 10
S1(config-if)#switchport mode access
S1(config-if)#exit
S1(config)#int fa0/3
S1(config-if)#switchport access vlan 30
S1(config-if)#switchport mode access
S1(config-if)#exit
S1(config)#int fa0/4
S1(config-if)#switchprt access vlan 20
S1(config-if)#switchport mode access
S1(config-if)#exit
S1(config)#int g0/1
S1(config-if)#switchport mode trunk
S1(config-if)#exit
```

## S2

```
Switch>en
Switch#hostname S2
S2#conf t
S2(config)#int fa0/1
S2(config-if)#switchport access vlan 30
S2(config-if)#switchport mode access
S2(config-if)#exit
S2(config)#int fa0/2
S2(config-if)#switchport access vlan 20
S2(config-if)#switchport mode access
S2(config-if)#exit
S2(config)#int fa0/3
S2(config-if)#switchport access vlan 10
S2(config-if)#switchport mode access
S2(config-if)#exit
S2(config)#int fa0/4
S2(config-if)#switchprt access vlan 10
S2(config-if)#switchport mode access
S2(config-if)#exit
S2(config)#int fa0/5
S2(config-if)#switchprt access vlan 40
S2(config-if)#switchport mode access
S2(config-if)#exit
S2(config)#int fa0/6
S2(config-if)#switchprt access vlan 40
S2(config-if)#switchport mode access
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
Router#hostname R1
R1#conf t
R1(config)#int Loopback0
R1(config-if)#ip address 5.5.5.5 255.255.255.255
R1(config-if)#exit
R1(config)#int fa0/0.10
R1(config-if)#ip address 192.168.10.254 255.255.255.0
R1(config-if)#encapsulation dot1Q 10
R1(config-if)#ip helper-address 192.168.40.67
R1(config-if)#ip access-group commercial in
R1(config-if)#ip nat inside
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
R1(config)#crypto isakmp policy 1
R1(config-isakmp)#encr aes
R1(config-isakmp)#authentication pre-share
R1(config-isakmp)#group 5
R1(config)#crypto isakmp key secret address 10.0.0.10
R1(config)#crypto ipsec transform-set TS esp-aes esp-sha-hmac
R1(config)#crypto map mymap 1 ipsec-isakmp
R1(config-map)#set peer 10.0.0.10
R1(config-map)#set transform-set TS
R1(config-map)#match address crypto
R1(config-map)#exit
R1(config)#int fa0/1
R1(config-if)#ip address 200.0.0.253 255.255.255.0
R1(config-if)#ip ospf network point-to-point
R1(config-if)#ip ospf area 0
R1(config-if)#ip access-group laptop in
R1(config-if)#ip nat outside
R1(config-if)#crypto map mymap
R3(config-if)#no shut
R1(config-if)#exit
R1(config)#router ospf 1
R1(config-router)#network 200.0.0.0 0.0.0.255 area 0
R1(config-router)#exit
R1(config)#ip route 0.0.0.0 0.0.0.0 200.0.0.254
R1(config)#ip access-list extended crypto
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
R1(config)#ip nat inside source list 2 interface FastEthernet0/1 overload
R1(config)#ip nat inside source list nat interface FastEthernet0/1 overload
```

## R2

```
Router>en
Router#hostname R2
R2#conf t
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
R4(config)#interface Loopback0
R4(config-if)#ip address 1.1.1.1 255.255.255.255
R4(config-if)#exit
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
R3(config)#hostname R5
R3(config)#interface Loopback0
R3(config-if)#ip address 1.1.1.1 255.255.255.255
R3(config-if)#exit
R3(config)#interface FastEthernet0/0
R3(config-if)#ip address 10.0.0.6 255.255.255.252
R3(config-if)#no shut
R3(config-if)#ip ospf network point-to-point
R3(config-if)#ip ospf 1 area 0
R3(config-if)#ip nat outside
R3(config-if)#crypto map mymap
R3(config-if)#exit
R3(config)#interface FastEthernet0/1
R3(config-if)#ip address 192.168.1.254 255.255.255.0
R3(config-if)#ip nat inside
R3(config-if)#no shut
R3(config)#router ospf 1
R3(config-router)#network 10.0.0.4 0.0.0.3 area 0
R3(config-router)#exit
```

