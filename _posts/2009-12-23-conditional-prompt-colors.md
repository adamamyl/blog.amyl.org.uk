---
title: Conditional Prompt colo(u)rs
author: adam
excerpt: setting a colored prompt; different color for different machines based on hostname, oh yes.
layout: post
permalink: /2009/12/conditional-prompt-colors/
categories:
  - bash
  - ENV
  - machine set-up
  - misc
tags:
  - bash
  - color
  - colour
  - conditional
  - dotfiles
  - ENV
  - hostname
  - prompt
  - PS1
---
I often work on several different machines, for different projects and things. It&#8217;s bloody annoying when I get the wrong machine!

I thought. I know what, I&#8217;ll make all of these machines use a colored prompt, and make that lot of machines use a different one.

(At this point, I should say that my dotfiles, and a variety of other things are kept in a subversion repo. Most of those bits are my-eyes-only (particularly a lot of the very badly/hastily thrown together scripts), but a few bits I&#8217;m gradually releasing.)

After mentioning this on [twitter][1], a couple of people have been interested in how I did it.

The solution is quite easy, work out the hostname, and from that determine the &#8216;class&#8217; of machine, and then apply some colors. The [archwiki][2] was useful in getting out the colors to use; along with underlining, and emboldening (I **never** use underlining, except in manuscript: ghastly thing that obscures text).

Whilst not perfect (the color parts could be set as a variable, and then passed to the PS1 line; I could have used &#8220;else&#8221; clauses&#8230;), it works. For me, so, erm, here&#8217;s [my .bashrc][3] &#8212; you want from the `# work out machine name/domain:` line.

A simple switch wotsits in `screen(1)`, and

`$ cd ~/pseudohome &#038;&#038; svn up`

followed with a 

` $ . .bashrc`

is how I deploy (some people have an &#8216;svn up&#8217; in their start-up scripts, I don&#8217;t).

Comments here, if you want to.

 [1]: http://twitter.com/adamamyl/status/6945837040
 [2]: http://wiki.archlinux.org/index.php/Color_Bash_Prompt#Wolfman.27s
 [3]: http://code.amyl.org.uk/adam/dotfiles/bashrc