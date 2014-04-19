---
title: 'Mailman and Googlemail -> Gmail: A three step approach&#8230;'
author: adam
layout: post
permalink: /2010/06/mailman-googlemail-to-gmail/
categories:
  - Uncategorized
tags:
  - bash
  - geek
  - lazy
  - mailman
  - scripts
  - sysadmin
---
I thought other listadmins might be having fun with gmail now being available in the UK (rather than &#8220;googlemail&#8221;, as it has been for a while (despite &#8216;gmail&#8217; originally being available, back in the days of invitation only)), and thought I&#8217;d share my hackish way around this, so listfolks can post from their gmail.com addresses.

It&#8217;s not pretty, but works for me &#8212; pre-requisite, Mark&#8217;s very useful &#8220;non-members&#8221; script: <http://www.msapiro.net/scripts/non_members>

```bash
mkdir ~/tmp/gmail

#find who you need to work with:  
list_lists -b | while read L
    do
        list_members ${L} | grep googlemail > ~/tmp/gmail/${L}
    done

#Zap annoucement lists from the files, remove empty files, too.

#Let them post!  
/var/lib/mailman/bin$ ls -1 ~/tmp/gmail | while read L
do 
    sed 's/@googlemail.com/@gmail.com/' ~/tmp/gmail/${L} | while read X
        do
            ./non_members --list=${L} --filter=accept --add ${X} --verbose
        done
done
```

(nb: the path (/var/lib/mailman/bin) is from a Debian machine &#8212; Mailman installed via packages &#8212; and in my case /var/lib/mailman/bin being in  
my `${PATH}` &#8212; so replace those as appropriate in your cases.)

Which seems to have done the trick.
