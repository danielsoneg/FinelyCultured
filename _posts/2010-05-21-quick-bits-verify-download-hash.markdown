---
layout: post
title: Quick Bits - Verify the Hash of a Download
tags: [technology, software]
---
Just whipped up a script to check the SHA1 & MD5 hashes of a file and compare it to the clipboard. I rolled it into an Automator workflow so it can be attached as a folder action.

Basically, the script takes a parameter, which is the full file name, runs openssl sha1 and openssl dgst on it, and compares each value to what’s in the clipboard. If either signature match the clipboard, it pops up a growl notification confirming that the signature matches. If it doesn’t get a match, it growls the Sha1 and MD5 tags.

Get it here: [hashworkflow.zip](http://dl.dropbox.com/u/102389/hashworkflow.zip)

You’ll need [Growl](http://growl.info), if you don’t have it, and you’ll need to install growlnotify, which comes with Growl.

Instructions:
-------------
1.Unzip.
2.Open the .workflow
3.Go to File -> Save As: (may not be necessary…)
4.Ctrl+click on your Downloads folder and select Services -> Folder Actions Setup…
5.Select “Hash Downloads.workflow”

Issues:
-------
The script uses pbpaste to read the clipboard. Since there’s no way to guarantee pbpaste is giving back text, this may cause a problem when there’s large amounts of data on the clipboard. Clipboard contents are assigned to a variable using the syntax PB=`pbpaste`, so there shouldn’t be a risk of accidentally running a command that isn’t intended, but I haven’t extensively tested for this.

Disclaimer
----------
I’ve tested this for approx. 6 minutes. I make no claims about the safety of this software, and take no responsibility for the results of running it. I took shortcuts and the code sucks. Caveat Downloador.