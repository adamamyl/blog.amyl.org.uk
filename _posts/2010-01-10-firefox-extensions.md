---
title: Firefox Extensions
author: adam
layout: post
permalink: /2010/01/firefox-extensions/
categories:
  - bash
  - indolence
  - machine set-up
  - scripts
  - software
tags:
  - bash
  - geek
  - install
  - new machine
  - scripts
  - set-up
  - tech
---
Thought this might double up as a note of the firefox extensions I currently have installed &#8212; I&#8217;ve tried getting this to script, but, the source file isn&#8217;t something I&#8217;m over-familiar with, and getting fields to match-up ain&#8217;t happening, due to my crapness.

Anyhow, I would appear to have these firefox extensions installed:

*   [Adblock Plus][1]
*   AutoPager
*   BetterFlickr
*   [Better YouTube][2]
*   [Delicious Bookmarks][3]
*   DownloadHelper
*   [Echofon][4]
*   Extended Statusbar
*   [Fast Video Downloader (with SearchMenu)][5]
*   [Firebug][6]
*   Firefox (default)
*   Firefox (en-GB)
*   [Flagfox][7]
*   [Flashblock][8]
*   [Gmail Manager][9]
*   [Greasefire][10]
*   [Greasemonkey][11]
*   Image Download
*   Image Zoom
*   Inline Code Finder for Firebug
*   is.gd Creator
*   [JavaScript Options][12]
*   keyconfig
*   [Magic&#8217;s Video Downloader][13]
*   oldbar
*   [Password Exporter][14]
*   Save Image in Folder [sic]
*   [ShowIP][15]
*   [SkipScreen][16]
*   TinyUrl Creator
*   Ubuntu Firefox Modificiations
*   URL Fixer
*   [VMware Remote Console Plug-In][17]
*   Xulrunner (en-GB)
*   YesScript

A few of those don&#8217;t have links I can identify from the URI.

Want some code that vaguely does this for you?  

```bash
#!/bin/sh
#
# ffexts:
#   list firefox extensions: names and URIs for download/homepage
#
# Copyright (c) 2010 Adam McGreggor. Some rights reserved.
# Email: &#60;adam@amyl.org.uk&#62; Web: &#60;http://blog.amyl.org.uk&#62;
#
# $Id: ffexts 119 2010-01-10 00:38:04Z adam $
#
set -e
MOZDIR=~/.mozilla/firefox
PROFDIR=`ls -lha ${MOZDIR} | grep default | awk '{print $NF}'`
FILE=extensions.rdf
INFILE=${MOZDIR}/${PROFDIR}/${FILE}
OF=~/tmp/ffexts
OUTFILE=~/pseudohome/nas-docs/firefox-extensions-$(date '+%Y%m%d')
# check for existing outfile, as we'll be
# appending; if so, zap it
if [ -e ${OUTFILE} ]; then
    rm ${OUTFILE}
fi
# grab the interesting bits from the RDF file
for K in name homepageURL
do
   # nice fix-up, eh?
    grep "NS1:${K}" ${INFILE}  | sed -e "s/NS1:${K}=//" \
            -e 's/"//g' -e 's/>//' \
            -e 's/^[ \t]*//' | sort | uniq > ${OF}-${K}
    # using wc here is entirely optional <img src='http://blog.amyl.org.uk/wp-includes/images/smilies/icon_wink.gif' alt=';)' class='wp-smiley' />
    wc -l ${OF}-${K}
    # append
    cat ${OF}-${K} >> ${OUTFILE}
done
```

 [1]: http://adblockplus.org/
 [2]: http://ginatrapani.org/workshop/firefox/betteryoutube/
 [3]: http://delicious.com
 [4]: http://echofon.com/
 [5]: http://www.applian.com/fast-video-download/
 [6]: http://www.getfirebug.com/
 [7]: http://flagfox.net/
 [8]: http://flashblock.mozdev.org/
 [9]: http://www.longfocus.com/firefox/gmanager/
 [10]: http://skrul.com/blog/projects/greasefire
 [11]: http://mozmonkey.com/
 [12]: http://www.oxymoronical.com/web/firefox/jsoptions
 [13]: http://www.magic-imv.ro/vd/
 [14]: http://passwordexporter.fligtar.com
 [15]: http://code.google.com/p/firefox-showip/
 [16]: http://skipscreen.com/
 [17]: http://www.vmware.com/
