---
layout: post
title:  "Baking with Cycles"
date:   2015-08-03 19:08:00
categories: jekyll rendering cycles
---

Imagine you want to create a short movie without any animation (except
the moving camera). I believe you call it a fly-through. One of the
challenges for a __global illumination__ renderer is to bring the
rendering time for __interiors__ down to a realistic value. Normally
the exterior can be rendered fast during daytime because the only
light source (or at least the main light source) is the __sun__ and
the __sky__.

So, let's create a simple fly-through scene, a house with 5 windows
and a door, the sun and sky simulation, and an animated camera and
it's target (to define where to look at):

<p class="text-center"><img src="/assets/cycles_baking_camera.png"
alt="Camera movement in Blender's viewport." width="740"
class="img-thumbnail"/></p>

You can see in the screenshot above that the camera is moved from
outside into the house within the first 5 seconds (120 frames of a 24
fps animation). The remaining 5 seconds it is moving inside the house
from the door to the south-east corner. To control where the camera is
looking at we use a __target__ object which is animated in a circular
way, so that the camera looks at the door while outside and circles
around in the room while moving inside the house. Because the movement
of the camera and the rotation around the z-axis (z is up in
[Blender][blender]) happens relatively fast, we should consider using
camera __motion blur__. So here is a still frame (215 out of 240
frames) where the camera is pretty much at the end of the rotation and
is aiming for the door and one of the windows:

<p class="text-center"><img
src="/assets/blender_v2.75a_100_samples.png" alt="Cycles with 100
samples." width="740" class="img-thumbnail"/></p>

You can clearly see the motion blur around the edges of the door
opening and the window. What we want to focus at are the __dark
areas__ (where we get light only from bouncing sun light or from the
sky) inside the room, it's __noise level__ and the __rendering
times__. In this case we instructed [Cycles][cycles] (Blender's
internal global illumination renderer) to use __one hundred samples__
per pixel (per frame). The rendering time for this frame is around
__35 seconds__.

To achieve a better quality (a lower noise level) we have to trade for
a __higher rendering time__:

<p class="text-center"><img
src="/assets/blender_v2.75a_1000_samples.png" alt="Cycles with 1000
samples." width="740" class="img-thumbnail"/></p>

One __thousand samples__ per pixel (per frame) gives us a rendering
time of about __5 minutes__ and __49 seconds__.

So, let's talk about the effect of __baking__ first, before I describe
what baking actually means.

Here is a __one hundred samples__ per pixel (per frame) rendering of
the same frame using baked textures to accelerate rendering:

<p class="text-center"><img
src="/assets/blender_v2.75a_100_samples_baked.png" alt="Cycles with
100 samples using baked textures." width="740"
class="img-thumbnail"/></p>

The rendering time goes down from 35 seconds to about __10 seconds__.

Even if we use one __thousand samples__ per pixel (per frame) the
rendering time goes down (from 5 minutes and 49 seconds to
approximately __one minute__ and __34 seconds__).

<p class="text-center"><img
src="/assets/blender_v2.75a_1000_samples_baked.png" alt="Cycles with
1000 samples using baked textures." width="740"
class="img-thumbnail"/></p>

But you hardly see any improvement for the dark areas inside the
room. Only the brighter edges along the entry and the window show less
noise, but they are noisy anyway due to motion blur.

So what exactly is __baking__ doing?

Well, similar to game engines, it's creating __textures__ which can be
used to play back the fly-through animation in real-time. As you can
see in the OpenGL screenshot (rendering) below, I only created
uv-coordinates for the floor and the inner wall polygons:

<p class="text-center"><img
src="/assets/blender_v2.75a_opengl_baked.png" alt="OpenGL (preview)
using baked textures." width="740" class="img-thumbnail"/></p>

The outside, in this case only a green ground plane and a sky from the
__sun & sky simulation__ (visible only in the images rendered by
Cycles), and the polygons at the rim of the openings are still using
full global illumination calculations while being rendered with
Cycles, but the interior walls can benefit from the baked textures, as
you saw looking at the resulting rendering times.

So, __how__ can you __create__ those __baked textures__ from within
Blender?

<p class="text-center"><img src="/assets/cycles_baking_materials.png"
alt="How to bake the textures?" width="740"
class="img-thumbnail"/></p>

Each inner wall polygon needs texture coordinates. The materials being
used on those polygons are pure __diffuse for baking__, and will use a
textured __emission__ material once we baked the global illumination
into textures.

In the screenshot above you see one of the __Image Texture__ nodes,
which have to be selected in the __Node Editor__ for baking. Be aware
that you have to link the __Diffuse BSDF__ output to the __Material
Output__ node via the __Surface__ slot before you should press the
__Bake__ button.

You have to repeat the baking process for each material (ground
polygons and each inner wall polygons), therefore you will bake five
textures in this scene.

Once you have done this you can re-connect the __Emission__ nodes to
the __Material Output__ nodes and play back the animation in real-time
in the viewport. You can also render the whole animation using the
baked textures with Cycles and use the __Video Sequence Editor__ to
combine the individual images for each frame into a single movie file.

One thing I haven't talked about (yet) is that you might want to
post-process the baked textures before you use them.

Here is the last frame for an animation using __1k textures__
(1024x1024 pixels). Click on the image to see the movie on
[YouTube][youtube]:

<div id = 'Ly_JkkT_OTo' class="youtube"><a
href="http://www.youtube.com/embed/Ly_JkkT_OTo"><img
src='/assets/simple_house_baked_1k_0001-0240.png' width = 740
class="img-thumbnail"/></a></div>

For convenience you can also download the original AVI file from
[here][avi1k].

Increasing the resolution to __2k textures__ (2048x2048) doesn't
necessarily lead to a better result, as you can see here:

<div id = 'vxU7Mse3IRE' class="youtube"><a
href="http://www.youtube.com/embed/vxU7Mse3IRE"><img
src='/assets/simple_house_baked_2k_0001-0240.png' width = 740
class="img-thumbnail"/></a></div>

For convenience you can also download the original AVI file from
[here][avi2k].

What I ended up doing is to bake 2k textures, but __blur__ them
(within Blender's __Node Editor__ and rendering with __Compositing__
enabled). Using those results in this movie:

<div id = 'i_m2MN6GFDs' class="youtube"><a
href="http://www.youtube.com/embed/i_m2MN6GFDs"><img
src='/assets/simple_house_baked_2k_blurred10_0001-0240.png' width = 740
class="img-thumbnail"/></a></div>

For convenience you can also download the original AVI file from
[here][avi2k_blurred].

I think you can't really see the differences very well on YouTube. If
you really want to compare the resulting movie files, please
__download__ them via the links provided above (right click and save
from there).

For convenience you can download the [Blender file][blend_file] for the short
animation and the baked 2k [textures][textures] (already being blurred).

[blender]:       https://www.blender.org/
[cycles]:        https://www.blender.org/manual/render/cycles/index.html
[youtube]:       https://www.youtube.com
[avi1k]:         https://www.janwalter.org/Download/Movies/simple_house_baked_1k_0001-0240.avi
[avi2k]:         https://www.janwalter.org/Download/Movies/simple_house_baked_2k_0001-0240.avi
[avi2k_blurred]: https://www.janwalter.org/Download/Movies/simple_house_baked_2k_blurred10_0001-0240.avi
[blend_file]:    https://www.janwalter.org/Download/Scenes/simple_house.blend
[textures]:      https://www.janwalter.org/Download/Scenes/simple_house_textures.zip