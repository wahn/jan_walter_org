---
layout: post
title:  "Simple Room: Cycles"
date:   2015-01-30 16:52:30
categories: jekyll rendering cycles
---

The simple room scene is described in the __Rendering with Radiance__
[book][book] (chapter 1.3). In the [repository][repository] you will
find two versions of the scene in [Blender][blender]'s file
format. Once you loaded the __simple\_room\_cycles.blend__ into
Blender you are ready to render the scene with [Cycles][cycles] from
the first camera (__cam1__) perspective:

<p class="text-center"><img src="/assets/simple_room_cycles_cam1.png"
alt="Cycles: Simple Room - camera 1" width="300"
class="img-thumbnail"/> <img src="/assets/simple_room_cycles_cam2.png"
alt="Cycles: Simple Room - camera 2" width="300"
class="img-thumbnail"/></p>

<p class="text-center"><img src="/assets/simple_room_cycles_cam3.png"
alt="Cycles: Simple Room - camera 3" width="300"
class="img-thumbnail"/> <img src="/assets/simple_room_cycles_cam4.png"
alt="Cycles: Simple Room - camera 4" width="300"
class="img-thumbnail"/></p>

To render the other three available camera perspectives you have to

1. select a frame number (1-4)

2. this results in updating keyframed values (e.g. sun & sky or light
emitter intensities)

3. before you hit the render button don't forget to select the render
camera for this particular frame (cam1-4)

4. hit the render button (or press __F12__)

<p class="text-center"><img
src="/assets/blender_cycles_simple_room2.png" alt="How to render the
other camera perspectives." width="600" class="img-thumbnail"/></p>

The Blender file(s) for the simple room scene can be found both in the
[repository][repository] as well as in the [download][download]
section.

[book]:       http://radsite.lbl.gov/radiance/book/index.html
[repository]: https://github.com/wahn/export_multi/tree/master/01_simple_room/blend
[blender]:    http://www.blender.org
[cycles]:     http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Cycles
[download]:   https://www.janwalter.org/download
