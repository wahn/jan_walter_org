---
layout: post
title:  "Inventure Place: Painting with Day Light"
date:   2014-08-21 15:26:55
categories: jekyll rendering indigo
---

The __National Inventors Hall of Fame__ ([NIHF][nihf]) is an American
not-for-profit organization dedicated to the inventors and their
inventions. Founded in 1973, its primary mission is to "honor the
women and men responsible for the great technological advances that
make human, social and economic progress possible." - see Wikipedia
[link][nihf].

<p class="text-center"><img src="/assets/invplace_photo_01.png"
alt="View across the street" width="320" class="img-thumbnail"/> <img
src="/assets/invplace_photo_02.png" alt="Side view on Inventure Place"
width="320" class="img-thumbnail"/></p>

The [photos][photos] above were found through an Internet search (via
__Google__) and were taken by __Thom Sheridan__ from Akron, Ohio.

It looks like __Charles Ehrlich__ created a 3D model and __Greg Ward__
rendered some images with [Radiance][radiance], a suite of tools for
performing lighting simulation (originally written by Greg). Here is
one of the camera perspectives, defined in the original view files,
rendered by __Radiance__:

<img src="/assets/invplace_radiance_augAM_f3w.png" alt="Radiance
rendering from the 3rd floor looking west." width="650"
class="img-thumbnail"/>

I imported the scene (defined by Radiance specific __*.rad__ files)
into [Blender][blender], modified it slightly (mainly the glass
windows, which should be __volumetric__ for most renderers), and
exported to several renderers. Currently my favourite image (from the
same camera perspective) was rendered by [Indigo][indigo]:

<img src="/assets/invplace_indigo_augAM_f3w.png" alt="Indigo rendering
from the 3rd floor looking west." width="650" class="img-thumbnail"/>

All light comes from the sun and sky. Radiance allows to __exclude__
certain modifiers (or materials) __from__ the __indirect
calculations__ to speed up the indirect computation (see __-ae__ and
__-aE__ option of the [rpict][rpict] program). This might be an
explanation for some of the __missing features__ visible in the Indigo
image, but not in the Radiance version. Anyway, I plan to render more
images, with several renderers, and bring up the topic (regarding this
scene) again in the future. For now I just wanted to introduce the
scene, the geometry, and show some results. The scene and all images
can be found in this [repository][repo].

[nihf]:     https://en.wikipedia.org/wiki/National_Inventors_Hall_of_Fame
[photos]:   https://secure.flickr.com/photos/thomsheridan/3906615249/in/photostream
[radiance]: https://en.wikipedia.org/wiki/Radiance_%28software%29
[blender]:  http://www.blender.org
[repo]:     https://github.com/wahn/export_multi/tree/master/08_invplace
[indigo]:   http://www.indigorenderer.com
[rpict]:    http://radsite.lbl.gov/radiance/man_html/rpict.1.html