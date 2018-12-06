---
layout: post
title:  "Hairy Stuff"
date:   2018-02-01 12:30:00
categories: jekyll rendering pbrt
---

Last month I was working on a lot of **new test scenes** for my
[Rust][rust] implementation of the [PBRT][pbrt] renderer. But a big
chunk of my time went into implementing the [curve shape][issue_37]
needed for the geometry of hair, and a [material][issue_38], which
implements a [hair scattering][hair_scattering] model.

## Hair

Both test scenes come originally from [Cem Yuksel][cemyuksel], but can
be downloaded by cloning a git repository described [here][scenes-v3].

<p class="text-center"><img src="/assets/curly-hair_pbrt_rust.png"
alt="Curly hair provided by Cem Yuksel" width="720"
class="img-thumbnail"/></p>

The image above was rendered in 1024x1024 **resolution** with 1024
**pixel samples**, using a [HDR][hdr] image, instead of the [Portable
FloatMap Image Format][pfm] (PFM).

```shell
> rg Film -A 1
straight-hair.pbrt
6:Film "image"
7-   "integer xresolution" [ 1024 ] "integer yresolution" [ 1024 ]

curly-hair.pbrt
5:Film "image"
6-    "integer xresolution" [ 1024 ] "integer yresolution" [ 1024 ]
> rg hdr
straight-hair.pbrt
1100028: LightSource "infinite" "string mapname" [ "textures/Skydome.hdr" ] 

curly-hair.pbrt
3291601: LightSource "infinite" "string mapname" [ "textures/Skydome.hdr" ]
```

BTW the `rg` program used above is called [ripgrep][rg] and is a
line-oriented search tool that recursively searches your current
directory for a regex pattern while respecting your gitignore rules.

<p class="text-center"><img src="/assets/straight-hair_pbrt_rust.png"
alt="Straight hair provided by Cem Yuksel" width="720"
class="img-thumbnail"/></p>

The straight hair version used only 512 **pixel samples**.

If you want to render both scenes with my [Rust
implementation][rs_pbrt] of PBRT I provide slighltly modified versions
for [download][download], which should work from version
[v0.2.7][v0_2_7] on and later:

```shell
> tar tfvz hair_pbrt.tar.gz
drwxrwxrwx jan/users         0 2018-02-02 12:25 hair/
-rw-r--r-- jan/users 237072788 2018-01-31 11:27 hair/straight-hair.pbrt
drwxrwxrwx jan/users         0 2018-01-31 11:27 hair/textures/
-rw-r--r-- jan/users    790841 2018-01-31 11:20 hair/textures/Skydome.exr
-rw-r--r-- jan/users    113311 2018-01-31 11:27 hair/textures/Skydome.hdr
-rw-r--r-- jan/users 710196608 2018-01-31 19:55 hair/curly-hair.pbrt
```

So, what did I modify (and why)?

### HDR instead of Portable FloatMap (PFM)

The current version of [rs_pbrt][rs_pbrt] does **not** support
[PFM][pfm] files. You can compile with [OpenEXR][openexr] support
using the [openexr crate][openexr_crate], but that might be tricky on
some platforms, so I also support [HDR][hdr] via the [image
crate][image_crate].

### No include lines in PBRT

This is probably easy to fix in one of the future versions of
[rs_pbrt][rs_pbrt], but right now it was not on my agenda, so I simply
replaced lines like this:

```shell
> rg Include
straight-hair.pbrt
20:	Include "models/straight-hair.pbrt" 

curly-hair.pbrt
15:	Include "models/curly-hair.pbrt" 

sphere-hairblock.pbrt
19:  Include "models/block.pbrt"
```

By just cutting & pasting the lines from mentioned files to replace
the include lines. But I created an [issue][issue_41] for this, so it
get fixed in a future version.

## Other Test Scenes

<p class="text-center"><img src="/assets/classroom_pbrt_rust.png"
alt="Japanes Classroom by NovaZeeke" width="720"
class="img-thumbnail"/></p>

[classroom_pbrt.tar.gz][classroom_pbrt]

<p class="text-center"><img src="/assets/kitchen_pbrt_rust.png"
alt="Country Kitchen by Jay-Artist" width="720"
class="img-thumbnail"/></p>

[kitchen_pbrt.tar.gz][kitchen_pbrt]

<p class="text-center"><img src="/assets/living-room-2_mitsuba.png"
alt="The White Room by Jay-Artist rendered by Mitsuba" width="720"
class="img-thumbnail"/></p>

[living-room-2_pbrt.tar.gz][living-room-2_pbrt]

[The White Room][bitterli] (above) comes from Benedikt Bitterli’s
**Rendering Resources** and I rendered only a noisy version with
[rs_pbrt][rs_pbrt], which can be found [here][other_scenes]. The image
above was rendered by [Mitsuba][mitsuba]. This is a good scene to
render again, once I have implemented [bi-directional][bi-directional]
path-tracing.

<p class="text-center"><img src="/assets/staircase_pbrt_rust.png"
alt="The Wooden Staircase by Wig42" width="720"
class="img-thumbnail"/></p>

[staircase_pbrt.tar.gz][staircase_pbrt]

[rust]: https://www.rust-lang.org
[pbrt]: http://www.pbrt.org
[issue_37]: https://github.com/wahn/rs_pbrt/issues/37
[issue_38]: https://github.com/wahn/rs_pbrt/issues/38
[issue_41]: https://github.com/wahn/rs_pbrt/issues/41
[hair_scattering]: http://www.pbrt.org/hair.pdf
[cemyuksel]: http://www.cemyuksel.com
[scenes-v3]: http://www.pbrt.org/scenes-v3.html
[hdr]: https://en.wikipedia.org/wiki/RGBE_image_format
[pfm]: http://www.pauldebevec.com/Research/HDR/PFM
[rg]: https://github.com/BurntSushi/ripgrep
[rs_pbrt]: https://github.com/wahn/rs_pbrt
[download]: https://www.janwalter.org/Download/Scenes/PBRT/hair_pbrt.tar.gz
[v0_2_7]: https://github.com/wahn/rs_pbrt/wiki/Release-Notes#v027
[openexr]: http://www.openexr.com
[openexr_crate]: https://crates.io/crates/openexr
[image_crate]: https://crates.io/crates/image
[classroom_pbrt]: https://www.janwalter.org/Download/Scenes/PBRT/classroom_pbrt.tar.gz 
[kitchen_pbrt]: https://www.janwalter.org/Download/Scenes/PBRT/kitchen_pbrt.tar.gz 
[living-room-2_pbrt]: https://www.janwalter.org/Download/Scenes/PBRT/living-room-2_pbrt.tar.gz 
[staircase_pbrt]: https://www.janwalter.org/Download/Scenes/PBRT/staircase_pbrt.tar.gz 
[bitterli]: https://benedikt-bitterli.me/resources
[other_scenes]: https://github.com/wahn/export_multi/tree/master/other_scenes/img
[mitsuba]: http://www.mitsuba-renderer.org
[bi-directional]: https://github.com/wahn/rs_pbrt/issues/29
