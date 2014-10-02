---
layout: post
title:  "Salle de bain: Lagoa"
date:   2014-10-02 11:58:53
categories: jekyll rendering lagoa
---

Here is the first result for [Lagoa][lagoa]:

<img src="/assets/salle_de_bain_lagoa.png" alt="Lagoa rendering of a
modern bathroom (from Blend Swap)." width="960"
class="img-thumbnail"/>

Lagoa features __MultiOptics__, the world’s first photorealistic
interactive 3D renderer on the __cloud__. My current workflow with
Lagoa is to export __OBJ__ files from within Blender, upload those
(and textures used) to the Lagoa web site, apply materials there, and
render. Here a couple of things I came across:

* You have to be careful about the __normals__, otherwise you might
  end up with black parts in your scene where the normals point away
  from the camera. You can actually see some black parts on the wooden
  bath tube, where the normals were not corrected.

* __Matching__ Lagoa's camera with Blender's camera is tricky. I would
  assume that you can use __the camera__ position and the positon of
  an __empty__, which you used for auto-tracking within Blender, but
  that didn't work correctly. So I just tried to match the camera
  closely by navigating in the browser and comparing with other images
  rendered by various renderers (see post [here][post]).

* The __materials__ need more attention. The rendering I got so far
  doesn't show the specular reflections (e.g. in the wooden parts)
  yet.

* What's nice about Lagoa is that you can start rendering in the
  __browser/cloud__ (not on your local machine) and while you adjust
  materials you see the results immediately.

* I did have some __problems__ though sending the __render__ job into
  the __background__ and picking up the resulting image later. I
  waited for example two hours to let the render refine and the
  resulting image showed some black parts, whereas it rendered fine
  interactively in the browser. It would be helpful to be able to see
  the background job rendering (I mean the image, not just the time
  slider).

[Repository][repo]:

* __Name:__ [Salle de bain][salle-de-bain]
* __Author:__ [nacimus][nacimus]

[lagoa]:         http://home.lagoa.com/
[post]:          https://www.janwalter.org/jekyll/rendering/cycles/2014/09/12/salle-de-bain-cycles-lxs-igs.html
[repo]:          https://github.com/wahn/export_multi/tree/master/09_salle_de_bain
[salle-de-bain]: http://www.blendswap.com/blends/view/73937
[nacimus]:       http://www.blendswap.com/users/view/nacimus
