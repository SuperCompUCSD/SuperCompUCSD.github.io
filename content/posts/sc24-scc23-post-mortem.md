---
title: "SCC23 Postmortem & Supercomputing 2024 info"
date: 2024-04-10
author: ["khai", "paco", "zixian"]
draft: false
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

We committed very late that it was unlikely that our team could get a system with at least A100s. Probably due to the AI boom happening at that time the likelihood of receiving NVIDIA GPUs became more and more unlikely over time. This had been pretty important because the MLPERF benchmark only ran on NVIDIA GPUs. What we settled on was 12xMI210s with the latest 2xAMD Genoa 9684X cpus per node allowing us to have a whole 2304 MB of L3 cache per node. A cluster of AMD CPUs and GPUs was what we had used at SC22, so we were somewhat familiar. Additionally MI210's have 1 instead of 2 GCD, the CDNA 2 "Aldebaran", but they are still essentially the MI250. So the are the same software wise with the exception of less computation but with a relatively low TDP we could split between our nodes more optimally than we had the year before . Along with a Spectrum 2 200G switch and a couple of I/O Accelerators LIQID 4500 "Honey Badger", we had the following possible configurations for our cluster architecture, all dependant on the power characteristics of our cluster.

![](/post-media/scc23-postmortem/cluster-architecture.svg)

The main things to note from our final cluster architecture is the moving of all cluster storage to a 40G FDR connection and all compute communications were reserved to a 200G connection. This was done primarily via putting both connections on disjoint `/25` subnets. Aside from intra-cluster communication we had a 1G connection to each node for access to the cluster and internet uplink. To monitor the power usage of the cluster we employed the usage of a Raspberry Pi (show as 'Grafana Laptop' in the diagram above) running Grafana and querying the IPMI interface remotely circumventing the utilization of cluter resources to peer into the power consumption, though something we noted was the IPMI interface was on average 100 to 150W below that of the actual cluster power consumption.

Here are the names for each of the machines:<br>
**Raspberry Pi**: *The Hat*<br>
**Node 1 with the LIQID cards**: *The Cat*<br>
**Node 2 with the LIQID cards**: *Thing 1*<br>
**Node 3 with the LIQID cards**: *Thing 2*<br>

Pretty clever, right?

