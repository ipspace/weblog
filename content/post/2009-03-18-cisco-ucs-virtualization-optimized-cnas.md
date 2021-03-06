---
author: slowe
categories: Information
comments: true
date: 2009-03-18T10:18:36Z
slug: cisco-ucs-virtualization-optimized-cnas
tags:
- Cisco
- Networking
- Virtualization
- VMware
- vSphere
- UCS
- Hardware
title: Cisco UCS Virtualization-Optimized CNAs
url: /2009/03/18/cisco-ucs-virtualization-optimized-cnas/
wordpress_id: 1244
---

There's some great information being shared in the comments to my ["More on Cisco UCS"][1] post. So good, in fact, that I thought it entirely and completely appropriate to bring that information into the limelight with a full-blown post.

If you look back at the diagram that's included in that UCS post, toward the bottom you'll see a very small blurb about some Cisco UCS Network Adapters that are optimized for efficiency and performance, compatibility, and virtualization. In a nutshell, the idea here is that there are three different CNA families targeted at different markets: high-performance Ethernet, compatibility with existing driver stacks, and virtualization. Users will choose the CNA that best suits their needs. For the purposes of this post, I'd like to discuss the virtualization-optimized CNA.

The idea here is that the virtualization-optimized CNA (what is being referred to as "Palo") will leverage a number of different technologies in virtualized environments:

* It will utilize SR-IOV (Single Root I/O Virtualization), a PCI SIG standard for allowing a physical network adapter to present multiple virtual adapters to upper-level software, in this case the hypervisor. This eliminates the need for the hypervisor to manage the physical network adapter and allows VMs to attach directly to one of the SR-IOV virtual adapters (or, as Brad Hedlund put it in a comment to [my original article][1], an "SR-IOV slice of the adapter").

* It will utilize Intel I/O Acceleration Technology (Intel I/OAT) to minimize bottlenecks in the hardware and allow the server to better cope with massive dataflows like those generated by 10GbE adapters.

* It will use Intel Virtual Machine Device Queues (VMDq) to improve traffic management within the server and decrease the processing burden on the VMM, i.e., the hypervisor.

Together, these technologies can be referred to as [Intel VT-c](http://www.intel.com/network/connectivity/solutions/virtualization.htm). The virtualization-optimized drivers will also take advantage of [Intel VT-d](http://software.intel.com/en-us/articles/intel-virtualization-technology-for-directed-io-vt-d-enhancing-intel-platforms-for-efficient-virtualization-of-io-devices/) to provide hardware-assisted DMA remapping and better protection and performance of direct-assigned devices.

"OK," you say. "But where is all this leading?" Good question! Let's bring it all together.

Today, in the VMware space, virtual machines are connected to a vSwitch because connecting them directly to a physical adapter just isn't practical. Yes, there is VMDirectPath, but for VMDirectPath to really work it needs more robust hardware support. Otherwise, you lose useful features like VMotion. (Refer back to my VMworld 2008 session notes from [TA2644][2].) So, we have to manage physical switches and virtual switches---that's two layers of management and two layers of switching. Along comes the Cisco Nexus 1000V. The 1000V helps to centralize management but we still have two layers of switching.

That's where the "Palo" adapter comes in. Using VMDirectPath "Gen 2" (again, refer to my TA2644 notes) and the various hardware technologies I listed and described above, we now gain the ability to attach VMs directly to the network adapter and _eliminate the virtual switching layer entirely._ Now we've both centralized the management and eliminated an entire layer of switching. And no matter how optimized the code may be, the fact that the hypervisor doesn't have to handle packets means it has more cycles to do other things. In other words, there's less hypervisor overhead. I think we can all agree that's a good thing.

&lt;aside&gt;I've clashed with a couple of different people thus far because of differences on perspective with regard to UCS. OK, specifically, it's been regarding people who insist that UCS isn't a blade server. OK, UCS as an overall _system_ is not a blade server, but the B-Series blades are a significant part of this overall system---so to say that Cisco's isn't building blade servers really isn't accurate. They **are** building blade servers, but these are blade servers with an as-yet-unseen level of integration with other technologies. If there is one area in which UCS stands apart from any other blade server-related solution on the market, it would be this level of integration, especially the integration with virtualization technology.&lt;/aside&gt;

Chad Sakac of EMC touches on this very lightly in his [latest post](http://virtualgeek.typepad.com/virtual_geek/2009/03/interesting-dialog-on-the-cisco-ucs-stuff-and-a-bit-of-detail.html). Being who I am, though, I much prefer digging a bit deeper to better understand exactly what's going on.

UCS experts, feel free to correct me or clarify my statements in the comments. Thanks!

[1]: {{< relref "2009-03-16-more-on-cisco-ucs.md" >}}
[2]: {{< relref "2008-09-18-ta2644-networking-io-virtualization.md" >}}
