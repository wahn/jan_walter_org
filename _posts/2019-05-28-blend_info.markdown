---
layout: post
title:  "[blend_info] Rust and Blender"
date:   2019-05-28 12:10:00
categories: jekyll blender rust blendinfo
---

# Rust and Blender

[Rust][rust] is a programming language which is blazingly fast and
memory-efficient. I started to learn that language several years ago
and my biggest (public) project until today can be found
[here][rs_pbrt].

[Blender][blender] is a free and open source 3D creation suite. I once
worked for NaN (Not a Number), the company which once was responsible
for Blender, in 2000/2001 and I was responsible for the
[Python][python] API back then.

For my [rendering research][multi_exporter] I developed a couple of
[export scripts][blender_add_ons] for Blender in Python. I started
using those to **import** [Radiance][radiance] scenes into Blender,
and the **main idea** was to keep the scene geometry and camera
settings, and **not** to store any renderer specific data with it, but
rather mis-use Blender's material settings to store e.g. material or
shader information, and finally use heuristics to translate those
settings into materials and shaders used for individual (external)
renderers. This way one scene could be used to export to as many
renderers I could get hold of. In 2014 I spoke at the [Blender
Conference][bconf] about this topic and a video of my talk is still
available on YouTube.

## Blender 2.80

With [Blender 2.80][blender_2_8] on the horizon a lot of things have
changed, so I wanted to reflect a bit about the things I had done so
far, and where to go from here.

# Scene Database

For my rendering research I would like to keep some scenes in a format
which can easily be used (for free), and be converted to as many
renderers as possible. Since I started comparing renderers many free
and commercial renderers have invested into **add-ons** for Blender
and many of them are far better integrated than my own export
scripts. Here are some examples:

- [appleseed][appleseed] and it's [Blender add-on][blenderseed].
- [Arnold][arnold] and it's [Blender add-on][btoa].
- [Indigo Renderer][indigo] and it's [Blender add-on][blendigo].
- [LuxCoreRenderer][luxcorerender] and it's [Blender
  add-on][blendluxcore].
- [Maxwell][maxwell] and it's [Blender add-on][bmaxwell].
- [OctaneRender][octane] has a Blender add-on too, just search for it
  in the [forum][octaneforum].
- [PBRT][pbrt] and it's [Blender add-on][pbrt_v3_iile].
- Pixar's [RenderMan][renderman] and it's [Blender add-on][prmanforblender].

I'm sure there are more, but I think you got my point. Just try to
install a couple of them and try to come up with a scene which works
for all of them. The price you pay is that all those add-ons store
renderer specific information with the scene and I really do not want
this in my scene database.

## DNA

Blender has a DNA, which allows even old Blender executables to load
files which were saved by a far newer version of Blender (and many
things changed since then). I never saw this in any commercial
product, actually most of them are pretty bad in loading scenes from
different versions of their own software.

# Rust

So, to explore a bit the Blender binary file format and provide tools
to read and use them I started a new [repository][rs_blender] on
[Codeberg][codeberg]. Finally I want to read Blender files directly,
and render them with my own [renderer][rs_pbrt]. But on my way to
develop such a thing, there are many other possibilities, e.g. one
could convert Blender files to a **new file format**, which does not
only work for Blender and one single renderer, but would allow any
Digital Content Creation (DCC) tool to save to, and any renderer to
read from. Anyway, that's a complicated topic and let's start far
simpler, by **exploring Blender's file format**. You get the source
code of Blender for **reverse engineering** (create a debug version
and single step through file related code with a debugger) and some
Rust code (provided by me) to see what I have figured out so far ...

# blend_info 0.1.0

Version 0.1.0 of my **blend_info** tool allows you to read .blend
files from various Blender versions, but does not handle byte swapping
and other conversions (yet).

## get the source and compile

First [install Rust][install]. To keep it up to date use
[rustup][rustup]:

```sh
rustup update
```

Then clone the [repository][rs_blender] and compile **blend_info**:

