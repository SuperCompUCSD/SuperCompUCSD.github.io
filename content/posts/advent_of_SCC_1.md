---
title: "Advent of the SuperComputing Club: Day 1"
date: 2023-11-27
author: ["khai"]
publishDate: 2023-12-01T08:00:00Z
draft: false
---


# Day 1:  `gping, mtr and nmap`


## pings
In most cases when trying to network connectivity `ping` is just enough for checking the latency of a connection to a given host.
```bash
19:23:31 ~ $ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=54 time=6.64 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=54 time=15.2 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 6.644/10.901/15.159/4.257 ms
```
but there are often cases when you want to view a graphed form of your ping statistics over time, that is where `gping` comes in.

```
10:30:51 ~ $ gping -s 1.1.1.1
1.1.1.1 (1.1.1.1)                 last 6.11ms min 5.19ms max 58.9ms avg 8.458ms jtr 360µs  p95 23.1ms  t/o 0
56.199ms│
        │
        │
        │                                                                                              •
        │                                                                                              •
        │                                                                                              •
        │                                                                                              •
47.611ms│                                                                                              •
        │                                                                                              •
        │                                                                                             ••
        │                                                                                             ••
        │                                                                                             ••
        │                                                                                             ••
39.023ms│                                                                                             ••
        │                                                                                             ••
        │                                                                                             ••
        │                                                                                             ••
        │                                                                                             ••
        │                                                                                             ••
30.435ms│                                                                                         •   ••
        │                                                                                         •   • •
        │                                                                                         •   • •
        │                                                                                         •• •  •
        │                                                                                         •• •  •
        │                                                                                         •• •  •
        │                                           •                                             •• •  •
21.847ms│                                           •                                             •• •  •
        │                                           •                                             •• •  •
        │                                           •                                             •• •  •
        │                                           •                                             • ••  •
        │                                           •                                             • ••  •
        │•                                   •      •                                            •  ••  •
13.259ms│•                      •            •      ••                                           •  ••  •
        │•                      •   •    •   •      ••                                           •  •   ••
        │ •               •    ••   •    •  ••      ••                                           •      ••
        │ •               ••   • • •• •  •  ••  •   ••••     •  • •             •        ••      •      ••
        │  ••••• ••• •• •• ••• • •••• • • • •• •••••••• •••• •• • ••• •    •  •• •    •••• ••  ••       •• •  ••
        │  • •  • • •  •••    •      • •• •• •••     •  •  ••• • •• ••••••••••    ••••     • ••          ••••• ••
4.671ms │
        └─────────────────────────────────────────────────────────────────────────────────────────────────────────
 19:25:14                                                 19:25:29                                        19:25:44
```
(note I use `-s` here due to hugo having issues rendering braille characters as monospace)

On top of being able to graph out ping with the `cmd` argument you're able to do the same for any arbirary command other than just ping, for example with `sleep`
```bash
19:28:15 ~ $ gping -s --cmd 'sleep 0.1'
sleep 0.1                         last 102.203min 101.138max 102.509avg 102.083mjtr 27µs   p95 102.467mt/o 0
109.654ms│
         │
         │
         │
         │
         │
         │
106.549ms│
         │
         │
         │
         │
         │
103.444ms│
         │
         │
         │
         │             ••                                            ••    ••
         │ ••••••••  ••  ••••••••     ••••••  ••  ••  •••••••••••   •  ••••  •••••••••••••••  ••••••••  •••••••••
100.339ms│         ••            •  ••      ••  ••  ••           •••                        ••        ••
         │                        ••
         │
         │
         │
         │
         │
97.234ms │
         │
         │
         │
         │
         │
94.129ms │
         │
         │
         │
         │
         │
91.024ms │
         └────────────────────────────────────────────────────────────────────────────────────────────────────────
  19:28:31                                               19:28:46                                         19:29:01
```


## routes
similar is true for the `traceroute` command
```bash
19:32:55 ~ $ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  100.80.240.2 (100.80.240.2)  3.450 ms  3.407 ms  3.397 ms
 2  m-core-vlan2966-gw.ucsd.edu (132.239.255.114)  3.367 ms  3.357 ms  3.255 ms
 3  vlan3574-nat-in-fg-vip.ucsd.edu (172.16.251.60)  3.252 ms  3.243 ms  3.233 ms
 4  * vl3575-nat-out-m-core.ucsd.edu (132.239.251.58)  3.181 ms  3.173 ms
 5  mx0-vlan2761-gw.ucsd.edu (132.239.254.162)  3.165 ms  3.157 ms  3.149 ms
 6  sand1-agg-01--ucsd--100g--01.cenic.net (137.164.23.176)  3.664 ms  2.594 ms  2.554 ms
 7  tus-agg8--sand1-agg-01--300g--01.cenic.net (137.164.11.84)  6.180 ms  5.434 ms  5.417 ms
 8  141.101.72.12 (141.101.72.12)  5.255 ms  5.240 ms  5.223 ms
 9  * 162.158.88.5 (162.158.88.5)  5.334 ms 172.70.208.4 (172.70.208.4)  6.057 ms
10  one.one.one.one (1.1.1.1)  5.149 ms  5.134 ms  5.121 ms
```
with the same command if we use `mtr` it will give us a persistant traceroute along with running ping statistics to a given hop.
```bash
19:32:59 ~ $ mtr 1.1.1.1
                                              My traceroute  [v0.95]
magnapinnidae (100.80.240.228) -> 1.1.1.1 (1.1.1.1)                                      2023-11-30T19:33:43-0800
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                                         Packets               Pings
 Host                                                                  Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 100.80.240.2                                                        0.0%    22    3.1   4.8   2.4  37.9   7.4
 2. m-core-vlan2966-gw.ucsd.edu                                        20.0%    21    2.7   3.7   2.6   6.9   1.1
 3. vlan3574-nat-in-fg-vip.ucsd.edu                                     0.0%    21   10.3   4.6   2.4  14.5   3.0
 4. vl3575-nat-out-m-core.ucsd.edu                                     33.3%    21    2.8   3.5   2.5   4.5   0.6
 5. mx0-vlan2761-gw.ucsd.edu                                            0.0%    21    3.7   4.3   2.6   9.9   1.9
 6. sand1-agg-01--ucsd--100g--01.cenic.net                              0.0%    21    3.5   6.3   2.9  27.5   5.6
 7. tus-agg8--sand1-agg-01--300g--01.cenic.net                          0.0%    21    8.6   7.4   5.9  14.0   2.0
 8. 141.101.72.12                                                       0.0%    21   14.2   9.5   5.8  36.7   6.8
 9. 172.70.204.4                                                        0.0%    21   67.2  14.2   5.8  67.2  16.4
10. one.one.one.one                                                     0.0%    21    5.5   6.8   5.5  13.0   1.8
```
we can even change how its displayed with by hitting `d`

