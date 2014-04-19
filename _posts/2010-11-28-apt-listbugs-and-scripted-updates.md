---
title: apt-listbugs and suite-wide scripted upgrades
author: adam
layout: post
permalink: /2010/11/apt-listbugs-and-scripted-updates/
categories:
  - debian
  - ENV
  - geek
  - scripts
  - software
  - tech
---
Having finally got fed up with logging in, individually, to upgrade each of the [no2id][1] machines and jails, a bit ago, I decided to write a script to do the &#8216;hard work&#8217; for me.

This worked fine, until today, when I noticed [apt-listbugs][2] complaining, and causing the script to fail to dist-upgrade.

Not a problem, thought I. I&#8217;m sure others have had this issue too. Being lazy, I thought first point of call would be the internets. I&#8217;d have thought something like:

```
"DEBIAN_FRONTEND=noninteractive" "apt-listbugs"
```

might have done the trick. It didn&#8217;t (that I could find).

So I went back to doing what a lot of the new-breed of &#8216;devops&#8217; fail to do, and what I&#8217;m quite hypocritical of; looking at the manpage.

The [manpage][3] provides us with this gem:

> ENVIRONMENT VARIABLES  
> o APT\_LISTBUGS\_FRONTEND If this variable is set to &#8220;none&#8221;  
> apt-listbugs will not execute at all, this might be useful if  
> you would like to script the use of a program that calls  
> apt-listbugs. 

So there we go.

```bash
 for M in $MACHINES
 do
     echo "Connecting to ${M}.no2id.net"
-    ssh root@${M}.no2id.net 'export TERM=xterm; export DEBIAN_FRONTEND=noninteractive; apt-get update &#038;&#038; echo "" &#038;&#038; echo "" &#038;&#038; echo "This is "'${M}'".no2id.net" &#038;&#038; echo "" &#038;&#038; echo "" &#038;&#038; apt-get dist-upgrade'
+    ssh root@${M}.no2id.net 'export TERM=xterm; export DEBIAN_FRONTEND=noninteractive; export APT_LISTBUGS_FRONTEND=none; apt-get update &#038;&#038; echo "" &#038;&#038; echo "" &#038;&#038; echo "This is "'${M}'".no2id.net" &#038;&#038; echo "" &#038;&#038; echo "" &#038;&#038; apt-get dist-upgrade'
done
```

hopefully, this will help others, whose first port of call is the internets, and not manpages. 

You may, however be sensible &#8212; and have had the time to roll out [Puppet][4] (ugh, when did they change their website! Why&#8253;) or [Chef][5] though.

 [1]: http://www.no2id.net
 [2]: http://packages.debian.org/search?keywords=apt-listbugs
 [3]: http://manpages.debian.net/cgi-bin/man.cgi?query=apt-listbugs
 [4]: http://www.puppetlabs.com
 [5]: http://www.opscode.com/chef
