---
date: 2026-06-17
title: "Lluvia port to Rust!"
linkTitle: "Lluvia port to Rust!"
description: "Lluvia port to Rust!"
author: Juan Adarve
resources:
- src: "**.{png,jpg,svg}"
  title: "Image #:counter"
---

It has been almost 3 years since the last blog post of the project. A lot of things have happened since then, both in my personal and professional life :smiley:. In particular to the project, it reached a stall point. The C++ core library is stable and the node library provides several complext algorithms (particularly in optical flow) that showcase the engine capabilities. Integration with Mediapipe proved the idea of using Lluvia on Mobile Android devices and compilation on Raspberry PI showed it can be used on embedded devices as well.

All that said, I lost rythm on the project, and could not find a way to use it in some productive. It was a good way to keep my C++ skills sharp, and certainly it helped a lot. Now, I moved my professional work to Rust :crab:. Nice language, once you get used to the borrow-check.

Given that, and as an incentive to improve my Rust, I decided to rewrite the project from scratch in Rust. The new [`lluvia-rs` repository](https://github.com/jadarve/lluvia-rs) in Github is organized as a Cargo workspace that will combine several crates:

* `lluvia_vk`: The same engine as Lluvia C++, using Vulkan API via the [`vulkano` crate](https://github.com/vulkano-rs/vulkano). Here I want to get access to all Vulkan capabilities, including the video encode/decode extensions for creating high-performance native apps.
* `lluvia_webgpu`: A port using WebGPU API via the [`wgpu` crate](https://github.com/gfx-rs/wgpu). This will allow running on Web browsers, something new for me.
* `lluvia_media`: A mew idea for creating parsers for several media formats (e.g. MPEG Transport Stream, MP4, etc), something that might come handy at work.
* `bindings/python/pyo3-lluvia-vk`: `Pyo3` wrappers to use the crate from Python. This is a key component to improve the development experience. 

Currently I'm focused on the first one, `lluvia_vk`. This are some technical choises:

* Use [Roblox's Luau](https://luau.org/) as the scripting language to describe the compute nodes. It's like Lua, but with types! That helps a lot when developing new nodes and allows IDEs to provide better autocompletion.
* I'm exploring using [Slang shading language](https://shader-slang.org/) to code the GPU shaders. This way, I can reuse the same shader code in `lluvia_vk` and in `lluvia_webgpu`, in theory.

In addition to support compute shaders and their composition in complex pipelines, I want to also support rendering pipelines. Here I have two motivations. First, an old dream of mine about developing my own video game, and secondly, I want to learn about Gaussian Splatting techniques, from scratch, the hard way :unamused:.

{{< alert title="C++ support for the project:" >}}
For now, I have no plans on extending the current C++ implementation of the project. If you are using it in an actual production environment, I would love to know more about it, and maybe we can look into adding new funtionalities.
{{< /alert >}}

I still need to stabilize the first version of the Rust API and port the nodes from the C++ project to Slang and Luau before I feel confortable releasing it on [crates.io](https://crates.io/).