---
title: "Advent of Supercomputing: Day 3"
date: 2023-12-03
author: ["khai"]
#publishDate: 2023-12-03T08:00:00Z
draft: false
---

# Day 3: `gnuplot`

Because `gnuplot` supports its own languages for parsing and displaying data, it can be its own long rabbit hole see: http://www.gnuplot.info/. So instead we will cover only a super small subset of it.

## Basics
the basics of displaying both data and functions begins with the `plot` command in gnuplot

```
$ gnuplot
gnuplot> plot sin(x)
```
![sinx](/post-media/advent_2023_3/sinx.jpg)

note that we can also chain multiple functions together using commas (this also carries over to any dataset we use as input)

```
$ gnuplot
gnuplot> plot [][-2:2] sin(x), x, x-(x**3)/6 
```
![multi-plot](/post-media/advent_2023_3/sinx-multi.jpg)
in this example note that `[][-2:2]` simply means let the range of the y-axis to be on the interval `[-2,2]` and automatically determine the range for the x-axis

## Working with data
let the following data be the contents of a file named `gflop`
```
# Gflops per blocksize -- with a theoretical peak as a reference
250.00	1,231.25	1969
275.00	980.63	1969
300.00	1,383.11	1969
325.00	1,230.21	1969
350.00	1,455.04	1969
375.00	1,318.66	1969
400.00	1,507.56	1969
425.00	1,459.33	1969
450.00	1,508.65	1969
475.00	1,502.53	1969
500.00	1,567.21	1969
525.00	1,519.84	1969
550.00	1,593.53	1969
575.00	1,569.37	1969
600.00	1,624.60	1969
625.00	1,589.73	1969
650.00	1,606.94	1969
675.00	1,602.36	1969
700.00	1,632.07	1969
725.00	1,592.40	1969
750.00	1,632.98	1969
775.00	1,604.62	1969
800.00	1,630.14	1969
825.00	1,624.44	1969
850.00	1,613.97	1969
875.00	1,604.04	1969
900.00	1,619.33	1969
925.00	1,583.05	1969
950.00	1,622.03	1969
975.00	1,608.84	1969
1,000.00	1,585.21	1969
1,025.00	1,590.91	1969
1,050.00	1,607.77	1969
1,075.00	1,607.29	1969
1,100.00	1,606.71	1969
1,125.00	1,601.59	1969
1,150.00	1,589.23	1969
1,175.00	1,568.58	1969
1,200.00	1,558.65	1969
1,225.00	1,548.97	1969
1,250.00	1,519.78	1969
1,275.00	1,546.26	1969
1,300.00	1,547.10	1969
1,325.00	1,513.70	1969
1,350.00	1,483.74	1969
1,375.00	1,514.94	1969
1,400.00	1,504.72	1969
1,425.00	1,491.88	1969
1,450.00	1,500.26	1969
1,475.00	1,458.50	1969
1,500.00	1,482.84	1969
```
```
> gnuplot
gnuplot> plot [][0:2000] "data.dat" using 1:2 title "Gflops/node" with linespoints, \
                         "data.dat" using 1:3 title "Theoretical Peak" with lines 
```

