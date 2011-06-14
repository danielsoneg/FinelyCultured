---
layout: post
title: Building Documentation With Mediawiki
tags: technology
---
I've been talking with a lot of people lately about what I do, and I've gotten a lot of questions about our use of Mediawiki - largely in the "Why are you using that?" vein. The first few times I didn't have a great answer, for reasons anyone who's really worked with Mediawiki knows - it's just not that pleasant a system, and it feels crufty, especially when I'm talking to Agile Ruby Devs. It's worked great for our systems, though, and I wanted to share some of the benefits we've gotten, as well as some of the drawbacks of working with Mediawiki.

(Incidentally, if you're checking the Wiki link above before August 30th or so, you're not seeing the latest version of the Wiki, which includes around 9 months of improvements.)

Who Am I to Talk About This?
----------------------------
For the last 18 months I've been working at Embarcadero Technologies converting the whole of the RADStudio/Delphi/C++Builder documentation over to a MediaWiki based system. As of right now, I have a 7-server continuous-build system designed to build and support both product and language documentation in four languages. I've got 18 wikis running off a single codebase with custom skins, custom extensions, and scripts writing and retrieving around half a million pages on a pretty regular basis. It's fair to say at this point I'm familiar with deploying, maintaining, and working with Mediawiki, both the good and the bad.

Why Mediawiki?
--------------
While it brings its share of problems, Mediawiki does a phenomenal amount of what we needed right out of the box. Given that we've got an extremely limited budget and an even more limited development team to support a professional commercial product, Mediawiki's capabilities have been invaluable to us.

Here's what we're getting right away:
The Good
========
Easy Syntax
-----------
This was the #1 reason we went with Mediawiki - We wanted something where anyone could write new documentation easily, and eventually we wanted to open up the documentation to contributions from customers. Mediawiki has an easy-to-use syntax, and it's a syntax that many people are already familiar with. Anyone trying to get user interaction on a site knows that even if you write the content for the user you're still asking too much of them - we wanted as low a barrier to entry as possible, and it's a fair guess everyone who hits our site has at least Seen Wiki syntax at some point. So far it's working: one of my best moments at work was when our localization manager complained in jest that our writers were too productive for his budget. We've seen a 40% increase in output over the old XML-based system, which is great, and it's even easier to bring new people up to speed, which is crucial.

Localization
------------
I'm not sure how strong a consideration this was at the beginning, but boy has it saved us time and stress. Our documentation goes into four different languages, so having a system that handled Japanese out of the box has been crucial for us. Mediawiki has a robust localization framework built in, which is another part of the system we didn't have to write.

Accessibility
-------------
I'm honestly not sure where we sit on the regulations on this one - I'm not sure if our company is required to have a 509 solution. Fortunately, I don't have to worry about it, because it's baked in right out of the box.

Scale
-----
Our documentation is around 70,000 pages per language, times four languages, times two product versions. Half of the pages are duplicated because of how our scripts work, so we're at ballpark 750,000 pages on the DocWiki system. We needed a system we knew could handle anything we threw at it, and Mediawiki's one of the few that have been field tested to be able to stand up to just about anything. Other CMS's may be able to handle this sort of load, but we absolutely knew Mediawiki could - we don't have the resources to plow a few months of dev into a system and have it collapse once we put all our documentation in.

Revision Management
-------------------
Built in, out of the box, including diffs and comments. We transitioned away from SVN to Mediawiki, so the revision tracking made it very easy for us to track our changes. It's got easy-to-use diffs and an RSS feed, so we can keep tabs both on the Wiki and on the writers. It's also got a detailed log, so we can tell who was responsible for anything that happens on the system.

Robust User Management
----------------------
Including Roles. There's some quirks here, but overall, it's another thing we didn't have to worry about. I've been able to expand on this quite a bit to allow mass user imports from our other systems, user searches, and a few other neat tricks that have made our lives easier.

API
---
We auto-generate a lot of our content, so we needed a clean way to edit certain pages without affecting others. Mediawiki has an API baked in, and the mwclient python scripts work wonders. Another part of the system we didn't have to build.

Extensibility & Community
-------------------------
Because we're using a packaged solution for a fairly specialized use case, extensibility was part of the spec. Mediawiki has a shockingly large number of hooks for extensions, and really allows us to do just about anything we want with the right combination of calls. It's also got an enormous developer community, including the WikiMedia foundation, which contributes its code back to the MW community. As a bonus, you can see anything that's in use on the WikiMedia servers, which guarantees the code has been put through its paces.

Skinability
-----------
In our case, we were able to change the look of the default Mediawiki skin enough to get a distinct look, but at the same time, it's still clearly Mediawiki. This is a bonus for us, since it tells users what system they're using and saves us a whole lot of user training.

Lamp & Caching
--------------
Say what you will about the LAMP stack, but we're on an extremely restricted development budget - we needed something that worked. LAMP gets us up and running in 20 minutes on a stack that's in use practically everywhere - there's not a problem we're going to hit that someone else hasn't seen already. Mediawiki also supports APC, Squid & Memcached right out of the box - with everything set up, we're catching almost everything somewhere in the caches.

What's Bad?
===========
It's Unstructured
-----------------
Mediawiki is not meant for structured content - it's basically built for a flat hierarchy. This is a bit of a problem when you're dealing with any serious product documentation, and was especially a problem for us, as we had highly structured content. We had to invent our way around the table of contents, the index, and the tagging we needed to make our shippable help files.

Search Is a Joke
----------------
The built-in Mediawiki search is just absolutely atrocious - even Wikimedia's using Lucene instead. If you have any reasonable expectation of users finding content on your site, you need a different solution.

Architectural Limits
--------------------
Because of how Mediawiki stores inter-page links and category information, pages that are extremely link-heavy tank the wiki when you try to save them. Looks like a database lock, but I'm not totally sure. It took me a long time to track this issue down, but that's my leading contender for the mysterious "The wiki's dead!" bug.

Spaghetti Code
--------------
Especially in the themes. Or at least, a codebase that's so large and daunting it might as well be spaghetti code. Either way, it really looks like code which has been developed by a large number of different people, none of whom knew each other. Which it basically is, and it's a triumph for that, but it's not terribly readable.

PHP
---
Yeah, we all knew it was coming - PHP seriously sucks as a language. It's the most universally-deployable web dev language around, but then again, McDonalds is the most universally-deployed hamburger-ingestion venue around - doesn't make it good.

In a future post, I'll talk a bit more about the specific challenges we faced and how we dealt with them. I think we've got some pretty cool stuff going into our wiki setup, and maybe someone will even find it useful.

If you have any questions, by all means, leave them in the comments below, and I'll address them as best I can.

Thanks!