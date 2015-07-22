---
layout: post
title:  "OSL Patterns for Three Renderers"
date:   2015-06-28 15:40:00
categories: jekyll rendering osl
---

[Blender][blender] could soon be the main tool for [OSL][osl] shader
development. The application supports OSL for quiet some time for it's
internal global illumination renderer [Cycles][cycles]. But it's
getting more and more interesting since [Pixar][Pixar] released a
[non-commercial][nc-prman] [RenderMan][renderman] version, which does
not only implement uni-directional and bi-directional [path
tracing][path-tracing] but also supports OSL for writing shaders or
patterns (using e.g. [PxrOSL][pxrosl]). At the same time another
popular, production ready, and widely used renderer, [Arnold][arnold],
will soon support OSL. Arnold is already using OSL in a special
version being used at [Sony Picture Imageworks][spi], but the publicly
available version will follow soon. Here some early tests:

<p class="text-center"><img src="/assets/osl_rib_ass_cycles_01.png"
alt="Blender being used for OSL shader development." width="740"
class="img-thumbnail"/></p>

Above you see a screenshot of Blender, with a piece of an __OSL
shader__ being visible in the lower part. Not only can the shader
source code be changed and automatically be compiled, but the result
can be watched in __real-time__ in a Cycles render __preview__
directly within the 3D viewport. That's extremely useful during shader
or pattern development. You obviously can also __change__ the values
for all the shader __parameters__ and see the effect in
real-time. Once you are happy with the result you can export to
__PRMan__ (Pixar's RenderMan implementation) an/or __Arnold__ and
render the resulting scene description files (__.rib__ for PRMan, and
__.ass__ for Arnold). The results will differ slightly for global
illumination, because of different rendering algorithms being used and
non-compatible base shaders, but at least the OSL based patterns will
look exactly the same.

[blender]:      https://www.blender.org/
[osl]:          http://opensource.imageworks.com/?p=osl
[cycles]:       https://www.blender.org/manual/render/cycles/index.html
[Pixar]:        http://www.pixar.com
[nc-prman]:     http://renderman.pixar.com/view/non-commercial-renderman
[renderman]:    http://renderman.pixar.com
[path-tracing]: https://en.wikipedia.org/wiki/Path_tracing
[pxrosl]:       https://renderman.pixar.com/resources/current/RenderMan/PxrOSL.html
[arnold]:       https://www.solidangle.com/
[spi]:          http://www.imageworks.com/
