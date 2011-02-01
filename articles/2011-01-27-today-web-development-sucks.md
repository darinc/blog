--- 
title: Today, Web Development Sucks
date: 27/01/2011

It seems clear to me that today, web development sucks. 

It's hairy, scary, maybe even downright abominable. Here I'll try and explain why I think our current tool chains are inadequate, and where I think we should be going to fix it.

<div class="pleasant right small">
  <img src="/images/abominable-snowman.jpg" alt="SNOWMAN!">
  <p>I feel like this guy when making websites</p>
</div>

The core of the problem revolves around the most exciting domain in web application development today: Javascript. The explosion of Javascript has given rise to amazing applications of stellar quality for quite some time now, but I see them coming only from teams with gobs of time and expertise.

## A quick recap

Ruby developers write all their business logic and database interaction in Rails or Sinatra or on their own and feel right at home. They can use all the wonderful facilities local to Ruby, just like the Pythonistas or PHP or Scala writers can with their language of choice, and everyone ends up with an application stack that does the job. These apps can spit out great looking HTML, accept nothing but exactly valid data from users, present it back to the them in the most effective and beautiful way possible, all while keeping memory consumption at 10% and CPU usage negligible. We've come so far since the days of cobbling together strings in PHP with inline database calls. Using these abstractions and frameworks has made us faster and our products better and allows us to leverage the collective intelligence of the open source world.

Then, the team or client or whoever decides that well, that fragment could be loaded using AJAX, or, hey, maybe we could add some animations to this drop down menu, or, hot damn, we should start validating stuff client side too!

This decision ushers in a new era of code duplication and implementation inconsistency. Services must be adapted to spit out JSON data for interpretation and rendering client side, or have their view code refactored to be accesible by fragment for use in AJAX calls. Developers end up reimplementing things like the server-side validations in Javascript in both their and the users interest, but code duplication severely limits maintainability, introduces a whole new class of possible errors, and requires proficiency in all languages involved, and is therefore most undesirable. All this is possible and done often, but lots of work.

## Javascript Heavy Applications

<div class="pleasant">
  <img src="/images/mobile-me-mobile-you.png" alt="Seriously.">
  <p>Who has time to do this to their <a href="http://news.ycombinator.com/item?id=1930802">freaking logo??</a></p>
</div>

