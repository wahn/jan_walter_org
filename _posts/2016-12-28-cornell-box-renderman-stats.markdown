---
layout: post
title:  "Cornell Box: Debugging and Optimization for Pixar’s RenderMan"
date:   2016-12-27 17:37:00
categories: jekyll rendering renderman
---

I already talked about this scene in a [previous post][post]. Today I
would like to mention a [tutorial][tutorial], written by __Patrik
S. Hadorn__ a couple of days ago. To play around with a simple scene
and the statistics given by [Pixar's RenderMan][prman] I have choosen
the [Cornell Box][cornell_box] scene, which I already provide as
a [Blender scene][scene] in the [download] [download] section.

<p class="text-center"><img src="/assets/cornell_box_prman.png"
alt="Cornell Box rendered by RenderMan."  width="500"
class="img-thumbnail"/></p>

## Blender Addon

Basically the scene is setup for Blender's internal renderer,
called [Cycles][cycles]. [Pixar][pixar] itself provides a
Blender [addon][addon] and you can read more about it on
their [forum][forum] __RenderMan for Blender__ (the link should work
once you registered and are logged in).

The [Blender scene][scene] mentioned above can be loaded into Blender
and after you installed the [addon][addon] you should be able to
render with RenderMan. As far as I remember I created a __world__
(while Cycles was still active), and gave it a black __background__
color before switching to __RenderMan__. Make sure the world settings
remain black after switching the renderers. Then you can select
e.g. the Cornell Box (which has three materials on one mesh), select
the material tab, and press the button labeled with __Convert All
Cycles to Renderman__. After that you should be able to render with
RenderMan. Just to make sure that all the settings are the same I
provide a [special version][prman-version] of the scene, which assumes
that you have a working addon installed, that it can find the
renderer, and that you got a license for RenderMan. You can install
the [non-commercial][nc-prman] version to get started. Once you press
the render button you have to wait until the renderer is done, but you
will see the image before and can follow the progress while it's
rendering. The final statistics are available as an XML file which
should look like this:

<p class="text-center"><img src="/assets/cornell_box_prman_stats.png"
alt="Cornell Box statistics rendered by RenderMan." width="740"
class="img-thumbnail"/></p>

[post]:           https://www.janwalter.org/jekyll/rendering/renderman/2015/04/13/cornell-box-renderman.html
[tutorial]:       https://community.renderman.pixar.com/article/1673/debugging-and-optimization-1.html
[prman]:          http://renderman.pixar.com
[cornell_box]:    http://www.graphics.cornell.edu/online/box/history.html
[scene]:          https://www.janwalter.org/Download/Scenes/cornell_box_blend.tar.gz
[download]:       https://www.janwalter.org/download
[cycles]:         https://www.blender.org/manual/render/cycles/index.html
[Pixar]:          http://www.pixar.com
[addon]:          https://github.com/bsavery/PRMan-for-Blender/releases
[forum]:          https://renderman.pixar.com/forum/forumdisplay.php?s=&forumid=166
[prman-version]:  https://www.janwalter.org/Download/Scenes/cornell_box_prman_blend.tar.gz
[nc-prman]:       http://renderman.pixar.com/view/non-commercial-renderman
[xml]:            https://www.janwalter.org/assets/cornell_box_stats.xml
