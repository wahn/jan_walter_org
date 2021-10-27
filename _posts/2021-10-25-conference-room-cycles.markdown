---
layout: post
title:  "[video] Conference Room Blender Cycles"
date:   2021-10-25 14:00:00
categories: jekyll rendering cycles rs-pbrt
---

In the past I tried several things to get feedback from my readers (of
this and related blogs). I opened a **forum**, I tried to provide very
detailed information, etc. From now on I will only create short videos
to demonstrate that stuff actually works on my machine, some
additional information about what you see, and an email address for
you to contact me in case you have questions. You find the email
address on the [imprint][imprint] page, but here it is (just in case):

admin [at] janwalter [.] org

# Video

<div id='du74w_BIUDY' class="youtube"><a
href='https://youtu.be/du74w_BIUDY'><img
src='/assets/conference_room_cycles_video.png' height="740"
class="img-thumbnail"/></a></div>

## Description

What you see in the video above (if you click on the screenshot) was
recorded on Windows 11, basically opening a scene within Blender (by
drag&drop) and making Blender fullscreen. On the upper right you see
the `Outliner` with the display mode `Same Types`, which in this case
are five different cameras. By pressing `F12` you can render an image
looking through the selected camera. After the first image was
rendered (via Cycles), I did select another camera in the `Outliner`
and used a shortcut (`Ctrl Numpad 0`) to look through the selected
camera. I do render an image for all 5 cameras in the scene.

[Download][download] `conference_room_v2_79.zip` and try for yourself:

```shell
$ unzip -l conference_room_v2_79.zip 
Archive:  conference_room_v2_79.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
 14109472  2021-10-26 17:38   conference_room_v2_79.blend
---------                     -------
 14109472                     1 file
```

# Rendering Research

I provided a lot of scenes in various scene descriptions, so people
can download the scenes and render them directly with a renderer of
their choice (in case I did have a license for it, and was able to
create a scene description for that particular renderer). First I used
**Blender** to create such a scene (a lot of them came from the
**Radiance** renderer) and I wrote my own little [addon][addon] (for
Blender) in **Python** to export the scene to various (renderer
specific) scene description files.

I will continue to provide Blender scenes in the future (mainly
because Blender is open source and my [own renderer][rs-pbrt] is able
to render *some* `.blend` files directly). The other file format I
will provide is for **Houdini** (Indie). Some examples are already
online, e.g. a simple [Cornell Box][cbox], and the [Attic][attic]
scene by **Gleb Alexandrov**. 

In the future I might be able to use **USD** directly, e.g. **Arnold**
can render those, but for now I will restrict myself to those
renderers which can be used from within Houdini (e.g. via a USD
**Hydra render delegate**) or Blender. Some standalone scene
descriptions might be provided from time to time, but I'm planning to
make my own renderer more and more production ready and therefore I
will take scenes e.g. from the Blender Cloud, change them, so they
work directly as input for my own renderer, and provide scenes for
Houdini and Blender (version 2.79 for now).

## rs-pbrt

So, why bother? There is nothing special about a Blender (2.79) scene
being rendered by Cycles (Blender's internal GI renderer). Well,
except that this scene can be rendered directly by [rs-pbrt][rs-pbrt]
from the **command line** and your are able to **switch the camera**
from the command line as well:

<div id='g20kkc90omQ' class="youtube"><a
href='https://youtu.be/g20kkc90omQ'><img
src='/assets/conference_room_rs_pbrt_video.png' height="740"
class="img-thumbnail"/></a></div>

## blog

I will write a little `blog` post describing the details about the
video and provide a link once it's published.

[imprint]:  https://www.janwalter.org/imprint
[addon]:    https://bitbucket.org/wahn/blender-add-ons/wiki/io_scene_multi
[rs-pbrt]:  https://www.rs-pbrt.org/about
[cbox]:     https://www.janwalter.org/rendering/cornell-box/index.html
[attic]:    https://www.janwalter.org/rendering/attic/index.html
[download]: https://www.janwalter.org/assets/blend/conference_room_v2_79.zip
[rs-pbrt]:  https://www.rs-pbrt.org
