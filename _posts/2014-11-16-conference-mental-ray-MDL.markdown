---
layout: post
title:  "Conference Room: First Results for Mental Ray using MDL"
date:   2014-11-16 16:06:44
categories: jekyll rendering mental_ray
---

We have seen the [Conference Room][conf-room] scene before (in other
posts). It was originally created for [Radiance][radiance]. The room
does (or at least did) exist in reality and was painstakingly measured
and re-created in a text editor ([vi][vi]). See __README__ file and
credits in the [repository][repo], where I keep my test scenes in
various formats. Here are the first results for [mental
ray][mental-ray] using their new [MDL][mdl] materials (see NVIDIA's
technical documentation about their __Material Definition Language__).

<img src="/assets/conference_room_mental_ray_current_01.png"
alt="First camera perspective rendered by mental ray" width="650"
class="img-thumbnail"/>

<img src="/assets/conference_room_mental_ray_door1_01.png"
alt="Second camera perspective rendered by mental ray" width="650"
class="img-thumbnail"/>

Both images were rendered in [OpenEXR][openexr] format and converted
to __Portable Network Graphics__ ([PNG][png]). After that I adjusted
the brightness by raising the exposure via OSX' __Preview__ program.

[conf-room]:          https://www.janwalter.org/renderforum/index.php?topic=15.0
[radiance]:           http://radsite.lbl.gov/radiance
[vi]:                 https://en.wikipedia.org/wiki/Vi
[repo]:               https://github.com/wahn/export_multi/tree/master/04_conference_room
[mental-ray]:         http://www.nvidia-arc.com/mentalray.html
[mdl]:                http://www.nvidia-arc.com/products/iray/technical-documentation.html
[openexr]:            http://www.openexr.org
[png]:                http://en.wikipedia.org/wiki/Portable_Network_Graphics
