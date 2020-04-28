---
layout: post
title:  "LuxCoreRender 2.3"
date:   2020-03-17 12:45:00
categories: jekyll rendering luxcore
---

# Scenes Shipping with LuxCoreRender

Here some images from LuxCoreRender's [example
scenes][example_scenes]:

<p class="text-center"><img
src="/assets/luxrender2_3_cannelle_et_fromage.png" alt="Cannelle &
Fromage scene from LuxCoreRender examples." width="740"
class="img-thumbnail"/></p>

```
RenderTime: 02:10.30
```

<p class="text-center"><img src="/assets/luxrender2_3_luxcore2_1_benchmark.png"
alt="LuxCore 2.1 Benchmark scene from LuxCoreRender examples."
width="740" class="img-thumbnail"/></p>

```
RenderTime: 01:10.39
```

<p class="text-center"><img src="/assets/luxrender2_3_dlsc.png"
alt="LuxCore 2.1 Benchmark scene from LuxCoreRender examples."
width="740" class="img-thumbnail"/></p>

```
RenderTime: 02:06.22
```

<p class="text-center"><img src="/assets/luxrender2_3_danish_mood.png"
alt="Danish Mood scene from LuxCoreRender examples."
width="740" class="img-thumbnail"/></p>

```
RenderTime: 02:40.36
```

<p class="text-center"><img src="/assets/luxrender2_3_hall_bench.png"
alt="Hall Bench scene from LuxCoreRender examples."  width="740"
class="img-thumbnail"/></p>

```
RenderTime: 01:18.72
```

<p class="text-center"><img
src="/assets/luxrender2_3_procedural_leaves.png" alt="Procedural
Leaves scene from LuxCoreRender examples."  width="740"
class="img-thumbnail"/></p>

```
RenderTime: 00:21.01
```

<p class="text-center"><img src="/assets/luxrender2_3_lux_ball.png"
alt="LuxBall scene from LuxCoreRender examples."  width="740"
class="img-thumbnail"/></p>

```
RenderTime: 00:41.61
```

<p class="text-center"><img
src="/assets/luxrender2_3_pointiness_examples.png" alt="Pointiness
Examples scene from LuxCoreRender examples."  width="740"
class="img-thumbnail"/></p>

```
RenderTime: 02:25.86
```

<p class="text-center"><img
src="/assets/luxrender2_3_rainbow_colors_and_prism.png" alt="Rainbow
Colors and Prism scene from LuxCoreRender examples."  width="740"
class="img-thumbnail"/></p>

The laser rays hit a lens that diffuses rays only vertically (due to
its shape), then these rays enter the prism and travel to the location
of the empty. By upscaling lens1 the rainbow appears shorter
vertically, or longer if downscaled. Luxcore with bidir+metropolis
enabled can handle very complex lighting correctly if enough time is
given.

<p class="text-center"><img
src="/assets/luxrender2_3_rainbow_colors_and_prism_blender.png"
alt="Rainbow Colors and Prism scene within Blender."  width="740"
class="img-thumbnail"/></p>

[example_scenes]: https://luxcorerender.org/example-scenes
