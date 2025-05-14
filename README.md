# Commands Pràctica 2 de Xarxes

## Minicom setup

```minicom
Serial device: /dev/ttyS0
Bps/Par/Bits: 9600 8N1
Hardware Flow Control: No
```

## Bàsic

* Asignar una adreça IP a una interfície

```bash
sudo ifconfig <interfície> <ip_address> up
```

* Habilitar protocol

```minicom
aaa authentication <protocol> local
```

* Esborrar configuració guardada

```minicom
rm /flash/working/boot.cfg
rm /flash/certified/boot.cfg
reload
write terminal
```

### Gestió de nivell 2

* Visualitzar únicament l'estat dels ports

```minicom
show interfaces [ethernet | fastethernet | gigaethernet] [slot[/port[-port2]]] port
```

* Visualitzar l'estat dels ports i el domini de broadcast al que estan associats

```minicom
show vlan [vid] port [slot/port |link_agg]
```

* Visualitzar taula d'encaminaments de nivell 2

```minicom
show mac-address-table [permanent | reset | timeout | learned] [mac_address] [slot slot |slot/port]
[linkagg link_agg] [vid]
```

* Visualitzar estadístiques de la taula d’encaminament de nivell 2

```minicom
show mac-address-table count [mac_address] [slot slot |slot/port] [linkagg link_agg] [vid]
```

* Visualitzar el temps de vigència de les adreces de nivell 2

```minicom
show mac-address-table aging-time [vlan vid]
```

* Afegir o modificar una entrada en la taula d’encaminament de nivell 2

```minicom
mac-address-table [permanent | reset | timeout] mac_address {slot/port | linkagg link_agg} vid [bridging
| filtering]
```

* Eliminar una entrada de la taula d’encaminament de nivell 2

```minicom
no mac-address-table [permanent | reset | timeout | learned] [mac_address {slot/port | link_agg} vid]
```

* Modificar el temps de vigència de les adreces de nivell 2:

```minicom
mac-address-table aging-time seconds [vlan vid]
no mac-address-table aging-time [vlan vid]
```

## Configuració de ports

> Les comandes de configuració de les interfícies (ports) totes tenen el prefix:

```minicom
interfaces [ethernet | fastethernet | gigaethernet] slot[/port[-port2]]...
```

> La comanda continuarà amb la característica o funcionalitat a configurar:

* Establir la velocitat de treball del port

```minicom
speed {auto | 10 | 100 | 1000 | 10000 | max {10 | 100 | 1000}}
```

* Habilitar l’auto negociació

```minicom
autoneg {enable | disable | on | off}
```

* Tipus de cablejat

```minicom
crossover {auto | mdix | mdi | disable}
```

* Mode de funcionament

```minicom
duplex {full | half | auto}
```

* Habilitar la interfície

```minicom
admin {up | down}
```

* Habilitar el control de broadcast i multicast

```minicom
flood
flood multicast (sol es pot especificar slot)
flood rate Mbps
```

* Inicialitzar els comptadors d’estadístiques de nivell 2

```minicom
no l2 statistics
```

### Visualització d'interfícies

> Les comandes d'informació de les interfícies totes tenen el prefix:

```minicom
show interfaces [ethernet | fastethernet | gigaethernet] slot[/port[-port2]] ....
```

> Si no s’afegeix cap altre paràmetre a la comanda s’ens mostrarà detallada de les interfícies especificades. Entre els que es poden afegir hi ha:

* Visualitzar les capacitats de les interfícies i la configuració actual

```minicom
capability
```

* Visualitzar el control de flux configurat

```minicom
flow [control]
```

* Mostrar informació de paquets Rx/Tx

```minicom
accounting
```

* Mostrar informació de paquets i bytes Rx/Tx

```minicom
counters
```

* Mostrar informació de paquets erronis

```minicom
counters errors
```

* Visualitzar els paràmetres configurats

```minicom
status
```

* Visualitzar l’estat dels ports

```minicom
port
```

