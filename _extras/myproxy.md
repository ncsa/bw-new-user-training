---
layout: page
title: "SSH with MyProxy"
permalink: /myproxy/
---

<br />
## MyProxy

_**Important!**_ *MyProxy does not work with training accounts.*

With a full Blue Waters account, you can use
[MyProxy](http://grid.ncsa.illinois.edu/myproxy) suite of tools to access Blue
Waters without having to enter the password for the duration of 12 hours. You
can get MyProxy as part of [Globus Toolkit](http://toolkit.globus.org/toolkit).
Please, [download](http://toolkit.globus.org/toolkit/downloads) and install
Globus Toolkit following the instructions on their website. Use the latest
stable version that is currently available.

MyProxy let's you obtain certificates that can verify your identity to Blue
Waters for an extended period of time. To obtain them, execute:

~~~
$ myproxy-logon -T -s tfca.ncsa.illinois.edu
~~~
{: .language-bash}

When you execute the above command, it will ask you to enter a PASSCODE, which
is your password + token. Note, that you have to use `-T` flag only the first ever
time you use `myproxy-logon`. Obtained certificates will be active for 12
hours and will let you log in to Blue Waters with a mere:

~~~
$ gsissh bw
~~~
{: .language-bash}

so you don't have to enter your passcode every single time. All this
information is documented at [{{ site.getting-started }}]({{
site.getting-started }})