```sh
# clone repo using HTTPS
git clone https://codeberg.org/wahn/rs_blender.git
# make sure to compile version 0.1.0 (using a tag)
cd rs_blender/blend_info/
git checkout v0.1.0
# compile blend_info
cargo build --release
# decompress example scene from Blender 2.80
cd ../blend
bunzip2 startup_file.blend.bz2
# go back to the directory where you compiled
cd ../blend_info/
```

Now let's run the executable:

```sh
# first without any arguments
./target/release/blend_info 
error: The following required arguments were not provided:
    <path>

USAGE:
    blend_info <path>

For more information try --help
```

It tells you to provide a **path** to a file, but let's first look at
some other command line options and the **help**:

```sh
# print the help
./target/release/blend_info --help
blend_info 0.1.0
Jan Walter <jan@janwalter.com>
Print some information about a Blender scene file.

USAGE:
    blend_info <path>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <path>    The path to the file to read
# now print just the version (both -V, as well as --version should work)
./target/release/blend_info --version
blend_info 0.1.0
./target/release/blend_info -V
blend_info 0.1.0
```

Let's do something wrong:

```sh
# the source code is *not* a Blender file
./target/release/blend_info src/main.rs
ERROR: Not a .blend file
First 12 bytes:
[117, 115, 101, 32, 115, 116, 100, 58, 58, 102, 115, 58]
# even an empty file shouldn't crash
touch empty
./target/release/blend_info empty
ERROR: Not a .blend file
First 12 bytes:
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

Finally run it on a real Blender scene:

```sh
# there is too much info
./target/release/blend_info ../blend/startup_file.blend
# output (first line is the header, last line reports bytes read)
BLENDER-v280
...
656112 bytes read
# but the bytes counted (and read) match
ls -lag ../blend/startup_file.blend
# output
-rw-r--r-- 1 jan 656112 May 17 18:43 ../blend/startup_file.blend
```

Now let's use **grep** to gather some information:

```sh
# how many objects are there? In Blender they get stored with prefix OB
./target/release/blend_info ../blend/startup_file.blend | grep '  "OB'
# three
  "OBCamera" has 1 data blocks
  "OBCube" has 3 data blocks
  "OBLight" has 1 data blocks
# camera data starts with CA
./target/release/blend_info ../blend/startup_file.blend | grep '  "CA'
# output
  "CACamera" has 0 data blocks
# light data starts with LA (former name was lamps)
./target/release/blend_info ../blend/startup_file.blend | grep '  "LA'
# output
  "LALight" has 3 data blocks
# what about meshes? They use the prefix ME
./target/release/blend_info ../blend/startup_file.blend | grep '  "ME'
  "MECube" has 11 data blocks
```

So the first goal will be to extract some mesh data from a Blender
file. Version 0.1.0 can not do that yet, but it helps figuring out
what we are looking for and where we can find the data:

```sh
./target/release/blend_info ../blend/startup_file.blend | grep "Mesh {" -A 49
  Mesh {
    ID id;
    AnimData *adt;
    BoundBox *bb;
    Ipo *ipo;
    Key *key;
    Material **mat;
    MSelect *mselect;
    MPoly *mpoly;
    MLoop *mloop;
    MLoopUV *mloopuv;
    MLoopCol *mloopcol;
    MFace *mface;
    MTFace *mtface;
    TFace *tface;
    MVert *mvert;
    MEdge *medge;
    MDeformVert *dvert;
    MCol *mcol;
    Mesh *texcomesh;
    BMEditMesh *edit_mesh;
    CustomData vdata;
    CustomData edata;
    CustomData fdata;
    CustomData pdata;
    CustomData ldata;
    int totvert;
    int totedge;
    int totface;
    int totselect;
    int totpoly;
    int totloop;
    int act_face;
    float loc[3];
    float size[3];
    float rot[3];
    short texflag;
    short flag;
    float smoothresh;
    char cd_flag;
    char _pad;
    char subdiv;
    char subdivr;
    char subsurftype;
    char editflag;
    short totcol;
    Multires *mr;
    void *_pad1;
    Mesh_Runtime runtime;
  }
