---
layout: post
title:  "Render Comparison on GitHub"
date:   2020-03-26 16:45:00
categories: jekyll github rendering
---

# GitHub Repository

Times are changing, so I started a new repository, called
[render_comparison][render_comparison] on **GitHub**.

## A Short History

Before the year 2000 my rendering research was purely a **hobby**, but
the main idea was to get hold of as many renderers I could and render
as many images as I was able to. There was no science in it, just joy
to learn about rendering and what was available at that time.

In 2000 I made a big decision. I left my well paid job and started to
work for [Ton Roosendaal][ton], or better for his company, **NaN**
(Not a Number), in the Netherlands. During that time I wrote a lot of
Python scripts to import and export data into/from
[Blender][blender]. This was also the first time I **worked from
home** (as many of you are currently forced to do). First home was in
**Berlin**, Germany, every 6 weeks I travelled to **Amsterdam** or
**Eindhoven**, then I moved my home to **London**, UK, until NaN went
bancrupt and I had to find a new job.

From that time on rendering research (or **research** in general) was
my **job** (or at least part of my job). I worked for [Moving Picture
Company][mpc], **Mill Film**, the former film department of [The
Mill][mill], both in **London**. Then I moved to Greater **Los
Angeles**, USA, and worked for [Digital Domain][dd] and [mental
images][mi] (later part of [Nvidia][nvidia]). I moved back to
**Berlin** (still working for Nvidia), to **London** (back to The
Mill), and finally back to **Berlin**.

In the meantime rendering moved away from [REYES][reyes] (e.g. early
RenderMan by **Pixar**), or pure [ray tracing][ray_tracing]
(e.g. mental ray by **mental images**), towards (uni-directional)
[path tracing][path_tracing] (e.g. **Arnold** by Solid Angle, now
Autodesk) and/or other **global illumination** techniques. If you want
to learn about those, the best advice I can give you is to read the
[PBRT][pbrt] **book** (since October 2018 the full text is available
for free in an [online edition][pbr-book]). The [C++ code][pbrt-v3]
and my own [Rust implementation][rs-pbrt] is there to learn from ...

## Back to the Repository

I went through several phases and some are still online, others are
lost in time and space. Lets forget about the time **before** global
illumination (it's still worth to study and have knowledge about
it). One approach was to start with [Radiance][radiance] (images taken
from my own [slides][slides]):

<p class="text-center"><img
src="/assets/blender_conf_2014_slides_radiance.png" alt="Slide from
Blender Conference in 2014" width="740" class="img-thumbnail"/></p>

And **somehow** go from there to other global illumination (GI)
renderers:

<p class="text-center"><img
src="/assets/blender_conf_2014_slides_other_gi_renderers.png"
alt="Slide from Blender Conference in 2014" width="740"
class="img-thumbnail"/></p>

Traces of this approach can be found here:

1. [Radiance vs. YouNameIt][radiancevsyounameit]
2. [export_multi][export_multi]
3. [rs-pbrt-test-scenes][rs-pbrt-test-scenes]
4. [download][download] section of this web page

Now, let's dig into the **new repository** by looking at one of the
simplest scenes to start with.

# The Cornell Box

<p class="text-center"><img
src="/assets/render_comparison_cornell_box_cycles.png" alt="Cornell
Box rendered by Cycles" width="740" class="img-thumbnail"/></p>

## Blender

### [Cycles][cycles] (free)

Just load the provided scene (`cornell_box/blend/cornell_box.blend`
from the [repository][render_comparison]) and press **F12**. The
screenshot below shows Blender version 2.79, but higher Blender
versions should work as well:

<p class="text-center"><img
src="/assets/cornell_box_blender_v279_cycles.png" alt="Windows 10
screenshot of the Blender v2.79 Cornell Box." width="740"
class="img-thumbnail"/></p>

### [appleseed][appleseed] (free)

After installing [blenderseed][blenderseed] and selecting `appleseed`
as your `Render Engine` you should be able to just press **F12** to
render:

