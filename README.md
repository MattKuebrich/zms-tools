# zmstools

These web utilities convert between .zms ([Z-MUSIC](https://web.archive.org/web/20240126144946/http://www.z-z-z.jp/zmusic/)) FM instrument definitions and [VOPM](http://picopicose.com/software.html) .opm sound banks and vise versa. 

You can paste in multiple instrument definitions and download the results as a .zms or .opm file. 

I made this so I could use FM instruments from .zms files found [on the web](http://most.bigmoney.biz/g0org/music/?cat=50&paged=4) in VOPM (and hence in a modern DAW). And conversely, to use the more user-friendly VOPM interface to design FM instruments and be able to easily move them back into Z-MUSIC MML. 

You use load these in the browser here:    
zms2opm (need updating)<br>
[opm2zms](https://mattkuebrich.com/zmstools/opm2zms.html)

Note: These are HTML/Javascript utilities, as I wanted them to be simple to use. The downside is they aren't viable for batch processing tons of individual files. 

# Z-MUSIC format

[Z-MUSIC](https://web.archive.org/web/20240126144946/http://www.z-z-z.jp/zmusic/) is a music driver for the Sharp X68000 computer. Z-MUSIC songs are written in Music Macro Language (MML) and saved as .zms source files. The FM instruments can be formatted in two ways, both which are supported in the zms2opm utility.

AL/FB separated format looks like this, where the FM algorithm and feedback parameters are separate values.

```
(@1	/ rap-sd
/	 AR  DR  SR  RR  SL  OL  KS  ML DT1 DT2 AME
	 31,  1, 31,  4, 12,  0,  0,  2,  0,  2,  0
	 31, 10, 10,  8,  8,  0,  0,  2,  0,  0,  0
	 31, 10, 10,  8,  8,  0,  1,  0,  0,  2,  0
	 31, 15, 10,  8, 15,  0,  0,  3,  0,  0,  0
/	 AL  FB  OM PAN  WF SYC SPD PMD AMD PMS AMS
	  4,  5, 15,  3,  3,  0,255,127,127,  7,  7)
```

They can also be like this, where AF is a combined value for both algorithm and feedback, packed into one (0-63) value. 

```
(v1,0	/ rap-sd
/	 AF  OM  WF SYC SPD PMD AMD PMS AMS PAN
	 44, 15,  3,  0,255,127,127,  7,  7,  3, 0
/	 AR  DR  SR  RR  SL  OL  KS  ML DT1 DT2 AME
	 31,  1, 31,  4, 12,  0,  0,  2,  0,  2,  0
	 31, 10, 10,  8,  8,  0,  0,  2,  0,  0,  0
	 31, 10, 10,  8,  8,  0,  1,  0,  0,  2,  0
	 31, 15, 10,  8, 15,  0,  0,  3,  0,  0,  0)
```

You can use commas, spaces or returns to seperate  values, so this is also valid:
```
(v1 0 44 15 3 0 255 127 127 7 7 3 0 31 1 31 4 12 0 0 2 0 2 0 31 10 10 8 8 0 0 2 0 0 0 31 10 10 8 8 0 1 0 0 2 0 31 15 10 8 15 0 0 3 0 0 0)
```

### Other Notes: 
* Make sure to paste in only the instrument definitions, not any other bits of MML.

* Rather than just output the instrument definitions, I've formatted the .zms file so it can be played without further modification. It just plays a few notes using the first defined instrument.

* Z-MUSIC instrument definitions don't contain a noise enable or noise frequency setting (which is present in VOPM). It is set with the ``@o`` macro and only works when using Channel 8. When converting from .opm to .zms, I've added rudimentary support for noise by adding the noise macro when it's enabled in the .opm instrument, but if you convert multiple instruments (with multiple noise settings), it won't really work. 

* If you do further editing to .zms files, it's important that they are saved using Windows-style CRLF line endings. I had troubles on Mac with files mysteriously not working because of this. In Visual Studio Code, you can switch between them by clicking the LF or CRLF text located on the lower right of the window. The utility should save them correctly though.

* The easiest way I've found to play .zms files without too much fuss is use [ZMDRIVE](https://t.co/ovnG4JLugE) for Windows (which also works in Wine).

# VOPM format

OPM (aka MiOPMdrv) sound banks can be loaded into the [VOPM/VOPMex](http://picopicose.com/software.html) VST plugin, which emulates the YM2151 (OPM) sound chip found in the x68000. 

 Here's how the same instrument definition looks in MiOPMdrv format:

 ```
//MiOPMdrv sound bank Paramer Ver2002.04.22 
//LFO: LFRQ AMD PMD WF NFRQ
//@:[Num] [Name]
//CH: PAN	FL CON AMS PMS SLOT NE
//[OPname]: AR D1R D2R	RR D1L	TL	KS MUL DT1 DT2 AMS-EN

@:0 rap-sd
LFO:255 127 127   3   0
CH:  3   5   4   7   7 120   0
M1: 31   1  31   4  12   0   0   2   0   2   0
C1: 31  10  10   8   8   0   0   2   0   0   0
M2: 31  10  10   8   8   0   1   0   0   2   0
C2: 31  15  10   8  15   0   0   3   0   0   0
```

### Other Notes:

* The converter saves an entire OPM bank of 128 instruments every time, even if only a few instruments are converted. The others will just be empty. This works best when importing them into VOPM.

# Similar projects
* [opm2mml by exodusmodules](https://nfggames.com/forum2/index.php?topic=5992.0)
* [mdxtools by vampirefrog](https://github.com/vampirefrog/mdxtools)
* [hootvopm by whut](https://git.lain.church/whut/hootvopm)
* [vgm2opm by Aidan Lawrence](https://github.com/AidanHockey5/MegaMIDI/tree/master/tools/vgm2opm)
* [MDXPG by SAM](https://web.archive.org/web/20180718095132/http://www.geocities.jp/sam_kb/VOPM/MDXPG/index.html)

# References
* [Z-MUSIC 2 documentation (Japanese)](https://nfggames.com/X68000/Documentation/Zmusic/zmusic2.txt)
* [OPM File Format wiki page @ vgmrips.net](https://vgmrips.net/wiki/OPM_File_Format)
* [New Voices and The OPM File Format by Aidan Lawrence](https://www.aidanlawrence.com/mega-midi-a-playable-version-of-my-hardware-sega-genesis-synth/#opmformat)



