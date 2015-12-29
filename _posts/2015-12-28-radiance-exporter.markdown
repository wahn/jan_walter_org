---
layout: post
title:  "Some Converted Scenes for Radiance"
date:   2015-12-28 12:40:00
categories: jekyll rendering radiance
---

Recently I worked a bit on the exporter for [Radiance][radiance]
files. Traditionally my first test scenes came from the Radiance
[book][book]. After that I was digging a bit through some
[models][models] from the Radiance [gallery][gallery] site.

Starting from the [Salle de bain][salle-de-bain] scene I was looking
for other sources to get some test scenes and found some on [Blend
Swap][blend-swap]:

<img src="/assets/salle_de_bain_cycles.png" alt="Cycles rendering of a
modern bathroom (from Blend Swap)." width="960"
class="img-thumbnail"/>

The current state of the Radiance exporter can't export textures
(yet), but we can export the scene for Radiance and render it:

<img src="/assets/salle_de_bain_radiance_no_textures.png"
alt="Radiance rendering of a modern bathroom (from Blend Swap)."
width="960" class="img-thumbnail"/>

Other examples I recently converted to Radiance originally come from
__lighting challenges__ on the [3drender.com][3drender_com] web
site. Here some examples:

<img src="/assets/bedroom_radiance_no_textures.png" alt="Bedroom
rendered by Radiance."  width="960" class="img-thumbnail"/>

<img src="/assets/neon_and_chrome_radiance_no_textures.png" alt="Neon
and Chrome scene rendered by Radiance."  width="960"
class="img-thumbnail"/>

<img src="/assets/hallway_radiance_no_textures.png" alt="Hallway scene
rendered by Radiance."  width="960" class="img-thumbnail"/>

The last example I want to show here (again without textures) is from
the [Blender][blender]'s web site [demo files][demo-files]:

<img src="/assets/classroom_radiance_no_textures.png" alt="Classroom
scene rendered by Radiance."  width="960" class="img-thumbnail"/>

The original scene rendered by [Cycles][cycles] using some compositing
looks like this:

<p class="text-center"><img src="/assets/classroom_cycles.png"
alt="Classroom rendered by Cycles." width="740"
class="img-thumbnail"/></p>

You can download the Radiance files (and the same scene for other
renderers) [here][download].

[radiance]:      http://radsite.lbl.gov/radiance
[book]:          http://radsite.lbl.gov/radiance/book/index.html
[models]:        http://radsite.lbl.gov/radiance/pub/models/index.html
[gallery]:       http://radsite.lbl.gov/radiance/frameg.html
[salle-de-bain]: http://www.blendswap.com/blends/view/73937
[blend-swap]:    http://www.blendswap.com
[3drender_com]:  http://www.3drender.com/challenges
[blender]:       https://www.blender.org
[demo-files]:    https://www.blender.org/download/demo-files
[cycles]:        https://www.blender.org/manual/render/cycles/index.html
[download]:      https://www.janwalter.org/download