<p class="text-center"><img
src="/assets/cornell_box_blender_v282_appleseed.png" alt="Windows 10
screenshot of Blender v2.82 and appleseed." width="740"
class="img-thumbnail"/></p>

### [Indigo Renderer][indigo] (commercial)

After installing [Blendigo][indigo-blender] and selecting `Indigo`
as your `Render Engine` you should be able to just press **F12** to
render:

<p class="text-center"><img
src="/assets/cornell_box_blender_v282_indigo.png" alt="Windows 10
screenshot of Blender v2.82 and Indigo Renderer." width="740"
class="img-thumbnail"/></p>

### [LuxCoreRender][luxcorerender] (free)

After installing [BlendLuxCore][blendluxcore] and selecting `LuxCore`
as your `Render Engine` you should be able to just press **F12** to
render:

<p class="text-center"><img
src="/assets/cornell_box_blender_v282_luxcore.png" alt="Windows 10
screenshot of Blender v2.82 and LuxCoreRender." width="740"
class="img-thumbnail"/></p>

### [OctaneRender][octane-render] (commercial)

There is a OctaneRender for Blender plugin. Windows version only. The
download links provided (on their user forum) are for **Enterprise
License Holders**. For me that wasn't really a plug-in (or addon as
they call it in Blender world), but rather a specially compiled
Blender v2.81 with `Octane` (Render Engine) support (press **F12**).

<p class="text-center"><img
src="/assets/cornell_box_blender_v282_octane.png" alt="Windows 10
screenshot of Blender v2.81 and OctaneRender." width="740"
class="img-thumbnail"/></p>

## Houdini

Even though there was a [RenderManForBlender][rendermanforblender]
addon, it looks like it's kind of abandoned, therefore I went down a
different route and you need a [Houdini Indie][houdini-indie] license
for rendering e.g. with Pixars RenderMan (PRMan).

### [RenderMan][renderman] (commercial)

There is a **non-commercial** RenderMan version which works with
Houdini (Indie):

<p class="text-center"><img
src="/assets/cornell_box_houdini_prman.png" alt="Windows 10 screenshot
of Houdini 18 with PRMan 23." width="740" class="img-thumbnail"/></p>

## Standalone

### [Maxwell][maxwell] (commercial)

For Maxwell you need a commercial license (at least that's what I
got). On the left you see **Maxwell Studio**, on the right **Maxwell
Render**, both using two GeForce RTX 2080 Ti cards for rendering.

<p class="text-center"><img
src="/assets/cornell_box_maxwell_studio.png" alt="Windows 10
screenshot of Maxwell Studio and Maxwell Render." width="740"
class="img-thumbnail"/></p>

### [Guerilla Render][guerillarender] (commercial)

Even though the Guerilla Render is a commercial product you can get an
**unlimited** license for free to discover its features.

<p class="text-center"><img src="/assets/cornell_box_guerilla.png"
alt="Windows 10 screenshot of Guerilla Render." width="740"
class="img-thumbnail"/></p>

### [PBRT][pbrt] (free)

For PBRT you have to build the renderer yourself by **compiling** it
from sources (either follow the instructions for the [C++
code][pbrt-v3] or follow the instructions for Rust on the [getting
started][getting-started] with `rs-pbrt` page). After that you can
render like this:

```shell
$ cd cornell_box/pbrt
# C++
$ ~/builds/pbrt/release/pbrt cornell_box.pbrt
pbrt version 3 (built Apr  1 2019 at 17:45:44) [Detected 28 cores]
Copyright (c)1998-2018 Matt Pharr, Greg Humphreys, and Wenzel Jakob.
The source code to pbrt (but *not* the book contents) is covered by the BSD License.
See the file LICENSE.txt for the conditions of the license.
...
$ display -gamma 2.2 pbrt.exr
# Rust
$ ~/git/github/rs_pbrt/target/release/rs_pbrt cornell_box.pbrt
pbrt version 0.8.1 [Detected 28 cores]
Copyright (c) 2016-2020 Jan Douglas Bert Walter.
Rust code based on C++ code by Matt Pharr, Greg Humphreys, and Wenzel Jakob.
...
$ display pbrt.png
```

