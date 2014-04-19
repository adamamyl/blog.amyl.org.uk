---
title: 'Dell Order Checking: via cron'
author: adam
layout: post
permalink: /2008/07/dell-order-checking-via-cron/
categories:
  - indolence
  - scripts
tags:
  - bash
  - cron
  - dell
  - diff
  - lazy
  - ordering
  - scripts
  - sysadmin
  - wget
---
Ok. So let&#8217;s start off with a fairly obvious statement. I&#8217;m indolent. And I can write scripts. This is a dangerous, nay, perilous pairing&#8230;

So, with this existing laptop really getting on my nerves, and the lack of email coming from Dell regarding the new &#8216;un I ordered, I thought I&#8217;d tidy up some diff-scripts used $ELSEWHERE, and re-appropriate for quick-and-dirty order-tracking.

Fairly simple: fetch a web-page, in this case the order page (which is accesssible with the order number & email address used for the order), compare it with an existing copy (should it exist), and mail specified addresses when/if there are changes. Do this whenever (@hourly works fine for me), and forget about website visiting.

Bingo.

So, erm, just in case anyone else wants it (yes, the licensing blurb is probably about the same length as the code itself, i dunno why I bother, but maybe someone&#8217;s got some hints/tips/comments&#8230;), &#8216;[dell-order-status][1]&#8216; (it&#8217;s a tidied up version of the one I&#8217;m actually using, so i may need a nudge to update the web-version if I change the one in use)

 [1]: http://amyl.org.uk/~adam/dell-order-status "dell-order-status"