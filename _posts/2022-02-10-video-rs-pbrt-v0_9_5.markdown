---
layout: post
title:  "[video] rs-pbrt v0.9.5 and Blender 3.0"
date:   2022-02-09 11:00:00
categories: jekyll video rendering pbrt
---

# Video

<div id='NdCEeZwXhhk' class="youtube"><a
href=https://youtu.be/NdCEeZwXhhk'><img
src='/assets/rs-pbrt_v0.9.5.png' height="740"
class="img-thumbnail"/></a></div>

So, I started working on a **library** (called
[blend_info][blend_info]) to deal with reading data from Blender‘s
binary `.blend` files. In the past I was able to render some **Blender
v2.79** files, now I abstracted the library to deal (in theory) with
all Blender versions (using the **DNA** information). I tested with
two scenes for Blender 2.79 and **Blender 3.0** (which should be quiet
different from the GUI perspective). With v2.79 I got away with a
**naming convention** and mis-using the **old material settings** (pre
[Cycles][cycles] renderer). Blender 3.0 forced me to get some material
settings from **shader nodes** and the library allows to **follow
pointers** from one data structure to the next. Prove of concept:
Both, the **Cornell Box**, as well as the **Radiance Conference
Room**, render with both Blender scene versions. Get the latest
Blender scenes [here][gitlab].

[blend_info]: https://sr.ht/~wahn/rs-blender
[cycles]:     https://www.cycles-renderer.org
[gitlab]:     https://gitlab.com/jdb-walter/rs-pbrt-test-scenes/-/tree/master/blend
