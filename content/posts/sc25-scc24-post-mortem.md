---
title: "SCC24 Postmortem & Supercomputing 2025 info"
date: 2025-03-04
author: ["paco", "org"]
description: 'Description of SCC24 and upcoming 2025'
draft: true
math: true
---


## SC25 info

As of the publication of this post the application for UCSD’s 2025 team has opened. For those applying the deadline is April 7th and status updates will be sent out the following week. More details are listed on the application page.

![](/images/SC25Logo.svg)


On top of participating as a team in the student cluster competition, the Supercomputing convergence offers opportunities for students to attend the convergence as a [Student Volunteer](https://sc24.supercomputing.org/students/student-volunteers/) and [Scinet Volunteer](https://sc24.supercomputing.org/scinet/). **Note** that the time commitment for student volunteers is the week of the conference and for SCInet volunteers being two weeks the week of the conference and the week preceding the conference. For those applying if you're from UCSD and are accepted as volunteers for either program please reach out to Mary Thomas, so we have sufficient lead time to allocate travel funding support.

# Student Cluster Competition 2024 postmortem

## Competition Details
- All software and hardware we used must be publicly available at the start of the competition.
- 4.5 kW limit for entire cluster, 2 kW limit per node
- Benchmarks are: HPL, MLPERF inference, and a mystery benchmark revealed at the start of benchmarking
- Applications are: NAMD, ICON, Reproducibility (data flow lifecycle analysis tool DataLife), and a mystery application revealed at the start of application tasks

### Competitors and Results

### System Specifications 

Our final system, <u>Ocean Vista</u>:

![](/post-media/scc24-postmortem/cluster-architecture.svg)

Our system for 2024 is very similar to our system from [scc23](../sc24-scc23-post-mortem), consister of 3 nodes: 2 GPU node, 1 CPU node with Liqid IO cards. However, this year our cluster is distringuished by it's heterogenous nature. Let's look at parts and vendors list: 

| Parts | Vendor |
| --- | --- |
| 3x 1meter copper cables from | Pivotal Optics |
| 3x Broadcom 400bps NICs | Broadcom, but Micas Networks were the middle men |
| 1 400gbps Switch | Micas Networks |
| R283-Z93-AAF1 (Server) + AMD CPUs | Applied Data Systems + Gigabyte |
| G293-Z22-AAP1 (Server) + AMD CPUs | Applied Data Systems + Gigabyte |
| G493-ZB2-AAP1 (Server) + AMD CPUs | Applied Data Systems + Gigabyte |
| 2x4500 IO Acc. "Honey Badger" | LIQID |
| 4x MI210s | HPE (Michael Woodacre) |
| 4x MI210s | AMD |
| 4x Infinity Fabric Links (2 used) | AMD |

Here is the graph matching the hardware with the source of vendor:

![image](/post-media/scc24-postmortem/sponsor-diagram.svg)

*how did we get here*? It was not easy. We had a lot of help. A ***lot*** of help.

Besides lending the team hardware, we received generous financial donations from Micas Networks and Intel, as well as the mentoring and help from faculty and staff from SDSC, UCSD, and the CSE department.

### Preparing for Benchmarks and Applications

- HPCFund access
- HPL, MLPerf, NAMD, ICON, Reproducibility, sysadmin


### Sourcing Hardware (?)

- datacenter access

The earliest hardware we got was the [32 Port 400Gb Switch](https://micasnetworks.com/product/m2-w6920-32qc2x/) from Micas Networks. Our relations started with Team SCC23. Since then, Micas Networks has been one of our essential supporters. They generously lent us their hardware since May, and this support pushed our team to really reach the 400Gb bandwidth we wanted.

Over the summer we were told by our primary system vendor that the hardware we've been speccing out was unattainable due to a lack of support. This upsetting news came in late July and quickly became a source of stress since the hardware proposal was due in 2 months. We were late into the cycle, with only an idea of what we needed, and no physical means of compute.  

The meetings from then forward consisted of scouting out alternative possibilities with SDSC mentors, contacts, suppliers, etcetra.  This was a long process, and it would mean that we would not have a system. We spoke with local vendors about systems. Our own sponsors looked at their contacts. And the sponsor network really grew. It was a lot of emails, calls, and meetings just to get one promise of a component at a time.

The first system integrator was Applied Data Systems, who even went through the effort of driving a node up to SDSC. This was the first machine we started to work on, "*Percy*" (though early on we dubbed it Ryan after the sponsor). This node, however, would not be able to accomodate MI210 GPUs due to the slots of the FHFL 2PCIe width slots. Then Aeon Computing provided us with the second system that could fit 4 pairs of MI210s.

At the same time we were trying to even necessary peripherals. Our system integrators couldn't find them, but our sponsors at AMD were able to promise 4xMI210s and their Infinity Fabric Links. And Micas Networks had hooked us up with their Broadcom contacts and labs. So they were also getting 3 400Gb NICs for us.

Are you keeping track? We have a single node, a 400Gb switch, a promise of another machine, AMD will support us but it's not clear when or how we'll get hardware, we still need 4xMI210s, still speccing out a new system, getting 400Gb NICs soon, still need some kind of cable that supports this speed, don't forget about our LIQID Honey Badger cards, and we have our hardware nodes.

In the next month, Michael Woodacre from HPE was able to provide us with a generous 4xMI210s, which he sent to us and arrived shortly after. Trying to put them into our system we realized that the power cables packaged with the system were VHPWR and not EPS12 that the MI210s need, so we couldn't even use these. Now we had a system from Aeon computing and bringing our system up to 2/3. And despite now having 400Gb NICs and a 400Gb switch, we needed to configure it. 

Finally, through another email chain we spoke with International Computer !FIXME (ICC-USA) for a promise of a final node that could fit the MI210s, loan of 8xEPS12V from Aeon Computing, and spoke with Pivotal Optics, a main cable supplier of SDSC, and their engineers for a final 400Gb Direct Access Copper cable of 3m.

I want you to realize that during this whole time our teams are working on building and testing the software at the same time the system integration is happening in real time. At no point does the system look the same every week.

Now, knwoing our issues SDSC has accomodated the team with Express Shipping, and will allow us to ship Monday morning to arrive on Friday/Saturday of our competition. And we have a load of hardware/software support problems.

We've finally got all hardware of our final cluster but: the AMD GPU BIOS from HPE are different BIOS versions, and are set to read only, even with their free firmware tools. The copper cables aren't working. We've installed the NICs and set up the Switch, but the copper cables are not working, even after going to the bios and correcting the relevant BIOS tools. And we do not have our cluster up working.


### Dawn of the final weekend

<!--TODO: Fill with Dawn of the final day, or some zelda majoras mask reference -->

Friday, 3 days until shipping. We are spending all day at SDSC working. The network is still not setup and the switch is still having some issues. But luckily, Kent Tsui, a Micas Network Engineer, is in San Diego and is willing to help us. At the same time, Wyatt from Liqid also comes to get an idea of our work and show some of the hardware LIQID is working on. So midday Kent comes over and helps us directly. It seems that their team had never used copper cables before. Optical cables expected some sort of port training, a communication between the switch and the NICs to operate on the same speeds and standards. Turning this feature of the NICs off in the BIOS was all we needed. We then just ran some testing:

```
[pacolord@percy bin]$  ib_write_bw -d bnxt_re0 -i 1 -F -R -D 10 --report_gbits -q 3 10.0.0.10
---------------------------------------------------------------------------------------
                    RDMA_Write BW Test
 Dual-port       : OFF          Device         : bnxt_re0
 Number of qps   : 3            Transport type : IB
 Connection type : RC           Using SRQ      : OFF
 PCIe relax order: ON
 ibv_wr* API     : OFF
 TX depth        : 128
 CQ Moderation   : 1
 Mtu             : 1024[B]
 Link type       : Ethernet
 GID index       : 3
 Max inline data : 0[B]
 rdma_cm QPs     : ON
 Data ex. method : rdma_cm
---------------------------------------------------------------------------------------
 local address: LID 0000 QPN 0x2c01 PSN 0xd2cf40
 GID: 00:00:00:00:00:00:00:00:00:00:255:255:10:00:00:12
 local address: LID 0000 QPN 0x2c03 PSN 0xbd282a
 GID: 00:00:00:00:00:00:00:00:00:00:255:255:10:00:00:12
 local address: LID 0000 QPN 0x2c04 PSN 0xc9572c
 GID: 00:00:00:00:00:00:00:00:00:00:255:255:10:00:00:12
 remote address: LID 0000 QPN 0x2c26 PSN 0x356926
 GID: 00:00:00:00:00:00:00:00:00:00:255:255:10:00:00:10
 remote address: LID 0000 QPN 0x2c0f PSN 0x55318
 GID: 00:00:00:00:00:00:00:00:00:00:255:255:10:00:00:10
 remote address: LID 0000 QPN 0x2c71 PSN 0x7d7282
 GID: 00:00:00:00:00:00:00:00:00:00:255:255:10:00:00:10
---------------------------------------------------------------------------------------
 #bytes     #iterations    BW peak[Gb/sec]    BW average[Gb/sec]   MsgRate[Mpps]
 65536      4159603          0.00               363.47             0.693268
---------------------------------------------------------------------------------------
```
Note that a `iperf3` isn't a good test and won't reach 363Gb. By using the ib benchmarks we're able to test that RDMA is availale on these nodes and are able to reach higher bandwidth since we don't have to wait for the CPU's permissions.

Setting up the firewall and installation of docker breaks our MPI instance on Sunday. So we finally figure out it's trying to route MPI processes through an incorrect port and Bryan Chin realized specifying the port or configuring the default solves this issue. When we finally have it working, Sunday afternoon, we're running final tests and we begin to ship the cluster, somewhere along 8pm.

### Shipping the system

We ship the cluster at 6am.


## Competing

### Final Score Breakdown

- many corrections later: 
**Our team won 4th overall and 1st among U.S. and European teams!**


Item                        | unweighted score | weighted score
:---------------------------|-----------------:|------------:
Poster                   5% | 69.400      /100 |  3.470
Benchmarking            15% | 70.700      /100 | 10.605
NAMD                    15% | 54.348      /100 |  8.152
ICON                    15% | 30          /100 |  4.500
Reproducibility         15% | 54          /100 |  8.100
Mystery Application     15% | 22.400      /100 |  3.360
System                  15% | 95          /100 | 14.250
Scavenger Hunt           5% | 94          /100 |  4.700
Total                  100% |                  | 57.137 / 100

### Conference Experience(?)

## Takeaways and Thanks

We'd like to extend thanks to the awesome contributions of the entirety of [SDSC](https://www.sdsc.edu/) and [CSE](https://cse.ucsd.edu/) at UCSD. Specifically the efforts of our mentors Mary P. Thomas, Martin Kandes, Mahidhar Tatineni, and Bryan Chin.

