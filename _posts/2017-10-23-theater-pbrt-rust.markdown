---
layout: post
title:  "Theater: Rust version of PBRT"
date:   2017-10-23 13:27:00
categories: jekyll rendering pbrt
---

The Candlestick Point State Recreation Area (SRA) Community Theater
was designed by __Mark Mack Architects__, and modelled by __Charles
Ehrlich__ and __Greg Ward__ (see [README][readme] file in the
repository). It was originally designed to be rendered
with [Radiance][radiance]. After importing the scene
into [Blender][blender] some patterns used in the original scene are
gone and I replaced them by materials with a single color.

## Pbrt

Here are the results for the [Rust version][rs_pbrt] of the
[Pbrt][pbrt] renderer:

<img src="/assets/theater_pbrt_rust_corner.png" alt="Pbrt rendering of one
of the corner seat perspectives." width="740" class="img-thumbnail"/>

<img src="/assets/theater_pbrt_rust_row3.png" alt="Pbrt rendering from the
perspective of a third row seat." width="740" class="img-thumbnail"/>

<img src="/assets/theater_pbrt_rust_stage.png" alt="Pbrt rendering
from the perspective of someone on stage." width="740"
class="img-thumbnail"/>

## Other Renderers

The scene can be found in the [download][download] section of my web
site. [Images][images] rendered by other software can be found in a
__repository__ on [GitHub][github].

[readme]:         https://github.com/wahn/export_multi/tree/master/06_theater
[radiance]:       http://radsite.lbl.gov/radiance
[blender]:        http://www.blender.org
[pbrt]:           http://www.pbrt.org
[rs_pbrt]:        https://github.com/wahn/rs_pbrt
[download]:       https://www.janwalter.org/download
[images]:         https://github.com/wahn/export_multi/tree/master/06_theater/img
[github]:         https://github.com
