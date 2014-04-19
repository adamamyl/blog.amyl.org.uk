---
title: 'Can&#8217;t upload images to a WordPress blog?'
author: adam
layout: post
permalink: /2008/10/cant-upload-images-to-a-wordpress-blog/
categories:
  - openrights
  - rant
  - software
  - Uncategorized
tags:
  - detective work
  - help
  - images
  - internet weenies
  - ORG
  - uploads
  - WordPress
---
If, like the guys in the [<acronym title="Open Rights Group">ORG</acronym>][1] HQ, you find yourself unable to upload images in a WordPress Blog, and are befuddled by the reams of posts &c on the web, well, here&#8217;s another idea that might sort you out:

Remove the crap from old installs, in your ~/.mozilla directory. And by crap, well, I mean:

*   appreg
*   mozver.dat
*   pluginreg.dat
*   plugins/

```bash
cd .mozilla
rm -rf plugins/
rm appreg
rm mozver.dat
cd firefox/
rm -rf pluginreg.dat
```

Astonishingly, by removing a couple of bits of pelt, the uploads worked for the guys.

Seeing as this is my blog, and people expect me to be miserable, here&#8217;s my whine: why the fuck don&#8217;t people SEARCH THE FUCKING ARCHIVES on forums before posting a new question? I gave up, and went back to old-fashioned testing.

Oh, and I should probably plug a fairly new site I recently discovered: [stackoverflow][2] &#8212; about all I can whine about with that is that is uses OpenID&#8230;

 [1]: http://www.openrightsgroup.org
 [2]: http://stackoverflow.com/