* Visualitzar l’estat del control de broadcast i multicast

```minicom
flood rate
```

* Visualitzar estadístiques de trànsit

```minicom
traffic
```

### Control de flux

* Configurar l’enviament de pause frame

```minicom
interfaces [ethernet | fastethernet | gigaethernet] slot[/port[-port2]] flow {enable | disable | on
| off}
```

* Configurar el control de flux en transmissió

```minicom
flow [ethernet | fastethernet | gigaethernet] slot[/port[-port2]]
no flow [ethernet | fastethernet | gigaethernet] slot[/port[-port2]]
```

* Configurar el temps d’espera abans de reprendre les transmissions

```minicom
flow [ethernet | fastethernet | gigaethernet] slot[/port[-port2]] wait [time]microseconds
flow [ethernet | fastethernet | gigaethernet] slot[/port[-port2]] no wait [time]
```

* Visualitzar l’estat del control de flux

```minicom
show interfaces [ethernet | fastethernet | gigaethernet] slot[/port[-port2]] flow [control]
```

## Diagnòstics

### Port mirroring

* Configurar port mirroring

```minicom
port mirroring port_mirror_sessionid source slot/port[-port2] [slot/port[-port2]...] destination slot/-port [bidirectional |inport |outport] [unblocked vlan_id]
```

* Habilitar o deshabilitar port mirroring:

```minicom
port mirroring port_mirror_sessionid {enable | disable}
```

* Eliminar port mirroring

```minicom
no port mirroring port_mirror_sessionid
```

* Visualitzar les configuracions de port mirroring

```minicom
show port mirroring status [port_mirror_sessionid ]
```

### Port monitoring

* Configurar port monitoring

```minicom
port monitoring <port_monitor_sessionid> source slot/port [{no file | file filename [size filesize] | [overwrite
{on | off}]}][inport | outport | bidirectional] [timeout seconds][enable | disable]
```

* Pausar, reprende o deshabilitar una sessió

```minicom
port monitoring port_monitor_sessionid {disable | pause | resume}
```

* Eliminar una sessió

```minicom
no port monitoring port_monitor_sessionid
```

* Visualitzar l'estat de port monitoring

```minicom
show port monitoring status [port_monitor_sessionid]
```

* Visualitzar l'arxiu de captura

```minicom
show port monitoring file [port_monitor_sessionid]
```

### Switch health monitoring

* Visualitzar les estadístiques de tots els recursos de l’equip

```minicom
show health [slot/port] [statistics]
```

* Visualitzar les estadístiques d’un recurs

```minicom
show health all {memory | cpu | rx | txrx}
```

* Visualitzar els llindars establerts per les alarmes

```minicom
 show health threshold [rx | txrx | memory | cpu | temperature]
```

* Visualitzar l'interval de les mesures

```minicom
show health interval
```

* Establir el llindar per les alarmes

```minicom
health threshold [rx percent | txrx percent | memory percent | cpu percent | temperature degrees ]
```

* Establir l'interval de les mesures

```minicom
health intervalseconds
```

* Inicialitzar les estadístiques

```minicom
health statistics reset
```

## Seguretat nivell 2

* Activar, desactivar o eliminar la seguretat

```minicom
port-security slot/port [enable | disable]
no port-security slot/port
```

* Restringir l'aprenentatge

```minicom
port-security shutdown minutes
port-security slot/port maximum number
port-security slot/port mac mac_address
port-security slot/port no mac mac_address
port-security slot/port mac-range [low mac_address | high mac_address | low mac_address high mac_address]
```

* Especificar l’acció que es pendrà en complir-se la restricció

```minicom
port-security slot/port violation {restrict | shutdown}
```

* Reestablir un port bloquejat per una restricció

```minicom
port-security slot/port release
```

* Visualitzar l'estat de la seguretat

```minicom
show port-security [slot/port | slot | config-mac-range]
show port-security shutdown
```

## VLAN

* Visualitzar la configuració de les VLAN

