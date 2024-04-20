---
title: "SCC23 Postmortem & Supercomputing 2024 info"
date: 2024-04-10
author: ["khai", "paco", "zixian"]
draft: false
math: true
---


## SC24 info
As of the publication of this post the application for UCSD's 2024 team has opened [here](https://na.eventscloud.com/scc24). For those applying the deadline is April 21 and notifications will be sent out the following week on the 29th. Extra dates are listed on the application page.

![](/images/SC24Logo.svg)

On top of applying as a competitor for the student cluster competition, the Supercomputing convergence hosts two opportunities for students to volunteer and attend the convergence that being the roles of [Student Volunteer](https://sc24.supercomputing.org/students/student-volunteers/) and [Scinet Volunteer](https://sc24.supercomputing.org/scinet/). **Note** that the time commitment for student volunteers is the week of the conference and for SCInet volunteers being two weeks the week of the conference and the week preceding the conference. For those applying if you're from UCSD and are accepted as volunteers for either program please reach out to Mary Thomas, so we have sufficient lead time to allocate travel funding support.

# Student Cluster Competition 2023 postmortem
Formal announcements aside, Let's cover UCSD's experience at SCC23.

## Preparing
At the advent of the competition these were the specifications of the competition we were given.
- All software and hardware we used must be publicly available at the start of the competition.
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

We arrived Saturday afternoon into the hotel, just in time to meet the Tsinghua and the Boston BU<sup>3</sup> teams in the lobby. In the afternoon we began to set up our cluster of which you can see pictures in various places linked in the end of the article. And that late night, by speaking with the staff the PDUs going to be used could not accurately measure per node power, which meant that if any teams broke the 2K power limit they could get away with it. Because of some confusion with someone who wasn't directly responsible for the rules misinformation spread that we could achieve the same configuration of SCC22. That is, one big GPU filled node. 


By the next day, Saturday, news about the power measuring had spread and we opened up our machine again, pulled out our GPUs, and fit 8 of our MI210s into CAT. Our alternates, Triston and Jeremy, were busy completing HPL and Repro runs on the system as their last contributions before the commencments. Unfortunately during setting our booth our RPI's micro SD card had been bent, so we needed to reinstall the OS and set up our Grafana display. With no working KVM or a display our alternates and UCSD Student Volunteers went to target for a keyboard, and we borrowed a monitor from (we may remember incorrectly) the Peking team. Our on hand HDMI adapter for the RPI was so terrible it would turn off every couple seconds, complicating the installation. Though we had already set up the RPI once and had notes for the IPMI+Prometheus+Grafana we could still fall back on following instructions from home team member and supercomputing member Matei Gardus. Though to save time and effort the final set up required local forwarding the ip while ssh'd to the pi, which meant it was not directly viewable and did not ultimately provide the monitoring we had hoped for. 

That night our alternates and those without shifts in the morning worked hard to provide any last minute aid they can provide. Teams around us, especially with H100s, tested the power limit all night, with blaring alarms every few minutes that kept us awake breaking our eardrums and our minds after hours of these incessant sirens.

![](/post-media/scc23-postmortem/opening-nodes-scc23.jpg)


Monday began, but it was not yet time for the competition. Being at the booth floor for 2 days before setting up the change was drastic. There were booths everywhere. They corrected our misconception that we could divide our system's GPUs. *Uh-Oh*. So we, the competition team in the booth, once again opened up the systems and scrambled to bring GPUs to a configuration that would fit the competition requirements. This is what would now be our final configuration previously shown in the "Final Config" diagram.

But even after racking and connecting our cluster the network configuration no longer worked. Even the previously day we had broken our uplink working with the organizers network configuration. But today we were once again bottlenecked by having our lead Khai Vu as our most networking experienced guru. So we just sat there, trying to make sure we knew our benchmarks and trying to figure a connection, and waited. He had planned to take a later shift in the day. We messaged, we called, we did everything we could to figure out how to get to him. Now, before continuing, it is my belief (Francisco), that it shouldn't even be possible for a hotelier to be able to barge into someone's private room under the request of someone claiming to know him, but believe us we are grateful they let Bryan enter Khai's personal room with a hotel worker and woke him up. He came an hour later, wet hair and sat down to chug through some of the switch management for an hour. 

Less than an hour later it was past 8am we could no longer receive any external aid. As we began to get access to our cluster we began running what we could for HPL, HPCG, and MLPerf. These ended with mixed results, with most of them not reflecting the results we had on the same cluster the previous weeks. We also had good instructions for STREAM and OSU benchmarks, with some quick instructions by our home team, like James Choi. Very easy to parse and run, especially when some of the parameters were previously optimized. By the time benchmarks were nearly over we received a clarification that we are allowed to go overpower during the benchmarking section, just not during applications.

When 5pm we collected in front of our booths to reveal the mystery app. It was a CTF. Capture the Flag... Uh oh.... We could receive points for social engineering by the form of scanning QR Codes. We could be docked points by having vulnerabilties in our system and earn points if we found any ourselves in anything, like a strange executable or permissions on the file server they provided for submissions. More points could be earned via packet analysis by some pcap files that would be a mix of standard practice ones but also *our own packets*. And we had to facilitate their grading we had to compile a report of all we had done. 

The problem was, we were not really cyber security people. We had received no real training and most of us were far stronger on hardware and general computational programs. Most of the teams began to realize this was true for themselves as well...

*Speaking from my perspective (Francisco)*: 

*Meanwhile, the information for our applications like 3DMHD and MPAS-A was up. Though we had not done much to change our code, each of us had run them plenty of times, and we were relatively familiar with the documentation and the programs. Gloria had tested the parameters for so long she felt comfortable handling the 3 test cases given to her. And I, having seen the depth and various parts of MPAS-A, had expected to be asked to run some of the data prepping programs, the `init_atmosphere` core, and was afraid of some curveball. **Nope**. I took the two tickets that had been given to me, gasped in relief, and cashed in my 2 free drinks. That night Gloria and I began running our apps. We would be finished a little past midday the next day.*

For Tuesday we spent it all trying to understand the mystery app and do our part. We also took some shifts to explore around the booths. All afternoon Khai and Kyle were desperately trying to finish up the Reproduction Application Report. In conjunction with the University of Kansas they made good progress, and humorously found that the theoretical limits discussed in the paper were not so easily reached on our cluster because our hardware. Meanwhile our unfamiliarity with CTF led different teams to began colluding against the organizer. We spoke about working together, just at least to at least take this opportunity to learn.

On our last day Kyle and Khai had been working on the paper all night, and unfortunately had gone over power for around 12 seconds recorded, which happened with an unexpected spike a few hours after midnight. The Zurich team announced good overnight progress in their mystery app, but that they were no longer willing to work with other teams. So that idea was disbanded. Austin took the lead in finding weak information in the sample packets given to us, finding some bare text emails and some other vulnerable packets. We advanced on other fronts, but not very much. In the end we turned in what we had, which was relatively complete, and finished.

The next day, upon giving us the results, we saw we had placed 3rd overall. While there had been a lot of mishaps other teams had many as well. We also received 2 special awards. One because Francisco found BU<sup>3</sup>'s missing cluster, and another co-winning MLPerf because of Zixian's efforts in porting MLPerf to AMD's MI210's. Overall, pretty fun.


### Final Score Breakdown

**Our team won 3rd overall and 1st among U.S. teams!**


Item                        | unweighted score | weighted score
:---------------------------|-----------------:|------------:
Poster                   5% | 76          /100 |  3.800
Benchmarking            16% | 82.525      /100 | 13.204
MPAS-A                  16% | 50          /100 |  8.000
3DMHD                   16% | 92.183      /100 | 14.749
Reproducibility         16% | 88          /100 | 14.080
Systems and Mystery App 31% | 82.842      /100 | 25.681
Total                  100% |                  | 79.514 / 100

## Takeaways and Thanks

From SCC we learned that familiarizing yourself with the capabilities of your hardware is very important. And some of the best teams had this because they had a lot of time with their cluster, even beating teams that had focused on the overall beefyness of their hardware but had had little time with it. *start early, start often*. Get your stuff early. Enjoy the booths. Talking to booth presenters is a great way to network. They have a lot of merch but also engineers who can talk about their servers and hardware. That's not an opportunity accessible to everyone. And I don't how many times I saw Gloria return to the Microsoft booth for their robot arm barista. Come to the event with resume's, contacts, qr codes, link trees, vCards/MeCards, anything that can get your name out. It will be useful for the job fair as well. Double check your submissions and have someone else check as well. MPAS-A had been completed more than a day early, but had not submitted a correct file. And have fun. It's not everyday one gets to enjoy the Supercomputing Conference.

We'd like to extend thanks to the awesome contributions of the entirety of [SDSC](https://www.sdsc.edu/) and [CSE](https://cse.ucsd.edu/) at UCSD. Specifically the efforts of our mentors Mary P. Thomas, Martin Kandes, Mahidhar Tatineni, and Bryan Chin.

More pictures can be found via [Jeremy Tow's gallery](https://lychee.jeremyztow.com/gallery#lJbOq1zOLN7nrtH_z7_tTmW_) and the Students+SCC section on the [official SC23 site](https://scphoto.passgallery.com/-sc23/gallery).
