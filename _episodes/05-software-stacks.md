---
title: "Software Stacks"
teaching: 30
exercises: 0
questions:
- "What software is available on Blue Waters?"
objectives:
- "Learn how to use module environment"
- "Learn about four available Programming Environments"
- "Underhow to use Python on Blue Waters"
keypoints:
- "Software on Blue Waters lives in \"modules\""
- "\"Install\" and \"uninstall\" software with `module load` / `module unload`"
- "There are 4 types of Programming Environments on Blue Waters: Cray, Gnu, Intel, and PGI"
- "Python and its derivatives (Tensorflow, PyTorch) live in \"bwpy\" module and its sister modules."
---

## Modules

To help its users take full advantage of the hardware capabilities, Blue Waters provides
**modules**: pre-compiled software packages that are optimized for the Blue Waters' hardware.  To
get started with modules, let's execute:

~~~
$ module list
~~~
{: .language-bash}

~~~
Currently Loaded Modulefiles:
  1) modules/3.2.10.4                      18) cray-mpich/7.5.0
  2) eswrap/1.3.3-1.020200.1280.0          19) craype-interlagos
  3) cce/8.4.6                             20) torque/6.0.4
  4) craype-network-gemini                 21) moab/9.1.2-sles11
  5) craype/2.5.8                          22) java/jdk1.8.0_51
  6) cray-libsci/16.11.1                   23) globus/5.2.5
  7) udreg/2.3.2-1.0502.10518.2.17.gem     24) gsissh/6.2p2
  8) ugni/6.0-1.0502.10863.8.28.gem        25) xalt/0.7.6.local
  9) pmi/5.0.10-1.0000.11050.179.3.gem     26) scripts
 10) dmapp/7.0.1-1.0502.11080.8.74.gem     27) OpenSSL/1.0.2m
 11) gni-headers/4.0-1.0502.10859.7.8.gem  28) cURL/7.59.0
 12) xpmem/0.1-2.0502.64982.5.3.gem        29) git/2.17.0
 13) dvs/2.5_0.9.0-1.0502.2188.1.113.gem   30) wget/1.19.4
 14) alps/5.2.4-2.0502.9774.31.12.gem      31) user-paths
 15) rca/1.0.0-2.0502.60530.1.63.gem       32) gnuplot/5.0.5
 16) atp/2.0.4                             33) darshan/3.1.3
 17) PrgEnv-cray/5.2.82
~~~
{: .output}

`module list` command lists modules that are currently loaded into your environment.

To see what modules are available, execute:
~~~
$ module avail
~~~
{: .language-bash}
You should get a long list of available modules. Of course, if you know the name of the module, you
can narrow down your search with either
~~~
$ module avail py
~~~
{: .language-bash}

This will show all modules that start with `py`. To look for modules that have `py` in their name,
use the `-S` flag:
~~~
$ module avail -S py
~~~
{: .language-bash}

To demonstrate the effect of adding a module to your environment,
let's check the version of the GNU C compiler:

~~~
$ gcc --version
~~~
{: .language-bash}
~~~
gcc (SUSE Linux) 4.3.4 [gcc-4_3-branch revision 152973]
Copyright (C) 2008 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
~~~
{: .output}
This is the version of `gcc` that comes with the operating system installed on the login nodes.
Let's now load a module called `gcc/6.3.0`:
~~~
$ module load gcc/6.3.0
~~~
{: .language-bash}
If everything was successful, you should not see any message.
Now, if we check the version of `gcc` again, we get:
~~~
$ gcc --version
~~~
{: .language-bash}
~~~
gcc (GCC) 6.3.0 20161221 (Cray Inc.)
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
~~~
{: .output}
Voila! Essentially, we installed a new version of `gcc` just by executing the `module load` command!

## Programming Environments

Let's now try to load a module called `PrgEnv-gnu`:
~~~
$ module load PrgEnv-gnu
~~~
{: .language-bash}
~~~
PrgEnv-gnu/5.2.82(10):ERROR:150: Module 'PrgEnv-gnu/5.2.82' conflicts with the currently loaded module(s) 'PrgEnv-cray/5.2.82'
PrgEnv-gnu/5.2.82(10):ERROR:102: Tcl command execution failed: conflict PrgEnv-cray
~~~
{: .output}
It failed! The reason it failed is simple: changes that are introduced by `PrgEnv-gnu` module
conflict with those introduced by the `PrgEnv-cray` module, which is currently loaded.
These modules are, in fact, called **`Programming Environment`** modules.
The reason they are called so is simple: they make a lot of changes to your environment and,
therefore, can not be combined.

One might ask: is there a way to know what changes a particular module does to the environment?
For that purpose we can use `module show` command:

~~~
$ module show PrgEnv-gnu
~~~
{: .language-bash}
~~~
-------------------------------------------------------------------
/opt/cray/modulefiles/PrgEnv-gnu/5.2.82:

