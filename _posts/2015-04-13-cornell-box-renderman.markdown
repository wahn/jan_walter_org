---
layout: post
title:  "Cornell Box: First Results for Pixar’s RenderMan"
date:   2015-04-13 16:29:51
categories: jekyll rendering renderman
---

One of the most basic scenes for Global Illumination (GI) is the
[Cornell Box][cornell_box]. In the [download][download] section I
provide the scene for several renderers:

+ Arnold
+ Cycles
+ Indigo
+ Luxrender
+ Maxwell
+ mental ray
+ PRMan
+ Radiance

But here I want to focus on [Pixar's][Pixar] last release of
[RenderMan][renderman] (__PRMan__). You can download a free version of
the renderer for [non-commercial][nc-prman] usage. Pixar's scene
description is called [RIB][RIB] (__R__enderMan __I__nterface
__B__ytestream Protocol). If you download the Cornell Box RIB file and
render it with PRMan your resulting image should look like this:

<p class="text-center"><img src="/assets/cornell_box_prman.png"
alt="Cornell Box rendered by RenderMan."  width="500"
class="img-thumbnail"/></p>

For convenience the file is also available from Pixar's [Community
Resources][resources]. I will only point out the details about RIB
lines which are new or specific to the latest PRMan release:

{% highlight tcsh %}
...
Hider "raytrace" 
  "constant string integrationmode" ["path"]
  "constant int incremental" [1]
  "int minsamples" [32]
  "int maxsamples" [1032]
Integrator "PxrVCM" "PxrVCM"
  "int maxPathLength" [10]
  "int mergePaths" [1]
  "int connectPaths" [1]
...
WorldBegin
  Attribute "identifier" "name" "Light"
  AttributeBegin
    ...
    AreaLightSource "PxrAreaLight"
      "Light" "string shape" ["rect"]
      "float intensity" [50]
    Bxdf "PxrLightEmission" "Light"
    ...
  AttributeEnd
    ...
    Bxdf "PxrLMDiffuse" "box_Material" "color frontColor" [0.5 0.5 0.5]
    PointsPolygons
    ...
    Bxdf "PxrLMDiffuse" "box_Material" "color frontColor" [0.5 0.5 0.5]
    PointsPolygons
    ...
    Bxdf "PxrLMDiffuse" "cbox_Material0" "color frontColor" [0.4 0.4 0.4]
    PointsPolygons
    ...
    Bxdf "PxrLMDiffuse" "cbox_red1" "color frontColor" [0.5 0.0 0.0]
    PointsPolygons
    ...
    Bxdf "PxrLMDiffuse" "cbox_green2" "color frontColor" [0.0 0.5 0.0]
    PointsPolygons
    ...
WorldEnd
{% endhighlight %}

For [RIS][RIS] we need to use the [Raytrace Hider][hider] with
path-tracing. In theory we have the choice between __unidirectional__
and __bidirectional path tracing__, but in this case we have chosen
the bidirectional version by specifiying [PxrVCM][PxrVCM] as our
integrator. The remaining RIB file lines are pretty much standard,
except the __Surface__ shader lines got replaced by __Bxdf__ material
lines. For the Cornell Box we only need diffuse materials which are
explained in the [LMDiffuse][LMDiffuse] RenderMan 19
Documentation. The one and only light source in the scene is an area
light using the [PxrAreaLight][PxrAreaLight] shader.

Before you start to render your own version of the image above you
should set an environment variable called __$RMANTREE__:

{% highlight tcsh %}
setenv RMANTREE /Applications/Pixar/RenderManProServer-19.0
{% endhighlight %}

The syntax above is for [tcsh][tcsh], for [bash][bash] use the
following:

{% highlight bash %}
export RMANTREE=/Applications/Pixar/RenderManProServer-19.0
{% endhighlight %}

To render use the following command line:

{% highlight tcsh %}
$RMANTREE/bin/prman -progress cornell_box.rib
{% endhighlight %}

We will see other scenes later using __unidirectional path
tracing__. I modified the [multi_exporter][multi_exporter] Python
script for [Blender][blender] to use the new PRMan shaders and materials.

[cornell_box]:    http://www.graphics.cornell.edu/online/box/history.html
[download]:       https://www.janwalter.org/download
[Pixar]:          http://www.pixar.com
[renderman]:      http://renderman.pixar.com
[nc-prman]:       http://renderman.pixar.com/view/non-commercial-renderman
[RIB]:            http://renderman.pixar.com/resources/current/RenderMan/ribBinding.html
[resources]:      https://community.renderman.pixar.com/article/400/cornell-box.html
[RIS]:            https://renderman.pixar.com/resources/current/RenderMan/risOverview.html
[hider]:          https://renderman.pixar.com/resources/current/RenderMan/risOptions.html#raytrace-hider
[PxrVCM]:         https://renderman.pixar.com/resources/current/RenderMan/PxrVCM.html
[LMDiffuse]:      https://renderman.pixar.com/resources/current/RenderMan/PxrLMDiffuse.html
[PxrAreaLight]:   http://renderman.pixar.com/resources/current/RenderMan/PxrAreaLight.html
[tcsh]:           https://en.wikipedia.org/wiki/Tcsh
[bash]:           https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29
[multi_exporter]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[blender]:        https://www.blender.org
