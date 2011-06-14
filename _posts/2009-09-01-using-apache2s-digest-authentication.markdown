---
layout: post
title: Using Apache2's Digest Authentication
tags: [Technology, Software, How-To]
---
I finally got Apache's Digest authentication, and since there's a serious dearth of information online about getting Digest working, I decided I'd write up a bit. It's not difficult, but I've had far more trouble than I should have, and most of that's due to nonexistant documentation.

Why Digest?
-----------
Standard HTTP auth sends the password in plaintext, which is generally bad. On sites with an SSL cert, this is less of a problem, since the traffic is encrypted to begin with, but on smaller sites, it's a lot of overhead to set up SSL.
Digest authentication hashes the password before sending it - it's an MD5, which isn't great, but it's also not your password flapping in the breeze, so to speak. It's a good intermediate step to make sure you're not transmitting plaintext passwords anywhere.
What's the Drawback?
------
There's a very small performance hit, and it's not as easy to set up, but the biggest drawback is that you have to rebuild your apache users file.

So How Do I Do It?
------
Digest is very similar to standard Apache access configuration, with just a couple changes.

1. Make sure you have digest enabled - if you have shell access, type: "`a2enmod auth_digest`".  Otherwise, in your httpd.conf or apache2.conf file, include the line:

        $ LoadModule auth_digest_module modules/mod_auth_digest.so

2. Create your new Digest authentication file. At the shell, type:

        $ cd /var/www/
        $ htdigest -c .digest Internal Admin

    Note: This code creates a user named Admin in the realm "Internal" - we'll get to realms later.

    You'll be asked for a password, and then asked to confirm the password.

3. Create your .htaccess file in the directory you want to protect - in this case, we'll assume it's http://(yourdomain)/Private:

        BrowserMatch "MSIE" AuthDigestEnableQueryStringHack=On
        AuthType Digest
        AuthName "Internal"
        AuthDigestDomain /Private
        AuthUserFile /var/www/.digest
        Require user valid-user

    The first line is necessary to make Digest work for IE6. IE7 seems fine.

    AuthName is the realm you chose earlier - Digest allows you to make multiple "realms" to protect different directories.

    Also, note the AuthUserFile line - Apache docs swear that line should read AuthDigestFile. It shouldn't.

That's it - you're using Digest!

The code is interchangeable anywhere you'd normally use standard Apache password auth, and you can use all the other normal .htaccess commands as well. It's just a mildly more secure, and horrendously documented, alternative to standard HTTP authentication.

References
----
* [Apache.org: Authentication, Authorization and Access Control](http://httpd.apache.org/docs/current/howto/auth.html)
* [Apache.org: Apache Module mod_auth_digest](http://httpd.apache.org/docs/2.0/mod/mod_auth_digest.html)