```minicom
show vlan [vlan_id]
show vlan [vlan_id] port {slot/port | link_agg}
```

* Gestió de les VLAN (creació i eliminació)

```minicom
vlan vlan_id [enable | disable] [name description]
no vlan vlan_id
```

### VLANs estàtiques

* Associació estàtica de ports a una vlan

```minicom
vlan vlan_id port default {slot/port |link_agg}
vlan vlan_id no port default {slot/port |link_agg}
```

### VLANs dinàmiques

> Les VLAN dinàmiques permeten associar un port a una VLAN en funció del trànsit que hi circula. Per que un port pertanyi a una VLAN dinàmica s’han de complir dos requisits:

* El port ha de ser mòbil

```minicom
vlan port mobile slot/port [bpdu ignore {enable | disable}]
vlan no port mobile slot/port
show vlan port mobile [slot/port ]
```

* El trànsit ha de coincidir amb una regla

```minicom
vlan vlan_id dhcp mac mac_address
vlan vlan_id no dhcp mac mac_address
---
vlan vlan_id dhcp mac range low_mac_address high_mac_address
vlan vlan_id no dhcp mac range low_mac_address
---
vlan vlan_id dhcp port slot/port
vlan vlan_id no dhcp port slot/port
---
vlan vlan_id dhcp generic
vlan vlan_id no dhcp generic
---
vlan vlan_id binding mac-ip-port mac_address ip_address slot/port
vlan vlan_id no binding mac-ip-port mac_address
---
vlan vlan_id binding mac-port-protocol mac_address slot/port {ip-e2 | ip-snap | ipx-e2 | ipx-novell
| ipx-llc | ipx-snap | decnet | appletalk | ethertype type | dsapssap dsap/ssap tt | snap snaptype}

vlan vlan_id no binding mac-port-protocol mac_address {ip-e2 | ip-snap | ipx-e2 | ipx-novell | ipx-llc
| ipx-snap | decnet | appletalk | ethertype type | dsapssap dsap/ssap tt | snap snaptype}
---
vlan vlan_id binding mac-port mac_address slot/port
vlan vlan_id no binding mac-port mac_address
---
vlan vlan_id binding mac-ip mac_address ip_address
vlan vlan_id no binding mac-ip mac_address
---
vlan vlan_id binding ip-port ip_address slot/port
vlan vlan_id no binding ip-port ip_address
---
vlan vlan_id binding port-protocol slot/port {ip-e2 | ip-snap | ipx-e2 | ipx-novell | ipx-llc | ipx-snap
| decnet | appletalk | ethertype type |
dsapssap dsap/ssap tt | snap snaptype}

vlan vlan_id no binding port-protocol slot/port {ip-e2 | ip-snap | ipx-e2 | ipx-novell | ipx-llc |
ipx-snap | decnet | appletalk | ethertype type | dsapssap dsap/ssap tt | snap snaptype}
---
vlan vlan_id mac mac_address
vlan vlan_id no mac mac_address
---
vlan vlan_id mac range low_mac_address high_mac_address
vlan vlan_id no mac range low_mac_address
---
vlan vlan_id ip ip_address [subnet_mask]
vlan vlan_id no ip ip_address [subnet_mask]
---
vlan vlan_id ipx ipx_net [e2 | llc | snap | novell]
vlan vlan_id no ipx ipx_net
---
vlan vlan_id protocol {ip-e2 | ip-snap | ipx-e2 | ipx-novell | ipx-llc | ipx-snap | decnet | appletalk
| ethertype type | dsapssap dsap/ssap tt | snap snaptype}

vlan vlan_id no protocol {ip-e2 | ip-snap | ipx-e2 | ipx-novell | ipx-llc | ipx-snap | decnet | appletalk
| ethertype type | dsapssap dsap/ssap tt | snap snaptype}
---
vlan vlan_id user offset value mask
vlan vlan_id no user offset value
---
vlan vlan_id port slot/port
vlan vlan_id no port slot/port
```