**[Team Poster](https://www.studentclustercompetition.us/2023/Teams/California/index.html):**
![Post-SCC24](https://www.studentclustercompetition.us/2023/Teams/California/California.Poster.jpg)

### Receiving the system

Due to the before mentioned late commitment, we received our system three weeks before the beginning of the competition and two weeks before we had to ship it out. We were, however, blessed with both the resources of SDSC along with access to the cluster we took to SCC22 the year prior, as a result it allowed us for an environment to set up an Ansible playbook to set up every aspect of the system before we ever received it. This allowed us once the system arrived to stand up the system and working on benchmarks and applications roughly 24 hours we had received it. 

![](/post-media/scc23-postmortem/cluster-install.avif)
![](/post-media/scc23-postmortem/unpacking.avif)

Back when we had begun practicing on the SC22 cluster in the early Fall Quarter we knew we would **not** be allowed to run them in our club rooms because of the noise disturbance and safety hazard of such a heat generating machine. So we were allowed to bring one of our full racks down and into a space in the data center EE13, a permission we received from our data center operations head after some back in forth in finding solutions to bring anything into the data center without directly using up data center rack space we share with research groups and companies. This spot was where we began to keep our machines and were the SCC23 cluster would be placed when it arrived. 

We were surprised to find a lack of infiniband cables, but luckily NRP was willing to lend us some to reach that 200G connection. We racked the nodes. We inserted the IO accelerators and looked at the previous year's Michael Granado guide on tuning the BIOS to maximise the thorogoughput for these accelerators. We set up switches, our uplink via SDSC, a basic ssh connection, and then we had our whole config ready to go.

### Preparing for Benchmarks and Applications
While waiting for the arrival of the cluster we were blessed with still having access to last year's cluster, specifically access to "Neil" our 4xMI250 node from SCC22 which allowed the entire team to get tooling working around AMD's accelerator stack especially since our home institution didn't have doesn't have a system with the AMD accelerators. The main struggle we had was fronted by my colead, Zixian Wang, who spend the majority of his time optimizing MLPERF's performance on the AMD accelerators due to the less mature software stack for AI workloads. 

The tuning of HPL were put onto Austin Garcia and Triston Babers. Since AMD already had a container to run hpl in the form of [rocHPL](https://github.com/amd/InfinityHub-CI/tree/main/rochpl) it was more a matter of tuning the HPL input parameters to get optimal performance. One opportune night Triston full sent a HPL series of 1024 runes to brute force block size search, which due to not communicating with the team send everyone in a panic but as a result was able to find the optimal block size to be 384.



Our SCC23 Team, led by Zixian Wang, helped solve industrial and community-wise challenges to be able to run MLPerf Inference Benchmark on AMD GPUs. Backend support for AMD ROCM did not exist and no single submission uses AMD GPUs on MLPerf Inference Benchmark. Zixian and Khai figured out the appropriate libraries and versions that successfully enabled inferencing model through AMD GPUs. We collaborated with MLCommons, founder of MLPerf Benchmark, for inclusion of PyTorch, ONNX Runtime and MIGraphX for AMD ROCM in their CM Automation pipeline to run MLPerf Benchmark. We deployed optimal batch size, multi-gpu inferencing and quantization. **Our SCC23 Team WON MLPerf Inference Benchmark** [link](https://www.studentclustercompetition.us/2023/index.html) and Zixian received a "significant MLPerf Community contribution" award for his effort. According to Zixian, he spent 40+ hours per week on porting and optimizing MLPerf benchmark. 

MPAS-A was handled by Francisco Gutierrez. The application is relatively well documentented, deep, and has seen some development in porting over the GPUs recently, but unfortunately mainly through portland group's toolset (now NVIDIA's toolset after acquisition and integration). Building up the dependencies, especially the Parallel IO (PIO), gave some issues due to not linking MPI properly, which may also have been due to our custom version of Open-MPI 4.1.X with UCC and UCX to support RDMA on our hardware. Ultimately the (PIO) was built via Spack, which was a pleasant package manager to learn since it is also used in some HPC centers including Expanse.

![](/post-media/scc23-postmortem/neil-hpl.png)

For 3DMHD Considering it was a Fortran 77 code that ran only on cpu -- we believed it was better to learn the performance characteristics of the code and understanding the supporting papers rather than perform a complete rewrite in modern languages like other teams had done. We utilized Expanse to mimic the distributed scaling model we would get on the new cluster and the old cluster to gauge the performance of the new with their larger cpu caches. With our single node runtime performance based on grid parameters listed below. Khai Vu assisted in tooling around 3dmhd; all the domain knowledge was handled by Gloria Seo. 

Below is the runtime of mhd given a fixed problem size and varying the grid parameters NPEY and NPEZ.

![](/post-media/scc23-postmortem/neil-runtime.avif)
![](/post-media/scc23-postmortem/mhd-runtime.avif)

Repro at least before the cluster's arrival was handled primarily by Jeremy Tow, later by Kyle Smith and Khai Vu.

### Shipping the system

The night before we had to ship out the cluster the travel team put the system back into its boxes. Unfortunately the box for the switch had been lost so another box from some other switch was used. In the end the complete cluster was set to depart a few hours after it had been packaged. The average time leaving SDSC was around 0:30am. Thanks to our mentor Mary P. Thomas we had also secured and disassembled a dedicated half rack to bring with us to SCC.

## Competing

We arrived Saturday afternoon into the hotel, just in time to meet the Tsinghua and the Boston ($BU^3$) teams in the lobby. In the afternoon we began to set up our cluster of which you can see pictures in various places linked in the end of the article. And that late night, by speaking with the staff the PDUs going to be used could not accurately measure per node power, which meant that if any teams broke the 2K power limit they could get away with it. Because of some confusion with someone who wasn't directly responsible for the rules misinformation spread that we could achieve the same configuration of SCC22. That is, one big GPU filled node. 

By the next day, Saturday, news about the power measuring had spread and we opened up our machine again, pulled out our GPUs, and fit 8 of our MI210s into CAT. Our alternates, Triston and Jeremy, were busy completing HPL and Repro runs on the system as their last contributions before the commencments. Unfortunately during setting our booth our RPI's micro SD card had been bent, so we needed to reinstall the OS and set up our Grafana display. With no working KVM or a display our alternates and UCSD Student Volunteers went to target for a keyboard, and we borrowed a monitor from (we may remember incorrectly) the Peking team. Our on hand HDMI adapter for the RPI was so terrible it would turn off every couple seconds, complicating the installation. Though we had already set up the RPI once and had notes for the IPMI+Prometheus+Grafana we could still fall back on following instructions from home team member and supercomputing member Matei Gardus. Though to save time and effort the final set up required local forwarding the ip while ssh'd to the pi, which meant it was not directly viewable and did not ultimately provide the monitoring we had hoped for.

![](/post-media/scc23-postmortem/opening-nodes-scc23.jpg)

Monday 8am the competition began. They corrected our misconception that we could divide our system's GPUs. *Uh-Oh*. So we once again opened up the systems and scrambled to bring GPUs to a configuration that would fit the competition requirements. This is what would now be our final configuration previously shown in the "Final Config" diagram. 




### Final Score Breakdown

Item                        | unweighted score | weighted score
:---------------------------|-----------------:|------------:
Poster                   5% | 76          /100 |  3.800
Benchmarking            16% | 82.525      /100 | 13.204
MPAS-A                  16% | 50          /100 |  8.000
3DMHD                   16% | 92.183      /100 | 14.749
Reproducibility         16% | 88          /100 | 14.080
Systems and Mystery App 31% | 82.842      /100 | 25.681
Total                  100% |                  | 79.514 / 100

## Takeaways

## Thanks

More pictures can be found via [Jeremy Tow's gallery](https://lychee.jeremyztow.com/gallery#lJbOq1zOLN7nrtH_z7_tTmW_) and the Students+SCC section on the [official SC23 site](https://scphoto.passgallery.com/-sc23/gallery).
