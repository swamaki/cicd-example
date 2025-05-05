# A-LEAF2

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [DHCP Relay](#dhcp-relay)
  - [DHCP Relay Summary](#dhcp-relay-summary)
  - [DHCP Relay Device Configuration](#dhcp-relay-device-configuration)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [ARP](#arp)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)
- [EOS CLI Device Configuration](#eos-cli-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | default | 192.168.0.102/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   ip address 192.168.0.102/24
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 10.255.0.2 | default | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf default 10.255.0.2
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 0.pool.ntp.org | - | - | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp server 0.pool.ntp.org
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

## DHCP Relay

### DHCP Relay Summary

- DHCP Relay is disabled for tunnelled requests
- DHCP Relay is disabled for MLAG peer-link requests

### DHCP Relay Device Configuration

```eos
!
dhcp relay
   tunnel requests disabled
   mlag peer-link requests disabled
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | default | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=default -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| 100 | Vlan4094 | 169.254.0.0 | Port-Channel1000 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id 100
   local-interface Vlan4094
   peer-address 169.254.0.0
   peer-link Port-Channel1000
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 0 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**
- Global BPDU Guard for Edge ports is enabled.

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree edge-port bpduguard default
spanning-tree mst 0 priority 0
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 10 | Blue | - |
| 30 | Orange | - |
| 3001 | MLAG_L3_VRF_Prod | MLAG |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 10
   name Blue
!
vlan 30
   name Orange
!
vlan 3001
   name MLAG_L3_VRF_Prod
   trunk group MLAG
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## MAC Address Table

### MAC Address Table Summary

- MAC address table entry maximum age: 1800 seconds

### MAC Address Table Device Configuration

```eos
!
mac address-table aging-time 1800
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet5 | MLAG_A-LEAF1_Ethernet5 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet6 | MLAG_A-LEAF1_Ethernet6 | *trunk | *- | *- | *MLAG | 1000 |
| Ethernet7 | SERVER_HostA1_eth2 | *access | *10 | *- | *- | 7 |
| Ethernet8 | SERVER_HostA2_eth2 | *access | *30 | *- | *- | 8 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_A-SPINE1_Ethernet2 | - | 192.168.1.9/31 | default | 9214 | False | - | - |
| Ethernet2 | P2P_A-SPINE2_Ethernet2 | - | 192.168.1.11/31 | default | 9214 | False | - | - |
| Ethernet3 | P2P_A-SPINE3_Ethernet2 | - | 192.168.1.13/31 | default | 9214 | False | - | - |
| Ethernet4 | P2P_A-SPINE4_Ethernet2 | - | 192.168.1.15/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_A-SPINE1_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.1.9/31
!
interface Ethernet2
   description P2P_A-SPINE2_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.1.11/31
!
interface Ethernet3
   description P2P_A-SPINE3_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.1.13/31
!
interface Ethernet4
   description P2P_A-SPINE4_Ethernet2
   no shutdown
   mtu 9214
   no switchport
   ip address 192.168.1.15/31
!
interface Ethernet5
   description MLAG_A-LEAF1_Ethernet5
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet6
   description MLAG_A-LEAF1_Ethernet6
   no shutdown
   channel-group 1000 mode active
!
interface Ethernet7
   description SERVER_HostA1_eth2
   no shutdown
   channel-group 7 mode active
!
interface Ethernet8
   description SERVER_HostA2_eth2
   no shutdown
   channel-group 8 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel7 | SERVER_HostA1 | access | 10 | - | - | - | - | 7 | - |
| Port-Channel8 | SERVER_HostA2 | access | 30 | - | - | - | - | 8 | - |
| Port-Channel1000 | MLAG_A-LEAF1_Port-Channel1000 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel7
   description SERVER_HostA1
   no shutdown
   switchport access vlan 10
   switchport mode access
   switchport
   mlag 7
   spanning-tree portfast
   spanning-tree bpduguard enable
!
interface Port-Channel8
   description SERVER_HostA2
   no shutdown
   switchport access vlan 30
   switchport mode access
   switchport
   mlag 8
   spanning-tree portfast
   spanning-tree bpduguard enable
!
interface Port-Channel1000
   description MLAG_A-LEAF1_Port-Channel1000
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 1.1.1.2/32 |
| Loopback1 | VTEP IP | default | 2.2.1.1/32 |
| Loopback101 | Per-VRF Unique Loopback | Prod | 10.101.101.2/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VTEP IP | default | - |
| Loopback101 | Per-VRF Unique Loopback | Prod | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 1.1.1.2/32
!
interface Loopback1
   description VTEP IP
   no shutdown
   ip address 2.2.1.1/32
!
interface Loopback101
   description Per-VRF Unique Loopback
   no shutdown
   vrf Prod
   ip address 10.101.101.2/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan10 | Blue | Prod | 9014 | False |
| Vlan30 | Orange | Prod | 9014 | False |
| Vlan3001 | MLAG_L3_VRF_Prod | Prod | 9214 | False |
| Vlan4093 | MLAG_L3 | default | 9214 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan10 |  Prod  |  -  |  10.10.10.1/24  |  -  |  -  |  -  |
| Vlan30 |  Prod  |  -  |  10.30.30.1/24  |  -  |  -  |  -  |
| Vlan3001 |  Prod  |  192.0.0.1/31  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  192.0.0.1/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  169.254.0.1/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan10
   description Blue
   no shutdown
   mtu 9014
   vrf Prod
   ip address virtual 10.10.10.1/24
!
interface Vlan30
   description Orange
   no shutdown
   mtu 9014
   vrf Prod
   ip address virtual 10.30.30.1/24
!
interface Vlan3001
   description MLAG_L3_VRF_Prod
   no shutdown
   mtu 9214
   vrf Prod
   ip address 192.0.0.1/31
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 9214
   ip address 192.0.0.1/31
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 169.254.0.1/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 10 | 10010 | - | - |
| 30 | 10030 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| Prod | 50001 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description A-LEAF2_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 30 vni 10030
   vxlan vrf Prod vni 50001
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:00:01

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:01
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| Prod | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf Prod
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |
| Prod | false |

### ARP

Global ARP timeout: 1500

#### ARP Device Configuration

```eos
!
arp aging timeout default 1500
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65112 | 1.1.1.2 |

| BGP Tuning |
| ---------- |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65112 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 1.1.1.201 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 1.1.1.202 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 1.1.1.203 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 1.1.1.204 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 192.0.0.0 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 192.168.1.8 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.1.10 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.1.12 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.168.1.14 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 192.0.0.0 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Prod | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

- VPN import pruning is **enabled**

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Encapsulation |
| ---------- | -------- | ------------ | ------------- | ------------- |
| EVPN-OVERLAY-PEERS | True |  - | - | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 1.1.1.2:10010 | 10010:10010 | - | - | learned |
| 30 | 1.1.1.2:10030 | 10030:10030 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| Prod | 1.1.1.2:50001 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 65112
   router-id 1.1.1.2
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65112
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description A-LEAF1
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 1.1.1.201 peer group EVPN-OVERLAY-PEERS
   neighbor 1.1.1.201 remote-as 65100
   neighbor 1.1.1.201 description A-SPINE1_Loopback0
   neighbor 1.1.1.202 peer group EVPN-OVERLAY-PEERS
   neighbor 1.1.1.202 remote-as 65100
   neighbor 1.1.1.202 description A-SPINE2_Loopback0
   neighbor 1.1.1.203 peer group EVPN-OVERLAY-PEERS
   neighbor 1.1.1.203 remote-as 65100
   neighbor 1.1.1.203 description A-SPINE3_Loopback0
   neighbor 1.1.1.204 peer group EVPN-OVERLAY-PEERS
   neighbor 1.1.1.204 remote-as 65100
   neighbor 1.1.1.204 description A-SPINE4_Loopback0
   neighbor 192.0.0.0 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 192.0.0.0 description A-LEAF1_Vlan4093
   neighbor 192.168.1.8 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.1.8 remote-as 65100
   neighbor 192.168.1.8 description A-SPINE1_Ethernet2
   neighbor 192.168.1.10 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.1.10 remote-as 65100
   neighbor 192.168.1.10 description A-SPINE2_Ethernet2
   neighbor 192.168.1.12 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.1.12 remote-as 65100
   neighbor 192.168.1.12 description A-SPINE3_Ethernet2
   neighbor 192.168.1.14 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.1.14 remote-as 65100
   neighbor 192.168.1.14 description A-SPINE4_Ethernet2
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 1.1.1.2:10010
      route-target both 10010:10010
      redistribute learned
   !
   vlan 30
      rd 1.1.1.2:10030
      route-target both 10030:10030
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
      route import match-failure action discard
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf Prod
      rd 1.1.1.2:50001
      route-target import evpn 50001:50001
      route-target export evpn 50001:50001
      router-id 1.1.1.2
      neighbor 192.0.0.0 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 192.0.0.0 description A-LEAF1_Vlan3001
      redistribute connected route-map RM-CONN-2-BGP-VRFS
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 1.1.1.0/24 eq 32 |
| 20 | permit 2.2.1.0/24 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 192.0.0.0/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 1.1.1.0/24 eq 32
   seq 20 permit 2.2.1.0/24 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 192.0.0.0/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| Prod | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance Prod
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IPv4 Address | Source NAT IPv6 Address |
| -------------- | ----------------------- | ----------------------- |
| Prod | 10.101.101.2 | - |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf Prod address 10.101.101.2
```

## EOS CLI Device Configuration

```eos
!
agent KernelFib environment KERNELFIB_PROGRAM_ALL_ECMP='true'

```
