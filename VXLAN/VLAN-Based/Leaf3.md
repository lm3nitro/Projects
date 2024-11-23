# Leaf3
```
! device: LEAF3 (vEOS, EOS-4.21.1.1F)
!
! boot system flash:/vEOS-lab.swi
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model multi-agent
!
hostname LEAF3
!
spanning-tree mode mstp
no spanning-tree vlan 4094
!
enable secret sha512 $6$lvOVHeQsGnTNN78d$qyT9WDIiF9df0Dq6t.afLfR9xk5hN1/wJ6Xn7AEtrr0tUkzCCaJVj059MPNdbPfcHqvqmdKvhy6QDL0VgJ1y8/
no aaa root
!
username admin privilege 15 role admin secret sha512 $6$LYJnl1nz9L7p4IuO$XXzHpIvcPlZdaiA5An2oQLFOZ18JJ5FXXRilNk41cIJ/dDjUowuFLPbF5rW0QPgNpqb.k5aM.phL2Xpe0gJVT/
!
vlan 4094
name mlagpeer
trunk group mlagpeer
!
vlan 100
name customer
!
vrf definition mgt
!
interface Port-Channel10
switchport mode trunk
switchport trunk group mlagpeer
!
interface Ethernet1
mtu 9200
no switchport
ip address 10.10.10.5/31
!
interface Ethernet2
mtu 9200
no switchport
ip address 10.10.20.5/31
!
interface Ethernet3
description MLAG_PEER_LINK
channel-group 10 mode active
!
interface Ethernet4
description MLAG_PEER_LINK
channel-group 10 mode active
!
interface Ethernet5
!
interface Ethernet6
!
interface Loopback0
ip address 3.3.3.3/32
!
interface Loopback1
ip address 3.3.4.4/32
!
interface Management1
description mgt_via_SSH
vrf forwarding mgt
ip address 192.168.1.204/24
!
interface Vlan4094
description MLAG_PEER_LINK
ip address 10.0.0.2/31
!
ip route vrf mgt 0.0.0.0/0 192.168.1.254
!
ip routing
ip routing vrf mgt
!
mlag configuration
domain-id mlagpeer
local-interface Vlan4094
peer-address 10.0.0.3
peer-link Port-Channel10
!
router bgp 300
router-id 3.3.3.3
no bgp default ipv4-unicast
distance bgp 20 200 200
neighbor SPINES peer-group
neighbor SPINES remote-as 100
neighbor SPINES maximum-routes 12000 
neighbor 10.0.0.3 remote-as 300
neighbor 10.0.0.3 next-hop-self
neighbor 10.0.0.3 maximum-routes 12000 
neighbor 10.10.10.4 peer-group SPINES
neighbor 10.10.20.4 peer-group SPINES
redistribute connected
!
address-family ipv4
neighbor SPINES activate
neighbor 10.0.0.3 activate
!
management ssh
idle-timeout 15
!
vrf mgt
no shutdown
!
router bgp 300
maximum-paths 4 ecmp 4
neighbor EVPN peer-group
neighbor EVPN ebgp-multihop 3
neighbor EVPN maximum-routes 12000
neighbor 11.11.11.11 peer-group EVPN
neighbor 12.12.12.12 peer-group EVPN
neighbor EVPN update-source loopback0
neighbor 11.11.11.11 remote-as 100
neighbor 12.12.12.12 remote-as 100
neighbor EVPN send-community
address-family evpn
neighbor EVPN activate
!
exit
!
vlan 100
rd 3.3.3.3:100
route-target import 200:100
route-target export 300:100
redistribute learned
!
exit
!
interface vxlan 1
vxlan vlan 100 vni 100
vxlan source-interface loopback 1
!
exit
!
vlan 100
name customer
!
exit
!
end
```
