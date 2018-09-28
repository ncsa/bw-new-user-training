---
title: "Data Transfer"
teaching: 20
exercises: 10
questions:
- "How do I move data in and out of Blue Waters?"
objectives:
- "Know which tools to use and where to put data"
- "Learn how to use Globus Online and a `globus` tool"
keypoints:
- "Use `scp`, `rsync`, `wget`, `git` for \"small\" data transfers"
- "Use \"Globus Online\" for large/long data transfers"
- "Use Globus to store data in Nearline"
---

_**This episode is a stub and is beeing actively developed.**_
_Please feel free to share your ideas and/or contribute._

If we step back and consider the entire process of our interaction with Blue Waters,
it consists of three main steps:

1. Bringing data (input, code) onto the system
2. Running simulations and/or analysis
3. Moving data (results) from the system

In this episode we will discuss several ways of transfering data between your machine and Blue Waters.
Because this is an introduction on how to work with Blue Waters and not any specific scientific code,
we will use a tarball of Linux Kernel version 4.16.8.
Please download it [here](https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.16.8.tar.xz).

<br />
## Cunning `scp`

The simplest way to bring data from your computer to Blue Waters is by using the `scp` command.
`scp` command works similarly to the standard `cp` command and its general form is:

~~~
$ scp source destination
~~~
{: .language-bash}

For example, to upload Linux Kernel to Blue Waters, navigate to the directory where you saved
`linux-4.16.8.tar.xz` and execute:

~~~
$ scp linux-4.16.8.tar.xz bw:~/
~~~
{: .language-bash}

In this command we are uploading this file to our home directory on Blue Waters.
Note, that we are using `bw` shortcut that we created earlier.
Enter your password and let it upload the file.

Let us now examine both source and destination files by
obtaining the basic information about the files with `stat linux-4.16.8.tar.xz`
on your personal computer **and** on Blue Waters.
Note, that macOS users have to add `-x` flag to get the output in the form similar to that on Linux.
If your terminal emulator does not provide `stat` command, use `ls -lh`.
~~~
$ stat -x linux-4.16.8.tar.xz      # on your computer
~~~
{: .language-bash}
~~~
  File: "/Path/to/linux-4.16.8.tar.xz"
  Size: 103030932    FileType: Regular File
  Mode: (0640/-rw-r-----)         Uid: (  502/ macUser)  Gid: (   20/   staff)
Device: 1,5   Inode: 8609432423    Links: 1
Access: Wed May 10 11:12:13 2018
Modify: Wed May  9 03:00:40 2018
Change: Wed May 10 11:12:13 2018
~~~
{: .output}

And now on Blue Waters:
~~~
$ stat ~/linux-4.16.8.tar.xz
~~~
{: .language-bash}
~~~
  File: `/u/sciteam/username/linux-4.16.8.tar.xz'
  Size: 103030932 	Blocks: 201248     IO Block: 4194304 regular file
Device: 6c963686h/1821783686d	Inode: 144126968785316320  Links: 1
Access: (0640/-rw-r-----)  Uid: (47840/ username)   Gid: (14802/bw_staff)
Access: 2018-05-16 02:04:26.000000000 -0500
Modify: 2018-05-16 02:06:47.000000000 -0500
Change: 2018-05-16 02:06:47.000000000 -0500
 Birth: -
~~~
{: .output}

All timestamps have been reset to the time at which the upload took place!
If you repeat the upload or, instead, download the file, timestamps will be updated again.
This constitutes a problem if you rely on data timestamps in your analysis.
To preserve timestamps, use `-p` flag when transferring data with `scp`:

~~~
$ scp -p linux-4.16.8.tar.xz bw:~/
~~~
{: .language-bash}

But what happens if we (accidentally) execute the command again?
Well, `scp` will blindly transfer the file again, even though the destination file exists
and is identical to the one we're uploading. This is the first problem with the `scp` command.

Let's consider another scenario, in which we have to transfer a directory rather than a single file.
In such a case, we have to add `-r` flag:

~~~
$ scp -pr local-directory bw:~/path/
~~~
{: .language-bash}

That was not too difficult... But there is, in fact, a caveat: _`scp` follows symbolic links_.
This means that instead of transfering symbolic links as such, `scp` transfers actual files they
point to.

<br />
## `rsync`, the savior

`rsync` is a popular and, in many ways, a much better alternative to `scp`.
Without much ado, its most useful syntax is:
~~~
$ rsync -avP linux-4.16.8.tar.xz bw:~/
~~~
{: .language-bash}

~~~
building file list ...
1 file to consider

sent 92 bytes  received 20 bytes  6.79 bytes/sec
total size is 103030932  speedup is 919919.04
~~~
{: .output}

Note that command completes almost instantly.
`rsync` determined that there is, in fact file with the same contents on Blue Waters
and did not transfer it, saving us bandwidth and time!

> ## Training accounts
> 
> If you're using a training account, add `-e 'ssh bwbay.ncsa.illinois.edu ssh'`
> to the `rsync` command above:
> ~~~
> $ rsync -avP -e 'ssh bwbay.ncsa.illinois.edu ssh' linux-4.16.8.tar.xz bw:~/
> ~~~
> {: .language-bash}
{: .callout}

Note, that again we are using the `bw` shortcut that we created for the `ssh` command.
If we "accidentally" try uploading it again, `rsync` will, again, skip the upload.

<br />
## `wget` and `curl`

When files that you'd like to upload onto Blue Waters are located somewhere in the World Wide
Web, it makes sense to skip the step of downloading them to your computer and copy them directly
to Blue Waters. In such a case, you can use `wget` or `curl`:

~~~
$ wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.16.8.tar.xz
$ curl -O https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.16.8.tar.xz
~~~
{: .language-bash}

<br />
## `git`

If you're hosting your personal code in a `git` repository, use the `git clone` command:

~~~
$ git clone https://github.com/your-github-account/repository.git ~/source-code/
~~~
{: .language-bash}

<br />
## Large/long data transfers

When dealing with large data, we have to consider the details of the data transfer process.
Commands that we issued above use `bw` as one of the "end points" of the data transfer.
As we already know, `bw` is, in fact, a non-existent machine that connects us
to one of the three login nodes.
Thus, when we transfer data using `bw` as destination we, in fact use Blue Waters login nodes.
This can and, in fact, does cause problems for other Blue Waters users who happen to be logged in
to those login nodes.
Moreover, if you launch a long data transfer and there is a problem with network connection,
you will have to manually re-initiate the transfer.

For these purposes, Blue Waters has dedicated Input/Output (IO) nodes.
These nodes, however, can be accessed using a tool called Globus Online.

And again, without much ado, let's jump right in!

1. We need to create an account on [https://www.globus.org](https://www.globus.org):
   - Click "Log in"
   - Select your organization, use your Google or ORCiD iD, or create a Globus ID
   to sign in.
2. Navigate to the Transfer Files section at
[https://www.globus.org/app/transfer](https://www.globus.org/app/transfer)
3. To transfer data to or from your computer, click on Get Globus Connect Personal
link and follow the instructions to create your personal End Point.
4. Go back to the Transfer Files section, specify the source and destination End Points by clicking
on the two End Point text fields and specifying End Points. Blue Waters has two End Points that you
can use: **ncsa#BlueWaters** and **ncsa#Nearline**, the latter being  tape storage system.
5. Select files and folders that you wish to trasfer and click one of the big arrows
to initiate data transfer.