conflict	 PrgEnv
conflict	 PrgEnv-x1
conflict	 PrgEnv-x2
conflict	 PrgEnv-cray
conflict	 PrgEnv-intel
conflict	 PrgEnv-pgi
conflict	 PrgEnv-pathscale
setenv		 PE_ENV GNU
setenv		 XTOS_VERSION 5.2.82
setenv		 CRAYOS_VERSION 5.2.82
prepend-path	 PE_PRODUCT_LIST GNU:GCC
module		 load gcc/6.3.0
module		 swap craype/2.5.8
module		 load cray-libsci
module		 load udreg/2.3.2-1.0502.10518.2.17.gem
module		 load ugni/6.0-1.0502.10863.8.28.gem
module		 load pmi
module		 load dmapp/7.0.1-1.0502.11080.8.74.gem
module		 load gni-headers/4.0-1.0502.10859.7.8.gem
module		 load xpmem/0.1-2.0502.64982.5.3.gem
module		 load dvs/2.5_0.9.0-1.0502.2188.1.113.gem
module		 load alps/5.2.4-2.0502.9774.31.12.gem
module		 load rca/1.0.0-2.0502.60530.1.63.gem
module		 load atp
setenv		 CRAY_PRGENVGNU loaded
-------------------------------------------------------------------
~~~
{: .output}
Here you can see that this module conflicts with `PrgEnv-cray`, `PrgEnv-intel`, and `PrgEnv-pgi`.
Therefore, to load module `PrgEnv-gnu` into the system, we have to swap it with the `PrgEnv-cray`:

~~~
$ module swap PrgEnv-cray PrgEnv-gnu
~~~
{: .language-bash}
The order of module in this command matters: first, you list the module you want to be gone; next,
you list the module you want to be added to your environment.

## Influential modules

## Python = `bwpy`

Python is a very popular language in scientific computing due to its simplicity and versatility.
However, from a standpoint of a large supercomputer, its modularity presents a challenge because
it leads to a very large number of small files and, as a result, slow start-up times.
To solve this problem, Blue Waters team prepared a special module called `bwpy`.
This module installs an optimized version of Python with over 300 of most-popular modules,
such as Tensorflow, Pandas, and other.

We will talk about the Python module in the next episode.
However, you can consult with
[https://bluewaters.ncsa.illinois.edu/python](https://bluewaters.ncsa.illinois.edu/python)
for more information about Python on Blue Waters.


## Profiling Python code on Blue Waters

If you are interested in optimizing your Python code for Blue Waters,
have a look at the
[Python Profiling](https://bluewaters.ncsa.illinois.edu/python-profiling)
page on the Blue Waters portal.


## Python + MPI = `bwpy-mpi`

To take advantage of the fast Blue Waters interconnect from within Python, you have to load
the `bwpy-mpi` module. This module, however, was designed to be used on MOM nodes and issue a
warning if you try to use it on the login nodes:
~~~
$ module load bwpy
$ module load bwpy-mpi
~~~
{: .language-bash}
~~~
Warning: BWPY-MPI may break on a login node!
~~~
{: .output}

## Jupyter Notebooks == Modern Science ???

Well, for now, we will just say that:

1. this is possible
2. the procedure is documented [here](https://bluewaters.ncsa.illinois.edu/pythonnotebooks)

However, if you have any questions or problems - please let us know at
[help+bw@ncsa.illinois.edu]({{ site.bwhelpemail }}).


## Using GPUs

GPUs provide enormous computational power.
If your application requires or is capable of taking advantage of NVIDIA GPUs, you should be aware
of the following three modules:

 - `cudatoolkit`
 - `craype-accel-nvidia35` and `PrgEnv-pgi`

The first module is required for any application that uses GPUs and the latter two are required for
applications that use OpenACC. At the moment, the latest version of `cudatoolkit` installed on Blue
Waters is version 9.1 though it might change in the future.  `craype-accel-nvidia35` module, on the
other hand, is specific to the NVIDIA GPUs installed on Blue Waters.

~~~
$ module avail cuda
~~~
{: .language-bash}

~~~
------------------------ /opt/cray/modulefiles -------------------------
cudatoolkit/5.5.51-1.0502.9594.1.1
cudatoolkit/5.5.51-1.0502.9594.3.1
cudatoolkit/6.5.14-1.0502.9613.6.
cudatoolkit/6.5.14-1.0502.9836.8.1
cudatoolkit/7.0.28-1.0502.10280.4.1
cudatoolkit/7.0.28-1.0502.10742.5.1
cudatoolkit/7.5.18-1.0502.10743.2.1
cudatoolkit/9.1.85_3.10-1.0502.df1cc54.3.1(default)
~~~
{: .output}


## Compiling your own applications

If you have to compile your own application from source,
there are a few general rules that you have to follow:

1. Replace all calls to `gcc` and `icc` with `cc`
2. Do not use `mpicc`! Use `cc`.

Once your application is compiled, you can proceed with sending it for
execution to the Blue Waters job queue.

> ## Updating your Makefile for Blue Waters
>
> Given the Makefile below, update it to use on Blue Waters.
>
> ~~~
> $ cat Makefile
> ~~~
> {: .language-bash}
>
> ~~~
> # This is a Makefile that was created on a regular computer
> # Modify it to make it work on Blue Waters
> # IMPORTANT: Makefiles use Tabs, not spaces
>
> CC = gcc
>
> .DEFAULT_GOAL := simple_mpi.exe
> .PHONY: clean all
>
> simple_mpi.exe: src/simple_mpi.c
> 	$(CC) -o $@ $^
>
> clean:
> 	-@rm -f simple_mpi.exe
> ~~~
> {: .output}
>
{: .challenge}