```

So Blender's DNA information contains basically all the structs being
used on the C/C++ side of Blender's source code and allows to use the
information you can handle (and drop the one you can not). This is
pretty much how a **Mesh** would be stored in Blender 2.80 (and
probably earlier). For example **totvert** would tell us how many
vertices a mesh uses and they would be stored via a pointer to
**MVert** which would provide as many vertices in an array. So let's
find the DNA for **MVert**:

```sh
# how does MVert look like?
./target/release/blend_info ../blend/startup_file.blend | grep "MVert {" -A 5
  MVert {
    float co[3];
    short no[3];
    char flag;
    char bweight;
  }
```

Great, now let's go back and look at the data following the mesh
information for **MECube**:

```sh
./target/release/blend_info ../blend/startup_file.blend | grep '  "ME' -B 12
# we find a ME chunk of data with 1560 bytes including 11 data blocks
ME (1560)
  DATA[1] (SDNAnr = 0)
  DATA[2] (SDNAnr = 443)
  DATA[8] (SDNAnr = 68)
  DATA[1] (SDNAnr = 0)
  DATA[1] (SDNAnr = 443)
  DATA[12] (SDNAnr = 65)
  DATA[2] (SDNAnr = 443)
  DATA[24] (SDNAnr = 73)
  DATA[24] (SDNAnr = 71)
  DATA[1] (SDNAnr = 443)
  DATA[6] (SDNAnr = 70)
  "MECube" has 11 data blocks
```

So, we could guess from what we see within Blender that this mesh will
have 8 vertices, so **DATA[8]** might contain what we need to extract
those, but what does the **SDNAnr** tell us?

```sh
# blend_info conveniently prints SDNAnr in front of each struct ...
./.../blend_info ../blend/startup_file.blend | grep '\[SDNAnr = 68\]' -A 6
# ... so we can search and find it
  [SDNAnr = 68]
  MVert {
    float co[3];
    short no[3];
    char flag;
    char bweight;
  }
```

So, **DATA[6]** will most likely contain polygon data:

```sh
blend_info ../blend/startup_file.blend | grep '\[SDNAnr = 70\]' -A 7
# output
  [SDNAnr = 70]
  MPoly {
    int loopstart;
    int totloop;
    short mat_nr;
    char flag;
    char _pad;
  }
```

We can do that for every **SDNAnr** we come across, but for now, let's
just look at two more for the **DATA[24]** entries:

```sh
blend_info ../blend/startup_file.blend | grep '\[SDNAnr = 73\]' -A 4
# output
  [SDNAnr = 73]
  MLoopUV {
    float uv[2];
    int flag;
  }
# another search
blend_info ../blend/startup_file.blend | grep '\[SDNAnr = 71\]' -A 4
# output
  [SDNAnr = 71]
  MLoop {
    int v;
    int e;
  }
```

I'm not going to explain everything in detail. You have the **source
code** to study (both Blender and my Rust examples). Just to show you
that there are **different versions** with different **SDNAnr**
numbers:

```sh
# try another Blender file
blend_info ~/git/github/export_multi/.../cornell_box.blend | grep BLENDER
# different Blender version
BLENDER-v279
# lets find a mesh
blend_info cornell_box.blend | grep MEcornell_box
# output
  "MEcornell_box" has 12 data blocks
# show the data blocks with number of entries and SDNAnr
blend_info cornell_box.blend | grep MEcornell_box -B 13
# output
ME (1416)
  DATA[1] (SDNAnr = 9)
  DATA[1] (SDNAnr = 9)
  DATA[1] (SDNAnr = 0)
  DATA[1] (SDNAnr = 0)
  DATA[1] (SDNAnr = 473)
  DATA[8] (SDNAnr = 63)
  DATA[1] (SDNAnr = 473)
  DATA[12] (SDNAnr = 60)
  DATA[1] (SDNAnr = 473)
  DATA[20] (SDNAnr = 66)
  DATA[1] (SDNAnr = 473)
  DATA[5] (SDNAnr = 65)
  "MEcornell_box" has 12 data blocks