![gflop](/post-media/advent_2023_3/gflop.jpg)
Yes, this is some of the data generated for the reproducability paper for SCC23. note though that each series is inputted into plot in the format `[series data] [fields] [title] [display format]` note though that the order for each field specifying display parameters of the data does not matter outside of the actual series data itself which must always precede all such parameters. Within the interactive session if you want to save the sequence of commands and any other hidden parameters set by the interactive session call `save gflop.gp` in the interactive session to save it and should look something like.
```gnuplot
#!/usr/bin/gnuplot -persist
#
#    
#    	G N U P L O T
#    	Version 5.4 patchlevel 10    last modified 2023-10-20
#    
#    	Copyright (C) 1986-1993, 1998, 2004, 2007-2023
#    	Thomas Williams, Colin Kelley and many others
#    
#    	gnuplot home:     http://www.gnuplot.info
#    	faq, bugs, etc:   type "help FAQ"
#    	immediate help:   type "help"  (plot window: hit 'h')
# set terminal qt 0 font "Sans,9"
# set output
unset clip points
set clip one
unset clip two
unset clip radial
set errorbars front 1.000000 
set border 31 front lt black linewidth 1.000 dashtype solid
set zdata 
set ydata 
set xdata 
set y2data 
set x2data 
set boxwidth
set boxdepth 0
set style fill  empty border
set style rectangle back fc  bgnd fillstyle   solid 1.00 border lt -1
set style circle radius graph 0.02 
set style ellipse size graph 0.05, 0.03 angle 0 units xy
set dummy x, y
set format x "% h" 
set format y "% h" 
set format x2 "% h" 
set format y2 "% h" 
set format z "% h" 
set format cb "% h" 
set format r "% h" 
set ttics format "% h"
set timefmt "%d/%m/%y,%H:%M"
set angles radians
set tics back
unset grid
unset raxis
set theta counterclockwise right
set style parallel front  lt black linewidth 2.000 dashtype solid
set key notitle
set key fixed right top vertical Right noreverse enhanced autotitle nobox
set key noinvert samplen 4 spacing 1 width 0 height 0
set key maxcolumns 0 maxrows 0
set key noopaque
unset label
unset arrow
unset style line
unset style arrow
set style histogram clustered gap 2 title textcolor lt -1
unset object
unset walls
set style textbox  transparent margins  1.0,  1.0 border  lt -1 linewidth  1.0
set offsets 0, 0, 0, 0
set pointsize 1
set pointintervalbox 1
set encoding default
unset polar
unset parametric
unset spiderplot
unset decimalsign
unset micro
unset minussign
set view 60, 30, 1, 1
set view azimuth 0
set rgbmax 255
set samples 100, 100
set isosamples 10, 10
set surface implicit
set surface
unset contour
set cntrlabel  format '%8.3g' font '' start 5 interval 20
set mapping cartesian
set datafile separator whitespace
set datafile nocolumnheaders
unset hidden3d
set cntrparam order 4
set cntrparam linear
set cntrparam levels 5
set cntrparam levels auto
set cntrparam firstlinetype 0 unsorted
set cntrparam points 5
set size ratio 0 1,1
set origin 0,0
set style data points
set style function lines
unset xzeroaxis
unset yzeroaxis
unset zzeroaxis
unset x2zeroaxis
unset y2zeroaxis
set xyplane relative 0.5
set tics scale  1, 0.5, 1, 1, 1
set mxtics default
set mytics default
set mztics default
set mx2tics default
set my2tics default
set mcbtics default
set mrtics default
set nomttics
set xtics border in scale 1,0.5 mirror norotate  autojustify
set xtics  norangelimit autofreq 
set ytics border in scale 1,0.5 mirror norotate  autojustify
set ytics  norangelimit autofreq 
set ztics border in scale 1,0.5 nomirror norotate  autojustify
set ztics  norangelimit autofreq 
unset x2tics
unset y2tics
set cbtics border in scale 1,0.5 mirror norotate  autojustify
set cbtics  norangelimit autofreq 
set rtics axis in scale 1,0.5 nomirror norotate  autojustify
set rtics  norangelimit autofreq 
unset ttics
set title "" 
set title  font "" textcolor lt -1 norotate
set timestamp bottom 
set timestamp "" 
set timestamp  font "" textcolor lt -1 norotate
set trange [ * : * ] noreverse nowriteback
set urange [ * : * ] noreverse nowriteback
set vrange [ * : * ] noreverse nowriteback
set xlabel "" 
set xlabel  font "" textcolor lt -1 norotate
set x2label "" 
set x2label  font "" textcolor lt -1 norotate
set xrange [ * : * ] noreverse writeback
set x2range [ * : * ] noreverse writeback
set ylabel "" 
set ylabel  font "" textcolor lt -1 rotate
set y2label "" 
set y2label  font "" textcolor lt -1 rotate
set yrange [ * : * ] noreverse writeback
set y2range [ * : * ] noreverse writeback
set zlabel "" 
set zlabel  font "" textcolor lt -1 norotate
set zrange [ * : * ] noreverse writeback
set cblabel "" 
set cblabel  font "" textcolor lt -1 rotate
set cbrange [ * : * ] noreverse writeback
set rlabel "" 
set rlabel  font "" textcolor lt -1 norotate
set rrange [ * : * ] noreverse writeback
unset logscale
unset jitter
set zero 1e-08
set lmargin  -1
set bmargin  -1
set rmargin  -1
set tmargin  -1
set locale "en_US.UTF-8"
set pm3d explicit at s
set pm3d scansautomatic
set pm3d interpolate 1,1 flush begin noftriangles noborder corners2color mean
set pm3d clip z 
set pm3d nolighting
set palette positive nops_allcF maxcolors 0 gamma 1.5 color model RGB 
set palette rgbformulae 7, 5, 15
set colorbox default
set colorbox vertical origin screen 0.9, 0.2 size screen 0.05, 0.6 front  noinvert bdefault
set style boxplot candles range  1.50 outliers pt 7 separation 1 labels auto unsorted
set loadpath 
set fontpath
set psdir
set fit brief errorvariables nocovariancevariables errorscaling prescale nowrap v5
GNUTERM = "qt"
I = {0.0, 1.0}
VoxelDistance = 0.0
x = 0.0
## Last datafile plotted: "data.dat"
plot [][0:2000] "data.dat" using 1:2 title "Gflops/node" with linespoints , "data.dat" using 1:3 title "Theoretical Peak" with lines
#    EOF
```
as seen this will give you an expressly verbose file

