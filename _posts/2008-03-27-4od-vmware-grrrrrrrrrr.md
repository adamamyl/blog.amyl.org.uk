---
title: 4oD, VMWare, Grrrrrrrrrr.
author: adam
layout: post
permalink: /2008/03/4od-vmware-grrrrrrrrrr/
categories:
  - 4od
  - drm
  - openrights
  - rant
  - vmware
tags:
  - 4od
  - drm
  - rant
  - vmware
  - "why can't things be *easy*"
---
Ok, let&#8217;s start off with saying I&#8217;ve not used &#8220;Windows&#8221; properly for about 4years, now, having ditched it in favor of [umbungo][1], on my home machines.

The exception to this is that I use VMWare for when I need to use Windows, e.g., to do stuff in Visio, or sort out something like Page Breaks & Tables for a Word Document: two features \*really\* lacking in OpenOffice&#8230;

But now, I want to watch a news report off ITN. I should have just found a Torrent. It would have been a lot quicker. And saved the web of \*another\* rant-blog posting&#8230;

You&#8217;d have thought it would be fairly easy. Oh no. [4oD][2] don&#8217;t work on *nix:

> <a href="http://tanqueray.amyl.org.uk/~adam/blog/?attachment_id=5" rel="attachment wp-att-5" title="4od Screengrab"><img src="http://tanq.amyl.org.uk/%7Eadam/blog/wp-content/uploads/2008/03/4od-sorry.thumbnail.png" alt="4od Screengrab" /></a> Who can use 4oD?
> 
> 4oD is available online on your PC and through our digital television partners, BT Vision, Tiscali TV or Virgin Media. You&#8217;ll need to be a resident in the UK or the Republic of Ireland to watch 4oD.
> 
> On your PC  
> To be able to get 4oD on your PC, you will need:
> 
> * A PC with Windows XP or Windows Vista  
> * Internet Explorer 5.5 or above  
> * A broadband internet connection
> 
> Read about our minimum system requirements to find out more
> 
> Please note that Mac, Linux and non-XP users will currently not be able to use this service.  
> <http://www.channel4.com/4od/help.html>

So, over to the VM&#8230;

&#8216;cept that didn&#8217;t have: A recent enough Media Player, DotNet, a bazillion patches (post SP2) &#8230;

And installing all of those updates means that my 6GB partition weren&#8217;t big enough to actually carry on (bearing in mind this is just XP, Office 2003, a printer driver, Acrobat Reader&#8230;). Great. Should be easy enough to sort that out, he thinks.

Nope.

**Step One:** try to work out[how to delete][3] the snapshot made accidentally&#8230;

**Step Two:** oh, that crashes VMWare. So let&#8217;s [rebuild][4] VMWare&#8230;

**Step Three:** oh, it seems as though *that* might&#8217;ve worked&#8230;

**Step Four:** let&#8217;s try and [resize the partition][5]: yum. And **yay**

**Step Five:** see if Windows wants to play ball.  
Fuck. Nope, it now recognizes there&#8217;s two disks, not the one partition&#8230; so, erm, [over to the live CD and thence GPartEd][6]&#8230;

*(at this point, I thought, &#8220;fuck it&#8221;, I&#8217;m gonna blog)*

**Step Six:** ignore the gparted warnings, and see if that&#8217;s sorted it&#8230;

**Step Seven:** let&#8217;s reboot into Windows. Oh look, it wants **another** reboot. Fuckwittery.

**Step Seven(b):** well, guess what, it&#8217;s not worked. But I have had a brainfart. Despite saying &#8220;I don&#8217;t want a paging file, but you&#8217;re gonna give me one, anyhow&#8221;, let&#8217;s resize that to get some space on this VM Disk&#8230;.

So, erm, yeah. Maybe I&#8217;ll fix this a bit proper, but right now, i&#8217;ve had enough&#8230; I though *nix was meant to be the more difficult to use OS&#8230;

 [1]: http://www.ubuntu.com
 [2]: http://www.channel4.com/4od
 [3]: http://www.cutawaysecurity.com/blog/archives/88
 [4]: https://help.ubuntu.com/community/VMware/Server
 [5]: http://www.novell.com/coolsolutions/tip/15344.html
 [6]: http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1647