# MVert has a different SDNAnr
blend_info cornell_box.blend | grep '\[SDNAnr = 63\]' -A 6
# output
  [SDNAnr = 63]
  MVert {
    float co[3];
    short no[3];
    char flag;
    char bweight;
  }
```

Finally, if you don't like to study C/C++ or Rust code you can examine
Blender files with your favourite text editor or **hexdump**:

```sh
hexdump -C cornell_box.blend | head
00000000  42 4c 45 4e 44 45 52 2d  76 32 37 39 52 45 4e 44  |BLENDER-v279REND|
00000010  48 00 00 00 90 20 db 58  fe 7f 00 00 00 00 00 00  |H.... .X........|
00000020  01 00 00 00 01 00 00 00  fa 00 00 00 53 63 65 6e  |............Scen|
00000030  65 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |e...............|
00000040  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000060  00 00 00 00 00 00 00 00  00 00 00 00 54 45 53 54  |............TEST|
00000070  08 00 01 00 48 04 55 99  e8 7f 00 00 00 00 00 00  |....H.U.........|
00000080  01 00 00 00 80 00 00 00  80 00 00 00 00 00 00 60  |...............`|
00000090  00 00 00 60 00 00 00 60  00 00 00 60 00 00 00 60  |...`...`...`...`|
# or in Emacs using hexl-mode (startup_file.blend)
File Edit Options Buffers Tools Hexl Help
87654321  0011 2233 4455 6677 8899 aabb ccdd eeff  0123456789abcdef
00000000: 424c 454e 4445 522d 7632 3830 5245 4e44  BLENDER-v280REND
00000010: 4800 0000 504c 734e ff7f 0000 0000 0000  H...PLsN........
00000020: 0100 0000 0100 0000 fa00 0000 5363 656e  ............Scen
00000030: 6500 0000 0000 0000 0000 0000 0000 0000  e...............
```

Have fun exploring the DNA of Blender and stay tuned for the upcoming
version(s) of **blend_info** ...

[rust]:            https://www.rust-lang.org
[rs_pbrt]:         https://www.rs-pbrt.org/about
[blender]:         https://www.blender.org
[python]:          https://www.python.org
[multi_exporter]:  https://github.com/wahn/export_multi
[blender_add_ons]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[radiance]:        https://floyd.lbl.gov/radiance
[bconf]:           https://www.janwalter.org/jekyll/video/blender/conference/2014/10/27/blender-conference-2014-talk.html
[blender_2_8]:     https://www.blender.org/2-8
[luxcorerender]:   https://luxcorerender.org
[blendluxcore]:    https://github.com/LuxCoreRender/BlendLuxCore
[appleseed]:       https://appleseedhq.net
[blenderseed]:     https://github.com/appleseedhq/blenderseed
[renderman]:       https://renderman.pixar.com
[prmanforblender]: https://github.com/prman-pixar/RenderManForBlender
[arnold]:          https://www.arnoldrenderer.com
[btoa]:            https://github.com/tyler-furby/Arnold-For-Blender
[indigo]:          https://www.indigorenderer.com
[blendigo]:        https://www.indigorenderer.com/documentation/manual/indigo-blender/installation
[maxwell]:         https://www.nextlimit.com/maxwell
[bmaxwell]:        https://sourceforge.net/projects/bmaxwell
[octane]:          https://home.otoy.com/render/octane-render
[octaneforum]:     https://render.otoy.com/forum
[pbrt]:            https://www.pbrt.org
[pbrt_v3_iile]:    https://github.com/giuliojiang/pbrt-v3-IILE
[rs_blender]:      https://codeberg.org/wahn/rs_blender
[codeberg]:        https://codeberg.org
[install]:         https://www.rust-lang.org/tools/install
[rustup]:          https://github.com/rust-lang-nursery/rustup.rs
