---
layout: post
title:  "Inventure Place: Indigo vs. Luxrender"
date:   2014-09-02 13:51:26
categories: jekyll rendering indigo
---

In a previous post I already mentioned the __National Inventors Hall
of Fame__ ([NIHF][nihf]). Today I want to present some more pictures
of the same [scene][repo] rendered during my holidays with
[Indigo][indigo] and [Luxrender][luxrender].

The first camera perspective shows the building from the __3rd floor__
looking __eastwards__:

<img src="/assets/invplace_indigo_augAM_f3e.png" alt="Indigo rendering
from the 3rd floor looking east." width="650" class="img-thumbnail"/>

<img src="/assets/invplace_luxrender_augAM_f3e.png" alt="Luxrender rendering
from the 3rd floor looking east." width="650" class="img-thumbnail"/>

The first image is rendered by __Indigo__ and looks more blueish than
the second one, rendered by __Luxrender__. Both images rendered in
over 64 hours (the Luxrender one I allowed 70 hours). I have chosen
the camera/tonemapper settings so that the __staircase__ in the middle
was slightly __overexposed__ (mainly white), the __sky__ is still
visible as __blue__ on the top, but the bright sun light casting the
shadow of the staircase against the walls in the lower part still
shows the stripes separating the windows on the top of the building.

Otherwise both images a prettly close to each other, they both show
the [caustics][caustics], created by reflections of the airplane
wings, in the upper left corner of the image. The Indigo rendering
seems to allow more bounce light (or a higher energy bouncing around),
so the blueish color seems to be caused by the [color
bleeding][color-bleeding] of the blue floor material onto walls and the
(grey) ceiling.

The second camera perspective turns the camera around and shows the
building from the __3rd floor__ looking __westwards__:

<img src="/assets/invplace_indigo_augAM_f3w.png" alt="Indigo rendering
from the 3rd floor looking west." width="650" class="img-thumbnail"/>

<img src="/assets/invplace_luxrender_augAM_f3w.png" alt="Luxrender rendering
from the 3rd floor looking west." width="650" class="img-thumbnail"/>

This time we can see that the __sun light__ is entering the building
not only through the top windows, but also through the __west glass
front__. The staircases are well lit, but not overexposed, the sky is
too bright to show blue, but the reflections show the other (east)
glass front and some blue sky from that direction. The darker color in
the lower part of the (west) glass front is caused by a building
__obstructing__ the sun rays. We can see __stripe patterns__ on the
rounded walls which are caused by reflections of the __railings__.

The last camera perspective shows __a staircase__ from below
(looking eastwards):

<img src="/assets/invplace_indigo_augAM_st.png" alt="Indigo rendering
of the staircase." width="650" class="img-thumbnail"/>

<img src="/assets/invplace_luxrender_augAM_st.png" alt="Luxrender rendering
of the staircase." width="650" class="img-thumbnail"/>

The upper left corners are slightly overexposed, you can see how the
__color bleeding__ picks up far more blueish color in the __Indigo__
rendering. The __Luxrender__ version is missing stronger reflections
in the railing material. This could be changed in future versions of
the [exporter][multi-exporter].

Finally I want to talk about the __blue__ vs. the __black stripe__ in
the lower part of both images. The CG [pinhole camera][pinhole-camera]
is placed directly on the (blue) floor, which is impossible to do with
a real camera, but in this case it leads to the visibility of the
floor within the __Indigo__ image, whereas the __Luxrender__ camera
seems to clip the floor from the viewing frustrum, which leads to a
black color. Anyway, placing the camera slightly higher should solve
the problem and the bouncing of the light happens correctly in both
cases.

[nihf]:           https://en.wikipedia.org/wiki/National_Inventors_Hall_of_Fame
[indigo]:         http://www.indigorenderer.com
[luxrender]:      http://www.luxrender.net/en_GB/index
[repo]:           https://github.com/wahn/export_multi/tree/master/08_invplace
[caustics]:       https://en.wikipedia.org/wiki/Caustic_%28optics%29
[color-bleeding]: https://en.wikipedia.org/wiki/Color_bleeding_%28computer_graphics%29
[multi-exporter]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[pinhole-camera]: https://en.wikipedia.org/wiki/Pinhole_camera_model