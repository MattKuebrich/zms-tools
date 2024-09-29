# zmstools

These web utilities convert between .zms ([Z-MUSIC](https://web.archive.org/web/20240126144946/http://www.z-z-z.jp/zmusic/)) instrument definitions and [VOPM](http://picopicose.com/software.html) .opm sound banks and vise versa.
You can paste in multiple instrument definitions and download the results as a .zms or .opm file. 

I made this so I could use FM instruments from .zms files I found on the web in VOPM (and hence in a modern DAW). And conversely, to use the more user-friendly VOPM interface to design FM instruments and be able to easily move them back into Z-MUSIC MML.

You use load these in the browser here:<br>
<a href="https://mattkuebrich.com/zmstools/zms2opm.html">zms2opm</a><br>
<a href="https://mattkuebrich.com/zmstools/opm2zms.html">opm2zms</a>

# ZMS

[Z-MUSIC](https://web.archive.org/web/20240126144946/http://www.z-z-z.jp/zmusic/) is a music driver for the Sharp X68000 computer. Z-MUSIC songs are written in Music Macro Language (MML) and saved as .zms source files. The FM instruments can be formatted in two ways, both which are supported in the zms2opm utility. 

AL/FB separated format looks like this, where the FM algorithm and feedback parameters are separate values.

```
(@1	/ rap-sd.   
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
(v1,0	/ rap-sd.
/	 AF  OM  WF SYC SPD PMD AMD PMS AMS PAN
	 44, 15,  3,  0,255,127,127,  7,  7,  3, 0
/	 AR  DR  SR  RR  SL  OL  KS  ML DT1 DT2 AME
	 31,  1, 31,  4, 12,  0,  0,  2,  0,  2,  0
	 31, 10, 10,  8,  8,  0,  0,  2,  0,  0,  0
	 31, 10, 10,  8,  8,  0,  1,  0,  0,  2,  0
	 31, 15, 10,  8, 15,  0,  0,  3,  0,  0,  0)
```

### Other Notes: 
* Make sure only  paste in the instrument definitions, not any other bits of MML.

* Rather than just output the instrument definitions, I've formatted the .zms file so it can be played without further modification. It just plays a few notes using the first defined instrument.

* If you do further editing to .zms files, it's important that they are saved using Windows-style CRLF line endings. I had troubles on Mac with files mysteriously not working because of this. In Visual Studio Code, you can switch between them by clicking the LF or CRLF text located on the lower right of the window. The utility should save them correctly though.

* The easiest way I've found to play .zms files without too much fuss is use the <a href="https://t.co/ovnG4JLugE">ZMDRIVE</a> for Windows (which also works in Wine).

# OPM

OPM (aka MiOPMdrv) sound banks can be loaded into the <a href="http://picopicose.com/software.html">VOPM/VOPMex</a>
 VST plugin, which emulates the YM2151 (OPM) sound chip found in the x68000. 

### Other Notes:

* This saves an entire OPM bank of 128 instruments every time, even if only a few instruments are converted. The others will just be empty. This works best when importing them into VOPM.

* The "noise" switch in VOMP isn't implemented yet, as it is set with a seperate Z-MUSIC macro, not in the instrument definition.

# Similar projects
* <a href="https://nfggames.com/forum2/index.php?topic=5992.0">opm2mml by exodusmodules</a>
* <a href="https://github.com/vampirefrog/mdxtools">mdxtools by vampirefrog</a>
* <a href="https://git.lain.church/whut/hootvopm">hootvopm by whut</a>



 




