---
title: "SCC23 Postmortem & Supercomputing 2024 info"
author: khai
date: 2024-04-10
draft: true
---
## SC24 info
As of the publication of this post the application for UCSD's 2024 team has opened [here](https://na.eventscloud.com/scc24). For those applying the deadline is April 21 and notifications will be sent out the following week on the 29th. Extra dates are listed on the application page.

![](/images/SC24Logo.svg)

On top of applying as a competitor for the student cluster competition, the Supercomputing convergence hosts two opportunities for students to volunteer and attend the convergence that being the roles of [Student Volunteer](https://sc24.supercomputing.org/students/student-volunteers/) and [Scinet Volunteer](https://sc24.supercomputing.org/scinet/). **Note** that the time commitment for student volunteers is the week of the conference and for SCInet volunteers being two weeks the week of the conference and the week preceding the conference. For those applying if you're from UCSD and are accepted as volunteers for either program please reach out to Mary Thomas, so we have sufficient lead time to allocate travel funding support.

# Student Cluster Competition 2023 postmortem
Formal announcements aside, Let's cover UCSD's experience at SCC23.

## Preparing
At the advent of the competition these were the specifications of the competition we were given.
- All software and hardware we used must be publically available at the start of the competition.
- 4 kW limit on a 3 node cluster
- Benchmarks are: HPL, HPCG, and MLPERF inference and the optional OSU and Stream benchmarks
- Applications are: MPAS-A, 3DMHD, and Reproducibility (a numerical method to reduce the computation intensity of Cholesky Decomposition)

### Speccing out the system

We committed very late that it was unlikely that our team could get a system with at least A100s and so committed to a cluster of 12xMI210s (which we later scaled down to 8x) with the latest 2xAMD Genoa 9684X cpus per node allowing us to have a whole 2304 MB of L3 cache per node. Along with a Spectrum 2 200G switch, we had the following possible configurations for our cluster architecture, all dependant on the power characteristics of our cluster.

![](/post-media/scc23-postmortem/cluster-architecture.svg)

The main things to note from our final cluster architecture is the moving of all cluster storage to a 40G FDR connection and all compute communications were reserved to a 200G connection. This was done primarily via putting both connections on disjoint `/25` subnets. Aside from intra-cluster communication we had a 1G connection to each node for access to the cluster and internet uplink. To monitor the power usage of the cluster we employed the usage of a Raspberry Pi running Grafana and querying the IPMI interface remotely circumventing the utilization of cluter resources to peer into the power consumption, though something we noted was the IPMI interface was on average 100 to 150W below that of the actual cluster power consumption.

### Receiving the system

Due to the before mentioned late commitment, we received our system three weeks before the beginning of the competition and two weeks before we had to ship it out. We were however blessed with both the resources of SDSC along with access to the cluster we took to SCC22 the year prior, as a result it allowed us for an environment to set up an Ansible playbook to set up every aspect of the system before we ever received it. This allowed us once the system arrived to stand up the system and working on benchmarks and applications roughly 24 hours we had received it.

![](/post-media/scc23-postmortem/cluster-install.avif)
![](/post-media/scc23-postmortem/unpacking.avif)

### Preparing for Benchmarks and Applications
While waiting for the arrival of the cluster we were blessed with still having access to last year's cluster, specifically access to Neil our 4xMI250 node from SCC22 which allowed the entire team to get tooling working around AMD's accelerator stack especially since our home institution didn't have doesn't have a system with the AMD accelerators. The main struggle we had was fronted by my colead, Zixian Wang, who spend the majority of his time optimizing MLPERF's performance on the AMD accelerators due to the less mature software stack for AI workloads. 

The tuning of HPL were put onto Austin Garcia and Triston Babers. Since AMD already had a container to run hpl in the form of [rocHPL](https://github.com/amd/InfinityHub-CI/tree/main/rochpl) it was more a matter of tuning the HPL input parameters to get optimal performance. One opportune night Triston full sent a HPL series of 1024 runes to brute force block size search, which due to not communicating with the team send everyone in a panic but as a result was able to find the optimal block size to be 384.

![](/post-media/scc23-postmortem/neil-hpl.png)

For 3DMHD Considering it was a Fortran 77 code that ran only on cpu -- we believed it was better to learn the performance characteristics of the code and understanding the supporting papers rather than perform a complete rewrite in modern languages like other teams had done. We utilized Expanse to mimic the distributed scaling model we would get on the new cluster and the old cluster to gauge the performance of the new with their larger cpu caches. With our single node runtime performance based on grid parameters listed below. All I myself did was assisting in tooling around 3dmhd; all the domain knowledge was handled by Gloria Seo. 

Below is the runtime of mhd given a fixed problem size and varying the grid parameters NPEY and NPEZ.

![](/post-media/scc23-postmortem/neil-runtime.avif)
![](/post-media/scc23-postmortem/mhd-runtime.avif)

Repro at least before the cluster's arrival was handled primarily by Jeremy Tow, later by Kyle Smith and myself.

### Shipping the system

the night before we had to ship out the cluster (that being 

## Competing




### Final Score Breakdown
```
Item                        | unweighted score | weighted score
---------------------------------------------------------------
Poster                   5% | 76          /100 |  3.800
Benchmarking            16% | 82.525      /100 | 13.204
MPAS-A                  16% | 50          /100 |  8.000
3DMHD                   16% | 92.183      /100 | 14.749
Reproducibility         16% | 88          /100 | 14.080
Systems and Mystery App 31% | 82.842      /100 | 25.681
Total                  100% |                  | 79.514 / 100
```

## Takeaways

## Thanks
