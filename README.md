# Producto-1---CCN3-
Laboratorio de redes 

# Script del producto 

todos los comandos del producto: Pnetlab

----------------------------------SWM1---------------------------------------
enable
configure terminal
hostname SWM1
ip routing
ipv6 unicast-routing

vtp domain EMPRESA
vtp mode server
vtp password cisco

vlan 10
name ESTUDIANTES
vlan 20
name DOCENTES
vlan 60
name GESTION
vlan 70
name VOZ
exit

int r e0/1-3
switchport trunk encapsulation dot1q
switchport mode trunk
exit

int r e1/0-2
switchport trunk encapsulation dot1q
switchport mode trunk
exit

int r e0/1-3
channel-group 1 mode active
exit

int e0/0
switchport trunk encapsulation dot1q
switchport mode trunk
exit

int port-channel 1
switchport trunk encapsulation dot1q
switchport mode trunk
exit

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,60,70,80 root primary

int vlan 10
ip add 172.24.0.2 255.255.248.0
standby 10 ip 172.24.0.1
standby 10 priority 110
standby 10 preempt
exit

int vlan 20
ip add 172.24.8.2 255.255.255.0
standby 20 ip 172.24.8.1
standby 20 priority 90
standby 20 preempt
exit

int vlan 60
ip add 172.24.10.66 255.255.255.224
standby 60 ip 172.24.10.65
standby 60 priority 90
standby 60 preempt
exit

int vlan 70
ip add 172.24.10.98 255.255.255.224
standby 70 ip 172.24.10.97
standby 70 priority 110
standby 70 preempt
exit

interface vlan 70
ip address 172.24.10.97 255.255.255.224
no shutdown
exit

router ospf 1
router-id 3.3.3.3
network 172.24.0.0 0.0.7.255 area 10
network 172.24.8.0 0.0.0.255 area 10
network 172.24.10.64 0.0.0.31 area 10
network 172.24.10.96 0.0.0.31 area 10
network 172.24.10.128 0.0.0.15 area 10
passive-interface default
no passive-interface e0/0
no passive-interface e0/1
no passive-interface e0/2
no passive-interface e0/3

exit

do wr

----------------------------------SWM2---------------------------------------
enable
configure terminal
Hostname SWM2
ip routing
ipv6 unicast-routing

vtp domain EMPRESA
vtp mode client
vtp password cisco

int r e0/1-3
switchport trunk encapsulation dot1q
switchport mode trunk
exit

int r e1/0-2
switchport trunk encapsulation dot1q
switchport mode trunk
exit

int r e0/1-3
channel-group 1 mode active
exit

int e0/0
switchport trunk encapsulation dot1q
switchport mode trunk
exit

int port-channel 1
switchport trunk encapsulation dot1q
switchport mode trunk
exit

spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,60,70,80 root secondary

int vlan 10
ip add 172.24.0.3 255.255.248.0
standby 10 ip 172.24.0.1
standby 10 priority 90
standby 10 preempt
exit

int vlan 20
ip add 172.24.8.3 255.255.255.0
standby 20 ip 172.24.8.1
standby 20 priority 110
standby 20 preempt
exit

int vlan 60
ip add 172.24.10.67 255.255.255.224
standby 60 ip 172.24.10.65
standby 60 priority 110
standby 60 preempt
exit

int vlan 70
ip add 172.24.10.99 255.255.255.224
standby 70 ip 172.24.10.97
standby 70 priority 90
standby 70 preempt
exit

router ospf 1
router-id 4.4.4.4
network 172.24.0.0 0.0.7.255 area 1
network 172.24.8.0 0.0.0.255 area 1
network 172.24.10.64 0.0.0.31 area 1
network 172.24.10.96 0.0.0.31 area 1
network 172.24.10.128 0.0.0.15 area 1
passive-interface default
no passive-interface e0/0
no passive-interface e0/1
no passive-interface e0/2
no passive-interface e0/3
exit

do wr

----------------------------------SWAC1---------------------------------------
enable
configure terminal
Hostname SWAC1
ip routing
ipv6 unicast-routing

vtp domain EMPRESA
vtp mode client
vtp password cisco
int r e0/0, e0/1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
int e1/0
switchport mode access
switchport access vlan 20
exit

