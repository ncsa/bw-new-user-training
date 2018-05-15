---
title: "Blue Waters' Filesystems"
teaching: 30
exercises: 0
questions:
- "Where can I store my programs, simulation results, large files to share with team members?"
- "Where can I offload data to share it with my outside collaborators?"
objectives:
- "Understand the purpose of Blue Waters' filesystems."
- "Know how to check usage"
keypoints:
- "There are 3 online filesystems"
---

## Home directory

When you log in to Blue Waters, you arrive at your home directory.
Execute:
~~~
$ pwd
~~~
{: .language-bash}

~~~
/u/sciteam/username
~~~
{: .output}

This is a path to your home directory.
You should use it for things like storing your source code, compiling programs, etc.
It has a relatively modest quota of **1 TB** and **5 M (million)** files.
You can exceed this quota for the duration of 7 days, at which point you will be required to
remove files to continue using Blue Waters.
There is, however a hard limit of **1.5 TB** and **10 M** files that you will not be allowed
to exceed.
Note, that this directory is backed up by Blue Waters on a daily basis and if you accidentally
delete a file or a directory - email us at [help+bw@ncsa.illinois.edu]({{ site.bwhelpemail }}).

## Projects' space

Another directory that you have been given access to is the so-called projects directory.
The purpose of this directory is to share large files between members of the group.
The actual path to this directory depends on the **PSN** (Project's Short Name) of your project.
To find out PSN of your project, execute:

~~~
$ projects
~~~
{: .language-bash}
~~~
Username: username
Primary Project
-------------
xyz | Project description
-------------
Available Projects
-------------
xyz | Project description
~~~
{: .output}

Here, we can see that our PSN is: `xyz`. Therefore, our project directory is located at:
`/projects/sciteam/xyz`.
Projects directory has a quota of **5 TB** and **25 M** files,
a hard limit of **5.5 TB**, no hard limit on the number of files, and, as before
a 7-day grace period.
Unlike home directories, the quota and a hard limit that apply to this directory
are shared by all members of the project.
Just like home directories, your project folder is backed up on a daily basis.

## Scratch  space

Finally, you can also access the so-called **scratch** directory.
It is located under `/scratch/sciteam/username`, has a quota of **500 TB** and **50 M** files,
a hard limit of **550 TB**, no hard limit on the number of files, and a 7-day grace period.
Like your home directory, only you have access to it.
Like the scratch directory, the quota and a hard limit are shared by all group members.

**Scratch space is not backed up!**

Because of the much higher limits, scratch directory is usually the place from where
users submit jobs and store the immediate results of their simulations or analyses.
There is, however, a **30-day purging policy**: all files not modified within the last 30 days
are purged automatically. Therefore, make sure to move important data from your scratch directory
as soon as possible.


### `quota` command

To check how much space you are using in all three filesystems, use the `quota` command:

~~~
$ quota
~~~
{: .language-bash}

~~~
Lustre Quota Information
================================================================================================

[ username ].....................................................................................
Filesystem           bytes     quota     limit    grace       files     quota     limit    grace
/projects            3.2MiB      -         -      -             0K        -         -      -
/scratch           996.0KiB      -         -      -             0K        -         -      -
/u                   3.9GiB    1.0TiB    1.5TiB   -           121K        5M       10M     -

______________________________________Project_group_quotas______________________________________

[ /scratch ]....................................................................................
Group                bytes     quota     limit    grace       files     quota     limit    grace
TRAIN_barx           4.0KiB  500.0TiB  550.0TiB   -             0K       50M        -      -

[ /projects ]...................................................................................
Group                bytes     quota     limit    grace       files     quota     limit    grace
TRAIN_barx           4.0KiB    5.0TiB    5.5TiB   -             0K       25M        -      -

Nearline Quota Information for user
================================================================================================
username       Used:   4.2TiB of   5.0TiB Assessed on: Mon Jan 14 17:11:01 2018

Nearline Quota Information for groups
================================================================================================
TRAIN_barx     Used:     0    of  50.0TiB Assessed on: Mon Jan 14 17:11:01 2018
~~~
{: .output}

## Online storage

The three directories that we have just discussed are stored on different filesystems
and are accessible from anywhere on Blue Waters.
Because of that, these filesystems together form the called **online** storage system.

<br />
## Nearline

In the output of the `quota` command, you might have noticed the lines that start with **Nearline**.
Nearline is a tape storage system that provides space for data that you don't plan to access
frequently but still would like to share it with your collegues.
You can not access it as easily as other directories we mentioned above, but we will discuss
how to use it in the next episode.