The situation becomes hellish when you move to the SPA ([Single Page Application](http://en.wikipedia.org/wiki/Single-page_application)) domain. Designing and developing a  [Sproutcore](http://www.sproutcore.com/) app from scratch is way, way more work than the mediocre static HTML Rails or CakePHP or Pylons version providing similar functionality. With Sproutcore and company you write fat models on the client side with all sorts of domain logic and requirements-fulfilling-goodness, but then you go to save them to the server, and you start pulling your hair out. Like any sane developer, you must revalidate everything server side, requiring you to rewrite identical validations and apply them again. The dumb database-esque backend promised to you by the SPA ideals (see [Cloudkit](http://getcloudkit.com/)) can't actually be dumb or malicious users would have a field day. Before said data ever gets to the server as it would in a traditional form POST, you have to design and twice implement a transport strategy to marshal and load your data over the wire. With Javascript SPAs, you must transport it, revalidate it, and then store it server side, whereas with traditional methods, you rely on the browser to do the transport, and at the last and safest moment do your validation only once.

These code repetition issues are compounded in the view layer, where applications must be able to render some data on both the server and dynamically in the client, providing graceful degradation and fast first page loads. Some people use a second set of templates in a second language on the client side to wire up their Javascript to create the same HTML the server would. Again, this is far from optimal because it is harder to maintain and gives rise to inconsistencies depending on where things are rendered. The Mustache solution, where the same template language has engines in a multitude of different server side and client side languages, come to mind, but Mustache isn't nearly there. With Mustache, you must define a class (on both sides, in each implementation) to manage the rendering of each individual view, which for me is still an unacceptable amount of duplication. The often used solution to the rendering issue is to do all or almost all rendering client side, which raises perceived response times for users and cripples caching's effects on user experience. UI Kits solve these issues and thus are becoming more and more prevalent, as evidenced by [Sproutcore](http://www.sproutcore.com/), [Cappucino](http://cappuccino.org/), [Uki](http://ukijs.org/), and [Qooxdoo](http://qooxdoo.org/)'s admirable efforts to bring standardized pure JS view hierarchies to the browser. These frameworks are extraordinary and supremely useful, but succumb to the validation issues mentioned above, are hard to optimize for search engines, and provide no or little ready-to-go backend interaction code.

Also of note are tertiary symptoms like Coffeescript. Its creation and ensuing explosion onto the scene speaks worlds of our current toolset's inadequacies: people can't stand writing code in the language they are forced to. As a community we've started inventing cool new stuff on the serverside in an attempt to fix the clientside issues. We do as much as we can in the environment we control so we don't have issues in the environment we don't. Coffeescript, [therubyracer](https://github.com/cowboyd/therubyracer), [Akephalos](https://github.com/bernerdschaefer/akephalos), and [Yahoo's attempts at rendering YUI elements serverside](http://www.infoq.com/interviews/node-ryan-dahl#question9) so they degrade all convince me of this.

## The issue and the current solutions

The issue in my mind comes down to this: *apps are already too big to write twice*. Big entities can write abstractions that fix all this business, like Google's GWT, or they can man up and pour resources into creating or enhancing the existing frameworks, like Apple and Eloqua with Sproutcore. For punks like me, none of these options are viable, but applications need to be better than they are now. Instead, we use things like Backbone.js or Sammy.js or Faux, which provide some welcome and useful instrumentation but nothing on the scale I want. The Rails for Javascript apps has yet to arrive.

## Some Successful Examples

 - [Eloqua10](http://www.eloqua.com/products/take-the-tour.html)
 
 The biggest Sproutcore app I could find outside of MobileMe. Eloqua has a couple devs who are also core Sproutcore devs, contributing a bunch back to the community and open sourcing a lot of their UI work. My issue is that it took all that work on the framework for them to get a product out of it. I don't have to make equivalently complex and intense applications, but I still want to be able to use the framework without having to be a core team member.

 - [Gmail](http://mail.google.com/)

 An astounding accomplishment on the web today. _Update: Gmail isn't built using GWT. Whoops._ Again, Google built the GWT so they didn't have to write code twice, but I don't want to be stuck in the Java world or be forced to learn the whole GWT and make any open source buddies of mine learn it too.

 - [NewsBlur](http://www.newsblur.com/)

 A RSS reader SPA application written by Samuel Clay of DocumentCloud fame. Well put together, but look at the [code](https://github.com/samuelclay/NewsBlur/tree/master/media/js/)! To me it looks like it was a major pain to write. All the templating is done using `jQuery.make` and company. Its deceptively complicated and weighs in at 600KB of compressed JS.

 - [QuietWrite](http://www.quietwrite.com/)

 A pretty basic writing app built using [Backbone.js](http://documentcloud.github.com/backbone/). Successful because its so small and can be done using Backbone and jQuery in glorious tandem. A nice little app, but I would wager this same tech stack would not work for an app twice or four times the size. Backbone is an awesomely tiny library to start with, because it isn't meant to do everything, just provide some useful skeletoning. Building Eloqua10 using Backbone wouldn't work.

## What can be done about all of this?

I believe Rails is so successful because it forced its conventions (good ones) upon people, and people learned that being told the right way to do something isn't so bad. I want something that applies to _both_ the server side and client side in the same way. Both sides. If someone were to make an all Javascript Rails which covered the entire spectrum, I would use it. This super framework would have these attributes:

 - Well defined and proper conventions requiring a minimum of configuration. Backbone.JS and Sproutcore both suffer from agnosticism syndrome, where they try and cater to the widest audience by not making any decisions about the backend for the developer. I am not as smart as the framework developer, so I would prefer they come up with the best solution they can for the data bottleneck that is the internet, and let me work with it within the same framework on both ends of the wire. If people want to spend time making their own transport layers, let them, but give me one to start with. This isn't so hard because the framework is the code on both ends and must only conform to itself.

 - Flawless and transparent code sharing between the client and the server that is so good I forget it even happens. If I wrote everything in the same environment, file structure, and using the same tooling, and on its own it figured out the bare minimum to send to the client and ran it there, I would bask in happiness for eternity. This is a monumentally difficult task as far as I can tell, but this is the killer feature. Write once, run on the continuum between the server and browser, and forget that theres actually a difference between the two. Validations can be shared and server dependent ones can be run there, views are written once and pushed to the browser as needed, and SEO works fine because the page can be rendered entirely server side. Other goodness that could emerge from this: fancy algorithms compressing data which goes across the wire, client side access to all the database's capabilities, insta-REST, and absolutely tiny asset packages for download.

 - A radical departure from the jQuery mindset of DOM querying and manipulation, and use a UI kit instead. We aren't in Kansas any more folks, its time to go where every other platform has gone and use the language to its fullest. The DOM should become an implementation detail which is touched as little as possible, and developers should work with virtual, extendable view classes as they do in Cocoa,QtGui, or Swing. If we want to build desktop class applications we need to adopt the similar and proven paradigms from the desktop world. Sproutcore, Cappucino, Uki, and Qooxdoo have realized this and applied these successfully.

 - An extensive UI Kit implementing all the stuff we are used to and then some. I mean building in handy stuff like sexy autocompletes or grids with searching and pagination. This stuff is traditionally found as plugins which in the jQuery world makes sense, because it's only needed on one page of one site out of a million developed with jQuery. This is no longer the case though! Desktop class applications all need these types of super rich widgets, as evidenced by their availability in things like the iOS SDK. It's ok if the framework is feature rich (some may read this as bloated) because it will be smart and only package what it needs to send to the browser, remember? Standardizing these components in the framework makes it easy to drop your data in and get desktop class functionality with rock solid reliability quickly.

 - A departure from the routing paradigm found in Rails, Sinatra, Sammy, and Backbone. The traditional one URL maps to one controller action routing table no longer applies. Think about applications like Wave, where the page is composed of many virtual windows, each somewhat independent and which all generate a multitude of state combinations. Theres no good way to express 'the inbox is minimized, the contact info for my friend Mo is open, and Wavelet ID #5 is open for editing' in URL segments. We aren't dealing with paths in the same way we used to, the wavelet id isn't subordinate to the inbox's info or a member of the contact info folder, so why does it make sense to apply the directory having subordinate file members paradigm to this UI state? I'm not sure what it should look like, but for complex UIs with several independent state possibilities, the old way doesn't cut it.
 
 - A gorgeous creative commons theme designed by Sofa. _In my dreams_.

I see an opportunity for the next DHH to rise. I wish I were talented enough to accomplish such a feat, but alas, I tripped and fell trying to climb an escalator going in the wrong direction today, and thus convinced myself I am not the man for the job. Are you the man/woman who could do this? If so, please try! You'd have at least one zealous developer, and I'd tell all my friends about it. 

Lets get away from maintaining two separate apps and back to thinking about the future folks.