spanning-tree mode rapid-pvst

do wr

----------------------------------SWAC2---------------------------------------
enable
configure terminal
Hostname SWAC2
ip routing
ipv6 unicast-routing

vtp domain EMPRESA
vtp mode client
vtp password cisco
int r e0/0, e0/1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
int e2/0
switchport mode access
switchport access vlan 10
exit

spanning-tree mode rapid-pvst

do wr

----------------------------------SWAC3---------------------------------------
enable
configure terminal
Hostname SWAC3
ip routing
ipv6 unicast-routing

vtp domain EMPRESA
vtp mode client
vtp password cisco
int r e0/0, e0/1
switchport trunk encapsulation dot1q
switchport mode trunk
exit
int e3/0
switchport mode access
switchport access vlan 60
exit

spanning-tree mode rapid-pvst

do wr

----------------------------------SW-----------------------------------------
enable
configure terminal
hostname SW
ip routing
ipv6 unicast-routing

vlan 50
name DATACENTER

int e0/1
switchport mode access
switchport access vlan 50
exit

int e0/0
switchport trunk encapsulation dot1q
switchport mode trunk
exit

do wr

----------------------------------SWA13---------------------------------------

enable
configure terminal
hostname SW
ip routing
ipv6 unicast-routing

vlan 30
name ACADEMICOS
vlan 40
name FINANZAS

int e1/0
switchport mode access
switchport access vlan 30
exit

int e2/0
switchport mode access
switchport access vlan 40

int e0/0
switchport trunk encapsulation dot1q
switchport mode trunk
exit

do wr

----------------------------------R1-CORE---------------------------------------
enable
configure terminal
hostname R1-CORE
ipv6 unicast-routing
int loopback0
ip add 8.8.8.8 255.255.255.255
no shutdown
exi

int e0/2
ip ospf network point-to-point
no shutdown
exi

int s1/0
ip add 172.24.10.161 255.255.255.252
ipv6 add 2001:25:24:A00::1/64
ipv6 add FE80::1 link-local
no shutdown
exit

int e0/0
no shutdown
exi

int e0/0.10
encapsulation dot1q 10
ip add 172.24.0.1 255.255.248.0
ipv6 add 2001:25:24:10::1/64
ipv6 add FE80::10 link-local
ip helper-address 172.24.10.62
exit

int e0/0.20
encapsulation dot1q 20
ip add 172.24.8.1 255.255.255.0
ipv6 add 2001:25:24:20::1/64
ipv6 add FE80::20 link-local
ip helper-address 172.24.10.62
exit


int e0/0.60
encapsulation dot1q 60
ip add 172.24.10.65 255.255.255.224
ipv6 add 2001:25:24:60::1/64
ipv6 add FE80::60 link-local
ip helper-address 172.24.10.62
exit

int e0/0.70
encapsulation dot1q 70
ip add 172.24.10.97 255.255.255.224
ipv6 add 2001:25:24:70::1/64
ipv6 add FE80::70 link-local
ip helper-address 172.24.10.62
exit

int e0/1
no shutdown
exi

ip route 172.24.10.128 255.255.255.240 null0

ip route 0.0.0.0 0.0.0.0 s1/0

router ospf 1
router-id 1.1.1.1
network 172.24.0.0 0.0.7.255 area 1
network 172.24.8.0 0.0.0.255 area 1
network 172.24.10.160 0.0.0.3 area 0
network 8.8.8.8 0.0.0.0 area 0
passive-interface default
no passive-interface e0/0
no passive-interface e0/1
no passive-interface s1/0
default-information originate
log-adjacency-changes
exit

ip dhcp excluded-address 172.24.0.1 172.24.0.10
ip dhcp excluded-address 172.24.8.1 172.24.8.10
ip dhcp excluded-address 172.24.9.1 172.24.9.10
ip dhcp excluded-address 172.24.9.129 172.24.9.139
ip dhcp excluded-address 172.24.10.65 172.24.10.75

ip dhcp pool VLAN10
network 172.24.0.0 255.255.248.0
default-router 172.24.0.1
dns-server 8.8.8.8
exit