```bash
                                              My traceroute  [v0.95]
magnapinnidae (100.80.240.228) -> 1.1.1.1 (1.1.1.1)                                      2023-11-30T19:37:14-0800
Keys:  Help   Display mode   Restart statistics   Order of fields   quit

                             Last  84 pings
 1. 100.80.240.2             ..................................................1..1.....13.......................
 2. m-core-vlan2966-gw.ucsd. ?1.?..?..?.??1.?..?...??..??.....???...?...1.......?1...?.?.1??..........????..?....
 3. vlan3574-nat-in-fg-vip.u ................................................1.11.......3a.......................
 4. vl3575-nat-out-m-core.uc ..?..??..???.?.?.?.....?.??.?.?....?.....???.2.?....??.??.?.?.??..?...1....?.?.11?..
 5. mx0-vlan2761-gw.ucsd.edu 2..........1.2...........................1...1..2..1.1.....3.........1..............
 6. sand1-agg-01--ucsd--100g ..1...................1.......1...............12...11..1..1.......................11
 7. tus-agg8--sand1-agg-01-- 1..1.....111.....1..........11...............2..1.1.1.....11............111.......1.
 8. 141.101.72.12            1......1.1..1.11........2...1.11...1.....2111b211...3....11a.11....1.11.1.2.11....1
 9. 172.70.204.4             ...11.1.....1....11.1...1.11111...11.11......311.11.2111..12...1.1...1...1..1..11..
10. one.one.one.one          ..............................1................1...111....221......1.1.............

Scale:  .:7 ms  1:25 ms  2:54 ms  3:95 ms  a:147 ms  b:211 ms  c:287 ms  >
```
where `?` denotes any dropped packets and `>` denotes that the ping to that given host returned faster than the previous hop due to how routes are dynamically determined. Note this is just the brief of it you can also export the data for any external analysis.


## exploration
outside of pure connectivity we also may want to know what ports and possibly services are open to those and that is where `nmap` comes in

for basic utiization for scanning a subnet `nmap 10.0.0.1/24` is much better interms of scanning a subnet rather than a bash for loop with ping `for i in $(seq 1 255); do ping -c1 -t2 10.0.0.$i; done`
If you try such on UCSD-PROTECTED wifi it will give you
```bash
19:44:22 ~ $ nmap 100.80.240.228/20
Starting Nmap 7.94 ( https://nmap.org ) at 2023-11-30 19:44 PST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify
 valid servers with --dns-servers
Nmap scan report for 100.80.240.1
Host is up (0.0000010s latency).
All 1000 scanned ports on 100.80.240.1 are in ignored states.
Not shown: 1000 filtered tcp ports (net-unreach)

Nmap scan report for 100.80.240.2
Host is up (0.00s latency).
All 1000 scanned ports on 100.80.240.2 are in ignored states.
Not shown: 1000 filtered tcp ports (net-unreach)

Nmap scan report for 100.80.243.97
Host is up (0.00s latency).
All 1000 scanned ports on 100.80.243.97 are in ignored states.
Not shown: 1000 filtered tcp ports (net-unreach)

Nmap scan report for 100.80.244.4
Host is up (0.0000010s latency).
All 1000 scanned ports on 100.80.244.4 are in ignored states.
Not shown: 1000 filtered tcp ports (net-unreach)

Nmap scan report for 100.80.245.65
Host is up (0.0000010s latency).
All 1000 scanned ports on 100.80.245.65 are in ignored states.
Not shown: 1000 filtered tcp ports (net-unreach)

Nmap scan report for 100.80.246.39
Host is up (0.0000010s latency).
All 1000 scanned ports on 100.80.246.39 are in ignored states.
Not shown: 1000 filtered tcp ports (net-unreach)

Nmap done: 4096 IP addresses (6 hosts up) scanned in 31.99 seconds
```
along with the AP blacklisting your mac address for roughly 10 minutes.
