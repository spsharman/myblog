---
layout: post
title:  "ACI: Tenants, VRFs, BDs, and EPGs"
author: Steve Sharman
date:   2018-04-05 00:00:00 +0000
published: true
categories: ACI
tags: ACI
comments: true
---
In this post we're going to take a high level look at some of the building blocks that are used when configuring an ACI network. If you're coming from a traditional networking background you should easily be able to map the ACI constructs with standard NXOS constructs.

## Tenants
Tenants are simply fabric wide configuration "zones" on the network which can be used represent different environments such as "Production" and "Pre-Production". There are three built in Tenants (infra, mgmt, common), objects that are built in the "common" Tenant can be consumed by other Tenants.

<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture1.png" style="width:100% !important;height:100% !important;" />

<!--
<a href="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture1.png" data-lightbox="picture1-large" data-title="">
<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture1.png" style="width:100%; height:100%;" />
</a>
-->

You can think of an ACI Tenant as being _similar_ to a VDC on a Nexus 7k, however one major difference is that when configuring an ACI Tenant you don't reserve blocks of interfaces.

<br>
## VRFs
VRFs on an ACI fabric are really just the same as a VRF on any other network device. What's nice with an ACI network is that when you define a VRF it's automatically available on every leaf switch in the fabric.

<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture2.png" style="width:100% !important;height:100% !important;" />

<!--
<a href="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture2.png" data-lightbox="picture2-large" data-title="">
<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture2.png" style="width:100%; height:100%;" />
</a>
-->

```
apic1# show vrf
 Tenant      Vrf         Consumed Contracts    Provided Contracts    Description                              
 ----------  ----------  --------------------  --------------------  ----------------------------------------
 Ciscolive   vrf-01      -                     permit_to_Active_Dir                                           
                                               ectory                                                         
 common      copy        -                     -                                                              
 common      default     -                     -                                                              
 common      vrf-01      -                     -                                                              
 infra       ave-ctrl    -                     -                                                              
 infra       overlay-1   -                     -                                                              
 iomart      vrf-01      -                     -                                                              
 iomart      vrf-02      -                     -                                                              
 mgmt        inb         -                     -                                                              
 mgmt        oob         -                     -                                                              
 micdoher    vrf-01      -                     -                                                              
 rwhitear    vrf-01      -                     -                                                              
 ssharman    vrf-01      -                     -                                                              
 ssharman    vrf-02      -                     -                                                              
 ssharman    vrf-03      -                     -                                                              
 ssharman    vrf-04      -                     -                                                              
apic1#
```

