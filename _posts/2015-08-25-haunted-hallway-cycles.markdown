---
layout: post
title:  "Haunted Hallway"
date:   2015-08-25 11:20:00
categories: jekyll rendering cycles
---

One of the __Lighting Challenges__ on [3drender.com][3drender_com] was
to light a hallway in a way that it appears to be __haunted__. So I
looked at __Dan Wade's__ [original rendering][dan_wade] and the render
challenge [gallery][hauntedhallway] for inspiration while grabbing the
geometry from the web site. After setting up the scene for
[Blender][blender] and it's global illumination renderer
[Cycles][cycles] I decided to try out the __volume rendering__, a
feature available for Cycles since version [2.70][blender_2_70] of
Blender:

<p class="text-center"><img
src="/assets/hallway_cycles_volumetric.png" alt="Using Cycle's 'Volume
Scatter' for god rays." width="740" class="img-thumbnail"/></p>

Basically I liked the __lateral tilted camera__ (see
[Wikipedia][wikipedia] article about __tripods__) from some of the
examples because it's unbalanced view adds to the scariness of the
scene. Another element I liked was the usage of __green__ and __red__
light sources. Some examples added blood to the scene to emphasize the
danger which is associated with the color red. Finally I wanted to
experiment with [god rays][crepuscular] and volumetric lighting.

Here is an earlier test with a red light bulb, but using the sun and
sky simulation, which I tinted slightly green.

<p class="text-center"><img src="/assets/hallway_cycles.png"
alt="Using the sund and sky simulation instead." width="740"
class="img-thumbnail"/></p>

I wanted to get the lighting right first before continuing to work on
the textures and materials used. Only the wooden floor and ceiling use
textures so far, beside the little patch of the brick wall. I did
experiment with other renderers though and will write about that in
another post. The Blender scene(s) (with and without volumetric
lighting) can be found in the [download][download] section. The
__textures__ have to be downloaded separately. Over time you will find
the scene in other file formats for various renderers.

Credits
-------

* __Challenge #8: Haunted Hallway__
* Modeled by __Dan Wade__, concept by __Gary Tonge__.
* from [http://www.3drender.com/challenges][3drender_com]

[3drender_com]:   http://www.3drender.com/challenges
[dan_wade]:       http://forums.cgsociety.org/showthread.php?f=185&t=436232
[hauntedhallway]: http://www.3drender.com/challenges/hauntedhallway/index.htm
[blender]:        https://www.blender.org/
[cycles]:         https://www.blender.org/manual/render/cycles/index.html
[blender_2_70]:   http://wiki.blender.org/index.php/Dev:Ref/Release_Notes/2.70/Cycles
[wikipedia]:      https://en.wikipedia.org/wiki/Tripod_%28photography%29
[crepuscular]:    https://en.wikipedia.org/wiki/Crepuscular_rays
[download]:       https://www.janwalter.org/download