ip dhcp pool VLAN20
network 172.24.8.0 255.255.255.0
default-router 172.24.8.1
dns-server 8.8.8.8
exit

ip dhcp pool VLAN30
network 172.24.9.0 255.255.255.128
default-router 172.24.9.1
dns-server 8.8.8.8
exit

ip dhcp pool VLAN40
network 172.24.9.128 255.255.255.128
default-router 172.24.9.129
dns-server 8.8.8.8
exit

ip dhcp pool VLAN60
network 172.24.10.64 255.255.255.224
Default-router 172.24.10.65
dns-server 8.8.8.8
exit

ip access-list extended ACL_ESTUDIANTES_GESTION
remark El dep. de Estudiantes no pueden tener acceso al dep. de gestion
deny icmp 172.24.0.0 0.0.7.255 172.24.10.64 0.0.0.31
permit icmp any any

int e0/0.60
ip access-group ACL_ESTUDIANTES_GESTION out
exit

access-list 20 remark Se debe de permitir que solo gestión administre los dispositivos
access-list 20 permit 172.24.10.64 0.0.0.31

line vty 0 4
access-class 20 in
exit

ip access-list extended ACL_EST_ISP
remark El dep. de estudiantes no tiene permitido acceder al ISP
deny ip 172.24.0.0 0.0.7.255 any
permit ip any any

interface e0/2
ip access-group ACL_EST_ISP out
exit

ip access-list extended ACL_VOZ_RESTRICT
remark El dep. de Voz no pueden obtener acceso hacia otras redes
permit udp any eq 67 any eq 68
deny ip 172.24.10.96 0.0.0.31 any
permit ip any any
exit

interface e0/0.70
ip access-group ACL_VOZ_RESTRICT in
exit


do wr


----------------------------------R2---------------------------------------
enable
configure terminal
hostname R2
ipv6 unicast-routing

int s1/0
ip add 172.24.10.162 255.255.255.252
ipv6 add 2001:25:24:A00::2/64
ipv6 add FE80::2 link-local
no shutdown
exit

int e0/1
no shutdown
exit

int e0/1.50
encapsulation dot1q 50
ip add 172.24.10.1 255.255.255.192
ipv6 add 2001:25:24:50::1/64
ipv6 add FE80::50 link-local
ip helper-address 172.24.10.161
exit

int e0/0
no shutdown
exit

int e0/0.30
encapsulation dot1q 30
ip add 172.24.9.1 255.255.255.128
ipv6 add 2001:25:24:30::1/64
ipv6 add FE80::30 link-local
ip helper-address 172.24.10.161
exit

int e0/0.40
encapsulation dot1q 40
ip add 172.24.9.129 255.255.255.128
ipv6 add 2001:25:24:40::1/64
ipv6 add FE80::40 link-local
ip helper-address 172.24.10.161
exit

ip route 172.24.10.144 255.255.255.240 null0

ip route 0.0.0.0 0.0.0.0 s1/0

router ospf 1
router-id 2.2.2.2
router ospf 1
router-id 2.2.2.2
network 172.24.9.0 0.0.0.127 area 2
network 172.24.9.128 0.0.0.127 area 2
network 172.24.10.160 0.0.0.3 area 0
passive-interface default
no passive-interface e0/0
no passive-interface e0/1
no passive-interface s1/0
area 2 range 172.24.9.0 255.255.255.128
default-information originate
exit

access-list 10 remark Los estudiantes no pueden acceder al departamento de Finanzas.
access-list 10 deny 172.24.0.0 0.0.7.255
access-list 10 permit 172.24.9.128 0.0.0.128

interface e0/0.40
ip access-group 10 out
exit

ip access-list extended ACL_ESTUDIANTES_DATACENTER
remark El dep. de Estudiantes no deben de tener acceso al Datacenter
deny ip 172.24.0.0 0.0.7.255 172.24.9.1 0.0.0.31
permit ip any any
exit

interface e0/1.50
ip access-group ACL_ESTUDIANTES_DATACENTER in
exit

access-list 90 remark EL dep. de Academicos no pueden obtener acceso al dep. de finanzas
access-list 90 deny 172.24.9.0 0.0.0.127
access-list 90 permit any

int e0/0.40
ip access-group 90 out


do wr
