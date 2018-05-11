---
title: "Introduction to Blue Waters"
teaching: 0
exercises: 0
questions:
- "Blue Waters: What? Where? and Why?"
objectives:
- "Learn about Blue Waters' history abd hardware"
- "Understand why Blue Waters does not participate in TOP-500"
- "Learn basic facts about the system"
- "Understand system's goals, scale, and capabilities"
keypoints:
- "Blue Waters enables Petascale Science not possible anywhere else"
- "Key emphasis is on *sustained* petascale computing"
---

#### Introduction

Blue Waters is one of the most powerful supercomputers in the world. A few facts about the system:

 - it is a **petascale** computing system
 - it is located on the premises of the **University of Illinois at Urbana-Champaign**
 - it is funded by the **National Science Foundation**
 - it is the largest system ever built by [**Cray Inc.**][cray]
 - it is housed in a dedicated building called **National Petascale Computing Facility**
 - it is capable of delivering more than 10<sup>15</sup> (quadrillion) calculations per
   second on a sustained basis and more than 13 quadrillion calculations at its peak performance.
 - its website is located at: **<{{ site.bwportal }}>**

At this point, we don't want to bother you with the in-depth details about the system.
If you are interested, you can watch the videos below:

**About Blue Waters**

<iframe width="610" height="359" src="https://www.youtube.com/embed/iyLD55PzXWs"
        frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

**Blue Waters launch day**

<iframe width="610" height="343" src="https://www.youtube.com/embed/r5eTe5sp-TA"
        frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


#### Sustained Petascale

The main goal of Blue Waters is to support computational science projects that require a lot
of computational power and that are not possible anywhere else. Besides the scale of the projects,
Blue Waters' goal is to support a wide range of petascale science applications
on a **sustained** basis. This is where the term "Sustained Petascale" comes from.
It turns out that this decision affects how the system is operated. An immediate
result is that Blue Waters does not participate in the famous list called [TOP500][top500].
This is a list of the 500 supercomputers with the fastest peak *computing speed*.
The reason is simple: computing speed is a single characteristic of a supercomputer that,
despite its importance, affects only specific types of applications. Getting the top score
in this list is, therefore, not a priority for the Blue Waters system.

#### A few basic facts

This section would be incomplete without at least some basic information about
the system itself. So, here it is:

 - The system has nodes with and without GPUs
 - 22,636 nodes without GPUs are called XE and have
    - 32 AMD Bulldozer cores
    - 64 GB of RAM (some have 128 GB)
 -  4,228 nodes with GPUs are called XK and have
    - 16 AMD Bulldozer cores
    - 32 GB of RAM (some have 64 GB)
    - one NVIDIA GK110 (K20X) "Kepler" with
      - 2,688 cores
      - 6 GB of memory
 - All nodes are connected by the Cray Gemini torus interconnect
 - Total disk space (called "online" storage): 26.4 PB (Peta bytes)
 - There is also 250+ PB of "near-line" (to distinguish from "online" and "offline") storage space
   that uses tapes for long-term storage.

But please, don't try to memorize these numbers. We list them here so you can quickly find them in case you need them.

*More detailed information*: <{{ site. bwportal }}/hardware-summary>

Now, let's get started with the main part of the lesson and see how to work on the Blue Waters system!

[top500]: https://www.top500.org/lists/top500/
[cray]: https://www.cray.com
