---
layout: post
title:  "[libblendinfo] Return information from Rust crate to C library"
date:   2022-05-10 11:00:00
categories: jekyll rust libblendinfo
---

# libblendinfo

**[Important Update!]** - scroll the the end ...

Can the **Rust** crate [blend_info][blend_info] be wrapped in a way
that it can be used from within **C**?

This question I asked to myself (see [ticket][ticket] on
sourcehut). There you can find links to another Rust crate and a blog
post describing the experience of [Exposing a Rust library to
C][greyblake].

## Keep FFI separate from Rust crate

One decision I made after reading the blog post, was that I wanted to
keep the Rust crate separate from it's (example) usage from within
C/C++. And sourcehut supports the work on a common [project][project],
which keeps certain aspects separate in it's own git [repositories][repositories]:

1. Rust crate: [https://git.sr.ht/~wahn/blend_info][git_blend_info]
2. C bindings: [https://git.sr.ht/~wahn/libblendinfo][libblendinfo]

## A Start

As a start I created a simple C example file (called **camera.c**):

```C
#include "blendinfo.h"

int main(void) {
  char* filename = "blend/factory_v279.blend";
  char* camera = "Camera";
  extract_struct(filename, camera);
}
```

So, you provide two strings, the name of the Blender file to read, and
the kind of information you are interested in (in this case cameras).

The header file (**blendinfo.h**) contains a single function prototype (in C):

```C
void extract_struct(char* filename, char* structname);
```

That same C function looks like this on the **Rust** side:

```Rust
#[no_mangle]
pub unsafe extern "C" fn extract_struct(fn_ptr: *const c_char,
                                        sn_ptr: *const c_char) {
...
    let read_dna_result = read_dna(...)
...
    let use_dna_result = use_dna(...)
...
}
```

## What's missing?

One thing which is obviously missing is the return of those bytes
being relevant to cameras to the caller (in C). Once that's solved the
C/C++ caller could simply create a struct like the one returned from
the standalone `blend_info` executable (written in Rust and using the
`blend_info` crate). See [official documentation][blend_info_docs]:

```
$ ./target/release/blend_info -n Camera blend/factory_v279.blend
Camera 248
struct Camera { // SDNAnr = 25
    ID id; // 120
    AnimData *adt; // 8
    char type; // 1
    char dtx; // 1
    short flag; // 2
    float passepartalpha; // 4
    float clipsta; // 4
    float clipend; // 4
    float lens; // 4
    float ortho_scale; // 4
    float drawsize; // 4
    float sensor_x; // 4
    float sensor_y; // 4
    float shiftx; // 4
    float shifty; // 4
    float YF_dofdist; // 4
    Ipo *ipo; // 8
    Object *dof_ob; // 8
    GPUDOFSettings gpu_dof; // 24
    char sensor_fit; // 1
    char pad[7]; // 7
    CameraStereoSettings stereo; // 24
}; // 248
```

If that return value is a memory block with exactly the same order of
bytes as needed (with proper [endianness][endianness]) the number of
bytes being returned should be a multiple of 248 bytes (but only for
this particular Blender version) and in C you should be able to just
use a pointer to step through those bytes and interpret them e.g. to
read the clipping start float value (`clipsta`) and do whatever you
need to do on the C/C++ side.

## Help

Why do I publish the current state, instead of waiting until the C/C++
library can be used or coming up with a more complicated example?

In my opinion the Rust crate already allows you to extract any
information you need from a Blender file (using the DNA
information). But one can not ignore the developers using C/C++ and
centuries of library development in those languages. This is your
chance to have a say, how the wrapper of the Rust code will look like
and how you wish to use it. The [rs-pbrt][rs-pbrt] crate/executable is
a full renderer written in Rust, with **Ambient Occlusion** (AO),
**Direct Lighting** (no Global Illumination), **Whitted’s
ray-tracing** algorithm, (Uni-directional) **Path Tracing** (Global
Illumination), **Bidirectional Path Tracing**, **Metropolis Light
Transport** (MLT), and **Stochastic Progressive Photon Mapping**
(SPPM),) which also accounts for attenuation from participating media
as well as scattering from surfaces. All of this is based on the
**third edition** of the [PBRT book][pbrt_book]. The C++ code for the
forth edition is alreay out there on GitHub, the book is not available
(yet). Anyway, the Rust code comes with an example file
`parse_blend_file`, which can render Blender files directly (instead
of using `.pbrt` files for version 3 of the file format described in
the book). This is using already the [blend_info][blend_info] Rust
crate to extract information directly from `.blend` files and is an
example usage for such a Rust integration. Once the C library is
finished there is no reason you could not use the extracted bytes for
other things, like an **OpenGL** or **Vulkan** based scene viewer
etc. ...

# Update

In the meantime I got some feedback on
[Reddit](https://www.reddit.com/r/rust/comments/uxm6e8/how_to_return_bytes_from_a_rust_crate_to_cc)
and the current state is:

From C (see example folder) you can ask e.g. for the bytes of a
**Camera** struct, Rust will deal with reading the `.blend` file and
returns a bunch of bytes (in a way that you can query the length of
the returned vector of u8 values). The standalone `blend_info` program
can be used to create a **C struct** in a header file:

[https://git.sr.ht/~wahn/blend_info](https://git.sr.ht/~wahn/blend_info)

Another **header file** (for the C bindings) can be created via the
Rust crate **safer-ffi**:

[https://crates.io/crates/safer-ffi](https://crates.io/crates/safer-ffi)

Have a look at the example code (*camera.c*):

```c
#include <stdio.h>
#include "blendinfo.h"
#include "camera_v279.h"

int main(void) {
  char* filename = "blend/factory_v279.blend";
  char* camera = "Camera";
  // call Rust function
  Vec_uint8_t bytes_read = extract_struct(filename, camera);
  // print returned bytes information
  printf("########################################\n");
  printf("%d bytes received from Rust\n", bytes_read.len);
  int i;
  for (i = 0; i < bytes_read.len; i++) {
    uint8_t byte_read = bytes_read.ptr[i];
    if (byte_read) {
      printf("bytes_read[%d] = %d\n", i, byte_read);
    }
  }
  // blend_info -n Camera blend/factory_v279.blend > examples/camera_v279.h
  // 1. replace unknown types/structs by char some_name[byte_count];
  // 2. replace pointers by void* pointers
  // 3. cast pointer to struct defined in header
  struct Camera_v279* cam_v279 = (struct Camera_v279*) bytes_read.ptr;
  // print some float values from Camera struct (for Blender v2.79)
  printf("########################################\n");
  printf("Camera.clipsta = %f\n", cam_v279->clipsta);
  printf("Camera.clipend = %f\n", cam_v279->clipend);
  printf("Camera.lens = %f\n", cam_v279->lens);
}
```

Happy to discuss the next steps in case anybody is interested ...

To contact me or discuss things please use the
[mailing-lists][mailing-lists]. You might have to learn how to use
[plaintext][plaintext] email to participate, but that's a small thing
to ask.

[blend_info]:      https://crates.io/crates/blend_info
[ticket]:          https://todo.sr.ht/~wahn/rs-blender/6
[greyblake]:       https://www.greyblake.com/blog/exposing-rust-library-to-c
[project]:         https://sr.ht/~wahn/rs-blender
[repositories]:    https://sr.ht/~wahn/rs-blender/sources
[git_blend_info]:  https://git.sr.ht/~wahn/blend_info
[libblendinfo]:    https://git.sr.ht/~wahn/libblendinfo
[blend_info_docs]: https://docs.rs/blend_info/0.2.8/blend_info
[endianness]:      https://en.wikipedia.org/wiki/Endianness
[rs-pbrt]:         https://www.rs-pbrt.org/about
[pbrt_book]:       https://www.pbrt.org
[mailing-lists]:   https://sr.ht/~wahn/rs-blender/lists
[plaintext]:       https://useplaintext.email
