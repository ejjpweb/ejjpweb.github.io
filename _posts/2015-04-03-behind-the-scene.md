---
layout: post
title: SOFT - Behind the scene
date: '2015-04-03T18:50:18+02:00'
tags:
- science
- battery
- nanotubes
- aoi
---

| ![](/imgs/inline_nm8pgsW0Hu1r9flmj_540.png)   |
|:--:|
|*nanotube/TiO<sub>2</sub> nanocomposite battery anodes*|



This graphical abstract for the TOC of this [article](http://www.sciencedirect.com/science/article/pii/S0378775314009203) was built up using almost exclusively the program [Art of Illusion](http://www.artofillusion.org/)(AOI). Art of Illusion is a free, open source 3D modeling and rendering studio which is very suitable for this kind of artwork and also it is easy to learn. 

The main idea for the picture including the nanocomposite required the inclusion of a multi-walled carbon nanotube. A first approach could be to import the atomic coordinates of the nanotube from a third party program and try to render the composite. However, there was a  more complication  because such nanotube should be cut along the axial axis to show the electron paths. Also, it was not easy to find the .xyz atomic coordinates of such multiwalled nanotube. Then, we decided another approach instead of crude atom-by-atom insertion of the nanotube inside the scene. As you can see in the detailed figure (bottom panel), the nanotube is really a cylinder sculpted using the boolean tool of AOI where a picture of overlapping layers of graphene has been mapped within its surface. The trick is that choosing the adequated point of view of the AOI camera, this detail become unnoticed behind of the scene.

