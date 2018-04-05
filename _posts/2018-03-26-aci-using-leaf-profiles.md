---
layout: post
title:  "ACI: Using Leaf Profiles"
author: Steve Sharman
date:   2018-03-26 22:49:00 +0000
published: true
categories: ACI
tags: ACI
---
In my previous [post]({{ site.url }}{{ site.baseurl }}/aci/2018/03/23/ACI-Configuring-network-interfaces.html) we looked at Leaf Profiles which contain a collection of Interfaces Selectors which in turn contain the physical interface(s) e.g. eth1/1.

There are two ways of using Leaf Profiles (Interface Policies)
* Device Centric Leaf Profiles
* Switch Centric Leaf Profiles

You won't find either of these terms in the documentation because I've completely made them up :)

# Device Centric Mode
I use the term Device Centric Leaf Profile to describe a Leaf Profile which has been constructed based on the type of device attaching to the network.

For example I might build a Leaf Profile which defines the interfaces used when my ESX Hosts connect to the network:

<img src="/steveBlog/assets/aci-using-leaf-profiles/picture1.png" style="width:100% !important;height:100% !important;" />

<br>

__Leaf Profile:__ ESX_Hosts

| Interface Selectors | Interfaces |Interface Policy Group | Interface Policies |
| :--:|:---:|:---:|:---:|
| Interface_Range |eth1/1-6 | ESX_Hosts | cdp_enabled |

<br>

Alternatively I might configure in this way where I specify the individual interfaces as opposed to a range of interfaces:

| Interface Selector | Interfaces |Interface Policy Group | Interface Policies |
| :--:|:---:|:---:|:---:|
| eth1 |eth1/1 | ESX_Hosts | cdp_enabled |
| eth2 |eth1/2 | ESX_Hosts | cdp_enabled |
| eth3 |eth1/3 | ESX_Hosts | cdp_enabled |
| eth4 |eth1/4 | ESX_Hosts | cdp_enabled |
| eth5 |eth1/5 | ESX_Hosts | cdp_enabled |
| eth6 |eth1/6 | ESX_Hosts | cdp_enabled |

<br>

What's nice with a Device Centric Model is that every physical switch where I attach the ESX_Hosts Profile will have interfaces 1/1-6 configured.

<img src="/steveBlog/assets/aci-using-leaf-profiles/picture2.png" style="width:100% !important;height:100% !important;" />

<br>

# Switch Centric Mode
I use the term Switch Centric Leaf Profile to describe a Leaf Profile which has been constructed based on the target switch.

For example I might build a Leaf Profile which defines the interfaces for a specific switch:

<img src="/steveBlog/assets/aci-using-leaf-profiles/picture3.png" style="width:100% !important;height:100% !important;" />

<br>

__Leaf Profile:__ Leaf_101

| Interface Selector | Interfaces |Interface Policy Group | Interface Policies |
| :--:|:---:|:---:|:---:|
| Interface_Range |eth1/1-3 | ESX_Hosts | cdp_enabled |
| Interface_Range |eth1/11-13 | Linux_Hosts | cdp_enabled |
| Interface_Range |eth1/21-23 | Windows_Hosts | cdp_enabled |

<br>

What's nice with this approach is that I have ultimate flexibility, the downside is that I would have to repeat the configuration for each switch (or pair of switches) in the network:

<img src="/steveBlog/assets/aci-using-leaf-profiles/picture4.png" style="width:100% !important;height:100% !important;" />

<br>

## Summary
There's a no right or wrong way of using Leaf Profiles, in fact you'll likely use both methods.

If you take the first approach you'll (potentially) have less configuration, however if you take the second approach you'll (potentially) have more configuration.

My personal choice is to use a Device Centric approach for compute switches, and a Switch Centric approach for edge racks where lots of flexibility is required.
