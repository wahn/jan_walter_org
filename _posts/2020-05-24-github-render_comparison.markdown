---
layout: post
title:  "Render Comparison"
date:   2020-05-24 15:00:00
categories: jekyll download rendering
---

# Download vs. Git Repository

The [render_comparison][render_comparison] repository on **GitHub**
was meant to hold several scenes for [Blender][blender] and
[Houdini][houdini-indie] to render with as many renderers I can
currently support (and have licenses for). Unfortunately those scenes
can get quiet big and I ran into problems like this:

```shell
$ git push
...
remote: error: GH001: Large files detected.
You may want to try Git Large File Storage - https://git-lfs.github.com.
...
```

Therefore I will host those files myself on my own server and provide
download links.

## Attic

<p class="text-center"><img src="/assets/attic_redshift.png"
alt="Attic scene rendered with Redshift." width="740"
class="img-thumbnail"/></p>

### Renderer Agnostic

- [textures](https://www.janwalter.org/Download/Scenes/attic_textures.tar.xz) (12-05-2020)
- [.obj files](https://www.janwalter.org/Download/Scenes/attic_obj.tar.gz) (27-05-2020)

### Renderer Specific

- [appleseed](https://www.janwalter.org/Download/Scenes/attic_appleseed.blend) -
  Blender (13-05-2020) - [forum post](https://forum.appleseedhq.net/t/attic/1002)
- [Cycles](https://www.janwalter.org/Download/Scenes/attic.blend) - Blender (13-05-2020)
- [Guerilla
  Render](https://www.janwalter.org/Download/Scenes/attic_guerilla.tar.gz) -
  Standalone (27-05-2020) - [forum
  post](http://www.guerillarender.com/forum/viewtopic.php?id=1347)
- [Indigo](https://www.janwalter.org/Download/Scenes/attic_indigo.blend) -
  Blender (13-05-2020) - [forum
  post](https://www.indigorenderer.com/forum/viewtopic.php?f=3&t=15475)
- [LuxCoreRender](https://www.janwalter.org/Download/Scenes/attic_luxcore.blend) -
  Blender (13-05-2020) - [forum
  post](https://forums.luxcorerender.org/viewtopic.php?f=2&t=2218)
- [Maxwell](https://www.janwalter.org/Download/Scenes/attic_maxwell.mxs) -
  Standalone (13-05-2020) - [forum
  post](https://forum.maxwellrender.com/viewtopic.php?f=48&t=45701&p=399852#p399852)
- [OctaneRender](https://www.janwalter.org/Download/Scenes/attic_octane.blend) -
  Blender (13-05-2020) - [forum post](https://render.otoy.com/forum/viewtopic.php?f=6&t=74972)
- [PBRT](https://www.janwalter.org/Download/Scenes/attic_pbrt.gz) - Standalone (12-05-2020)
- [Redshift](https://www.janwalter.org/Download/Scenes/attic_houdini_redshift.tar.gz) -
  Houdini (14-05-2020) - [forum
  post](https://www.redshift3d.com/forums/viewthread/31580)
- [RenderMan](https://www.janwalter.org/Download/Scenes/attic_houdini_prman.tar.gz) -
  Houdini (14-05-2020) - [forum
  post](https://renderman.pixar.com/forum/showthread.php?s=&threadid=43787)

[render_comparison]:   https://github.com/wahn/render_comparison
[blender]:             https://www.blender.org
[houdini-indie]:       https://www.sidefx.com/products/houdini/houdini-indie
