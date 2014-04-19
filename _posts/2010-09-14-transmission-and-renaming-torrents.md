---
title: Transmission and Renaming Torrents
author: adam
layout: post
permalink: /2010/09/transmission-and-renaming-torrents/
categories:
  - bash
  - scripts
tags:
  - git
  - github
  - mailer
  - rename
  - script
  - symlink
  - transmission
  - works for me
---
I&#8217;ve recently (ish) started using transmission as my torrent client; the change-over comes from my switching-things-off approach; instead of keeping [ktorrent][1] running on the laptop (and caneing my bandwidth), I can have something mainly work on the NAS which is always on (bar power-cuts/maint).

One of the things that I noticed was the [apparent lack][2] of [renaming][3] within Transmission (and the curious way earlier tickets are marked as duplicitous of later ones).

So, erm, I&#8217;ve [written something][4] that works for me. And hacked out the mailer-script to something a little cleaner&#8212; at least in my view.

The premise is that you&#8217;re using a POSIXish operating system &#8212; [my NAS][5] runs on Debian &#8212; and that all of your exports are within the /nas directory, and your torrents directory is /nas/torrents.

The changes needed are (with transmission not running, apparently) to include `/path/to/post-download` as the value for  
`script-torrent-done-filename` in `settings.json`

You&#8217;ll need to echo in `"/path/to/store/the/completed-file"` to a file named as per the torrent (see your `incomplete` directory for that), but with &#8220;`.move`&#8221; appended; the rest should all happen automagically.

You might find it useful to chown the directories you&#8217;ll be moving things to that of the user running the transmission processes; I tend to setgid to my GID too.

The other file, mvtor, is one for doing a manual move, specify the torrent as an arguement; e.g., `./mvtor "ubuntu-10.04.1-alternate-i386.iso"`

 [1]: http://ktorrent.org
 [2]: https://trac.transmissionbt.com/ticket/1220
 [3]: https://trac.transmissionbt.com/ticket/672
 [4]: http://github.com/adamamyl/transmission
 [5]: http://www.readynas.com/