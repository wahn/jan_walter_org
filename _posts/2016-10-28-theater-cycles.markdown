---
layout: post
title:  "Theater: Cycles"
date:   2016-10-28 13:51:00
categories: jekyll rendering cycles
---

The Candlestick Point State Recreation Area (SRA) Community Theater
was designed by __Mark Mack Architects__, and modelled by __Charles
Ehrlich__ and __Greg Ward__ (see [README][readme] file in the
repository). It was originally designed to be rendered
with [Radiance][radiance]. After importing the scene
into [Blender][blender] some patterns used in the original scene are
gone and I replaced them by materials with a single color.

## Cycles

Here are the results for [Cycles][cycles], __Blender's__ internal
global illumination renderer:

<img src="/assets/theater_cycles_corner.png" alt="Cycles rendering of
one of the corner seat perspectives." width="740"
class="img-thumbnail"/>

<img src="/assets/theater_cycles_row3.png" alt="Cycles rendering from
the perspective of a third row seat." width="740"
class="img-thumbnail"/>

<img src="/assets/theater_cycles_stage.png" alt="Cycles rendering from
the perspective of someone on stage." width="740"
class="img-thumbnail"/>

## Other Renderers

The scene can be found in the [download][download] section of my web
site. [Images][images] rendered by other software can be found in a
__repository__ on [GitHub][github].

[readme]:         https://github.com/wahn/export_multi/tree/master/06_theater
[radiance]:       http://radsite.lbl.gov/radiance
[blender]:        http://www.blender.org
[multi_exporter]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[plugin]:         https://bitbucket.org/wahn/blender-add-ons/wiki/io_scene_multi
[cycles]:         https://www.blender.org/manual/render/cycles/index.html
[download]:       https://www.janwalter.org/download
[images]:         https://github.com/wahn/export_multi/tree/master/06_theater/img
[github]:         https://github.com