A little known fact is that I do provide two executables which allow
you to render with `rs-pbrt` and **Arnold** (see [Blender to
Arnold][blender-to-arnold] blog):

```shell
# rs-pbrt
# -s, --samples <samples> pixel samples [default: 1]
# -i, --integrator <integrator>
#     ao, directlighting, whitted, path, bdpt, mlt, sppm, volpath
$ ./target/release/examples/parse_blend_file -s 16 -i path cornell_box.blend
$ display pbrt.png
```

The `--integrator <integrator>` option allows to render with:

1. `ao` - Ambient Occlusion (AO)
2. `directlighting` - Direct Lighting
3. `whitted` - Whitted’s Ray-Tracing
4. `path` - (Unidirectional) Path Tracing
5. `bdpt` - Bidirectional Path Tracing (BDPT)
6. `mlt` - Metropolis Light Transport (MLT)
7. `sppm` - Stochastic Progressive Photon Mapping (SPPM)
8. `volpath` - Path Tracing (Participating Media)

The `parse_blend_file` should work on every non-renderer-specific
`.blend` file I provide in the [repository][render_comparison], but
**not** on **every** `.blend` file in general. The **Arnold** version
is not ready for public consumption yet and there is a
[Arnold-For-Blender][arnold-for-blender] on the horizon.

[render_comparison]:   https://github.com/wahn/render_comparison
[ton]:                 https://en.wikipedia.org/wiki/Ton_Roosendaal
[blender]:             https://www.blender.org
[mpc]:                 https://www.moving-picture.com
[mill]:                https://en.wikipedia.org/wiki/The_Mill_(company)
[dd]:                  https://www.digitaldomain.com
[mi]:                  https://en.wikipedia.org/wiki/Mental_Images
[nvidia]:              https://www.nvidia.com
[reyes]:               https://en.wikipedia.org/wiki/Reyes_rendering
[ray_tracing]:         https://en.wikipedia.org/wiki/Ray_tracing_(graphics)
[path_tracing]:        https://en.wikipedia.org/wiki/Path_tracing
[pbr-book]:            http://www.pbr-book.org
[pbrt-v3]:             https://github.com/mmp/pbrt-v3
[rs-pbrt]:             https://www.rs-pbrt.org/about
[getting-started]:     https://www.rs-pbrt.org/blog/getting-started
[radiance]:            https://floyd.lbl.gov/radiance
[slides]:              https://www.janwalter.org/Download/PDF/blender_conf_2014_slides.pdf
[radiancevsyounameit]: https://www.janwalter.org/RadianceVsYouNameIt/index.html
[export_multi]:        https://github.com/wahn/export_multi
[rs-pbrt-test-scenes]: https://gitlab.com/jdb-walter/rs-pbrt-test-scenes
[download]:            https://www.janwalter.org/download
[appleseed]:           https://appleseedhq.net
[blenderseed]:         https://github.com/appleseedhq/blenderseed/releases
[cycles]:              https://www.cycles-renderer.org
[guerillarender]:      http://guerillarender.com
[indigo]:              https://www.indigorenderer.com
[indigo-blender]:      https://www.indigorenderer.com/documentation/manual/indigo-blender
[luxcorerender]:       https://luxcorerender.org
[blendluxcore]:         https://github.com/LuxCoreRender/BlendLuxCore
[maxwell]:             https://www.maxwellrender.com
[octane-render]:       https://home.otoy.com/render/octane-render
[pbrt]:                http://www.pbrt.org
[renderman]:           https://rmanwiki.pixar.com
[rendermanforblender]: https://github.com/prman-pixar/RenderManForBlender
[houdini-indie]:       https://www.sidefx.com/products/houdini/houdini-indie
[blender-to-arnold]:   https://www.janwalter.org/jekyll/blender/rs_pbrt/arnold/2020/01/08/blender-to-arnold.html
[arnold-for-blender]:  https://github.com/tyler-furby/Arnold-For-Blender
