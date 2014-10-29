---
layout: post
title:  "Bedroom: Arnold, Luxrender, Cycles, and Lagoa"
date:   2014-10-29 11:42:10
categories: jekyll rendering arnold
---

The bedroom __geometry__ is taken from one of the [lighting
challenges][3drender_com], most of the __textures__ I downloaded from
[http://www.cgtextures.com][cgtextures_com] (the sources for the other
textures can be found in a [README][repo] file). So far this scene
uses more textures than any other scene I have published yet, it
contains enough objects to be believable. Only __two light sources__
are illuminating the scene, the __sun__ light from outside the room,
entering through two windows with blinds (only one windows is visible
from the cmaera perspective), and a __light emitting bulb__ inside a lamp
shade.

Arnold
------

<img src="/assets/bedroom_arnold01.png" alt="Bedroom rendered by
Arnold." width="600" class="img-thumbnail"/>

There is not much I do not like about the Arnold version of this
scene. I think it's probably __too bright__ though, the Luxrender
version does separate the two light sources a bit better. The
__dumbbell__ is probably a bit too shiny and I would prefer a more
__rusty version__. The __guitar__ I would like to have a similar
__red__ to the Coca Cola labels, but otherwise the choice of textures
and materials works for me.

Luxrender
---------

<img src="/assets/bedroom_luxrender01.png" alt="Bedroom rendered by
Luxrender." width="600" class="img-thumbnail"/>

I granted the Luxrender version more rendering hours than the others,
simply because I was away for three days in Amsterdam for the Blender
conference and kept my desktop computer busy, rendering this scene. I
think the __weight between the two light sources__ works best for the
images rendered so far. The __dumbbell__ is similar to the Arnold
version but should be more rusty. Also the __red__ of the __guitar__
should be changed in the common Blender scene (before exporting). I
don't like the __radiator__, it's too shiny. Either my Blender
material should be changed or the exporter has bad (Luxrender)
heuristics for those kind of materials. Needs a bit of
investigation. The __aliasing__ artefacts __around the blends__ (and
the light bulb) are a known issue with Luxrender, but maybe there is a
workaround, I'm not aware of (yet). What happened to the texture of
the __Clockwork Orange__ poster? Somehow the contrast is gone. Why
does it not happen for the other textures? Is it the way it receives
light?

Cycles
------

<img src="/assets/bedroom_cycles01.png" alt="Bedroom rendered by
Cycles." width="600" class="img-thumbnail"/>

For Cycles the __most obvious__ artefact is that __the sun doesn't
seem to get through__ the windows correctly. Either I made a mistake
with related materials or the workaround would be to place area lights
outside and in front of the windows to force the effect. I will
__discuss__ with the Cycles developers. It would be a shame if we need
a workaround just for that one renderer.

Lagoa
-----

<img src="/assets/bedroom_lagoa02.png" alt="Bedroom rendered by
Lagoa." width="600" class="img-thumbnail"/>

I kind of like the Lagoa rendering as well, even though I think it's
too dark, but it separates the light bulb effect nicely (Lagoa has no
light emitters, so I replaced it by a __sphere light__ with a radius
similar to the original light bulb geometry). The __materials__ were
__not automatically__ generated and I used whatever was there in
Lagoa's user interface and modified it to something similar to the
Blender scene, but I thinks the rusty metal for the __dumbbell__ and
other more advanced materials work nicely. I also didn't succeed in
matching the direction of the sun 100% using their __daylight
system__, but at least the sun rays touch the blanket and bedsheet in
a similar angle.

Using imf_diff
--------------

Even though it's obvious that the sun doesn't get through with Cycles,
here is another example on how to use [imf_diff][imf_diff] from
[mental ray][mental-ray] to compare two images (using __false colors__
to emphasize the differences):

<img src="/assets/bedroom_imf_diff_arnold_cycles01.jpg"
alt="Difference between the Arnold and Cycles version." width="600"
class="img-thumbnail"/>

Credits
-------

* __Challenge #21: The Bedroom__
* Modeled by __David Vacek__, design by __David Tousek__.
* from [http://www.3drender.com/challenges][3drender_com]
* some textures from [http://www.cgtextures.com][cgtextures_com]

[repo]:           https://github.com/wahn/export_multi/blob/master/11_bedroom/textures/README
[3drender_com]:   http://www.3drender.com/challenges
[lagoa]:          http://home.lagoa.com/
[cgtextures_com]: http://www.cgtextures.com
[imf_diff]:       http://docs.autodesk.com/MENTALRAY/2012/CHS/mental%20ray%203.9%20Help/files/manual/node251.html
[mental-ray]:     http://www.nvidia-arc.com/mentalray.html
