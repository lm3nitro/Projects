# Spine1
```
! device: SP1 (vEOS, EOS-4.21.1.1F)
!
! boot system flash:/vEOS-lab.swi
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model multi-agent
!
hostname SP1
!
spanning-tree mode mstp
!
enable secret sha512 $6$lvOVHeQsGnTNN78d$qyT9WDIiF9df0Dq6t.afLfR9xk5hN1/wJ6Xn7AEtrr0tUkzCCaJVj059MPNdbPfcHqvqmdKvhy6QDL0VgJ1y8/
no aaa root
!
username admin privilege 15 role admin secret sha512 $6$LYJnl1nz9L7p4IuO$XXzHpIvcPlZdaiA5An2oQLFOZ18JJ5FXXRilNk41cIJ/dDjUowuFLPbF5rW0QPgNpqb.k5aM.phL2Xpe0gJVT/
!
vrf definition mgt
rd 1:1
!
interface Ethernet1
mtu 9200
no switchport
ip address 10.10.10.0/31
!
interface Ethernet2
mtu 9200
no switchport
ip address 10.10.10.2/31
!
interface Ethernet3
mtu 9200
no switchport
ip address 10.10.10.4/31
!
interface Ethernet4
mtu 9200
no switchport
ip address 10.10.10.6/31
!
interface Ethernet5
!
interface Ethernet6
!
interface Loopback0
ip address 11.11.11.11/32
!
interface Management1
description mgt_via_SSH
vrf forwarding mgt
ip address 192.168.1.200/24
!
ip route vrf mgt 0.0.0.0/0 192.168.1.254
!
ip routing
ip routing vrf mgt
!
peer-filter LEAFS
10 match as-range 100-300 result accept
!
router bgp 100
router-id 11.11.11.11
no bgp default ipv4-unicast
distance bgp 20 200 200
bgp listen range 10.10.10.0/24 peer-group UNDERLAY peer-filter LEAFS
neighbor UNDERLAY peer-group
neighbor UNDERLAY maximum-routes 12000 
redistribute connected
!
address-family ipv4
neighbor UNDERLAY activate
!
management ssh
idle-timeout 15
!
vrf mgt
no shutdown
!
router bgp 100
maximum-paths 4 ecmp 4
neighbor EVPN peer-group
neighbor EVPN ebgp-multihop 3
neighbor 1.1.1.1 peer-group EVPN
neighbor 2.2.2.2 peer-group EVPN
neighbor 3.3.3.3 peer-group EVPN
neighbor 4.4.4.4 peer-group EVPN
neighbor EVPN maximum-routes 12000
neighbor EVPN update-source loopback0
neighbor 1.1.1.1 remote-as 200
neighbor 2.2.2.2 remote-as 200
neighbor 3.3.3.3 remote-as 300
neighbor 4.4.4.4 remote-as 300
neighbor EVPN send-community
address-family evpn
neighbor EVPN activate
!
exit
!
end
```
