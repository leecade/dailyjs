---
layout: post
title: "Node Roundup: 0.11.16, io.js 1.1.0, Faster async, HTTP Tests"
author: Alex Young
categories:
- node
- iojs
- modules
- libraries
---

###Node 0.11.16 and io.js 1.1.0

[Node 0.11.16](http://blog.nodejs.org/2015/01/30/node-v0-11-16-unstable/) is out, bringing an update to OpenSSL, npm, and some core modules: url, assert, net, and url.  The [util.inspect](https://github.com/joyent/node/commit/bcff90e0c299ce472d3e00b8f886dac8e99478bd) patch is interesting because errors generated by the assert module now get formatted with `util.inspect` instead of `JSON.stringify`.  The idea behind this patch was to avoid errors due to circular references.

io.js 1.1.0 also has this patch, but there are again lots of unique commits that aren't in Node.  Using my limited Git forensic skills, I tried to compare commits between both projects, but I haven't found a good way to do it yet.  This is what I tried:

{% highlight javascript %}
git clone https://github.com/joyent/node
cd node
git remote add iojs https://github.com/iojs/io.js
git fetch iojs
git checkout iojs/v1.x -b iojs
git branch --contains df48faf
git cherry -v origin/master iojs
{% endhighlight %}

I noticed that `git branch` showed a commit that I know is only in iojs actually in the iojs branch I made based on the `v1.x` branch from `iojs/io.js`.  However, `cherry` shows commits that are in both Node and io.js, so I'm not sure what the best way of tracking unique commits to each project.

###neo-async

I use the [async](https://www.npmjs.com/package/async) module all the time without really worrying too much about performance, but Shunsuke Hakamata sent in neo-async (GitHub: [suguru03/neo-async](https://github.com/suguru03/neo-async), License: _MIT_, npm: [neo-async](https://www.npmjs.com/package/neo-async)), a drop-in replacement for async created by Suguru Motegi which aims to improve performance.

The benchmarks have been made on major browsers, Node (stable), and io.js.  Not every method is implemented, but the results the author has generated look compelling.  If you take a look at the `async.waterfall` benchmarks, async appears to get slower based on the number of tasks, while neo-async stays flat.

###Testing Node Unit Tests

Jani Hartikainen sent in an article about [HTTP testing with Node](http://codeutopia.net/blog/2015/01/30/how-to-unit-test-nodejs-http-requests/). It covers Mocha, Sinon, and uses stubs for the built-in http module:

> Let's start by adding a test that validates the get request handling. When the code runs, it calls `http.request`. Since we stubbed it, the idea is we’ll use the stub control what happens when `http.request` is used, so that we can recreate all the scenarios without a real HTTP request ever happening.
>
> To make sure the expected data is written into it, we need a way to check `write` was called. This calls for another test double, in this case a spy. Just like real secret agents, a spy will give us information about its target – in this case, the `write` function.

You could use this as the basis for faking requests to third-party APIs in your tests.