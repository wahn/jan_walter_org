---
layout: post
title:  "Theater: Arnold and Light Groups"
date:   2015-02-05 13:11:47
categories: jekyll rendering arnold
---

The Candlestick Point State Recreation Area (SRA) Community Theater
was designed by __Mark Mack Architects__, and modelled by __Charles
Ehrlich__ and __Greg Ward__ (see [README][readme] file in the
repository).

After importing the scene into [Blender][blender] and exporting it in
[Arnold][arnold]'s scene description file format I hand edited the
resulting __.ass__ file, turning lights on and off, to render a set of
lights (often called __light groups__ in other renderers).

Direct lighting using only __step__ and __wall lights__ (approx. 16
minutes render time):

<img src="/assets/theater_arnold_step_and_wall_lights.png" alt="Arnold
direct lighting of the step and wall lights." width="600"
class="img-thumbnail"/>

Direct lighting using only __entryway lights__ (approx. 2 minutes
render time):

<img src="/assets/theater_arnold_entryway_lights.png" alt="Arnold
direct lighting of the two entryways." width="600"
class="img-thumbnail"/>

Direct lighting using only __main lights__ (approx. 4 1/2 minutes
render time):

<img src="/assets/theater_arnold_main_lights.png" alt="Arnold direct
lighting of the main lights." width="600" class="img-thumbnail"/>

Direct lighting using __all lights__ (approx. 23 1/2 minutes render
time):

<img src="/assets/theater_arnold_direct_lighting.png" alt="Arnold
direct lighting of all light sources." width="600"
class="img-thumbnail"/>

__Global illumination__ using all lights (approx. 2 hours 30 minutes
render time):

<img src="/assets/theater_arnold_global_illumination.png" alt="Arnold
with global illumination." width="600"
class="img-thumbnail"/>

[readme]:  https://github.com/wahn/export_multi/tree/master/06_theater
[blender]: http://www.blender.org
[arnold]:  http://www.solidangle.com