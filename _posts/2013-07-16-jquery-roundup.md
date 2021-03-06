---
layout: post
title: "jQuery Roundup: Queuing Ajax Requests, imagefill.js, Feedback Me"
author: Alex Young
categories:
- jquery
- plugins
- ajax
- backbone.js
- images
- forms
- widgets
---

<div class="intro">
Note: You can send your plugins and articles in for review through our <a href="http://contact.dailyjs.com/project">contact form</a>.
</div>

###Queuing Ajax Requests

In [Queuing Ajax Requests in JS Web Apps](http://blog.alexmaccaw.com/queuing-ajax-requests), Alex MacCaw talks about how to queue requests with `jQuery.ajax` and safely retain the sequence of operations generated by Backbone and Spine applications.

> We still have a problem if a particular request fails, as the interface will now be out of sync with the database. I usually recommend treating this as an exceptional circumstance, and prompt the user to reload the page. Incidentally, this is exactly how Facebook and Twitter solve the problem.

The plugin can be used like this: `jQuery.ajax({type: 'POST', queue: true})`, and is available as a Gist at [maccman / jquery.ajax.queue.coffee](https://gist.github.com/maccman/5790509).

He's also recently published [Memory Management in JS Web Apps](http://blog.alexmaccaw.com/jswebapps-memory-management), which discusses how to properly clean up Backbone and Spine controllers when DOM elements are removed using jQuery's special events.

###imagefill.js

[imagefill.js](http://johnpolacek.github.io/imagefill.js/) (GitHub: [johnpolacek / imagefill.js](https://github.com/johnpolacek/imagefill.js/), License: _MIT/GPL_) by John Polacek is a jQuery plugin for making images fill their containers.  It can be configured to run once, or throttle the frequency at which container sizes are checked.  The author suggests this plugin could be useful for creating responsive sites.

###Feedback Me

Feedback Me (GitHub: [vedmack / feedback_me](https://github.com/vedmack/feedback_me), License: _MIT_, jQuery: [feedback_me](http://plugins.jquery.com/feedback_me/)) by Daniel Reznick is a UI widget that shows a feedback form that appears from the side of a page.  It is designed to work out of the box with jQuery UI and Bootstrap, and includes allows each label in the feedback form to be configured.