* Visualitzar les configuracions

```minicom
show vlan port mobile [slot/port ]
show vlan vlan_id rules
```

## 802.1Q (VLAN tagging)

* Activar o desactivar etiquetat de VLAN

```minicom
vlan vlan_id 802.1q {slot/port | aggregate_id} [description]
vlan vlan_id no 802.1q {slot/port | aggregate_id}
```

* Visualització de la configuració de 802.1Q

```minicom
show 802.1q {slot/port | aggregate_id}
```

## Agregació d'enllaços (dinàmic)

* Crear l'agregació d'enllaços en l'equip local

```minicom
lacp linkagg agg_num size size
[name name]
[admin state {enable | disable}] [actor admin key actor_admin_key]
[actor system priority actor_system_priority]
[actor system id actor_system_id]
[partner system id partner_system_id]
[partner system priority partner_system_priority]
[partner admin key partner_admin_key]
```

* Especificar els ports que formaran part de l’agregació

```minicom
lacp agg slot/port actor admin key actor_admin_key
```

* Assignar la VLAN per defecte de l’agregació

```minicom
vlan vlan_id port default agg_num
```

* Visualitzar la configuració

```minicom
show linkagg [agg_num]
show linkagg port [slot/port]
```

## Spanning Tree

* Visualitzar la configuració

```minicom
show spantree [instance]
show spantree [instance] ports [forwarding | blocking | active | configured]
```

* Activar/desactivar spanning tree

```minicom
vlan vlan_id [1x1 | flat] stp {enable | disable}
```

* Establir mode de funcionament

```minicom
bridge mode {flat | 1x1}
```

* Establir algoritme

```minicom
bridge [vlan_id] protocol {stp | rstp | mstp}
```

## Interfícies d'encaminament

* Gestió d'interfícies d'encaminament IP

```minicom
ip interface name [addressip_address] [mask subnet_mask] [admin [enable | disable]] [vlan vlan_id]
[forward | no forward] [local-proxy-arp | no local-proxy-arp] [e2 | snap] [mtu size] [primary | no primary]

no ip interface name
```

* Visualitzar la configuració

```minicom
show ip interface [name| emp | vlan vlan_id]
```

## Encaminament estàtic

* Gestió estàtica (manual) de la taula d'encaminament

```minicom
ip static-route ip_address[mask mask] gateway gateway [metric metric]
no ip static-route ip_address[mask mask] gateway gateway [metric metric]
```

* Visualitzar contingut de la taula d'encaminament de nivell 3

```minicom
show ip route [summary]
show ip router database [protocol type| gateway ip_address | dest ip_address mask]
```

## Encaminament dinàmic

* Carregar el software `RIP` a l'equip

```minicom
ip load rip
```

* Activar/desactivar `RIP`

```minicom
ip rip status {enable | disable}
```

* Definir la interfície en la que es vol emprar `RIP`

```minicom
ip rip interface {ip_address | interface_name}
no ip rip interface {ip_address | interface_name}
```

* Activar/desactivar `RIP` en la interfície definida

```minicom
ip rip interface ip_address status {enable | disable}
```

* Especificar el tipus de rutes que es volen distribuir per `RIP`

```minicom
ip rip redist {local | static | ospf | bgp}
no ip rip redist {local | static | ospf | bgp}
```

* Especificar, si cal, un filtre sobre les rutes a distribuir

```minicom
ip rip redist-filter {local | static | ospf | bgp} ip_address ip_mask
no ip rip redist-filter {local | static | ospf | bgp} ip_address ip_mask
```

* Habilitar (o deshabilitar) la redistribució

```minicom
ip rip redist status {enable | disable}
```

* Visualitzar configuració i funcionament de `RIP`

```minicom
show ip rip
show ip rip routes [ip_address ip_mask]
show ip rip interface [ip_address]
show ip rip peer [ip_address]
show ip rip redist [local] [static] [ospf] [bgp]
show ip rip redist-filter [local] [static] [ospf] [bgp]
```
