# Proyecto 1
### Jose Javier Santizo Díaz - 201607814

## Topología Proyecto 1
![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/1.png)

## Administración (CORE)

![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/2.png)

## Edificio Derecho

![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/3.png)

## Edificio Izquierdo

![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/4.png)

**Chapin Red** es una organización comprometida con la responsabilidad social, dedicada a organizar y ejecutar proyectos de ayuda humanitaria en comunidades vulnerables. Opera desde **cuatro edificios** distribuidos geográficamente, cada uno con funciones específicas, se nos contrato para optizar su servicio en sus edificios.

# Guía de Comandos para la Implementación de Red - Chapin Red

## Tabla de Subnetting
| VLAN | Subred | Hosts disponibles | Broadcast | Gateway |
|------|--------|------------------|-----------|---------|
| 10 | 192.188.14.0 /27 | 192.188.14.1 – 192.188.14.30 | 192.188.14.31 | 192.188.14.1 |
| 20 | 192.188.14.32 /27 | 192.188.14.33 – 192.188.14.62 | 192.188.14.63 | 192.188.14.33 |
| 30 | 192.188.14.64 /27 | 192.188.14.65 – 192.188.14.94 | 192.188.14.95 | 192.188.14.65 |
| 40 | 192.188.14.96 /27 | 192.188.14.97 – 192.188.14.126 | 192.188.14.127 | 192.188.14.97 |
| 99 | 192.188.14.128 /27 | 192.188.14.129 – 192.188.14.158 | 192.188.14.159 | 192.188.14.129 |

## DHCP1

![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/5.png)


## DHCP2

![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/6.png)


## Capa 2 (Switching)

### Creación de VLANs
Para segmentar los departamentos (Proyectos, Administración, Coordinación).

```

configure terminal

vlan 10
 name Naranja_EdificioIZQ_201607814
 exit

vlan 20
 name Verde_EdificioIZQ_201607814
 exit

vlan 30
 name Naranja_EdificioDER_201607814
 exit

vlan 40
 name Verde_EdificioDER_201607814
 exit

vlan 99
 name ADMIN_201607814
 exit

show vlan brief
```

### VTP (VLAN Trunking Protocol)

```
configure terminal
vtp mode server
vtp domain ChapinRed.local
vtp password chapinred123
vtp version 2  


show vtp status
show vtp password

configure terminal
vtp mode client
vtp domain ChapinRed.local
vtp password chapinred123
vtp version 2  

show vtp status
```

### EtherChannel (Agregación de Enlaces)

```
configure terminal
interface range gig0/1-2
 switchport mode trunk
 channel-group 1 mode active   ! LACP Activo

interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30   

show etherchannel summary
show etherchannel port-channel

configure terminal
interface range gig0/1-2
 switchport mode trunk
 channel-group 2 mode desirable   

interface port-channel 2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

### Spanning Tree Protocol (STP)

```
configure terminal
spanning-tree vlan 1 root primary

spanning-tree vlan 1 priority 4096

configure terminal
interface range fa0/1-10
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

show spanning-tree
show spanning-tree vlan 10
```

## Capa 2 (Switching)

### SVI (Switch Virtual Interfaces)

```
configure terminal

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown
 exit

show ip interface brief
show interfaces vlan 10
```

### Inter-VLAN Routing

```
configure terminal
ip routing

show ip route
```

### OSPF (Open Shortest Path First)

```
configure terminal

router ospf 1

router-id 1.1.1.1

network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0

network 10.0.1.0 0.0.0.3 area 0

end

show ip ospf neighbor
show ip ospf interface
show ip route ospf
```
## Evidencia de Ping (ADMIN a PC0 y Laptop2)

![Topologia](https://github.com/BatteryRem/REDES2_1S2026_201607814/blob/main/Proyecto1/pr1redes/7.png)
