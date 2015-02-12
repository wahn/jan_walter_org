---
layout: post
title:  "Conference Room: First Results for Cycles"
date:   2015-02-12 16:36:08
categories: jekyll rendering cycles
---

We have seen the [Conference Room][conf-room] scene before (in other
posts). It was originally created for [Radiance][radiance]. The room
does (or at least did) exist in reality and was painstakingly measured
and re-created in a text editor ([vi][vi]). See __README__ file and
credits in the [repository][repo], where I keep my test scenes in
various formats. Here are the first results for [Cycles][cycles],
[Blender][blender]'s new __global illumination__ renderer.

<img src="/assets/conference_room_cycles_current.png" alt="First
camera perspective rendered by Cycles" width="650"
class="img-thumbnail"/>

<img src="/assets/conference_room_cycles_door1.png" alt="Second camera
perspective rendered by Cycles" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_cycles_door2y.png" alt="Third camera
perspective rendered by Cycles" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_cycles_shaft.png" alt="Forth camera
perspective rendered by Cycles" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_cycles_xY.png" alt="Fifth camera
perspective rendered by Cycles" width="650" class="img-thumbnail"/>

The Blender file(s) for the conference room scene can be found both in
the [repository][repo] as well as in the [download][download] section.

[conf-room]: https://www.janwalter.org/renderforum/index.php?topic=15.0
[radiance]:  http://radsite.lbl.gov/radiance
[vi]:        https://en.wikipedia.org/wiki/Vi
[repo]:      https://github.com/wahn/export_multi/tree/master/04_conference_room
[blender]:   http://www.blender.org
[cycles]:    http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Cycles
[download]:  https://www.janwalter.org/download
