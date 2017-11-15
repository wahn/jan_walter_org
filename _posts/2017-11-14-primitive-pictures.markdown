---
layout: post
title:  "Primitive Pictures"
date:   2017-11-14 13:00:00
categories: jekyll rendering art
---

## Input

Take an input image like this:

<img src="/assets/invplace_luxrender_augAM_st.png" alt="Input for the
primitive program." width="740" class="img-thumbnail"/>

Create 100 __rotated ellipses__ and store every 5th frame as a
[Portable Network Graphics][png] (PNG) file:

```shell
./primitive -i invplace.png -o rotatedellipse_%03d.png -nth 5 -n 100 -m 7 -v
```

## Animated GIFs

Use those PNG files to create an animated GIF ([Graphics Interchange Format][gif]):

<img src="/assets/invplace_rotatedellipse.gif" alt="Using a hundred
rotated elipses." width="740" class="img-thumbnail"/>

```shell
convert -delay 1 -loop 0 rotatedellipse_???.png invplace_rotatedellipse.gif
```

Or use another mode (e.g. __polygon__):

<img src="/assets/invplace_polygon.gif" alt="Using a hundred
polygons." width="740" class="img-thumbnail"/>

## Use more primitives

<img src="/assets/invplace_rotatedellipse_1000.png" alt="Using a
thousand rotated elipses." width="740" class="img-thumbnail"/>

<img src="/assets/invplace_polygon_1000.png" alt="Using a thousand
polygons." width="740" class="img-thumbnail"/>

## Software used

You can find the source code for the ```primitive``` executable
(written in [Go][go]) on [GitHub][github]. There is also a version for
[macOS][macos]. The ```convert``` executable is part of
[ImageMagick][imagemagick].

Have fun with your own images and all the options:

```shell
./primitive -h
Usage of ./primitive:
  -a int
    	alpha value (default 128)
  -bg string
    	background color (hex)
  -i string
    	input image path
  -j int
    	number of parallel workers (default uses all cores)
  -m int
    	0=combo 1=triangle 2=rect 3=ellipse 4=circle 5=rotatedrect 6=beziers 7=rotatedellipse 8=polygon (default 1)
  -n value
    	number of primitives
  -nth int
    	save every Nth frame (put "%d" in path) (default 1)
  -o value
    	output image path
  -r int
    	resize large input images to this size (default 256)
  -rep int
    	add N extra shapes per iteration with reduced search
  -s int
    	output image size (default 1024)
  -v	verbose
  -vv
    	very verbose
```

[png]:         https://en.wikipedia.org/wiki/Portable_Network_Graphics
[gif]:         https://en.wikipedia.org/wiki/GIF
[go]:          https://en.wikipedia.org/wiki/Go_(programming_language)
[github]:      https://github.com/fogleman/primitive
[macos]:       https://primitive.lol
[imagemagick]: http://www.imagemagick.org
