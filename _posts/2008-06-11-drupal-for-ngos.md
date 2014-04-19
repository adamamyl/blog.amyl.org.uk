---
title: Drupal for NGOs
author: adam
layout: post
permalink: /2008/06/drupal-for-ngos/
categories:
  - drupal
  - geekcons
  - ngos
tags:
  - drupal
  - drupal for ngos
  - geek
  - load balance
  - meeting notes
  - ngos
  - websites
---
Ah, right, well, yesterday, I made it along to a new meet-up/group, [Drupal for NGOs][1]

bloody good turnout, and quite a few people with experience in the field, along with those of us who&#8217;ve been playing around, and those thinking about Making The Switch. (like what [no2id][2] are to be [doing][3] soonish)

I saw on the upcoming comments that someone was after a few notes, so, erm, here are mine&#8230;

**casestudy 1: greenpeace uk**

[Greenpeace UK][4] (gpuk) used to use a legacy system of coldfusion, dating from the late 90s, and strove to gain better communications with their supporters, to recruit/engage others (vide: user module, forum). One of the cool things that&#8217;s on their site are the context sensitive blocks — targeted ads and stuff. Rock on!

Navigation&#8217;s made possible via both menus and tags, making use of the [Tagadelic][5] module.

Their local groups functionality draws in data from elsewhere, and as the sign-up process mail goes to the punter and the co-ordinator for that group: useful. Local groups are a specific Content Type.

To tweak some of the text, the locale module&#8217;s used

An extensive list of modules were used, the ones in my notes include:

*   Deliver
*   [Diff][6]
*   [Forward][7]
*   [HTML corrector][8]
*   [Newsletter][9]
*   <a href"http://drupal.org/project/service_links">Service Links</a>
*   User
*   User Categories

For their data stuff (the idea to link in the Drupal with their supporters database/CRM) is to use [SOPERA][10], something that uses SOAP, and can act (as I understood it) as an intermediary.

For importing stuff, they used a couple of scripts, and then manual tidy-up.

**casestudy 2: oxfam international**

Migrated from Plone &#8594; Drupal (site not yet live)

They use 14 core modules, and 29 custom modules, a few the same as gpuk, but also [Custom Breadcrumbs][11] and [Lightbox 2][12]

From start to finish, it&#8217;s taken them about 6 or 7 months.

In terms of clean-up, i can&#8217;t read my notes&#8230;

Their press releases, though **were** structured data, and imported in.

Nightly builds take place from their subversion repo.

For content specific stuff, they&#8217;re making use of the Cue node.

Apart from those, there were a couple of other Q&As, some of which focused on large-scale sites/optimization/techy concerns &#8212; the idea of PHP optimizers (there&#8217;s a blog somewhere comparing versions of PHP & Apache), and running a lightweight httpd were suggested, such as [lighthttpd][13] / ergdex. along with something like squid/varnish. Some modules it seems involve onehelluvalot of database queries (250) per load, so for large-scale stuff, re-writing may prove useful.

There are some load sims out there, too.

For proxying, appliancesys.com (hum, maybe I mis-heard) was mentioned as having proven useful

Looks like the start of a London Drupal community, which could be useful&#8230;

 [1]: http://upcoming.yahoo.com/event/610734/
 [2]: http://www.no2id.net
 [3]: http://drupal.no2id.net
 [4]: http://www.greenpeace.org.uk/
 [5]: http://drupal.org/project/tagadelic
 [6]: http://drupal.org/project/diff
 [7]: http://drupal.org/project/forward
 [8]: http://drupal.org/project/htmlcorrector
 [9]: http://drupal.org/project/newsletter_checkbox
 [10]: http://www.sopera.de
 [11]: http://drupal.org/project/custom_breadcrumbs
 [12]: http://drupal.org/project/lightbox2
 [13]: http://www.lighttpd.net/