# Automation and Exporting
the file you saved from the interactive session can then be used to call to just display what was from the interactive session. Also we can do similar from an interactive session with `load gflop.gp`
```
$ gnuplot --persist gflop.gp
```
![persist](/post-media/advent_2023_3/persist.jpg)
note that this looks slightly different due to some scaling parameters not being exported

however for a script that displays the basic data of the saved scripted we simply need
```gnuplot
plot [][0:2000] "data.dat" using 1:2 title "Gflops/node" with linespoints , "data.dat" using 1:3 title "Theoretical Peak" with lines
```
if we prepend the lines `set terminal png` and `set output "gflop.png"` with our file now looking like
```gnuplot
set terminal png
set output "gflop.png"
plot [][0:2000] "data.dat" using 1:2 title "Gflops/node" with linespoints , "data.dat" using 1:3 title "Theoretical Peak" with lines
```
In this case we run `gnuplot gflop.gp` ommitting the `--persist` flag since we'll be outputting the above image to a file. To allow for arguments to be set within our gnuplot scripts we can use the syntax "$N" where N is some number 0-9 and is invokedd like `gnyplot <script name> [script arguments]` and the addressing is similar to bash argument addressing. For example, in the previous script if we replace "gflop.png" with "$0", calling our script will be along the lines of `gnuplot gflop.gp gflop.png` where the 1st argument of the script is the name of the image file.

## Eye candy
below I will list some examples of some more advanced gnuplot.

### [4d Heat map data](https://gnuplot.sourceforge.net/demo_6.0/heatmap_4D.html) 
![4d heat](/post-media/advent_2023_3/4d_heat.jpg)

### [Specular Highlighting](https://gnuplot.sourceforge.net/demo_6.0/pm3d_lighting.html)
![spec-light](/post-media/advent_2023_3/spec_lighting.jpg)

### [violin plots](https://gnuplot.sourceforge.net/demo_5.4/violinplot.html)
![violin](/post-media/advent_2023_3/violin.jpg)

### [3d projections on 2d](https://gnuplot.sourceforge.net/demo_5.4/projection.html)
![projection](/post-media/advent_2023_3/projection.jpg)
