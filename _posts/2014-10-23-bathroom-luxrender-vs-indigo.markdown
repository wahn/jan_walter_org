---
layout: post
title:  "Bathroom: Luxrender vs. Indigo"
date:   2014-10-23 12:10:12
categories: jekyll rendering luxrender
---

The __Creston Bathroom Model__ was created by __Greg Ward__, the
original author of [Radiance][radiance] as part of a bathroom
renovation project. Different materials and paints were used to give
the bathroom various appearances. In this Blog post we will talk about
the results I get with [Indigo][indigo] and [Luxrender][luxrender]
(using just one set of materials).

<p class="text-center"><img src="/assets/bathroom_luxrender_mir.png"
alt="Shower door visible in mirror" width="320"
class="img-thumbnail"/> <img src="/assets/bathroom_indigo_mir.png"
alt="Shower door visible in mirror" width="320"
class="img-thumbnail"/></p>

<p class="text-center"><img src="/assets/bathroom_luxrender_shwin.png"
alt="Shower and window" width="320"
class="img-thumbnail"/> <img src="/assets/bathroom_indigo_shwin.png"
alt="Shower and window" width="320"
class="img-thumbnail"/></p>

<p class="text-center"><img src="/assets/bathroom_luxrender_sink.png"
alt="Sink, toilet, and floor" width="320"
class="img-thumbnail"/> <img src="/assets/bathroom_indigo_sink.png"
alt="Sink, toilet, and floor" width="320"
class="img-thumbnail"/></p>

<p class="text-center"><img src="/assets/bathroom_luxrender_stall.png"
alt="Inside the shower stall" width="320"
class="img-thumbnail"/> <img src="/assets/bathroom_indigo_stall.png"
alt="Inside the shower stall" width="320"
class="img-thumbnail"/></p>

<p class="text-center"><img src="/assets/bathroom_luxrender_van.png"
alt="The bathroom vanity and basin" width="320"
class="img-thumbnail"/> <img src="/assets/bathroom_indigo_van.png"
alt="The bathroom vanity and basin" width="320"
class="img-thumbnail"/></p>

As you can see the camera perspective doesn't match 100% (I created an
[issue][issue] to fix it in the exporter) and __Indigo__ is forcing
you to use __depth-of-field__ ([DOF][dof]), but otherwise the lighting
and the tonemapping (in both cases linear) are pretty close to each
other.

What's also __missing__ are non-texture-based, __procedural
patterns__, as you can see here (for the wood, the marble, and the
floor tiles) in a [Radiance][radiance] and [Arnold][arnold] rendering:

<p class="text-center"><img src="/assets/bathroom_radiance_sink.png"
alt="procedural patterns, for the wood, the marble, and the floor
tiles" width="320" class="img-thumbnail"/> <img
src="/assets/bathroom_arnold_sink.png" alt="procedural patterns, for
the wood, the marble, and the floor tiles" width="320"
class="img-thumbnail"/></p>

As usual the scene files are available in a [repository][repository].

[radiance]:   https://en.wikipedia.org/wiki/Radiance_%28software%29
[luxrender]:  http://www.luxrender.net/en_GB/index
[indigo]:     http://www.indigorenderer.com
[issue]:      https://bitbucket.org/wahn/blender-add-ons/issue/46/export_multi-fix-camera-settings-for-y-res
[dof]:        https://en.wikipedia.org/wiki/Depth_of_field
[arnold]:     https://www.solidangle.com/arnold
[repository]: https://github.com/wahn/export_multi/tree/master/05_bathroom
