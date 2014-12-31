---
layout: post
title:  "Bedroom: Cycles (sun radius)"
date:   2014-11-01 16:04:24
categories: jekyll rendering cycles
---

__Brecht Van Lommel__, the original author of [Cycles][cycles], told
me that I should set the __sun size__ to __zero__ (in the
[Blender][blender] scene), so that Cycles will get through the windows
correctly. That was the question I had in a previous [post][post], and
indeed it fixed the problem:

<img src="/assets/bedroom_cycles02.png" alt="Bedroom rendered by
Cycles." width="600" class="img-thumbnail"/>

Here, for comparison the rendering I got when the radius was __not__
set to zero:

<img src="/assets/bedroom_cycles01.png" alt="Bedroom rendered by
Cycles." width="600" class="img-thumbnail"/>

And the resulting [imf_diff][imf_diff] output (in this case
__without__ the __false colors__):

<img src="/assets/bedroom_imf_diff_arnold_cycles02.jpg"
alt="Difference between the two Cycles renders." width="600"
class="img-thumbnail"/>

The updated Blender file can be downloaded from the
[repository][repo].

Credits
-------

* __Challenge #21: The Bedroom__
* Modeled by __David Vacek__, design by __David Tousek__.
* from [http://www.3drender.com/challenges][3drender_com]
* some textures from [http://www.cgtextures.com][cgtextures_com]

[cycles]:         http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Cycles
[blender]:        http://www.blender.org
[post]:           https://www.janwalter.org/jekyll/rendering/arnold/2014/10/29/bedroom-arnold-luxrender-cycles-lagoa.html
[imf_diff]:       http://docs.autodesk.com/MENTALRAY/2012/CHS/mental%20ray%203.9%20Help/files/manual/node251.html
[repo]:           https://github.com/wahn/export_multi/tree/master/11_bedroom/blend
[3drender_com]:   http://www.3drender.com/challenges
[cgtextures_com]: http://www.cgtextures.com
