---
layout: post
title:  "Theater: Luxrender"
date:   2016-10-17 11:11:00
categories: jekyll rendering luxrender
---

The Candlestick Point State Recreation Area (SRA) Community Theater
was designed by __Mark Mack Architects__, and modelled by __Charles
Ehrlich__ and __Greg Ward__ (see [README][readme] file in the
repository).

## Radiance

The original [Radiance][radiance] renderings show some patterns on the
floor and walls. The also simulate wavy curtains via some surface
normal perturbation:

<img src="/assets/theater_radiance_corner.png" alt="Radiance
rendering of one of the corner seat perspectives." width="740"
class="img-thumbnail"/>

<img src="/assets/theater_radiance_row3.png" alt="Radiance rendering
from the perspective of a third row seat." width="740"
class="img-thumbnail"/>

<img src="/assets/theater_radiance_stage.png" alt="Radiance rendering
from the perspective of someone on stage." width="740"
class="img-thumbnail"/>

After importing the scene into [Blender][blender] those patterns are
gone and I replaced them by materials with a single color. Exporting
the scene again (via my own [plugin][plugin]) in various renderer
specific file formats allows me to render the same scene from various
camera perspectives and compare the results.

## Luxrender

I recently re-rendered the scene with [Luxrender][luxrender]:

<img src="/assets/theater_luxrender_corner.png" alt="Luxrender
rendering of one of the corner seat perspectives." width="740"
class="img-thumbnail"/>

<img src="/assets/theater_luxrender_row3.png" alt="Luxrender rendering
from the perspective of a third row seat." width="740"
class="img-thumbnail"/>

<img src="/assets/theater_luxrender_stage.png" alt="Luxrender rendering
from the perspective of someone on stage." width="740"
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
[luxrender]:      http://www.luxrender.net/en_GB/index
[download]:       https://www.janwalter.org/download
[images]:         https://github.com/wahn/export_multi/tree/master/06_theater/img
[github]:         https://github.com
