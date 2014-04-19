---
title: cURL and Google Spreadsheets
author: adam
layout: post
permalink: /2011/12/curl-and-google-spreadsheets/
categories:
  - bash
  - curl
  - google-docs
  - scripts
---
I failed to find a good example of something that worked to pull a spreadsheet from google-docs using cURL. All that I found didn&#8217;t work, in one shape or another.

A bit of playing, and quite a bit of reading got this

```bash
#!/bin/bash
PASS=`cat /path/to/0600/google-password-file`
SHEET="https://spreadsheets.google.com/feeds/download/spreadsheets/Exportkey=addyourownsheetIDhere&#038;exportFormat=csv&#038;gid="</p>
<p>AUTH_TOKE=`curl --silent https://www.google.com/accounts/ClientLogin -d \
    Email=foo@example.org -d \
    Passwd=${PASS} -d \
    accountType=HOSTED -d \
    source=cURL-SpreadPull -d \
    service=wise | grep Auth\= | sed 's/Auth/auth/'`</p>
<p>curl --silent --output /path/to/file --header "GData-Version: 3.0" --header "Authorization: GoogleLogin ${AUTH_TOKE}" "${SHEET}${TAB}"
```

seemed to do the trick

`Exportkey` could be defined in the script, as a variable, thinking about it. You&#8217;ll need to supply that; I typically grab it from the web-based URI, but there is a warning in [the docs][1] about that:

<quote>  
*To determine the URL of a cell-based feed for a given worksheet, get the worksheets metafeed and examine the <link> element in which rel is http://schemas.google.com/spreadsheets/2006#cellsfeed. The href value in that element is the cell feed&#8217;s URI.*  
</quote>

YMMV.

I&#8217;ve added in `&#038;exportFormat=csv&#038;gid=` because I wanted CSV outputs, and gid&#8217;s value is provided via a for &#8230; and case deviation.

`--header "GData-Version: 3.0"` was needed to avoid the redirection.

Hopefully, this might be of benefit &#8212; as a working (when written) example of using curl and google docs/google spreadsheets.

 [1]: http://code.google.com/apis/spreadsheets/data/3.0/developers_guide.html#RetrievingCellFeeds
