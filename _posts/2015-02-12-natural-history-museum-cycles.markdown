---
layout: post
title:  "Natural History Museum: First Results for Cycles"
date:   2015-02-18 17:13:08
categories: jekyll rendering cycles
---

We have seen the __Natural History Museum__ scene before (in other
posts). The [Blender][blender] file was downloaded from the
[3drender.com][3drender_com] web site and cleaned up a bit
(e.g. removing empty meshes). The same model was used in a [lighting
tutorial][lighting-tutorial] for [Arnold][arnold]. In the
[repository][repo] I keep my test scenes in various formats. Here is
the first result for [Cycles][cycles], [Blender][blender]'s new
__global illumination__ renderer:

<img src="/assets/natural_history_museum_cycles_01.png" alt="The
Natural History Museum rendered by Cycles" width="650"
class="img-thumbnail"/>

The Blender file(s) for the __Natural History Museum__ scene can be
found both in the [repository][repo] (one file) as well as in the
[download][download] section (currently two files, one for Cycles, the
other one using materials for the other Blender internal renderer).

[blender]:           http://www.blender.org
[3drender_com]:      http://www.3drender.com/challenges
[lighting-tutorial]: https://support.solidangle.com/display/AFMUG/Lighting+the+Natural+History+Museum
[arnold]:            https://www.solidangle.com/arnold
[repo]:              https://github.com/wahn/export_multi/tree/master/10_natural_history_museum
[cycles]:            http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Cycles
[download]:          https://www.janwalter.org/download
