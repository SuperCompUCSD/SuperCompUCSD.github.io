---
title: "Advent of Supercomputing: Day 16"
date: 2023-12-16
author: ["paco"]
publishDate: 2023-12-16T08:00:00Z
draft: false 
---


# tesseract-ocr

Tesseract is an open source ocr (optical character recognition) engine. 
Besides being a powerfull library it also makes for a fun tool. It takes an *image* and interprets the *text/characters* found in it. Some options are changing to read from stdin or a file, as well as choosing to output to stdout or into some file .pdf, txt, etc.

Take for example the image `example.png`:

<div style="text-align:center" >
    <img src="/post-media/tesseract-example.png" alt="example-usage-from-wiki" width="400"/>
</div>

```bash
$ tesseract-ocr example.png - -l eng

The (quick) [brown] {fox} jumps!
Over the $43,456.78 <lazy> #90 dog
& duck/goose, as 12.5% of E-mail
from aspammer@website.com is spam.
Der ,schnelle” braune Fuchs springt
iiber den faulen Hund. Le renard brun
«rapide» saute par-dessus le chien
paresseux. La volpe marrone rapida
salta sopra il cane pigro. El zorro
marrén rapido salta sobre el perro
perezoso. A raposa marrom ripida
salta sobre o cdo preguigoso.
```

I'm sure others can bring better purpose to `tesseract-ocr` than I can. My daily use of it comes when a professor posts their notes/lectures as an image. Here is a small script that allows me to select an area of my screen and brings it to my clipboard for easy copy and pasting:
```bash
filetmp=$(mktemp -t wayshot-XXXX.png)
wayshot -s "$(slurp)" 
mv [0-9]*-wayshot.png $filetmp
tesseract-ocr $filetmp stdout -l eng | wl-copy
rm $filetmp
# Yes this script is not great, but gets the job done
```

Though this script makes use of `wayshot` for screenshots, `wl-copy` for the clipboard, and `slurp` for screen area selection. If you're using X you can probably do this in one line by using `scrot` and piping into `tesseract-ocr` and you don't have to go through the hoops of making this a script/function.


You can find the package in most linux distribution's repos. Make sure to download the language you are looking for as well as the program and the language data often come separately. If you want to learn more then check out their [docs](https://github.com/tesseract-ocr/tessdoc#usage). Read the manpage too.

You can thank Hewlett-Packard Enterprise and now Google for this. Unrelated, but check out the current [Top500](https://top500.org/lists/top500/list/2023/11/) list, HPE cray supercomputers hold the top 2 positions.
