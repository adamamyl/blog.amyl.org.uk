title: Some notes on LetsEncrypt
author: adam
layout: post
permalink: /2015/12/letsencrypt-part1/
categories:
  - web
  - encryption
  - ssl
  - certs
  - betasoftware
  - letsencrypt
  - tech
  - technotes

# LetsEncrypt

I've been playing with LetsEncrypt for a bit now, and on the whole, I like it. However, it's still early days, and I wanted to share my experiences.

For the examples I use, I'm looking at my own set-up (or more precisely, NO2ID's, which I superintend/am Tech Director of) — I find real examples are useful.


## Debian & Python
I started using `wheezy` but took this as an opportunity to upgrade to `jessie`; I wanted Apache 2.4, as well as a more modern version of Python.

(You might want to upgrade first; it'll make your life easier.)

Python versions are quite important, too: if you start on an older version, it's probably advisable to zap (`rm`) your virtualenv (/root/.local/…), after upgrading — it saves a lot of time and frustration.

You don't want to run into the [urllib3/OpenSSL issues](https://urllib3.readthedocs.org/en/latest/security.html#pyopenssl) that are loosely documented, but not always obvious; use **Python 2.7.9** (or more recent) and save yourself some ballache.

A `REQUIREMENTS` file might be useful (and even better, say, a `REQUIREMENTS.{debian,centos,freebsd,macos,ubuntu}` file).

## Rate limits
There [are rate limits](https://community.letsencrypt.org/t/rate-limits-for-lets-encrypt/6769).

However, it seems that the documentation (and the recommended tool) doesn't deign to mention this. Nor does `letsencrypt-auto` default to using the staging infra (use `letsencrypt-auto --server https://acme-staging.api.letsencrypt.org/directory`)

In the real-world, we should accept that people will go for the easy option, and no one reads long documents like terms and conditions. Especially not when one needs to view a PDF and is working entirely in a console.

The rate limits really hit me, and prevented an earlier adoption, as I created too many in live, not staging, especially at first, and when I had a "broken" (not to what letsencrypt could parse) configuration.


## Error messages
Aren't always that helpful or very useful, and not helped by information being scattered around in:

- the [RTD](https://letsencrypt.readthedocs.org/en/latest/)
- the [GitHub README](https://github.com/letsencrypt/letsencrypt/blob/master/README.rst)
- Python module docs (see below)
- Source code comments (*which wretched part's causing this so I can dig more*)
- Your letsencrypt log file, even with `--verbose` being passed as an ARG 
- [IRC](irc://irc.freenode.net#letsencrypt)
- A few StackOverflow questions + answers
- The [forum](https://community.letsencrypt.org/) (and issues with how up to date is this; is it canonical; gah, searching; it's not stackoverflow)

— or not existing at all.

This (lack of documentation, and left to fend for oneself), I think represents a real problem: especially if ordinary users (including the sort who screen grab terminal outputs (rather than just fucking copy and pasting — or even pipe-ing to pastebin/gists)) are going to use things.

At least there's not a Mailing list, as well as the forum, or worse, the mailing list (archive) being syndicated to seventeen different forums.

## Apache
In this case, I still use Apache. At some point, I'll be switching over to nginx, as I prefer nginx, and find it faster, but ho-hum. At time of writing, nginx support isn't as fully fledged as Apache, at least according to the docs (I have a hatred of forums).

There are some gotchas to be aware of, that are loosely documented, but have atrocious error messages that are, quite plainly misleading.

### Ports
You may need to bind Apache to use an IPv4 address of your interface to get things working.

	<IfModule ssl_module>
        Listen 443
	</IfModule>

	<IfModule mod_gnutls.c>
        Listen 443
	</IfModule>

	<IfModule mod_ssl.c>
		Listen 443
	</IfModule>

becomes

	<IfModule ssl_module>
        Listen 93.93.131.141:443
	</IfModule>

	<IfModule mod_gnutls.c>
        Listen 93.93.131.141:443
	</IfModule>

	<IfModule mod_ssl.c>
		Listen 127.0.0.1:443
	</IfModule>

(a plain `Listen 443` in `ports.conf` didn't help)
 
### Config files and quoting
If you have Rewrites in your configs, make sure they're quoted.

e.g, `apachectl configtest` will let you have something like

	    RewriteRule ^index\.php$ - [L]
        RewriteRule ^wp-admin$ wp-admin/ [R=301,L]
        RewriteCond %{REQUEST_FILENAME} -f [OR]
        RewriteCond %{REQUEST_FILENAME} -d
        RewriteRule ^ - [L]
        RewriteRule ^(wp-(content|admin|includes).*) $1 [L]
        RewriteRule ^(.*\.php)$ $1 [L]
        RewriteRule . index.php [L]
	
but to get `letsencrypt-auto` (or `letsencrypt --apache`) to run, you'll need
	
	    RewriteRule "^index\.php$" "-" [L]
        RewriteRule "^wp-admin$" "wp-admin/" [R=301,L]
        RewriteCond "%{REQUEST_FILENAME}" "-f" [OR]
        RewriteCond "%{REQUEST_FILENAME}" "-d"
        RewriteRule "^" "-" [L]
        RewriteRule "^(wp-(content|admin|includes).*)" "$1" [L]
        RewriteRule "^(.*\.php)$" "$1" [L]
        RewriteRule "." "index.php" [L]

(for WordPress — *why on earth one would put these in a .htaccess file when one can chuck 'em in the vhost config…*)

The error message here, is something along the lines of 'Apache not found'.

### Rewrites
There's an optional setting to define a rewrite to take insecure visitors to the secure version of the site.

This balks if you already have some, and isn't particularly good; as well as not leaving things in what I'd say is not an ideal state, especially if you have a lot of Rewrites (and you'll want to check in your -le-ssl.conf that these now point to **https://** not *http://*).

In an ideal world, something like

	<VirtualHost *:80>
		ServerName foo.example.com
		DocumentRoot /data/vhosts/foo.example.com
		
		Redirect	/foo	/baa
		Redirect	/one	/two
		RewritePermanent	/baz	/downloads/baz
	</VirtualHost>
	
becoming

	<VirtualHost *:443>
		ServerName foo.example.com
		DocumentRoot /data/vhosts/foo.example.com
		
		Redirect	/foo	/baa
		Redirect	/one	/two
		RewritePermanent	/baz	/downloads/baz
	</VirtualHost>
	
	<VirtualHost *:80>
		ServerName foo.example.com
		
		RewriteEngine on
    	RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [L,QSA,R=permanent]
	</VirtualHost>

would be awesome.

## Gotchas

### Constant update
 - On every run. So annoying. I wish this would timestamp, and just check once an hour/day/week…
 
### Revoking certificates
There's very little documentation on this, and I didn't find it particularly useful / working.

Which cert file should you pass as your parameter? (discovered via IRC)

`./letsencrypt-auto revoke --cert-path /etc/letsencrypt/archive/pressreleases.wp.no2id.net/fullchain1.pem`

seemed to revoke, but the certificates were still offered when I tried to create again. A `--delete` option might be useful in future versions.

You may want to do something involving `find` and xargs (in `/etc/letsencrypt/`), but y'know, [YMMV](http://www.urbandictionary.com/define.php?term=ymmv).

In testing, I wasn't too bothered, so did the LazyThing™ and issued and `rm -r /etc/letsencrypt` but you don't just copy and paste things you read on the intertubes, do you?

It'd be nice if there were a curses interface for revocation.

### No wildcard support (yet)
 - but see below…

### Defining the 'master' certificate
Let's say you're mitigating against the wildcard lack-of-support; and you want to have various sites on the same cert; no problem, except you might want to use the root domain, not a subdomain as the main factor here; I found a two-step approach works here:

 - create using apex and www (e.g. `no2id.net` and `www.no2id.net`)
 - when that's done, re-run and extend the certificate to include others (`newsblog.wp.no2id.net`, `wp.no2id.net`, `pressreleases.wp.no2id.net`); but note, if you **don't want** punters to see you have a cert for `admin.no2id.net` don't include that; of course, security through obscurity is not especially useful; YMMV. Create an extra cert (but note ratelimit) for say dev and administrative things.