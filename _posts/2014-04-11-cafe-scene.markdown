---
layout: post
title:  "Cafe Scene"
date:   2014-04-11 16:06:49
categories: jekyll rendering comparison
---

<img src="/assets/cafe_scene.png" alt="Simple room" width="964"
class="img-thumbnail"/>

Chapter 2 of the __Rendering with Radiance__ [book][book] introduces
this __Cafe Scene__. It shows exacly the same geometry lit by two
different light setups.

On the left two light emitting spheres (light bulbs) are covered by
lamp shades, so that most of light will be shed downwards and only a
bit of light bounces around and illuminates the upper part of the back
wall and the ceiling.

On the right you see an alternative in lighting using using the three
basic approaches to interior lighting design: light to look at
(__decorative__), light for objects to be seen (__accent__), and
general lighting to see by (__ambient__):

* In the original [Radiance][radiance] scene the middle light on the
  ceiling is actually a __spotlight__, located above the flower and
  the vase, providing dramatic focus to the table area.

* The diffuse __downlights__ will flank the table and provide a nice,
  airy, general illumination during the daytime. It is likely that
  they will be turned off at night; this will heighten the visual
  drama created by the spotlight.

* The __wall sconces__ are located above the mirror height, which
  keeps them out of the patrons' reach and improves the visual
  composition of the scene. By rotating the sconces 45 degrees, a more
  interesting shadow pattern is created.

Even though this scene is still pretty simple, it raises a couple of
questions.

* __Are the light emitters visible?__ In principle the light emitting
  spheres are visible in the first lighting condition (using the lamp
  shades). Nevertheless, if we don't move the camera and take a single
  picture of the scene, we hardly can detect the spheres within the
  very bright reflections of the inner lamp shades. Therefore we can
  get away by using more traditional point lights. In [Arnold][arnold]
  a __point light__ can even have a __radius__ which will affect the
  hard- or softness of the resulting shadows. In case we potentially
  see the light emitting spheres we still have two choices in
  __Arnold__: We can use the __emission__ parameters of a __standard__
  shader with some __sphere__ primitives, or we can tesselate the
  spheres into meshes and use Arnold's __mesh_light__. As we will see
  in another blog using a __mesh_light__ has the advantage that you
  can tweak the __samples__ per light emitter to further reduce the
  rendering time or raise the quality of the resulting images.

* __Can we tweak the rendering times by using simpler light sources?__
  The second lighting condition would also allow to use point lights
  for the light emitting spheres (part of the wall sconces) because
  they are hidden (from this perspective) by slightly light emitting
  lenses. But what about the three light emitting discs on the
  ceiling? The middle one was supposed to act like a __spotlight__ by
  "clipping" the diffuse output of the lamp into a cone. What if the
  renderer in question does not allow the usage of spotlights? In that
  case we have to model something to wrap the light emitter and clip
  the emitted light to a similar cone shaped beam of light. For
  __Arnold__ we can drastically reduce the render time and the noise
  by choosing carefully alternative light emitters and tweak light and
  option parameters for this scene (as seen below):

<p class="text-center"><img
src="/assets/cafe_scene_arnold_lights2.png" alt="Simple room"
width="500" align="middle" class="img-thumbnail"/></p>

* __Is there a way to separate the light emitters from their shape?__
  For example the light emitting disks at the ceiling can be replaced
  by spotlights with various cones to direct the light rays. But we
  still want to see three white disks. One solution would be to render
  those separately and composite them on top of the rendered
  image. But can we prevent this? Some renderers like [Povray][povray]
  allow the usage of [looks_like][looks_like] to have a visible object
  in the scene, but the light still comes from invisible light
  sources. In __Arnold__ we could for example use an __utility__
  shader and a __disk__ primitive parameter called __visibility__ to
  tell the renderer which rays a geometry is visible to. Of course
  none of this would be physically correct (or even physically
  motivated).

{% highlight tcsh %}
...
utility
{
 name MAlightring_util
 color_mode 0 # color
 shade_mode 2 # flat
 color 100 100 100
}

disk
{
...
 visibility 5 # AI_RAY_CAMERA & AI_RAY_REFLECTED
...
}
...
{% endhighlight %}

Posts about more complex scenes (and other aspects of rendering) will
follow soon ...

[radiance]:   http://radsite.lbl.gov/radiance
[book]:       http://radsite.lbl.gov/radiance/book/index.html
[arnold]:     http://www.solidangle.com
[povray]:     http://www.povray.org
[looks_like]: http://www.povray.org/documentation/view/3.6.1/315/