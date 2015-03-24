---
layout: post
title:  "Pixar’s Non-Commercial RenderMan"
date:   2015-03-24 10:23:54
categories: jekyll rendering renderman
---

[Pixar][Pixar] released yesterday a non-commercial RenderMan
version. They provide some information about it on their [web
site][nc-prman]. Most of the people will use it from within
[Maya][Maya] or [Katana][Katana], but I want to provide some basic
information about the stand alone renderer.

I installed it on __OS X__, but __Linux__ or __Windows__ versions
should work in a similar way. First you should set an environment
variable called __$RMANTREE__:

{% highlight tcsh %}
setenv RMANTREE /Applications/Pixar/RenderManProServer-19.0
{% endhighlight %}

The syntax above is for [tcsh][tcsh], for [bash][bash] use the
following:

{% highlight bash %}
export RMANTREE=/Applications/Pixar/RenderManProServer-19.0
{% endhighlight %}

To render our first picture we will go directly into a subfolder and
render one of the example [RIB][RIB] files coming with the free
version:

{% highlight tcsh %}
cd $RMANTREE/lib/examples/RIS/scenes/pattern/osl
$RMANTREE/bin/prman -progress -d tiff oslball.rib
{% endhighlight %}

[RIB][RIB] stands for the __R__enderMan __I__nterface __B__ytestream
Protocol.

The resulting 512x512 image will be called __oslball.tif__ as
described in the [RIB][RIB] file:

{% highlight tcsh %}
Display "oslball.tif" "tiff" "rgba"
Format 512 512 1
{% endhighlight %}

It should look similar to the image below:

<p class="text-center"><img src="/assets/oslball.png" alt="RenderMan
using OSL patterns."  width="512" class="img-thumbnail"/></p>

The __patterns__ were generated via the [OSL][OSL] shading language
and some examples (e.g. __oak.osl__) can be found in the __shaders__
subfolder.

I will update my [multi-exporter][multi_exporter] soon to support the
non-commercial RenderMan version from Pixar and provide [RIB][RIB]
files in the [download][download] section.

[Pixar]:          http://www.pixar.com
[nc-prman]:       http://renderman.pixar.com/view/non-commercial-renderman
[Maya]:           http://www.autodesk.com/products/maya/overview
[Katana]:         http://www.thefoundry.co.uk/products/katana
[tcsh]:           https://en.wikipedia.org/wiki/Tcsh
[bash]:           https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29
[RIB]:            http://renderman.pixar.com/resources/current/RenderMan/ribBinding.html
[multi_exporter]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[OSL]:            http://opensource.imageworks.com/?p=osl
[download]:       https://www.janwalter.org/download
