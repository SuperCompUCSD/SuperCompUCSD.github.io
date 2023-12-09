---
title: "Advent of Supercomputing: Day 9"
date: 2023-12-08
#date: 2023-12-09
#publishDate: 2023-12-09T08:00:00Z
author: ["khai"]
draft: false
---

# `lstopo` & `lstopo-no-graphics`

`lstopo` is a utility that shows the cpu topology of you system and any pcie devices attached. For example the graphical output when running this on my laptop gives.

![laptop](/post-media/advent_2023_9/laptop-topo.jpg)

As shown it will display: the varying levels of cache, corresponding cores, NUMA domains, mapped pcie buses. For a more indept output the following is a single node on the UCSD SCC23 competition cluster.

![cluster](/post-media/advent_2023_9/4numa.png)

With this we find that a single NUMA domain corresponds to 3 groups of cores or core compute dies in AMD nomenclature. On socket/package labeled "L#1" each group has has a device with the pcie bus id "\*3:00:0" - that coresponds to a single MI210. The number next to any given device is the link speed it has to the CPU in GB/s. As such the 32 GB/s corresponds to 16x lanes of PCIe 4.0.



## exporting

To export this diagram to view the cpu topology of a headless system this is where the "[filename]" component comes in when you lookup `man lsptopo`
```
lstopo [ options ] ... [ filename ]
lstopo-no-graphics [ options ] ... [ filename ]
```

Where the export format is an `xml` file. Once you have the exported `xml` you can copy it to your local machine and view it with `lstopo -i <xml>`. To export either the xml or the the local machine's topology to an image simply make the last argument the file name of a png.
