---
layout: post
title: SOFT - The cover of thesis that never came to be (or too many chemicals here)
date: '2015-04-03T18:25:37+02:00'
tags: []
---
Some time ago a coworker asked me to design the cover for his PhD thesis. After he explained to me about that was the thing, I could more or less understand that dangerous organochlorine compounds entered from one side and the magical zeolite destroys them to generate harmless CO<sub>2</sub> and HCl spat from the other side.

I thought of composing a figure with all the chemicals that he had mentioned to me. And clearly to draw the zeolite would be a problem. By that time I was rendering all using [povray](http://www.povray.org/).

| ![](/imgs/inline_nm8nqrcCan1r9flmj_540.png)   |
|:--:|
|*The discarded cover*|


The procedure I followed to compose the above figure was:

1. I took a cif file of a zeolite from the [Crystallography Open Database](http://www.crystallography.net/) &nbsp;and expanded the structure up to the desired size with [Mercury](http://www.ccdc.cam.ac.uk/Solutions/CSDSystem/Pages/Mercury.aspx). Then I saved such crystal portion as a .xyz file type.  
2. The saved file was open using [G](http://gabedit.sourceforge.net/)[abedit](http://gabedit.sourceforge.net/) and there the other molecules are added from the library. &nbsp;It seems that carbonaceous based deposition happens within the zeolite pores, this is shown as plain huge on purpose carbon atoms.  
3. The full set was saved as xyz file type again (it is also possible as pdb filetype) and it was open in [P](http://www.pymol.org/)[y](http://www.pymol.org/)[MOL](http://www.pymol.org/). The zeolite is shown in “sticks” mode, the gas phase molecules as “balls and sticks” mode molecules and the carbon residue as spheres with increased diameter size. The image rendering was also made using PyMOL.  
4. The final figure is composed using [GIMP](http://www.gimp.org.es/) where a fancy brush of chalk arrows was used indicating the chemical reaction direction.  

Later my colleague told me that on the upper floor had not liked the cover. Too many chemical compounds in the figure. He ended up placing a photograph with a swarm of pipelines as the cover because in fact it was a Chemical Engineering PhD thesis. But I learned the versatility of PyMOL to compose and render images.

