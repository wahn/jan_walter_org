---
layout: post
title:  "Blender to Arnold."
date:   2020-01-08 11:00:00
categories: jekyll blender rs_pbrt arnold
---

# Blender

For my rendering research I create test scenes via
[Blender][blender]. At [FMX 2014][fmx] I talked about **Comparing
Global Illumination (GI) Renderers**. In the same year the German
magazine [Digital Production][dp] published an [interview][dp_pdf]
with me about that, and at the [Blender conference][blend_conf] I gave
another talk, which still can be watched on [YouTube][video].

A lot of things changed since then, but the main idea was to keep test
scenes created with free software (Blender), store **no** renderer
specific materials or shaders within the scene, and use
[Python][python] to export to renderer specific **scene descriptions**
using some heuristics to translate Blender's material settings to the
renderer specific shaders/materials.

The last three years I wrote a [Rust][rust] based implementation of a
physically based [renderer][renderer], based on the [book][book] (and
[C++ code][cpp]) provided by Matt Pharr, Wenzel Jakob, and Greg Humphreys.

In the future I will keep my test scenes in the Blender
[cloud][cloud], but also make them available on my web page for
[download]. It is hard to get access (or publish) production scenes.
The Blender cloud is one way to get such data, but also **Disney**
started to publish some [data sets][data_sets].

This year I will probably start working on some [USD][usd] tools, but
until I can use those, I will restrict myself to store Blender files,
parse them directly, and render with only two renderers,
[rs-pbrt][rs_pbrt] and [Arnold][arnold].

# rs-pbrt

<p class="text-center"><img
src="/assets/landscape_rust_pbrt_view_0.png" alt="Book title scene
rendered by rs-pbrt." width="740" class="img-thumbnail"/></p>

The image above is from the cover of the [book][book], but was
rendered by [rs-pbrt][rs_pbrt]. This [landscape][landscape] scene is
not available as a Blender scene, but there is a git
[repository][repository], where I collect those scenes, some in
**pbrt**, some in **blend** file format.

As described in the [v0.7.0][v0-7-0] release notes, there are three
executables, and one can be used to render Blender files directly:

```shell
> ./target/release/examples/parse_blend_file --help
pbrt 0.7.3
Parse a Blender scene file and render it.

USAGE:
    parse_blend_file [OPTIONS] <path>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --bootstrap_samples <bootstrap-samples>        bootstrap samples ...
    -c, --camera_name <camera-name>                    camera name
        --chains <chains>                              number of Markov chains
    -i, --integrator <integrator>                      ao, directlighting, ...
    -l, --light_scale <light-scale>                    global light scaling ...
    -m, --max_depth <max-depth>                        max length of a ...
        --mutations_per_pixel <mutations-per-pixel>    number of path mutations
    -s, --samples <samples>                            pixel samples ...
        --sigma <sigma>                                perturbation deviation
        --step_probability <step-probability>          prob of discarding ...
        --write_frequency <write-frequency>            frequency to write ...

ARGS:
    <path>    The path to the file to read
```

Most importantly you can switch the rendering algorithm between
Ambient Occlusion, Direct Lighting, Whitted’s Ray-Tracing,
(Unidirectional) Path Tracing, Bidirectional Path Tracing (BDPT),
Metropolis Light Transport (MLT), Stochastic Progressive Photon
Mapping (SPPM), and Path Tracing (Participating Media).

# Arnold

<p class="text-center"><img
src="/assets/barbershop_v2_79_arnold_01.png" alt="Barbershop scene
rendered via btoa v0.1.0 and Arnold 6.0.1.0." width="740"
class="img-thumbnail"/></p>

Even though there is an executable shipping with [rs-pbrt][rs_pbrt] to
render Arnold's scene description files (.ass) directly (via rs-pbrt),
it would be nice to compare both renderers by rendering Blender
files. For Arnold I decided to translate the **.blend** files into
**.ass** files first (via a Rust program called [btoa][btoa]), give
you the option to store the **.ass** files in **binary** format (if
you want), and render them from there using Arnold's **kick** command.

```shell
# .blend to .ass translation (output btoa.ass)
> ~/git/codeberg/rs_blender/btoa/target/release/btoa barbershop_v2_79.blend 
BLENDER-v279
...
445218280 bytes read
2048x858 [50%] = 1024x429
# use kick to create binary version (btoa_bin.ass)
> kick -resave btoa_bin.ass btoa.ass
00:00:00    65MB         | log started Thu Jan  9 14:06:20 2020
...
00:00:23   152MB         | Arnold shutdown
# use kick with command line options to render image above (arnold.png)
> kick -dp -dw -v 3 -bs 32 -bc hilbert -as 4 btoa_bin.ass -o arnold.png -of png
00:00:00    65MB         | log started Thu Jan  9 14:08:59 2020
...
00:00:41   173MB         | Arnold shutdown
# find more command line options
> kick -h
Arnold 6.0.1.0 [25372a4c] linux clang-5.0.0 oiio-2.1.4 ... 2019/12/04 07:45:07
Usage:  kick [options] ...
...
```

Here is another example, which can be found in the same
[repository][repository]:

<p class="text-center"><img src="/assets/bedroom_v2_79_arnold_01.png"
alt="Bedroom scene rendered via btoa v0.1.0 and Arnold 6.0.1.0."
width="740" class="img-thumbnail"/></p>

```shell
# creates btoa.ass
> btoa --light_scale 5.0 bedroom_v2_79.blend
# renders arnold.png
> kick -as 4 -dif 1 -ds 64 -spc 1 -ss 2 -trm 4 -ts 2 btoa.ass -o arnold.png
```

# Disclaimer

None of this is production ready, well documented, or works on
arbitrary Blender files. If I find time I will publish some hints how
to create those Blender files yourself (or convert existing ones into
something you can render with both [rs-pbrt][rs_pbrt] and
[Arnold][arnold]). But this is pretty much work in progress and as I
said, the goal will be to use [USD][usd] from some point on. But the
Blender files found on [GitLab][repository] or the ones I will store
in the Blender cloud and later make available for [download], will
render with both renderers.

[blender]:    https://www.blender.org
[arnold]:     https://www.arnoldrenderer.com
[fmx]:        https://fmx.de/program2014/speaker/858
[dp]:         https://www.digitalproduction.com
[dp_pdf]:     https://www.janwalter.org/Publications/DP1405_070-073_Multiexport_Beleg.pdf
[blend_conf]: https://conference.blender.org
[video]:      https://youtu.be/yEpniB6H9lQ
[python]:     https://www.python.org
[rust]:       https://www.rust-lang.org
[renderer]:   https://www.rs-pbrt.org/about
[book]:       http://www.pbr-book.org
[cpp]:        https://github.com/mmp/pbrt-v3
[cloud]:      https://cloud.blender.org/welcome
[download]:   https://www.janwalter.org/download
[data_sets]:  https://www.technology.disneyanimation.com/collaboration-through-sharing
[usd]:        https://graphics.pixar.com/usd/docs/index.html
[rs_pbrt]:    https://www.rs-pbrt.org/about
[landscape]:  https://gitlab.com/jdb-walter/rs-pbrt-test-scenes/-/wikis/pbrt-landscape
[repository]: https://gitlab.com/jdb-walter/rs-pbrt-test-scenes
[v0-7-0]:     https://www.rs-pbrt.org/blog/v0-7-0-release-notes
[btoa]:       https://codeberg.org/wahn/rs_blender/src/branch/master/btoa
