---
title: "Introduction to Blue Waters"
teaching: 30
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

## Foreword

Computational Science (not to be confused with Computer Science) emerged in
the twentieth century as a new approach to studying the world around us. It
utilizes computers to tackle research, engineering, and other problems that
are impossible to solve under the domes of its experimental and theoretical
sisters. Computational Science brings together theoretical algorithms,
experimental data, and advances in engineering and, therefore, is a
multidisciplinary field by design.

People quickly realized that more powerful computers not only allow them to
solve more problems, but enable them to tackle more complex problems
based on more realistic theoretical models and that account for more
experimental observations. And this is why people started thinking about
building large computers which they then called "Supercomputers".


## Blue Waters

So, what is Blue Waters? Blue Waters is a supercomputer and, in fact, one
of the most powerful ones in the world. A few facts:

 - it is a **petascale** computing system (peta = 10<sup>15</sup>)
 - it is located on the premises of the **University of Illinois at Urbana-Champaign**
 - it is funded by the **National Science Foundation**
 - it is the largest system ever built by [**Cray Inc.**][cray]
 - it is housed in a dedicated building called **National Petascale Computing Facility**
 - it is capable of delivering more than 10<sup>15</sup> (Quadrillion in the
   short scale or Billiard in the long scale) calculations each
   second on a sustained basis and more than 13 quadrillion calculations at its peak performance.
 - its website is located at: **<{{ site.bwportal }}>**

If you'd like to learn more about the system, you can watch the videos below
at your leisure.

**About Blue Waters**

<iframe width="610" height="359" src="https://www.youtube.com/embed/iyLD55PzXWs"
        frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

**Blue Waters launch day**

<iframe width="610" height="343" src="https://www.youtube.com/embed/r5eTe5sp-TA"
        frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


> ## Sustained Petascale
>
> The primary goal of Blue Waters is to support computational science projects that require a lot
> of computational power and that are not possible anywhere else. Besides the scale of the projects,
> Blue Waters' goal is to support a wide range of petascale science applications
> on a **sustained** basis. This is where the term "Sustained Petascale" comes from.
> It turns out that this decision affects how the system is operated. An immediate
> result is that Blue Waters does not participate in the famous list called [TOP500][top500].
> This is a list of the 500 supercomputers with the fastest peak *computing speed*.
> The reason is simple: computing speed is a single characteristic of a supercomputer that,
> despite its importance, affects only specific types of applications. Getting the top score
> in this list is, therefore, not a priority for a system that set the goal to provide sustained
> performance across all characteristics.
{: .callout}


## Architecture and hardware

Blue Waters is a Cray system that has:

 - [Nodes]({{ page.root }}/reference/#node) _with_ and _without_ GPUs
   - 22,636 "XE" nodes
      - 2 AMD Interlagos processors
      - 64 GB of RAM
   - 4,228 "XK" nodes
      - 1 AMD Interlagos processor
      - 32 GB of RAM
      - 1 NVIDIA GK110 (K20X) "Kepler" with
        - 6 GB of memory
        - 14 Streaming Multiprocessors (SMX) each providing
          - 192 single-precision cores (2,688 total)
          - 64 double-precision units (896 total)
          - 32 special function units (448 total)
          - 32 load/store units
 - All nodes are connected by the Cray Gemini 3D torus interconnect
 - Total disk space (called "online" storage): 26.4 PB (Peta bytes)
 - 250+ PB of "near-line" (to distinguish from "online" and "offline") storage space
   that uses tapes for long-term storage.

Each AMD Interlagos processor on Blue Waters has 8 AMD Bulldozer modules each
housing a floating point unit and two integer scheduling units.

Don't try to memorize the above numbers. We list them here so you can quickly
find them in case you need them. However, you can find more detailed
information at <{{ site.bwportal }}/hardware-summary>

Now, let's get started with the main part of the lesson and see how to work on the Blue Waters system!

[top500]: https://www.top500.org/lists/top500/
[cray]: https://www.cray.com