<br>
## Bridge Domains (VRFs)
Bridge Domains are layer 2 segments (under the covers they're switch local VLANs) which are carried over the ACI fabric in VXLAN frames.

<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture3.png" style="width:100% !important;height:100% !important;" />

<!--
<a href="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture3.png" data-lightbox="picture3-large" data-title="">
<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture3.png" style="width:100%; height:100%;" />
</a>
-->

Using the `fabric XYZ show vlan extended` command on two different switches I can see the Encap (incoming) VLAN and the switch local VLAN.
```
apic1# fabric 101 show vlan extended
----------------------------------------------------------------
 Node 101 (Leaf-101)
----------------------------------------------------------------

 VLAN Name                             Encap            Ports
 ---- -------------------------------- ---------------- ------------------------
 1    rwhitear:UCSD:6.5                vlan-2077        Eth1/21
 2    ssharman:test:bob                vlan-2075        Eth1/21
 9    infra:default                    vxlan-16777209,  Eth1/1, Eth1/11,
                                       vlan-3967        Eth1/21, Eth1/22, Po1
 11   common:10.52.248.192_27          vxlan-16252850   Eth1/21
 12   common:outside_vlan-8_host-mgmt  vxlan-15237054   Eth1/11, Eth1/21,
                                                        Eth1/22, Po1
 13   common:outside_vlans:vlan-       vlan-8           Eth1/11, Eth1/21,
      8_host-mgmt                                       Eth1/22, Po1
 15   rwhitear:Candid:candid-          vlan-2076        Eth1/21
      prod.0.9.1.832
 17   ssharman:192.168.10.x_24         vxlan-16580489   Eth1/21, Eth1/22
 18   ssharman:esx-infrastructure      vlan-2000        Eth1/21, Eth1/22
      :Host-mgmt
apic1#
```

```
apic1# fabric 102 show vlan extended
----------------------------------------------------------------
 Node 102 (Leaf-102)
----------------------------------------------------------------

 VLAN Name                             Encap            Ports
 ---- -------------------------------- ---------------- ------------------------
 9    infra:default                    vxlan-16777209,  Eth1/1, Eth1/11,
                                       vlan-3967        Eth1/21, Eth1/22, Po1
 11   common:10.52.248.192_27          vxlan-16252850   Eth1/21, Eth1/22
 12   common:outside_vlan-8_host-mgmt  vxlan-15237054   Eth1/11, Eth1/21,
                                                        Eth1/22, Po1
 13   common:outside_vlans:vlan-       vlan-8           Eth1/11, Eth1/21,
      8_host-mgmt                                       Eth1/22, Po1
 14   ssharman:outside_infra-          vxlan-15433636   Eth1/11, Eth1/21,
      ssharman-29                                       Eth1/22, Po1
 15   ssharman:lab-infrastructure      vlan-29          Eth1/11, Eth1/21,
      :infra-ssharman-29                                Eth1/22, Po1
 16   micdoher:KUBaM:KUBaM             vlan-2066        Eth1/22
 18   ssharman:192.168.10.x_24         vxlan-16580489   Eth1/21, Eth1/22
 19   ssharman:esx-infrastructure      vlan-2000        Eth1/21, Eth1/22
      :Host-mgmt
apic1#
```
Understanding the relationship between an incoming (Encap) VLAN, and a switch local VLAN is extremely important because when using switch show commands you must specify the switch local VLAN.

<br>
Bridge Domains may (or may not) have an associated anycast IP gateway - think VLAN 10, Interface VLAN 10.
```
apic1# fabric 101 show ip interface vlan 17
----------------------------------------------------------------
 Node 101 (Leaf-101)
----------------------------------------------------------------
IP Interface Status for VRF "ssharman:vrf-01"
vlan17, Interface status: protocol-up/link-up/admin-up, iod: 136, mode: pervasive
  IP address: 192.168.10.1, IP subnet: 192.168.10.0/24
  IP broadcast address: 255.255.255.255
  IP primary address route-preference: 1, tag: 0


apic1# fabric 102 show ip interface vlan 18
----------------------------------------------------------------
 Node 102 (Leaf-102)
----------------------------------------------------------------
IP Interface Status for VRF "ssharman:vrf-01"
vlan18, Interface status: protocol-up/link-up/admin-up, iod: 134, mode: pervasive
  IP address: 192.168.10.1, IP subnet: 192.168.10.0/24
  IP broadcast address: 255.255.255.255
  IP primary address route-preference: 1, tag: 0


apic1#
```
<br>
## Endpoint Groups (EPGs) and Contracts
EPGs are "security bubbles" for network attached devices. Admission to an EPG is based on switch/interface/VLAN or virtual switch, for example VLAN 10 on interface 101/1/1 = EPG web.

EPGs are mapped to a single Bridge Domain.

Communication between EPGs is disabled by default, permitting communication requires a Contract (ACL).

<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture4.png" style="width:100% !important;height:100% !important;" />

<!--
<a href="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture4.png" data-lightbox="picture4-large" data-title="">
<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture4.png" style="width:100%; height:100%;" />
</a>
-->

<br>
## Application Profiles
An Application Profile is simply a collection of EPGs and Contracts which may or may not represent an Application.

<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture5.png" style="width:100% !important;height:100% !important;" />

<!--
<a href="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture5.png" data-lightbox="picture5-large" data-title="">
<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture5.png" style="width:100%; height:100%;" />
</a>
-->

<br>
## Pulling this all together...
When building an ACI network a decision needs to be made where to configure the different objects.

Many customers build their forwarding functions (VRFs and Bridge Domains) in the "common" tenant and place their Application Profiles and EPGs in a "user" tenant.

Alternatively customers may place their VRFs, BDs, App Profiles, and EPGs all in a single tenant.

<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture6.png" style="width:100% !important;height:100% !important;" />

<!--
<a href="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture6.png" data-lightbox="picture6-large" data-title="">
<img src="/myblog/assets/aci-tenants-vrfs-bds-epgs/old-images/picture6.png" style="width:100%; height:100%;" />
</a>
-->
